



# ALLORA WORKER. 

![image](https://github.com/user-attachments/assets/a97b85c0-f11c-42f3-94ab-4d9022b7f34a)


### Full guide

 Category: Node
 
 Updated: 18 July, 2024
 
 Author: admin
 
 Reading time: 4 Min

```
https://rejump.dev/how-to-run-worker-allora-hugging-face-worker/#system-requirements
```

# How to run Worker Allora: Hugging Face Worker

Published: 29 June, 2024

worker node allorad

## System Requirements

To participate in the Allora Network, ensure your system meets the following requirements:

Operating System this guide: `Linux`

CPU: `2 core`

Memory: `4 GB`.

Storage: `SSD` or `NVMe` with at least `20GB` of space.

## Install dependencies, Golang, Docker

Read more: `https://rejump.dev/how-to-worker-node-on-allora-network/`

## 0. Deploying a Hugging Face Model

In this example, we will use the Chronos model: amazon/chronos-t5-tiny(opens in a new tab). Chronos is a family of pretrained time series forecasting models based on language model architectures. A time series is transformed into a sequence of tokens via scaling and quantization, and a language model is trained on these tokens using the cross-entropy loss. Once trained, probabilistic forecasts are obtained by sampling multiple future trajectories given the historical context. Chronos models have been trained on a large corpus of publicly available time series data, as well as synthetic data generated using Gaussian processes. For simplicity, we will use Zero-shot forecasting, which refers to the ability of models to generate forecasts from unseen datasets.


### 1. Install allocmd

```
pip install allocmd --upgrade
```

### 2. Initializing the worker
```
mkdir -p faceworker/worker/data/head
mkdir -p faceworker/worker/data/worker
chmod -R 777 ./faceworker/worker/data
chmod -R 777 ./faceworker/worker/data/head
chmod -R 777 ./faceworker/worker/data/worker
```


`# Topic 2, 4, 6 (ETH, BTC, SOL) provide inferences on 10mins Prediction`

`# Topic 1, 3, 5 (ETH, BTC, SOL) provide inferences on 24hours Prediction`

**Example Wokername is faceworker**

```
allocmd generate worker --name faceworker --topic 2 --env dev
cd faceworker/worker
```


### 3. Creating the inference server

![image](https://github.com/user-attachments/assets/87d0a583-d81c-4040-be6e-21911dca7611)


**`Full code for 9 topics`**

```
from flask import Flask, Response
import requests
import json
import pandas as pd
import torch
from chronos import ChronosPipeline
 
# create our Flask app
app = Flask(__name__)
 
# define the Hugging Face model we will use
model_name = "amazon/chronos-t5-tiny"
 
# define our endpoint
@app.route("/inference/<string:token>")
def get_inference(token):
    """Generate inference for given token."""
    try:
        # use a pipeline as a high-level helper
        pipeline = ChronosPipeline.from_pretrained(
            model_name,
            device_map="auto",
            torch_dtype=torch.bfloat16,
        )
    except Exception as e:
        return Response(json.dumps({"pipeline error": str(e)}), status=500, mimetype='application/json')
 
    # get the data from Coingecko
    url = "https://api.coingecko.com/api/v3/coins/"
    if token.upper() == 'ETH':
        url += "ethereum"
    if token.upper() == 'SOL':
        url += "solana"
    if token.upper() == 'BTC':
        url += "bitcoin"
    if token.upper() == 'BNB':
        url += "binancecoin"
    if token.upper() == 'ARB':
        url += "arbitrum"       
    url += "/market_chart?vs_currency=usd&days=30&interval=daily"
    
    headers = {
        "accept": "application/json",
        "x-cg-demo-api-key": "CG-XXXXXXXXXXXXXXXXXXX" # replace with your API key
    }
 
    response = requests.get(url, headers=headers)
    if response.status_code == 200:
        data = response.json()
        df = pd.DataFrame(data["prices"])
        df.columns = ["date", "price"]
        df["date"] = pd.to_datetime(df["date"], unit = "ms")
        df = df[:-1] # removing today's price
        print(df.tail(5))
    else:
        return Response(json.dumps({"Failed to retrieve data from the API": str(response.text)}), 
                        status=response.status_code, 
                        mimetype='application/json')
 
    # define the context and the prediction length
    context = torch.tensor(df["price"])
    prediction_length = 1
 
    try:
        forecast = pipeline.predict(context, prediction_length)  # shape [num_series, num_samples, prediction_length]
        print(forecast[0].mean().item()) # taking the mean of the forecasted prediction
        return Response(str(forecast[0].mean().item()), status=200)
    except Exception as e:
        return Response(json.dumps({"error": str(e)}), status=500, mimetype='application/json')
 
# run our Flask app
if __name__ == '__main__':
    app.run(host="0.0.0.0", port=8000, debug=True)
```


Register `API_KEY` on Coingecko: `https://www.coingecko.com/en/api/pricing`

![image](https://github.com/user-attachments/assets/dd13fa45-55a5-4dcc-beb3-9e14f0a5fe00)



```
headers = {
    "accept": "application/json",
    "x-cg-demo-api-key": "CG-API_KEY" # replace with your API key
}
```


### 4. Modifying requirements.txt

```
flask[async]
gunicorn[gthread]
transformers[torch]
pandas
git+https://github.com/amazon-science/chronos-forecasting.git
```


### 5. Modifying main.py to call the inference server

`PYTHON`

```
import requests
import sys
import json
 
def process(argument):
    headers = {'Content-Type': 'application/json'}
    url = f"http://inference:8000/inference/{argument}"
    response = requests.get(url, headers=headers)
    return response.text
 
if __name__ == "__main__":
    # Your code logic with the parsed argument goes here
    try:
        if len(sys.argv) < 5:
            value = json.dumps({"error": f"Not enough arguments provided: {len(sys.argv)}, expected 4 arguments: topic_id, blockHeight, blockHeightEval, default_arg"})
        else:
            topic_id = sys.argv[1]
            blockHeight = sys.argv[2]
            blockHeightEval = sys.argv[3]
            default_arg = sys.argv[4]
            
            response_inference = process(argument=default_arg)
            response_dict = {"infererValue": response_inference}
            value = json.dumps(response_dict)
    except Exception as e:
        value = json.dumps({"error": {str(e)}})
    print(value)
```


### 6. Updating the Docker

Modifying `Dockerfile`

```
FROM alloranetwork/allora-inference-base:latest
 
RUN pip install requests
 
COPY main.py /app/
```


Create new Dockerfile_inference 

```
FROM amd64/python:3.9-buster
 
WORKDIR /app
 
COPY . /app
 
# Install any needed packages specified in requirements.txt
RUN pip install --upgrade pip \
    && pip install -r requirements.txt
 
EXPOSE 8000
 
ENV NAME sample
 
# Run gunicorn when the container launches and bind port 8000 from app.py
CMD ["gunicorn", "-b", ":8000", "app:app"]
```


### 7. Update config

![image](https://github.com/user-attachments/assets/9414d56c-cf0f-4051-9a11-7c503a53a13f)


Update your `hex_coded_pk`:

```
allorad keys export faceworker --keyring-backend test --unarmored-hex --unsafe
```

Update `boot_nodes`: `07/010/2024`

```
/dns4/head-2-p2p.testnet.allora.network/tcp/32092/p2p/12D3KooWDfXfD7TV7YnVu14fafcakjn74DLS6z4zTs4b1HSBWZ5i,/dns4/head-0-p2p.edgenet.allora.network/tcp/32080/p2p/12D3KooWQgcJ4wiHBWE6H9FxZAVUn84ZAmywQdRk83op2EibWTiZ,/dns4/head-1-p2p.edgenet.allora.network/tcp/32081/p2p/12D3KooWCyao1YJ9DDZEAV8ZUZ1MLLKbcuxVNju1QkTVpanN9iku,/dns4/head-2-p2p.edgenet.allora.network/tcp/32082/p2p/12D3KooWKZYNUWBjnAvun6yc7EBnPvesX23e5F4HGkEk1p5Q7JfK
```


Check it if they update new heads: `https://github.com/allora-network/networks/blob/main/edgenet/heads.txt`

### 8. Initializing the worker for production

```
allocmd generate worker --env prod
chmod -R +rx ./data/scripts
```

### 9. Update some bugs

Edit file `prod-docker-compose.yaml`

Add the inference service in the `prod-docker-compose.yaml` before the `worker` and the `head` services:

```
  inference:
    container_name: inference-hf
    build:
      context: .
      dockerfile: Dockerfile_inference
    command: python -u /app/app.py
    ports:
      - "8000:8000"
```

Change `--allora-chain-topic-id` to `number of topic`

![image](https://github.com/user-attachments/assets/0004cd60-7d49-4f5a-9759-9fb2b7c7a818)


### Final: Run a Worker Node

![image](https://github.com/user-attachments/assets/d5ca4ec3-69a1-45d1-9ff0-12f33ac0e061)


```
#BUILD AND RUN BACKGROUND
docker compose -f prod-docker-compose.yaml up --build -d
```

```
#VIEW LOGS 
docker compose -f prod-docker-compose.yaml logs -f
```

Check your worker on chain like this

![image](https://github.com/user-attachments/assets/30b7d23c-061a-4507-bb58-4a5d01740f7e)



Categorized in: Node

# END
