
# Validator

## Install Autonity
```
sudo apt install curl -y && source <(curl -s https://github.com/CzSpiker/TESTNET/blob/main/AUTONITY/ANOTHER/auto_install)
```
```
aut account new --keyfile ./piccadilly-keystore/wallet.key
```
If you already have wallet before
**Upload wallet.key to folder: piccadilly-keystore**

Check wallet 
```
aut account info
```

## Install Oracle

```
aut account new --keyfile ./piccadilly-keystore/oracle.key > ./piccadilly-keystore/oracle.txt
```
#### change xxx is pass of oracle
```
pass_oracle="xxx" && sudo apt install curl -y && source <(curl -s https://github.com/CzSpiker/TESTNET/blob/main/AUTONITY/ANOTHER/oracle_install)
```
#### Config oracle, register all site and get key api

https://currencyfreaks.com

https://openexchangerates.org

https://currencylayer.com

https://www.exchangerate-api.com

```
nano ${HOME}/autonity-oracle/plugins-conf.yml
```
Config same bellow with your api

<img src="https://docs.nodesync.top/~gitbook/image?url=https:%2F%2F2585830168-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FRzXvi3emVKzTi94K7StD%252Fuploads%252F5LO0BQcmnaJUGFU0sFNl%252Fautonity-oracleconfig.JPG%3Falt=media%26token=c1925e07-dc44-4a7d-b627-a35b668d215d&width=768&dpr=1&quality=100&sign=4672a88bfd495b89047afbb4de46a1e84d549074f3aa4ac684b1f93605d4f21e">

# Start Autonity Client and Oracle

Client
```
sudo systemctl daemon-reload
sudo systemctl enable autonityd
sudo systemctl restart autonityd
```
Oracle
```
sudo systemctl daemon-reload
sudo systemctl enable autoracled
sudo systemctl restart autoracled
```
# Check logs:
```
sudo journalctl -u autonityd -f -o cat
```
```
sudo journalctl -u autoracled -f -o cat
```
