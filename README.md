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
aztec-up 1.1.2
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
  --network alpha-testnet \
  --l1-rpc-urls RPC_URL  \
  --l1-consensus-host-urls BEACON_URL \
  --sequencer.validatorPrivateKeys "0xPrivatekey1,0xPrivatekey2,0xPrivatekey3" \
  --sequencer.publisherPrivateKey 0xPrivatekeyX
  --sequencer.coinbase 0xYourAddress \
  --p2p.p2pIp IP
```






































