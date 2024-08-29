# Story-Snapshot

### Install tool
```
sudo apt-get install wget lz4 aria2 pv -y
```

### Stop node
```
sudo systemctl stop story
sudo systemctl stop story-geth
```

### Download Geth-data
```
cd $HOME
if curl -s --head https://vps5.josephtran.xyz/Story/Geth_snapshot.lz4 | head -n 1 | grep "200" > /dev/null; then
    echo "Snapshot found, downloading..."
    aria2c -x 16 -s 16 https://vps5.josephtran.xyz/Story/Geth_snapshot.lz4 -o Geth_snapshot.lz4
else
    echo "No snapshot found."
fi
```

### Download Story-data
```
cd $HOME
if curl -s --head https://vps5.josephtran.xyz/Story/Story_snapshot.lz4 | head -n 1 | grep "200" > /dev/null; then
    echo "Snapshot found, downloading..."
    aria2c -x 16 -s 16 https://vps5.josephtran.xyz/Story/Story_snapshot.lz4 -o Story_snapshot.lz4
else
    echo "No snapshot found."
fi
```

### Remove old data
```
rm -rf ~/.story/story/data
rm -rf ~/.story/geth/iliad/geth/chaindata
```

### Extract Story-data
```
sudo mkdir -p /root/.story/story/data
lz4 -d Story_snapshot.lz4 | pv | sudo tar xv -C /root/.story/story/
```

### Extract Geth-data
```
sudo mkdir -p /root/.story/geth/iliad/geth/chaindata
lz4 -d Geth_snapshot.lz4 | pv | sudo tar xv -C /root/.story/geth/iliad/geth/
```

### Restart node
```
sudo systemctl start story
sudo systemctl start story-geth
```
# END
