# Batch Submitter

Contains an executable batch submitter service which watches L1 and a local L2 node and submits batches to the
`CanonicalTransactionChain` & `StateCommitmentChain` based on its local information.

## Configuration
All configuration is done via environment variables. See all variables at [.env.example](.env.example); copy into a `.env` file before running.


```bash
    yarn install
    yarn lint
    yarn clean
    yarn build
    yarn test
    yarn start
```

## Building & Running
1. Make sure dependencies are installed just run `yarn` in the base directory
2. Build `yarn build`
3. Run `yarn start`

## Controlling log output verbosity
Before running, set the `DEBUG` environment variable to specify the verbosity level. It must be made up of comma-separated values of patterns to match in debug logs. Here's a few common options:
* `debug*` - Will match all debug statements -- very verbose
* `info*` - Will match all info statements -- less verbose, useful in most cases
* `warn*` - Will match all warnings -- recommended at a minimum
* `error*` - Will match all errors -- would not omit this

Examples:
* Everything but debug: `export DEBUG=info*,error*,warn*`
* Most verbose: `export DEBUG=info*,error*,warn*,debug*`

## Build a DockerHub Batch Submitter

To build the Batch Submitter docker image:

```bash

docker build . --file Dockerfile --tag omgx/batch-submitter:latest
docker push omgx/batch-submitter:latest

```

This assumes that you have logged in to docker cli via `docker login`. When you run these, they will complain about missing .env variables, which you need to set, e.g., ROLLUP_CLIENT_HTTP.

## Testing & linting

### Local

- Run unit tests with `yarn test`
- See lint errors with `yarn lint`; auto-fix with `yarn lint --fix`

### Submission

You may test a submission locally against a local Hardhat fork. 

1. Follow the instructions [here](https://github.com/ethereum-optimism/hardhat) to run a Hardhat node. 
2. Change the Batch Submitter `.env` field `L1_NODE_WEB3_URL` to the local Hardhat url. Depending on which network you are using, update `ADDRESS_MANAGER_ADDRESS` according to the [omgx_contracts repo](https://github.com/omgnetwork/omgx_contracts/blob/main/dist/dumps/addresses.json).
3. Also check `L2_NODE_WEB3_URL` is correctly set and has transactions to submit.
3. Run `yarn build` to build your changes.
4. Start Batch Submitter with `yarn start`. It will automatically start submitting pending transactions from L2.
