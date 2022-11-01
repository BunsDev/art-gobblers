<img align="right" width="150" height="150" top="100" src="./assets/gobbler.png">

# Art Gobblers • [![CI](https://github.com/artgobblers/art-gobblers/actions/workflows/tests.yml/badge.svg)](https://github.com/artgobblers/art-gobblers/actions/workflows/tests.yml)

Art Gobblers is an experimental decentralized art factory by Justin Roiland and Paradigm.

## Background

Art Gobblers is a decentralized art factory owned by aliens. As artists make cool art, Gobblers gains cultural relevance, making collectors want the art more, incentivizing artists to make cooler art. It's also an on-chain game.

See our [overview of the system](https://www.paradigm.xyz/2022/09/artgobblers), as well as deep dives into some of the project's mechanisms, like [GOO](https://www.paradigm.xyz/2022/09/goo) and [VRGDAs](https://www.paradigm.xyz/2022/08/vrgda).

## Deployments

| Contract      | Mainnet                                                                                                                 | Goerli                                                                                                                         |
|---------------|-------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------|
| `ArtGobblers` | [`0x60bb1e2aa1c9acafb4d34f71585d7e959f387769`](https://etherscan.io/address/0x60bb1e2aa1c9acafb4d34f71585d7e959f387769) | [`0x60bb1e2aa1c9acafb4d34f71585d7e959f387769`](https://goerli.etherscan.io/address/0x60bb1e2aa1c9acafb4d34f71585d7e959f387769) |
| `Pages`       | [`0x600df00d3e42f885249902606383ecdcb65f2e02`](https://etherscan.io/address/0x600df00d3e42f885249902606383ecdcb65f2e02) | [`0x600df00d3e42f885249902606383ecdcb65f2e02`](https://goerli.etherscan.io/address/0x600df00d3e42f885249902606383ecdcb65f2e02) |
| `Goo`         | [`0x600000000a36f3cd48407e35eb7c5c910dc1f7a8`](https://etherscan.io/address/0x600000000a36f3cd48407e35eb7c5c910dc1f7a8) | [`0x600000000a36f3cd48407e35eb7c5c910dc1f7a8`](https://goerli.etherscan.io/address/0x600000000a36f3cd48407e35eb7c5c910dc1f7a8) |

## State Diagrams


![Gobbler Lifecycle](assets/state-machines/gobbler-lifecycle.png)
![Legendary Gobbler Auctions](assets/state-machines/legendary-gobbler-auctions.png)
![Page Auctions](assets/state-machines/page-auctions.png)

## Usage

You will need a copy of [Foundry](https://github.com/foundry-rs/foundry) installed before proceeding. See the [installation guide](https://github.com/foundry-rs/foundry#installation) for details.

To build the contracts:

```sh
git clone https://github.com/artgobblers/art-gobblers.git
cd art-gobblers
forge install
```

### Run Tests

In order to run unit tests, run:

```sh
forge test
```

For longer fuzz campaigns, run:

```sh
FOUNDRY_PROFILE="intense" forge test
```

For differential fuzzing against a python implementation, see [here](./analysis/README.md).

### Run Slither

After [installing Slither](https://github.com/crytic/slither#how-to-install), run:

```sh
slither src/ --solc-remaps 'ds-test/=lib/ds-test/src/ solmate/=lib/solmate/src/ forge-std/=lib/forge-std/src/ chainlink/=lib/chainlink/contracts/src/ VRGDAs/=lib/VRGDAs/src/ goo-issuance/=lib/goo-issuance/src/'
```


### Update Gas Snapshots

To update the gas snapshots, run:

```sh
forge snapshot
```

### Deploy Contracts

In order to deploy the art gobblers contracts, set the relevant constants in the `DeployMainnet` script, and run the following command(s):

```sh
export DEPLOYER_PRIVATE_KEY=$DEPLOYER_PRIVATE_KEY
export GOBBLER_PRIVATE_KEY=$GOBBLER_PRIVATE_KEY
export PAGES_PRIVATE_KEY=$PAGES_PRIVATE_KEY
export GOO_PRIVATE_KEY=$GOO_PRIVATE_KEY

forge script script/deploy/DeployMainnet.s.sol:DeployMainnet --rpc-url $RPC_URL --verify --etherscan-api-key $API_KEY
```

We use [profanity2](https://github.com/1inch/profanity2) to securely generate vanity addresses for the `ArtGobblers`, `Pages`, and `Goo` contracts. As a result, each of these contracts must be deployed using a unique private key. To simplify deployment, the deployment script ensures that only `DEPLOYER_PRIVATE_KEY` needs to be seeded with ETH, by automatically transferring 0.25 ETH from it to the other deployers before they are used.

To ensure security in case the private keys generated by [profanity2](https://github.com/1inch/profanity2) are [compromised](https://blog.1inch.io/a-vulnerability-disclosed-in-profanity-an-ethereum-vanity-address-tool-68ed7455fc8c), the script immediately revokes `GOBBLER_PRIVATE_KEY`'s ownership over `ArtGobblers` and transfers it to a configurable address. `Pages` and `Goo` do not grant any special authority to their respective deployers.

## Audits

The following auditors were engaged to review the project before launch:

- [samczsun](https://samczsun.com) (No report)
- [Spearbit](https://spearbit.com) (Report [here](https://github.com/spearbit/portfolio/blob/master/pdfs/ArtGobblers-Spearbit-Security-Review.pdf))
- [code4rena](https://code423n4.com) (Report pending)
- [Riley Holterhus](https://www.rileyholterhus.com) (No Report)

## License

[MIT](LICENSE) © 2022 Art Gobblers
