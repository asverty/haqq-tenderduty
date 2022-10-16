![haqq-logo-vertical be253a76](https://user-images.githubusercontent.com/65398578/196053994-fac5700d-ece2-4eae-a25b-208f22ca9fd4.png)
# Haqq ecosystem serves the Muslim community and promotes Islamic values. For details visit https://islamiccoin.net/
# Haqq tenderduty monitoring
### All actions for root (in case with users use `sudo`)
#### Update & upgrade os
```
apt update && sudo apt upgrade -y
```
#### Set utilities
```
apt install curl build-essential git wget jq make gcc tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
```
#### Docker install
```
apt install apt-transport-https ca-certificates curl software-properties-common -y
```
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
```
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
```
```
apt update
```
```
apt-cache policy docker-ce
```
```
apt install docker-ce -y
```
```
docker --version
```
#### Create folder and get config file
```
mkdir tenderduty && cd tenderduty
```
```
docker run --rm ghcr.io/blockpane/tenderduty:latest -example-config >config.yml
```
#### Edit config file (api key for telegram bot / telegram channel id / name of network / chain id / valoper address / rpc point)
```
# Telegram settings
telegram:
  # Alert via telegram? Note: also supersedes chain-specific settings
  enabled: no
  # API key ... talk to @BotFather
  api_key: '5555555555:AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA'
  # The group ID for the chat where messages will be sent. Google how to find this, will include better info later.
  channel: "-666666666"
```

```
# The various chains to be monitored. Create a new entry for each chain. The name itself can be arbitrary, but a
# user-friendly name is recommended.
chains:

  # The user-friendly name that will be used for labels. Highly suggest wrapping in quotes.
  "Haqq":
    # chain_id is validated for a match when connecting to an RPC endpoint, also used as a label in several places.
    chain_id: haqq_54211-3
    # Hooray, in v2 we derive the valcons from abci queries so you don't have to jump through hoops to figure out how
    # to convert ed25519 keys to the appropriate bech32 address
    valoper_address: haqqvaloper1qeg22wec94yauguddjc6eqrd4l45d4yguzxahx
```

```
# This section covers our RPC providers. No LCD (aka REST) endpoints are used, only TM's RPC endpoints
    # Multiple hosts are encouraged, and will be tried sequentially until a working endpoint is discovered.
    nodes:
      # URL for the endpoint. Must include protocol://hostname:port
      - url: https://rpc.tm.testedge2.haqq.network:443
```
#### Run container
```
docker run -d --name tndrdt -p "8888:8888" -p "28686:28686" --restart unless-stopped -v $(pwd)/config.yml:/var/lib/tenderduty/config.yml ghcr.io/blockpane/tenderduty:latest
```
#### Check logs
```
docker logs -f --tail 20 tenderduty
```
#### Check information in browser (insert your ip of tenderduty server instead 000 and don't delete port 8888)
```
http://000.000.008.800:8888/
```

