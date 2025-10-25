# polymarket-subgraph

## Subgraphs

- activity-subgraph

- fpmm-subgraph

- oi-subgraph

- orderbook-subgraph

- pnl-subgraph

- sports-oracle-subgraph

- wallet-subgraph (broken at matic block 5141000)

## Installing Dependencies

```bash
yarn
```

## Running the test suite

Run `yarn test` to run the test suite, which will run in a docker container. (Broken)

## Preparing `subgraph.yaml`

Each subgraph has dedicated yarn scripts for convenience.

First, to prepare `subgraph.yaml` and other templated files, run `yarn templatify:matic`.

It's recommended to run the codegen command for the subgraph you're working on, as it will only generate the types and schemas for that subgraph. To do this, run `yarn pnl:codegen`, `yarn activity:codegen` or `yarn polymarket:codegen`. If you want all subgraphs, run `yarn all:codegen`.

## Local Deployment

Copy `.env.example` to `.env` and fill in it.

```bash
cp .env.example .env
```

Then, run `docker compose up` to start the environment.

### Running on an M1 Chip

To run locally on an M1 chip, you'll need to build a local copy of the graph-node docker image. To do this, clone the [graph-node repo](https://github.com/graphprotocol/graph-node) and run the following commands:

```bash
# Remove the original image
docker rmi graphprotocol/graph-node:latest

# Build the image
./docker/build.sh

# Tag the newly created image
docker tag graph-node graphprotocol/graph-node:latest
```

Note: you likely will have to increase your Docker daemon memory capacity. In Docker desktop you can find this setting under Settings > Resources > Advanced.

### Deploying subgraphs

Once the docker compose environment is ready, create and deploy the subgraph:

```[bash]
yarn <subgraph>:create-local
yarn <subgraph>:deploy-local
```

Access the GraphQL editor at:

[activity-subgraph](http://localhost:8000/subgraphs/name/activity-subgraph/graphql)
[pnl-subgraph](http://localhost:8000/subgraphs/name/pnl-subgraph/graphql)
[oi-subgraph](http://localhost:8000/subgraphs/name/oi-subgraph/graphql)
[fpmm-subgraph](http://localhost:8000/subgraphs/name/fpmm-subgraph/graphql)
[orderbook-subgraph](http://localhost:8000/subgraphs/name/orderbook-subgraph/graphql)
[sports-oracle-subgraph](http://localhost:8000/subgraphs/name/sports-oracle-subgraph/graphql)
[wallet-subgraph](http://localhost:8000/subgraphs/name/wallet-subgraph/graphql)

### Restart graph node and clear volumes

```bash
docker compose down
```

```bash
sudo docker rm polymarket-subgraph-graph-node-1 && sudo docker rm polymarket-subgraph-ipfs-1 && sudo docker rm polymarket-subgraph-postgres-1 && sudo docker rm polymarket-subgraph-ganache-1
```

The names of you docker containers may vary; check the terminal.

## Goldsky Deployment

Build the subgraph with `yarn <subgraph>:build`, or use `yarn all:build`, and deploy with:

```bash
goldsky subgraph deploy <subgraph-name>/<version> --path ./build/
```

## Contracts

These subgraphs track contracts from the following repositories:

[https://github.com/gnosis/conditional-tokens-contracts]

[https://github.com/gnosis/conditional-tokens-market-makers]

[https://github.com/Polymarket/ctf-exchange]

[https://github.com/Polymarket/neg-risk-ctf-adapter]
