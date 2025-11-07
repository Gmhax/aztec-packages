# Aztec Monorepo

All the packages that make up [Aztec](https://docs.aztec.network).

- [**`barretenberg`**](/noir-projects): The ZK prover backend that provides succinct verifiability for Aztec. Also houses the Aztec VM.
- [**`l1-contracts`**](/l1-contracts): Solidity code for the Ethereum contracts that process rollups
- [**`noir-projects`**](/noir-projects): Noir code for Aztec contracts and protocol circuits.
- [**`yarn-project`**](/yarn-project): Typescript code for client and backend
- [**`docs`**](/docs): Documentation source for the docs site

## Popular packages

- [Aztec.nr](./noir-projects/aztec-nr/): A [Noir](https://noir-lang.org) framework for smart contracts on Aztec.
- [Aztec](./yarn-project/aztec/): A package for starting up local dev net modules, including a local 'sandbox' devnet, an Ethereum network, deployed rollup contracts and Aztec execution environment.
- [Aztec.js](./yarn-project/aztec.js/): A tool for interacting with the Aztec network. It communicates via the [Private Execution Environment (PXE)](./yarn-project/pxe/).
- [Example contracts](./noir-projects/noir-contracts/): Example contracts for the Aztec network, written in Noir.
- [End to end tests](./yarn-project/end-to-end/): Integration tests written in Typescript--a good reference for how to use the packages for specific tasks.
- [Aztec Boxes](./boxes/): Example starter projects.

## DeepWiki

In addition to the docs website, you can "talk" with this repo using [DeepWiki](https://deepwiki.com):

[![Ask DeepWiki](https://deepwiki.com/badge.svg)](https://deepwiki.com/AztecProtocol/aztec-packages)

## Issues Board

All issues being worked on are tracked on the [Aztec Github Project](https://github.com/orgs/AztecProtocol/projects/22). For a higher-level roadmap, check the [milestones overview](https://aztec.network/roadmap) section of our website.

## Debugging

Logging goes through the [Logger](yarn-project/foundation/src/log/) module in Typescript. `LOG_LEVEL` controls the default log level, and one can set alternate levels for specific modules, such as `debug; warn: module1, module2; error: module3`.

## Releases

Releases are driven by [release-please](https://github.com/googleapis/release-please), which maintains a 'Release PR' containing an updated CHANGELOG.md since the last release. Triggering a new release is simply a case of merging this PR to master. A [github workflow](./.github/workflows/release-please.yml) will create the tagged release triggering ./bootstrap.sh release to build and deploy the version at that tag.

## Contribute

There are many ways you can participate and help build high quality software. Check out the [contribution guide](CONTRIBUTING.md)!

## Syncing noir

We use marker commits and [git-subrepo](https://github.com/ingydotnet/git-subrepo) (for a subset of its intended use) to manage a mirror of noir. This tool was chosen because it makes code checkout and development as simple as possible (compared to submodules or subtrees), with the tradeoff of complexity around sync's.

## Development and CI

For a broad overview of the CI system take a look at [CI.md](CI.md).

For some deeper information on individual scripts etc (for developing CI itself), take a look at [ci3/README.md](ci3/README.md).



## 🔃 Update Sequencer Node (if you still encounter missed)

### Update docker-compose method Nodes
1- Stop node
```console
docker stop $(docker ps -q --filter "ancestor=aztecprotocol/aztec") && docker rm $(docker ps -a -q --filter "ancestor=aztecprotocol/aztec")

# Or

cd aztec
docker compose down -v
```

2- Delete old data
```bash
rm -rf ~/.aztec/alpha-testnet/data/
```

3- Update CLI commands
```bash
source ~/.bashrc
```

4- Open `docker-compose.yml`
```bash
nano docker-compose.yml
```
- Update private key:
* Update `VALIDATOR_PRIVATE_KEY: ${VALIDATOR_PRIVATE_KEY}` under `environment` with the following:
```
VALIDATOR_PRIVATE_KEYS: ${VALIDATOR_PRIVATE_KEYS}
```
* We added `s`

5- Open `.env`
```
nano .env
```
- Update private key:
* Update `VALIDATOR_PRIVATE_KEY` to `VALIDATOR_PRIVATE_KEYS`

4- Rerun your node
```
docker compose up -d
```


### CLI Method
* 1- Update your CLI start command to use `--sequencer.validatorPrivateKeys` (see added `s`) instead of `--sequencer.validatorPrivateKey`

Command:
- Open screen session
- Press Ctrl +C

Execute:
```
aztec start --node --archiver --sequencer \
  --network testnet \
  --l1-rpc-urls RPC_URL  \
  --l1-consensus-host-urls BEACON_URL \
  --sequencer.validatorPrivateKeys "0xPrivatekey1,0xPrivatekey2,0xPrivatekey3" \
  --sequencer.coinbase 0xYourAddress \
  --p2p.p2pIp IP
```



-------------------------------------------------------------------------------------------------------------------


### Run Sequencer Node

## Hardware Requirements
<table>
  <tr>
    <th colspan="3"> Sequencer Node HW Requirements </th>
  </tr>
  <tr>
    <td>RAM</td>
    <td>CPU</td>
    <td>Disk</td>
  </tr>
  <tr>
    <td><code>8-16 GB</code></td>
    <td><code>4-9 cores</code></td>
    <td><code>100+ GB SSD</code></td>
  </tr>
</table>


NOTE: This guide is for who passing ZKpassport, Register your each wallet address para malist sa queue. (Do it manually or reach the team on discord)
- Funds your per wallet 0.2 sepolia. 
### Method 1: Run via Docker

## 1. Install Dependecies
* Update packages:
```bash
sudo apt-get update && sudo apt-get upgrade -y
```

* Install Packages:
```bash
sudo apt install curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev  -y
```

* Install Docker: (if you have already docker ignore this)
```bash
sudo apt update -y && sudo apt upgrade -y
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done

sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update -y && sudo apt upgrade -y

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Test Docker
sudo docker run hello-world

sudo systemctl enable docker
sudo systemctl restart docker
```

---

## 2. Install Aztec Tools
```bash
bash -i <(curl -s https://install.aztec.network)
```
```bash
echo 'export PATH="$HOME/.aztec/bin:$PATH"' >> ~/.bashrc

source ~/.bashrc
```
* **Restart your Terminal** now to apply changes.
* Check if you installed successfully:
```bash
aztec
```

---

## 3. Update Aztec
```bash
aztec-up latest
aztec-up 2.0.2
```

## 4. Enable Firewall & Open Ports
```console
# Firewall
ufw allow 22
ufw allow ssh
ufw enable

# Sequencer
ufw allow 40400
ufw allow 8080
```

* Create `aztec` directory:
```bash
mkdir aztec
```

# * Get into `aztec` directory:
```bash
cd aztec
```
 
* 5. Create `.env`
```bash
nano .env
```

* Replace the following code in `.env`
```env
ETHEREUM_RPC_URL=RPC_URL
CONSENSUS_BEACON_URL=BEACON_URL
VALIDATOR_PRIVATE_KEYS=0xPrivatekey1
COINBASE=0xYourAddress
P2P_IP=P2P_IP
```
* Replace the following variables before you Run Node:
  * `RPC_URL` & `BEACON_URL`: Step 4
  * `0xYourPrivateKey`: Your EVM wallet private key starting with `0xPrivatekey1,0xPrivatekey2,0xPrivatekey3`
  * `0xYourAddress`: Your EVM wallet public address starting with `0x...`
  * `P2P_IP`: Your server IP (Step 7)


* 6. Create `docker-compose.yml`:
```bash
nano docker-compose.yml
```

* Replace the following code in `docker-compose.yml`
```yml
services:
  aztec-node:
    container_name: aztec-sequencer
    image: aztecprotocol/aztec:2.0.2
    restart: unless-stopped
    environment:
      ETHEREUM_HOSTS: ${ETHEREUM_RPC_URL}
      L1_CONSENSUS_HOST_URLS: ${CONSENSUS_BEACON_URL}
      DATA_DIRECTORY: /data
      VALIDATOR_PRIVATE_KEYS: ${VALIDATOR_PRIVATE_KEYS}
      COINBASE: ${COINBASE}
      P2P_IP: ${P2P_IP}
      LOG_LEVEL: info
    entrypoint: >
      sh -c 'node --no-warnings /usr/src/yarn-project/aztec/dest/bin/index.js start --network testnet --node --archiver --sequencer'
    ports:
      - 40400:40400/tcp
      - 40400:40400/udp
      - 8080:8080
    volumes:
      - /root/.aztec/testnet/data/:/data
```


* Run Node Docker:
```bash
docker compose up -d
```

* Node Logs:
 ```bash
docker compose logs -fn 1000
```


# Done





--------------------


## Run Multiple Validators
This step seems limited to only teams and individuals in active set. Team is encouraging teams to run 10 validators. Ask the team if you are going to run more validators

### Docker Method
1- Open `docker-compose.yml`
```
cd aztec
docker compose down -v
```

2- Add publisher key variable:
* Adding a publisher wallet will make you handle all the transactions of your validators with on wallet


3- Open `.env`
```
nano .env
```

4- Update private key:
* Update `VALIDATOR_PRIVATE_KEY` to `VALIDATOR_PRIVATE_KEYS`
* Values of `VALIDATOR_PRIVATE_KEYS` must be a comma (`,`) separated list. (`"0xPrivatkey,0xPrivatkey,0xPrivatkey"`)
* Coinbase field - Put one address only (ilagay mo lang yung wallet address pasok na sa validator set)

Execute: 
```
docker compose up -d
```

## Done for docker



### CLI Method
* 1- Update your CLI start command to use `--sequencer.validatorPrivateKeys` (see added `s`) instead of `--sequencer.validatorPrivateKey` if you want to run multiple validators.
  * The value of this should be a comma (`,`) separated list.
   

Example:
```
aztec start --node --archiver --sequencer \
  --network alpha-testnet \
  --l1-rpc-urls RPC_URL  \
  --l1-consensus-host-urls BEACON_URL \
  --sequencer.validatorPrivateKeys "0xPrivatekey1,0xPrivatekey2,0xPrivatekey3" \
  --sequencer.coinbase 0xYourAddress \
  --p2p.p2pIp IP
```
* Coinbase field - Put one address only (ilagay mo lang yung wallet address pasok na sa validator set)
## Done for CLI

NOTE: Register your added wallet na hindi pa listed sa queue.
* Do it manually or reach the team on discord.


----------------------------------------------------------------------------------------------------------------


# Update Sequencer Node v1.2.1

## Update docker-compose method Nodes

1. Stop node
```
docker stop $(docker ps -q --filter "ancestor=aztecprotocol/aztec") && docker rm $(docker ps -a -q --filter "ancestor=aztecprotocol/aztec")

# Or

cd aztec
docker compose down -v
```

2. Update CLI commands
```
source ~/.bashrc
aztec-up 1.2.1
```

3. Delete old data
```
rm -rf ~/.aztec/alpha-testnet/data/
```

4. Edit docker-compose.yml
```
nano docker-compose.yml
```
- Edit this line - image: aztecprotocol/aztec:1.2.0 to image: aztecprotocol/aztec:1.2.1

5. Re-run your node:
```
docker compose up -d
```

Check logs:
```
docker compose logs -fn 1000
```

## Done for Docker method

# Update CLI method Nodes

1. Stop node
```
screen -ls | grep -i aztec | awk '{print $1}' | xargs -I {} screen -X -S {} quit
```

2. Update CLI commands
```
source ~/.bashrc
aztec-up 1.2.0
```
3. Delete old data
```
rm -rf ~/.aztec/alpha-testnet/data/
```
- Create Session
```
screen -S aztec
```
# 4. Rerun using this CLI command
```
aztec start --node --archiver --sequencer \
  --network alpha-testnet \
  --l1-rpc-urls RPC_URL  \
  --l1-consensus-host-urls BEACON_URL \
  --sequencer.validatorPrivateKeys 0xYourPrivateKey \
  --sequencer.coinbase 0xYourAddress \
  --p2p.p2pIp IP
```
Replace the following variables before you Run the node:

- RPC_URL & BEACON_URL: Step 4
- 0xYourPrivateKey: Your EVM wallet private key starting with 0x...
- 0xYourAddress: Your EVM wallet public address starting with 0x...
- IP: Your server IP (Step 7)

# If you’re still getting the INT value type cannot accept a floating-point value error, try running this command:

<img width="1222" height="665" alt="image" src="https://github.com/user-attachments/assets/9ef771d6-bbfc-462e-803e-0f5e3d6594aa" />

## Docker method. 

-  Get into `aztec` directory & reset your data.

```bash
cd ~/aztec && \
docker compose down && \
rm -rf ~/.aztec/testnet/data/ && \
sed -i "s|--sequencer'|--sequencer --snapshots-url https://snapshots.aztec.graphops.xyz/files/'|" docker-compose.yml && \
docker compose pull && \
docker compose up -d
```

## CLI method

- Stop your node: Ctrl + C

```
rm -rf ~/.aztec/testnet/data/
```

```
aztec start --node --archiver --sequencer \
  --network testnet \
  --l1-rpc-urls <your_l1_rpc_urls> \
  --l1-consensus-host-urls <your_l1_consensus_host_urls> \
  --sequencer.validatorPrivateKeys <your_private_key> \
  --sequencer.coinbase <your_coinbase_address> \
  --p2p.p2pIp <your_ip> \
  --snapshots-url https://snapshots.aztec.graphops.xyz/files/
```



# Honk if you plonk


# Aztec Labs just published 2.0.2.

## Please update your node as soon as possible (no later than 48 hours from now) or risk getting slashed ✂️ .

- Open directories 
```
cd aztec && docker compose down -v
```

- Open `docker-compose.yml`:
```
nano docker-compose.yml
```
- Edit to this in ur yml file:

- Image: ``` aztecprotocol/aztec:2.0.2```
- entrypoint: > ```sh -c 'node --no-warnings /usr/src/yarn-project/aztec/dest/bin/index.js start --network testnet --node --archiver --sequencer'```
- Volumes ```- /root/.aztec/testnet/data/:/data```

- save it-Ctrl + O , Press Enter , Ctrl + X

```
docker compose up -d
```
```
docker compose logs -f --tail=100
```

- Note: if you encounter "WARN: sequencer Cannot propose block 1 at next L2 slot 1005 since the committee does not exist on L1"  just ignored

- Done

# Aztec update:
For those experiencing errors

<img width="1082" height="1000" alt="image" src="https://github.com/user-attachments/assets/b719f2d0-aa1c-4aba-8172-b6419721986f" />

## Docker:
```
cd aztec && docker compose down -v
```
```
docker compose up -d
```
```
docker compose logs -f --tail=100
```

## CLI 

- Stop node
```
screen -ls | grep -i aztec | awk '{print $1}' | xargs -I {} screen -X -S {} quit
```
- Delete data
```
rm -rf ~/.aztec/alpha-testnet/data/
```
- Create screen session
```
screen -S aztec
```

- Rerun:
```
aztec start --node --archiver --sequencer \
  --network alpha-testnet \
  --l1-rpc-urls RPC_URL  \
  --l1-consensus-host-urls BEACON_URL \
  --sequencer.validatorPrivateKeys 0xYourPrivateKey \
  --sequencer.coinbase 0xYourAddress \
  --p2p.p2pIp IP
```

- Dettach session: Ctrl A+D


# DONE 


## Aztec voting update:
```
cd aztec
docker compose down
rm -rf ~/.aztec/testnet/data/
```

- Allow 8880 port
```
sudo ufw allow 8880
```
- edit docker compose.yml
```
nano docker-compose.yml
```
- Paste this:
```
services:
  aztec-node:
    container_name: aztec-sequencer
    image: aztecprotocol/aztec:2.0.2
    restart: unless-stopped
    environment:
      ETHEREUM_HOSTS: ${ETHEREUM_RPC_URL}
      L1_CONSENSUS_HOST_URLS: ${CONSENSUS_BEACON_URL}
      DATA_DIRECTORY: /data
      VALIDATOR_PRIVATE_KEYS: ${VALIDATOR_PRIVATE_KEYS}
      COINBASE: ${COINBASE}
      P2P_IP: ${P2P_IP}
      LOG_LEVEL: info
    entrypoint: >
      sh -c 'node --no-warnings /usr/src/yarn-project/aztec/dest/bin/index.js start --network testnet --node --archiver --sequencer'
    ports:
      - 40400:40400/tcp
      - 40400:40400/udp
      - 8080:8080
      - 8880:8880 
    volumes:
      - /root/.aztec/testnet/data/:/data
```
- CTRL + O → Enter → CTRL + X

- Rerun
```
docker compose up -d
```
- Once it’s stable (you can wait 2–3 minutes more)
- Ctrl + C
- Paste this command:
```
curl -X POST http://localhost:8880 \
  -H 'Content-Type: application/json' \
  -d '{
    "jsonrpc":"2.0",
    "method":"nodeAdmin_setConfig",
    "params":[{"governanceProposerPayload":"0x9D8869D17Af6B899AFf1d93F23f863FF41ddc4fa"}],
    "id":1
  }'
```

- Confirm if the vote/proposal was applied:
```
docker logs -n 50 aztec-sequencer | grep governance
```
- If you see an image like the one below, it means your proposal has been applied.
<img width="1382" height="820" alt="image" src="https://github.com/user-attachments/assets/15f1accb-8f3e-4da6-bad1-3907b0be6abe" />



## DONE BRO


## Step-by-Step to update your RPC 

## Reth RPC guide
- Edit the file
```
cd ~/Ethereum
nano docker-compose.yml
```

- Replace everything with this updated content:
```
services:
  reth:
    image: ghcr.io/paradigmxyz/reth:v1.8.2
    container_name: reth
    restart: unless-stopped
    volumes:
      - ./Execution:/data
      - ./jwt.hex:/data/jwt.hex
    command:
      - node
      - --chain=sepolia
      - --full
      - --datadir=/data
      - --http
      - --http.addr=0.0.0.0
      - --http.api=eth,net,web3,admin
      - --http.corsdomain=*
      - --ws
      - --ws.addr=0.0.0.0
      - --ws.api=eth,net,web3,admin
      - --authrpc.addr=0.0.0.0
      - --authrpc.port=8551
      - --authrpc.jwtsecret=/data/jwt.hex
    ports:
      - 8545:8545
      - 8546:8546

  prysm:
    image: gcr.io/prysmaticlabs/prysm/beacon-chain:v6.1.2
    container_name: prysm
    restart: unless-stopped
    depends_on:
      - reth
    volumes:
      - ./Consensus:/data
      - ./jwt.hex:/data/jwt.hex
    command:
      - --sepolia
      - --datadir=/data
      - --execution-endpoint=http://reth:8551
      - --jwt-secret=/data/jwt.hex
      - --rpc-host=0.0.0.0
      - --grpc-gateway-host=0.0.0.0
      - --blob-storage-layout=by-epoch
      - --checkpoint-sync-url=https://checkpoint-sync.sepolia.ethpandaops.io
      - --genesis-beacon-api-url=https://checkpoint-sync.sepolia.ethpandaops.io
      - --accept-terms-of-use
      - --subscribe-all-data-subnets     # ✅ Required for Fusaka / Aztec Supernode mode
    ports:
      - 3500:3500
      - 4000:4000
```
- Changes made:
- Locked Reth to v1.8.2
- Locked Prysm to v6.1.2
- Added --subscribe-all-data-subnets for Prysm (supernode flag required by Aztec)
- Cleaned formatting for easier reading

## Save and exit
CTRL + O → Enter → CTRL + X

## Pull and restart the clients
```
docker compose pull && docker compose up -d
```

## Verify versions
```
docker exec -it reth reth --version && docker exec -it prysm /app/cmd/beacon-chain/beacon-chain --version
```
<img width="1080" height="150" alt="image" src="https://github.com/user-attachments/assets/930c9ac5-17fc-4fe0-b99a-f1c610444a84" />



## Geth RPC guide
- Edit the file
```
cd ~/Ethereum
nano docker-compose.yml
```

- Replace everything with this updated content:
```
services:
    geth:
        image: ethereum/client-go:v1.16.4
        container_name: geth
        network_mode: host
        restart: unless-stopped
        ports:
            - 30303:30303
            - 30303:30303/udp
            - 8545:8545
            - 8546:8546
            - 8551:8551
        volumes:
            - /root/ethereum/execution:/data
            - /root/ethereum/jwt.hex:/data/jwt.hex
        command:
            - --sepolia
            - --http
            - --http.api=eth,net,web3
            - --http.addr=0.0.0.0
            - --authrpc.addr=0.0.0.0
            - --authrpc.vhosts=*
            - --authrpc.jwtsecret=/data/jwt.hex
            - --authrpc.port=8551
            - --syncmode=snap
            - --datadir=/data
        logging:
            driver: "json-file"
            options:
                max-size: "10m"
                max-file: "3"

    prysm:
        image: gcr.io/prysmaticlabs/prysm/beacon-chain:v6.1.2
        container_name: prysm
        network_mode: host
        restart: unless-stopped
        volumes:
            - /root/ethereum/consensus:/data
            - /root/ethereum/jwt.hex:/data/jwt.hex
        depends_on:
            - geth
        ports:
            - 4000:4000
            - 3500:3500
        command:
            - --sepolia
            - --accept-terms-of-use
            - --datadir=/data
            - --disable-monitoring
            - --rpc-host=0.0.0.0
            - --execution-endpoint=http://127.0.0.1:8551
            - --jwt-secret=/data/jwt.hex
            - --rpc-port=4000
            - --grpc-gateway-corsdomain=*
            - --grpc-gateway-host=0.0.0.0
            - --grpc-gateway-port=3500
            - --min-sync-peers=3
            - --checkpoint-sync-url=https://checkpoint-sync.sepolia.ethpandaops.io
            - --genesis-beacon-api-url=https://checkpoint-sync.sepolia.ethpandaops.io
            - --subscribe-all-data-subnets     # ✅ Required for Fusaka / Aztec Supernode mode
        logging:
            driver: "json-file"
            options:
                max-size: "10m"
                max-file: "3"
```
- Changes made:
- Locked Reth to v1.8.2
- Locked Prysm to v6.1.2
- Added --subscribe-all-data-subnets for Prysm (supernode flag required by Aztec)
- Cleaned formatting for easier reading

## Save and exit
CTRL + O → Enter → CTRL + X

## Pull and restart the clients
```
docker compose pull && docker compose up -d
```

## Verify versions
```
docker exec -it geth geth version &&
docker exec -it prysm /app/cmd/beacon-chain/beacon-chain --version

```
<img width="1080" height="150" alt="image" src="https://github.com/user-attachments/assets/930c9ac5-17fc-4fe0-b99a-f1c610444a84" />



# Sequencer Update Guide v2.1.2

## Update docker-compose method Nodes

1. Stop node
```
docker stop $(docker ps -q --filter "ancestor=aztecprotocol/aztec") && docker rm $(docker ps -a -q --filter "ancestor=aztecprotocol/aztec")

# Or

cd aztec
docker compose down -v
```

2. Update CLI commands
```
source ~/.bashrc
aztec-up 2.0.4
```

3. Delete old data
```
rm -rf ~/.aztec/testnet/data/
```

4. Edit docker-compose.yml
```
nano docker-compose.yml
```
- Edit this line - image: aztecprotocol/aztec:2.0.2 to image: aztecprotocol/aztec:2.0.4

5. Re-run your node:
```
docker compose up -d
```

Check logs:
```
docker compose logs -fn 1000
```

## Done for Docker method

# Update CLI method Nodes

1. Stop node
```
screen -ls | grep -i aztec | awk '{print $1}' | xargs -I {} screen -X -S {} quit
```

2. Update CLI commands
```
source ~/.bashrc
aztec-up 2.0.4
```
3. Delete old data
```
rm -rf ~/.aztec/testnet/data/
```
- Create Session
```
screen -S aztec
```
# 4. Rerun using this CLI command
```
aztec start --node --archiver --sequencer \
  --network testnet \
  --l1-rpc-urls RPC_URL  \
  --l1-consensus-host-urls BEACON_URL \
  --sequencer.validatorPrivateKeys 0xYourPrivateKey \
  --sequencer.coinbase 0xYourAddress \
  --p2p.p2pIp IP
```
Replace the following variables before you Run the node:

- RPC_URL & BEACON_URL: Step 4
- 0xYourPrivateKey: Your EVM wallet private key starting with 0x...
- 0xYourAddress: Your EVM wallet public address starting with 0x...
- IP: Your server IP (Step 7)



## Governance Proposal:
```
cd aztec
docker compose down
rm -rf ~/.aztec/testnet/data/
```

- Allow 8880 port
```
sudo ufw allow 8880
```
- edit docker compose.yml
```
nano docker-compose.yml
```
- Add this port:
```
- 8880:8880 
```
- CTRL + O → Enter → CTRL + X

- Rerun
```
docker compose up -d
```
- Once it’s stable (you can wait 2–3 minutes more)
- Ctrl + C
- Paste this command:
```
curl -X POST http://0.0.0.0:8880 \
  -H 'Content-Type: application/json' \
  -d '{
    "jsonrpc":"2.0",
    "method":"nodeAdmin_setConfig",
    "params":[{"governanceProposerPayload":"0xDCd9DdeAbEF70108cE02576df1eB333c4244C666"}],
    "id":1
  }'
```

- Confirm if the vote/proposal was applied:
```
docker logs -n 50 aztec-sequencer | grep governance
```


# Sequencer update v2.1.2
## Stop previous sequencer
```
cd ~/aztec && \
docker compose down && \
rm -rf /root/.aztec/testnet/data && \
sed -i 's|^ *image: aztecprotocol/aztec:.*|    image: aztecprotocol/aztec:2.1.2|' docker-compose.yml && \
sed -i 's|--network alpha-testnet|--network testnet|g' docker-compose.yml && \
docker compose pull
```
- Type: `cd`
## Download the new update
```
bash -i <(curl -s https://install.aztec.network)
echo 'export PATH="$HOME/.aztec/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
aztec-up latest
aztec-up 2.1.2
```
## Download and install Foundryup (the installer)
```
 curl -L https://foundry.paradigm.xyz | bash
 source ~/.bashrc
 foundryup
```

- Export your RPC
```export ETH_RPC=https://your_rpc_here```
- Export your Pk
```export PRIVATE_KEY_OF_OLD_SEQUENCER=yoursequencerPK```
## Approve the 200k STAKE
```
cast send 0x139d2a7a0881e16332d7D1F8DB383A4507E1Ea7A "approve(address,uint256)" 0xebd99ff0ff6677205509ae73f93d0ca52ac85d67 200000ether --private-key "$PRIVATE_KEY_OF_OLD_SEQUENCER" --rpc-url $ETH_RPC
```
- After sending the transaction, verify it on Sepolia Etherscan: https://sepolia.etherscan.io/
<img width="1396" height="573" alt="image" src="https://github.com/user-attachments/assets/516bce3f-0f66-48d5-8951-8a5cfb925988" />

## Create your BLS keys
```
aztec validator-keys new \
  --fee-recipient 0x0000000000000000000000000000000000000000000000000000000000000000
```
- Send 0.1 ETH Sepolia to your attester address (shown in the command output).
- Your output will look like this:
<img width="641" height="95" alt="image" src="https://github.com/user-attachments/assets/8a21aabd-1065-4d07-85c0-0a538a818d83" />

## Add your address to the validator set
- Use your generated BLS key.
```
aztec \
  add-l1-validator \
  --l1-rpc-urls $ETH_RPC \
  --network testnet \
  --private-key $PRIVATE_KEY_OF_OLD_SEQUENCER \
  --attester <yourAttesterAddress> \
  --withdrawer <yourSequencerAddress> \
  --bls-secret-key <yourBLSSecretKey> \
  --rollup 0xebd99ff0ff6677205509ae73f93d0ca52ac85d67
```
<img width="1886" height="56" alt="image" src="https://github.com/user-attachments/assets/20180412-931e-480a-8edd-fc73d8d12706" />

- Now check you sequencer address here: https://dashtec.xyz/queue

## Execute your seqeuncer
```cd ~/aztec && docker compose up -d```
- Check logs.
```
docker compose logs -fn 1000
```

Done!







  















