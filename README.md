# Chainlink Functions Starter Kit

- [Chainlink Functions Starter Kit](#chainlink-functions-starter-kit)
  - [Overview](#overview)
  - [Motivation](#motivation)
  - [Supported Networks](#supported-networks)
    - [Mainnets](#mainnets)
    - [Testnets](#testnets)
  - [For Beginners](#for-beginners)
    - [Tutorials \& examples](#tutorials--examples)
  - [Quickstart](#quickstart)
    - [Requirements](#requirements)
  - [Steps on Live (Public) Testnets](#steps-on-live-public-testnets)
    - [Steps on local testnet](#steps-on-local-testnet)
  - [Environment Variable Management](#environment-variable-management)
    - [Using Remote Secrets (e.g. Github Gists)](#using-remote-secrets-eg-github-gists)
    - [Environment Variable Management Commands](#environment-variable-management-commands)
  - [Functions Command Glossary](#functions-command-glossary)
    - [Functions Commands](#functions-commands)
    - [Functions Subscription Management Commands](#functions-subscription-management-commands)
  - [Request Configuration](#request-configuration)
    - [JavaScript Code](#javascript-code)
      - [Functions Library](#functions-library)
      - [Importing Dependencies](#importing-dependencies)
    - [Modifying Contracts](#modifying-contracts)
    - [Local Simulations with the `localFunctionsTestnet`](#local-simulations-with-the-localfunctionstestnet)
    - [Managing Secrets](#managing-secrets)
  - [Automation Integration](#automation-integration)
  - [Gas Spikes](#gas-spikes)
  - [Troubleshooting](#troubleshooting)

## Overview

<p>Chainlink Functions allows users to request data from HTTP(s) APIs and perform custom computation using JavaScript.
It works by executing the request on a <a href="https://chain.link/education/blockchain-oracles#decentralized-oracles">decentralized oracle network</a> (DON).
When a request is initiated, each node in the DON executes the user-provided JavaScript code simultaneously.  Then, nodes use the <a href="https://docs.chain.link/architecture-overview/off-chain-reporting/">Chainlink OCR</a> protocol to come to consensus on the results.  Finally, the median result is returned to the requesting contract via a callback function.
<p>Chainlink Functions also enables users to securely share secrets with the DON, allowing users to access APIs that require authentication without exposing their API keys. Secrets are encrypted with threshold public key cryptography, requiring multiple nodes to participate in a decentralized decryption process such that no node can decrypt secrets without consensus from the rest of the DON.</p>

Nodes are compensated in LINK via a subscription billing model. You can see billing details [here](https://docs.chain.link/chainlink-functions/resources/subscriptions) and pricing for each network [here](https://docs.chain.link/chainlink-functions/supported-networks).

<p><b>Working with Chainlink Functions requires accepting the terms of service before you are able to create a subscription. Please visit <a href="https://functions.chain.link/">chain.link/functions</a>.</b></p>

## Motivation

This repo provides developers with a "works out of the box" experience as it comes preconfigured with dependencies and popular tooling like [Hardhat](https://hardhat.org). This is not a tutorial for the Hardhat toolchain. It assumes basic familiarity with Hardhat and the command line. We use HardHat CLI scripts to run Chainlink Functions commands and operations.

In order to set up your own project which uses Chainlink Functions, please refer to the [Functions Toolkit NPM package](https://www.npmjs.com/package/@chainlink/functions-toolkit).

## Supported Networks

> ⚠️⚠️⚠️
> As at 13 April 2024, Mumbai (anchored to Goerli) stopped producing blocks. Mumbai's deprecation had been announced in favour of a new Amoy testnet, anchored to Sepolia.

### Mainnets

- Ethereum : `ETHEREUM_RPC_URL`, `--network ethereum`, `ETHERSCAN_API_KEY`
- Polygon : `POLYGON_RPC_URL`, `--network polygon`, `POLYGONSCAN_API_KEY`
- Avalanche : `AVALANCHE_RPC_URL`, `--network avalanche`, `SNOWTRACE_API_KEY`
- Arbitrum : `ARBITRUM_RPC_URL`, `--network arbitrum`, `ARBISCAN_API_KEY`
- Base : `BASE_RPC_URL`, `--network base`, `BASESCAN_API_KEY`
- Celo : `CELO_RPC_URL`, `--network celo`, `CELOSCAN_API_KEY`

### Testnets

- Ethereum Sepolia: `ETHEREUM_SEPOLIA_RPC_URL`, `--network ethereumSepolia`, `ETHERSCAN_API_KEY`
- Polygon Amoy: `POLYGON_AMOY_RPC_URL`, `--network polygonAmoy`, `POLYGONSCAN_API_KEY`
- Avalanche Fuji: `AVALANCHE_FUJI_RPC_URL`, `--network avalancheFuji`, `SNOWTRACE_API_KEY`
- Arbitrum Sepolia: `ARBITRUM_SEPOLIA_RPC_URL`, `--network arbitrumSepolia`, `ARBISCAN_API_KEY`
- Base Sepolia: `BASE_SEPOLIA_RPC_URL`, `--network baseSepolia`, `BASESCAN_API_KEY`
- Optimism Sepolia: `OPTIMISM_SEPOLIA_RPC_URL`, `--network optimismSepolia`, `OP_ETHERSCAN_API_KEY`
- Celo Alfajores: `CELO_ALFAJORES_RPC_URL`, `--network celoAlfajores`, `CELOSCAN_API_KEY`

## For Beginners

If you're new to web3, it is recommended starting with the [Functions - Getting Started](https://docs.chain.link/chainlink-functions/getting-started/) guide before diving into the code.

The above document will help you:

- Set up a wallet
- Get funds
- Provides more detailed step-by-step instructions and further information

### Tutorials & examples

For other detailed tutorials and examples, check out the [Chainlink Functions Tutorials](https://docs.chain.link/chainlink-functions/tutorials/) to get started.

## Quickstart

### Requirements

Install **both** of the following:

- Node.js version [20](https://nodejs.org/en/download/)
- Deno version [1.36](https://deno.land/manual@v1.36.4/getting_started/installation) (or the latest release of Deno v1 if a later one is available)

## Steps on Live (Public) Testnets

1. Clone this repository to your local machine<br><br>. Also ensure that the testnet your wanting to deploy on is [supported](https://docs.chain.link/chainlink-functions/supported-networks) by Chainlink Functions.
2. Open this directory in your command line/terminal app, then run `npm install` to install all dependencies.<br><br>
3. Obtain the values for following environment variables (examples only - please see `./env.enc.example` for env vars you may need):
   - `PRIVATE_KEY` for your development wallet - `POLYGON_AMOY_RPC_URL`, `ETHEREUM_SEPOLIA_RPC_URL`, or `AVALANCHE_FUJI_RPC_URL`
   - `POLYGONSCAN_API_KEY`, `ETHERSCAN_API_KEY`, or `FUJI_SNOWTRACE_API_KEY` blockchain explore API keys depending on which network you're using
   - `COINMARKETCAP_API_KEY` (from [here](https://pro.coinmarketcap.com/))
   - `SECOND_PRIVATE_KEY` (optional) ,to accept the transfer of ownership using `./tasks/Functions-billing/accept.js` you will need to set this environment variable to act as the "transferee's" account.
     <br><br>
4. Set the required environment variables (see `./env.enc.example` for the correctly capitalized names of environment variables used in this repo). For improved security, Chainlink provides the NPM package [@chainlink/env-enc](https://www.npmjs.com/package/@chainlink/env-enc) which can be used to keep environment variables in a password encrypted `.env.enc` file instead of a plaintext `.env` for additional security. More detail on environment variable management and the tooling is provided in the [Environment Variable Management](#environment-variable-management) section.
   1. Set an encryption password for your environment variables to a secure password by running `npx env-enc set-pw`. This password needs to be set each time you create or restart a terminal shell session.<br>
   2. Use the command `npx env-enc set` to set the required environment variables.
   3. Set any other values you intend to pass into the _secrets_ object in _Functions-request-config.js_ .<br><br>
5. There are four files to notice that the default example will use:
   - `Functions-request-config.js` which contains the `request` object that has all the data necessary to trigger a Functions request. This config file also specifies which `source` code to pass to Functions. More information on request configuration is in the [Request Configuration section](#request-configuration).
   - `contracts/FunctionsConsumer.sol` is the consumer smart contract that will receive the Functions-related data from the request config, and trigger the functions request.
   - `calculation-example.js` contains example JavaScript code that will be executed by each node of the DON. This example performs complex calculations but no API requests.
   - `API-request-example.js` contains example JavaScript code which fetches data from APIs before processing the data <br><br>
6. Locally simulate the execution of your JavaScript source by running `npx hardhat functions-simulate-script`

7. Deploy and verify the consumer contract to an actual blockchain network by running `npx hardhat functions-deploy-consumer --network network_name_here --verify true`<br>**Note**: Make sure `<explorer>_API_KEY` is set if using `--verify true` depending on which network is used.<br><br>
8. Create and fund a new Functions billing subscription using the [Chainlink Functions UI](https://functions.chain.link) and add the deployed consumer contract as an authorized consumer to your subscription. You can also do this programmatically with `npx hardhat functions-sub-create --network network_name_here --amount LINK_funding_amount_here --contract 0x_deployed_client_contract_address_here`<br>**Note**: Ensure your wallet has a sufficient LINK balance before running this command. Testnet LINK can be obtained at <a href="https://faucets.chain.link/">faucets.chain.link</a>. Also make a note of your subscription Id as you will need it for most commands.<br>

9. Make an on-chain request by running:<br>`npx hardhat functions-request --network network_name_here --contract 0xDeployed_client_contract_address_here --subid subscription_id_number_here`. You will see a confirmation request, so hit `Y` and press enter. Once the request is fulfilled the console will show the response (decoded into the relevant return type) from the execution of your custom JS script.

10. You can also query the response that was stored in your Functions Consumer contract by runnning `npx hardhat functions-read --contract 0xConsumer_contract_address --network  your_network_name`

### Steps on local testnet

1. To do an end-to-end simulation using a local testnet you can first open a new terminal window and run `npm run startLocalFunctionsTestnet`. This will spin up a local blockchain testnet (the `localFunctionsTestnet`), on which you can simulate an end-to-end Functions request.

2. Follow the workflow steps above, including subscription creation, funding, deploying your Functions Consumer etc. but omit the `--network network_name_here` flag in your CLI commands as the default network will be the `localFunctionsTestnet`.

3. Running this end-to-end simulation will surface most errors in your smart contract and/or JavaScript source code and configuration.<br><br>

## Environment Variable Management

This repo uses the NPM package `@chainlink/env-enc` for keeping environment variables such as wallet private keys, RPC URLs, and other secrets encrypted at rest. This reduces the risk of credential exposure by ensuring credentials are not visible in plaintext as they are with [.env files](https://www.npmjs.com/package/dotenv).

By default, all encrypted environment variables will be stored in a file named `.env.enc` in the root directory of this repo. This file is `.gitignore`'d.

For a full list of the Env Var names (keys) that this repo uses and has defined please look at `./env.enc.example`.

First, set the encryption password by running the command `npx env-enc set-pw`.

> **NOTE:** On Windows, this command may show a security confirmation.

The password must be set at the beginning of each new session.
If this password is lost, there will be no way to recover the encrypted environment variables.

Run the command `npx env-enc set` to set and save environment variables.
These variables will be loaded into your environment when the `config()` method is called at the top of `networks.js`.

Use `npx env-enc view` to view all currently saved environment variables.
When pressing _ENTER_, the terminal will be cleared to prevent these values from remaining visible.

Running `npx env-enc remove VAR_NAME_HERE` deletes the specified environment variable.

The command `npx env-enc remove-all` deletes the entire saved environment variable file.

When running this command on a Windows machine, you may receive a security confirmation prompt. Enter `r` to proceed.

> **NOTE:** When you finish each work session, close down your terminal to prevent your encryption password from becoming exposes if your machine is compromised. You will need to set the same password on future session to decrypt the `.env.enc` file.

### Using Remote Secrets (e.g. Github Gists)

To upload and delete secrets gists that will remotely store your encrypted secrets, you need to first acquire a Github personal access token which allows reading and writing Gists.

1. Visit [https://github.com/settings/tokens?type=beta](https://github.com/settings/tokens?type=beta) and click "Generate new token"
2. Name the token and enable read & write access for Gists from the "Account permissions" drop-down menu. Do not enable any additional permissions.
3. Click "Generate token" and copy the resulting personal access token for step 4.
4. set the `GITHUB_API_TOKEN` environment variable using `npx env-enc set`
5. Specify `Location.Remote` for the `secretLocation` in _Functions-request-config.js_

### Environment Variable Management Commands

The following commands accept an optional `--path` flag followed by a path to the desired encrypted environment variable file.
If one does not exist, it will be created automatically by the `npx env-enc set` command.

The `--path` flag has no effect on the `npx env-enc set-pw` command as the password is stored as an ephemeral environment variable for the current terminal session.

| Command                     | Description                                                                                                                                       | Parameters            |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| `npx env-enc set-pw`        | Sets the password to encrypt and decrypt the environment variable file **NOTE:** On Windows, this command may show a security confirmation prompt |                       |
| `npx env-enc set`           | Sets and saves variables to the encrypted environment variable file                                                                               |                       |
| `npx env-enc view`          | Shows all currently saved variables in the encrypted environment variable file                                                                    |                       |
| `npx env-enc remove <name>` | Removes a variable from the encrypted environment variable file                                                                                   | `name`: Variable name |
| `npx env-enc remove-all`    | Deletes the encrypted environment variable file                                                                                                   |                       |

## Functions Command Glossary

[Functions Commands](#functions-commands) and [Subscription Management Commands](#functions-subscription-management-commands) commands can be executed in the following format:
`npx hardhat command_here --parameter1 parameter_1_value_here --parameter2 parameter_2_value_here`

Example: `npx hardhat functions-read --network polygonMumbai --contract 0x787Fe00416140b37B026f3605c6C72d096110Bb8`

### Functions Commands

| Command                            | Description                                                                                                                          | Parameters                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `compile`                          | Compiles all smart contracts                                                                                                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| `functions-simulate-script`        | Executes the JavaScript source code locally                                                                                          | `network`: Name of blockchain network, `configpath` (optional): Path to request config file (defaults to `./Functions-request-config.js`)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| `functions-deploy-consumer`        | Deploys the `FunctionsConsumer` contract                                                                                             | `network`: Name of blockchain network, `verify` (optional): Set to `true` to verify the deployed `FunctionsConsumer` contract (defaults to `false`)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| `functions-request`                | Initiates a request from a `FunctionsConsumer` contract using data from the Functions request config file                            | `network`: Name of blockchain network, `contract`: Address of the consumer contract to call, `subid`: Billing subscription ID used to pay for the request, `callbackgaslimit` (optional): Maximum amount of gas that can be used to call `fulfillRequest` in the consumer contract (defaults to 100,000 & must be less than 300,000), `slotid` (optional): Slot ID to use for uploading DON hosted secrets. If the slot is already in use, the existing encrypted secrets will be overwritten. (defaults to 0), `simulate` (optional, default true): Flag indicating if simulation should be run before making an on-chain request, `requestgaslimit` (optional): Gas limit for calling the sendRequest function (defaults to 1,500,000) `configpath` (optional): Path to request config file (defaults to `./Functions-request-config.js`) |
| `functions-read`                   | Reads the latest response (or error) returned to a `FunctionsConsumer` or `AutomatedFunctionsConsumer` contract                      | `network`: Name of blockchain network, `contract`: Address of the consumer contract to read, `configpath` (optional): Path to request config file (defaults to `./Functions-request-config.js`)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| `functions-deploy-auto-consumer`   | Deploys the `AutomatedFunctionsConsumer` contract and sets the Functions request using data from the Functions request config file   | `network`: Name of blockchain network, `subid`: Billing subscription ID used to pay for Functions requests, `verify` (optional, default false): Set to `true` to verify the deployed `AutomatedFunctionsConsumer` contract, `configpath` (optional): Path to request config file (defaults to `./Functions-request-config.js`)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| `functions-set-auto-request`       | Updates the Functions request in deployed `AutomatedFunctionsConsumer` contract using data from the Functions request config file    | `network`: Name of blockchain network, `contract`: Address of the contract to update, `subid`: Billing subscription ID used to pay for Functions requests, `interval` (optional): Update interval in seconds for Chainlink Automation to call `performUpkeep` (defaults to 300), `slotid` (optional) 0 or higher integer denoting the storage slot for DON-hosted secrets, `ttl` (optional) the minutes after which DON hosted secrets must be expired, `gaslimit` (optional): Maximum amount of gas that can be used to call `fulfillRequest` in the consumer contract (defaults to 250,000), `simulate` (optional, default true): Flag indicating if simulation should be run before making an on-chain request, `configpath` (optional): Path to request config file (defaults to `./Functions-request-config.js`)                       |
| `functions-check-upkeep`           | Checks if `checkUpkeep` returns true for an Automation compatible contract                                                           | `network`: Name of blockchain network, `contract`: Address of the contract to check, `data` (optional): Hex string representing bytes that are passed to the `checkUpkeep` function (defaults to empty bytes)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| `functions-perform-upkeep`         | Manually call `performUpkeep` in an Automation compatible contract                                                                   | `network`: Name of blockchain network, `contract`: Address of the contract to call, `data` (optional): Hex string representing bytes that are passed to the `performUpkeep` function (defaults to empty bytes)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| `functions-set-donid`              | Updates the DON ID for a consumer contract using the `donId` address from `networks.js`                                              | `network`: Name of blockchain network, `contract`: Address of the consumer contract to update                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| `functions-build-request`          | Creates a JSON file with Functions request parameters including encrypted secrets, using data from the Functions request config file | `network`: Name of blockchain network, `output` (optional): Output JSON file name (defaults to _Functions-request.json_), `simulate` (optional, default true): Flag indicating if simulation should be run before building the request JSON file, `configpath` (optional): Path to request config file (defaults to `./Functions-request-config.js`)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| `functions-build-offchain-secrets` | Builds an off-chain secrets object that can be uploaded and referenced via URL                                                       | `network`: Name of blockchain network, `output` (optional): Output JSON file name (defaults to `offchain-encrypted-secrets.json`), `configpath` (optional): Path to request config file (defaults to `./Functions-request-config.js`)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| `functions-upload-secrets-don`     | Encrypts secrets and uploads them to the DON                                                                                         | `network`: Name of blockchain network, `configpath` (optional): Path to request config file (defaults to `./Functions-request-config.js`), `slotid` Storage slot number 0 or higher - if the slotid is already in use, the existing secrets for that slotid will be overwritten, `ttl` (optional): Time to live - minutes until the secrets hosted on the DON expire (defaults to 10, and must be at least 5)                                                                                                                                                                                                                                                                                                                                                                                                                               |
| `functions-list-don-secrets`       | Displays encrypted secrets hosted on the DON                                                                                         | `network`: Name of blockchain network                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |

### Functions Subscription Management Commands

| Command                      | Description                                                                                                                                              | Parameters                                                                                                                                                                                                                                                                   |
| ---------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `functions-sub-create`       | Creates a new Functions billing subscription for Functions consumer contracts                                                                            | `network`: Name of blockchain network, `amount` (optional): Initial amount used to fund the subscription in LINK (decimals are accepted), `contract` (optional): Address of the consumer contract to add to the subscription                                                 |
| `functions-sub-info`         | Gets the Functions billing subscription balance, owner, and list of authorized consumer contract addresses                                               | `network`: Name of blockchain network, `subid`: Subscription ID                                                                                                                                                                                                              |
| `functions-sub-fund`         | Funds a Functions billing subscription with LINK                                                                                                         | `network`: Name of blockchain network, `subid`: Subscription ID, `amount`: Amount to fund subscription in LINK (decimals are accepted)                                                                                                                                       |
| `functions-sub-cancel`       | Cancels a Functions billing subscription and refunds the unused balance. Cancellation is only possible if there are no pending requests.                 | `network`: Name of blockchain network, `subid`: Subscription ID, `refundaddress` (optional): Address where the remaining subscription balance is sent (defaults to caller's address)                                                                                         |
| `functions-sub-add`          | Authorizes a consumer contract to use the Functions billing subscription                                                                                 | `network`: Name of blockchain network, `subid`: Subscription ID, `contract`: Address of the consumer contract to authorize for billing                                                                                                                                       |
| `functions-sub-remove`       | Removes a consumer contract from a Functions billing subscription                                                                                        | `network`: Name of blockchain network, `subid`: Subscription ID, `contract`: Address of the consumer contract to remove from billing subscription                                                                                                                            |
| `functions-sub-transfer`     | Request ownership of a Functions subscription be transferred to a new address                                                                            | `network`: Name of blockchain network, `subid`: Subscription ID, `newowner`: Address of the new owner                                                                                                                                                                        |
| `functions-sub-accept`       | Accepts ownership of a Functions subscription after a transfer is requested , before trying `functions-sub-accept` set env variable `SECOND_PRIVATE_KEY` | `network`: Name of blockchain network, `subid`: Subscription ID                                                                                                                                                                                                              |
| `functions-timeout-requests` | Times out expired Functions requests which have not been fulfilled within 5 minutes                                                                      | `network`: Name of blockchain network, `requestids`: 1 or more request IDs to timeout separated by commas, `toblock` (optional): Ending search block number (defaults to latest block), `pastblockstosearch` (optional): Number of past blocks to search (defaults to 1,000) |

## Request Configuration

Chainlink Functions requests can be configured by modifying values in the `requestConfig` object found in the _Functions-request-config.js_ file located in the root of this repository.

| Setting Name         | Description                                                                                                                                                                                                                                                                                                            |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `codeLocation`       | This specifies where the JavaScript code for a request is located. Currently, only the `Location.Inline` option is supported (represented by the value `0`). This means the JavaScript string is provided directly in the on-chain request instead of being referenced via a URL.                                      |
| `codeLanguage`       | This specifies the language of the source code which is executed in a request. Currently, only `JavaScript` is supported (represented by the value `0`).                                                                                                                                                               |
| `source`             | This is a string containing the source code which is executed in a request. This must be valid JavaScript code that returns a [Uint8Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array). See the [JavaScript Code](#javascript-code) section for more details.         |
| `secrets`            | This is an (optional) object which contains secret values that are injected into the JavaScript source code and can be accessed using the name `secrets`. This object can only contain string values. This object will be automatically encrypted by the tooling using the threshold public key before making request. |
| `secretsLocation`    | This (optional) value must be present if `secrets` are present. Values must be one of either `DONhosted` or `Remote`. This refers to the location of the Secrets - which can be User-hosted (Remote) at a URL or DON-hosted.                                                                                           |
| `args`               | This is an array of strings which contains values that are injected into the JavaScript source code and can be accessed using the name `args`. This provides a convenient way to set modifiable parameters within a request. If no arguments, then an empty array is passed.                                           |
| `expectedReturnType` | This specifies the expected return type of a request. It has no on-chain impact, but is used by the CLI to decode the response bytes into the specified type. The options are `uint256`, `int256`, `string`, or `bytes`.                                                                                               |

### JavaScript Code

The JavaScript source code for a Functions request can use any valid [Deno](https://deno.land/manual@v1.36.4/introduction) JavaScript, but _cannot_ use any imported modules.

The code must return a [Uint8Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array) which represents the response bytes that are sent back to the requesting contract.
Encoding functions are provided in the [Functions library](#functions-library).
Additionally, any external APIs to which requests are made must script must respond in **less than 9 seconds** and the JavaScript Code as a whole must return in **less than 10 seconds** or it will be terminated and send back an error (in bytes) to the requesting contract.

In order to make HTTP requests, the source code must use the `Functions.makeHttpRequest` function from the exposed [Functions library](#functions-library).
Asynchronous code with top-level `await` statements is supported, as shown in the file _API-request-example.js_.

#### Functions Library

The `Functions` library is injected into the JavaScript source code and can be accessed using the name `Functions`.

In order to make HTTP requests, use the `Functions.makeHttpRequest` method which takes an object as an argument with the following parameters.

```
{
  url: String with the URL to which the request is sent,
  method (optional): String specifying the HTTP method to use which can be either 'GET', 'POST', 'PUT', 'DELETE', 'PATCH', 'HEAD', or 'OPTIONS' (defaults to 'GET'),
  headers (optional): Object with headers to use in the request,
  params (optional): Object with URL query parameters,
  data (optional): Object or other value which represents the body sent with the request,
  timeout (optional): Number with the maximum request duration in ms (defaults to 3000 ms),
  responseType (optional): String specifying the expected response type which can be either 'json', 'arraybuffer', 'document', 'text' or 'stream' (defaults to 'json'),
}
```

The function returns a promise that resolves to either a success response object or an error response object.

A success response object will have the following parameters.

```
{
  error: false,
  data: Response data sent by the server,
  status: Number representing the response status,
  statusText: String representing the response status,
  headers: Object with response headers sent by the server,
}
```

An error response object will have the following parameters.

```
{
  error: true,
  message (may be undefined): String containing error message,
  code (may be undefined): String containing an error code,
  response (may be undefined): Object containing response sent from the server,
}
```

This library also exposes functions for encoding JavaScript values into Uint8Arrays which represent the bytes that a returned on-chain.

- `Functions.encodeUint256` takes a positive JavaScript integer number and returns a Uint8Array of 32 bytes representing a `uint256` type in Solidity.
- `Functions.encodeInt256` takes a JavaScript integer number and returns a Uint8Array of 32 bytes representing a `int256` type in Solidity.
- `Functions.encodeString` takes a JavaScript string and returns a Uint8Array representing a `string` type in Solidity.

Remember, it is not required to use these encoding functions. The JavaScript code must only return a [Uint8Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array) which represents the `bytes` that are returned on-chain.

#### Importing Dependencies

To import and use libraries in your Functions request JavaScript source code, you must use the async `import` function. Since this is an async function, you must remember to use the `await` keyword to wait for the dependency to be imported before it can be used as shown in the examples below.

```
const lodash = await import("http://cdn.skypack.dev/lodash");
const result = lodash.concat([1], 2);
return Functions.encodeString(JSON.stringify(result));
```

```
const { ethers } = await import("npm:ethers@6.9.0");
const myNumber = ethers.AbiCoder.defaultAbiCoder().decode(
  ["uint256"],
  "0x000000000000000000000000000000000000000000000000000000000000002a"
);
return Functions.encodeUint256(BigInt(myNumber.toString()));
```

> ⚠️ **Users are fully responsible for any dependencies their JavaScript source code imports. Chainlink is not responsible for any imported dependencies and provides no guarantees of the validity, availability or security of any libraries a user chooses to import or the repositories from which these dependencies are downloaded. Developers are advised to fully vet any imported dependencies or avoid dependencies altogether to avoid any risks associated with a compromised library or a compromised repository from which the dependency is downloaded.**

Chainlink Functions supports importing ESM-compatible modules with are supported by Deno within the JavaScript source code. It also supports importing some NPM packages [via the `npm:` specifier](https://docs.deno.com/runtime/manual/node/npm_specifiers) and some standard Node.js modules [via the `node:` specifier](https://docs.deno.com/runtime/manual/node/node_specifiers). Check out the [Deno documentation on importing modules](https://docs.deno.com/runtime/manual/basics/modules/) for more information or visit [deno.land/x](https://deno.land/x) to find 3rd party modules which have been built for Deno.

The total number of imports and the size of each import are restricted:

- You can import a maximum of 100 dependencies. Sub-dependencies required by the target library also count toward this limit.
- The total size of each imported dependency cannot be larger than 10 MB. This 10 MB size limit includes any sub-dependencies required by the target library.

All other [service limits](https://docs.chain.link/chainlink-functions/resources/service-limits) still apply to imported dependencies. This means the dependencies will not have access to the file system, environment variables or any other Deno permissions. If an imported library requires restricted permissions, importing the library may result in an error. Furthermore, dependencies are downloaded at runtime, meaning the time required to download a dependency is counted toward the total JavaScript source code execution time limit.

Sometimes imported dependencies use additional fetch requests to load additional code or resources. These fetch requests count toward the total number of HTTP requests that the JavaScript source code is allowed to perform. If the imported dependencies exceed this total number of allowed fetch requests, the import attempt will fail with an error.

### Modifying Contracts

Consumer contracts which initiate a request and receive a fulfillment can be modified for specific use cases. The only requirements are that the contract successfully calls `sendRequest` in the `FunctionsRouter`,
and that it correctly implements the `fulfillRequest` function which is called by `handleOracleFulfillment` in the inherited `FunctionsClient` contract (See _FunctionsClient.sol_ for details).</br>
At this time, the maximum amount of gas that _handleOracleFulfillment_ can use is 300,000 (please contact Chainlink Labs if you require a higher callback gas limit).

### Local Simulations with the `localFunctionsTestnet`

The Functions Toolkit NPM package provides the ability to create a local testnet blockchain on your machine which allows you to make simulated requests to debug your JavaScript code and smart contracts.
For more details, please see the [Functions Toolkit NPM package documentation](https://github.com/smartcontractkit/functions-toolkit/blob/main/README.md#local-functions-testnet).

In order to launch the `localFunctionsTestnet` in this project, open a new terminal window and run the command `npm run startLocalFunctionsTestnet`.
Then, you can interact with this local testnet blockchain as you would with a live testnet.

By default, all the `npx hardhat` commands in this project are configured to use this local testnet running on port `8545`, so you can omit the `--network` CLI argument (just don't forget to start the testnet first).

### Managing Secrets

Please refer to the [Functions Toolkit NPM package documentation](https://github.com/smartcontractkit/functions-toolkit/blob/main/README.md#functions-secrets-manager) for more details.

Secrets can be managed in either of two ways: user-hosted (`Location.Remote`) or DON hosted (`Location.DONHosted`).

This project uses DONHosted secrets by default, which means secrets from the `Functions-request-config.js` file are encrypted and then uploaded to the DON and automatically.

The CLI command to upload secrets to the DON is `npx hardhat functions-upload-secrets-don --slotid _0_or_higher --network network_name --ttl minutes_until_expired`.

## Automation Integration

Chainlink Functions can be used with [Chainlink Automation](https://docs.chain.link/chainlink-automation/introduction) in order to automatically trigger a Functions as specified intervals.

1. Create & fund a new Functions billing subscription by running `npx hardhat functions-sub-create --network network_name_here --amount LINK_funding_amount_here`<br>**Note**: Ensure your wallet has a sufficient LINK balance before running this command.<br><br>

2. Deploy the `AutomationFunctionsConsumer` contract by running `npx hardhat functions-deploy-auto-consumer --subid subscription_id_number_here --verify true --network network_name_here`<br>**Note**: Make sure `<blockexplorer>_API_KEY` environment variable is set when using `--verify true`.

   - This step will automatically add your consumer contract as an authorized user of your subscription. You can verify by running `npm functions-sub-info --network network_name_here --subid subscription_id_number_here`.

3. Encode the request parameters into CBOR and store it on chain with `npx hardhat functions-set-auto-request --network network_name_here  --subid subscription_id_number_here --interval automation-call-interval --slotid don_hosted_secret_slotId --ttl minutes_until_secrets_expiry --contract 0x_contract_address`<br>

> DON-Hosted secrets and expire after the specified `ttl` (which defaults to 10 minutes if no `ttl` is specified). If a request is sent after the `ttl` has expired, you will see error bytes returned to your consumer contract.

1. Register the `AutomationFunctionsConsumer` contract for upkeep via the Chainlink Automation web app here: [https://automation.chain.link/](https://automation.chain.link/). This example uses a "Custom Logic" Automation.
   - Be sure to set the `Gas limit` for the `performUpkeep` function to a high enough value. The recommended value is 1,000,000.
   - Once created, ensure the Automation upkeep has sufficient funds. You can add funds, pause or cancel the upkeep in the web app.
   - Find further documentation for working with Chainlink Automation here: [https://docs.chain.link/chainlink-automation/introduction](https://docs.chain.link/chainlink-automation/introduction)

Once the contract is registered for upkeep, check the latest response or error with the commands `npx hardhat functions-read --network network_name_here --contract 0x_contract_address`.

1. For debugging on your machine, use the command `npx hardhat functions-check-upkeep --network network_name_here --contract contract_address_here` to see if Automation needs to call `performUpkeep`. If this call returns `false` then the upkeep interval has not yet passed and `performUpkeep` will not execute. In order to test that `performUpkeep` will run correctly before registering the Automation upkeep, you can also trigger a request manually using the command `npx hardhat functions-perform-upkeep --network network_name_here --contract contract_address_here`

You can also attach a listener to a Subscription ID by updating the `subId` variable in `/scripts/listen.js`, and then running `npm run listen --network your_network_name` from the repo root in a new terminal so that it can keep listening as you develop. This script uses nodemon which restarts the script when you save files or when the listener returns a result.

## Gas Spikes

When on-chain traffic is high, transaction gas prices can spike unexpectedly. This may decrease the accuracy of the estimated requests costs or cause transactions to fail.
In order to mitigate these problems, ensure your Functions subscription balance has a sufficient buffer of two or more times the expected request cost in LINK.
Additionally, you can manually set a hardcoded transaction gas price in the HardHat tooling by modifying the `gasPrice` parameter in the _networks.js_ config file for a particular network.

## Troubleshooting

1. If you get strange (and scary large) error output in your terminal because a transaction failed, it is super helpful to use [tenderly.co](https://tenderly.co). Once you create an account, and a project look for "Transactions" in the tab list on the left, and past in your Transaction Hash. Tenderly will look across various networks for it. It will then show you the causes for the error especially if the contract has been verified. Here is a useful video on how to debug transactions with Tenderly:
   <iframe width="360" height="215" src="https://www.youtube.com/embed/90GN9Ut8LhU?si=iLhHegpG1Mq59qtJ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

2. When running Chainlink Functions make sure your subscription ID has your `FunctionsConsumer` contract added as an authorized consumer. Also make sure that your subscription has enough LINK balance. You do this by calling `npx hardhat functions-sub-info --network network_name_here --subid subscription_id_here` to see your subscription details. If the Functions Router calculates that your subscription's balance is insufficient it will revert with a `InsufficientBalance` custom Solidity error.

3. When running Chainlink Functions with Automation you also need to ensure the Chainlink Automation upkeeps are funded to run the automation calls. The fastest way to maintain your Automation LINK subscription balance is through the Chainlink Automation web app here: [https://automation.chain.link/](https://automation.chain.link/)

4. If you get a transaction failure when calling `npx hardhat functions-request` and its an out of gas error (you can tell from the block explorer or from [Tenderly](https://tenderly.co)) then you may need to add the optional `---requestgaslimit` flag with a value higher than than the default which is 1_500_000. For example: `npx hardhat functions-request --requestgaslimit 1750000`. Note that `./tasks/Functions-consumer/request.js` already has some logic around this that applies to some networks that require higher gas.

5. <b>BASE Sepolia / Optimism Sepolia:</b> if you see an error like `ProviderError: transaction underpriced: tip needed 50, tip permitted 0` then wait a few seconds and re-try. This can happen due to network spikes. Also double check the `./networks.js` file configs to make sure that `gasPrice` is set to `1000_000` as these networks can require higher request gas.
