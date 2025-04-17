# Brownie Fund Me Project

## Introduction
This project allows you to interact with smart contracts for a fundraising application using the Brownie framework. It demonstrates the full process of deploying, funding, and withdrawing from the contract on various Ethereum networks.

### Project Overview
- The project is built using **Brownie**, a Python-based development framework for smart contracts.
- The contracts are written in **Solidity** and deployed on **Ethereum testnets** and **local networks** (e.g., Ganache, Rinkeby).
- This project includes funding and withdrawing functionalities, along with scripts to test the system across multiple networks.

---

## Setup

### Prerequisites
Before you begin, ensure that you have the following installed on your system:

1. **Python 3.6+**: Required for Brownie and other dependencies.
2. **Brownie**: Install Brownie using the following command:
   ```bash
   pip install eth-brownie
   ```
3. **Ganache**: Either use Ganache UI or the command line version for local blockchain development. You can download Ganache from [here](https://www.trufflesuite.com/ganache).
4. **Infura Account**: If you're deploying to Rinkeby or other mainnets, you'll need an Infura API key.

---

## Dependencies, Deploying, and Networks

### Dependencies
To ensure everything works as expected, make sure you install the following dependencies:

- `chainlink-brownie-contracts`: Used for interacting with Chainlink oracles in your contract.
   - [GitHub Link](https://github.com/smartcontractkit/chainlink-brownie-contracts)

```bash
pip install chainlink-brownie-contracts
```

### remappings
When working with external dependencies like Chainlink contracts, make sure to add **remappings** to your `brownie-config.yaml` to ensure proper contract resolution during deployment.

### Deploy Script (V1)
This script handles the deployment of the fundraising contract to different networks. It includes settings for both testnets (e.g., Rinkeby) and local chains.

```bash
brownie run deploy.py --network rinkeby
```

#### `helpful_scripts.py`
This file contains utility functions to interact with the deployed contract, like funding and withdrawing.

#### `__init__.py`
Make sure to include an `__init__.py` file to structure your Brownie project in the right way and ensure smooth integration with other scripts.

### Deploying to Rinkeby
To deploy to **Rinkeby**, use the following:

1. Set your Infura API key in `.env`:
   ```bash
   INFURA_PROJECT_ID=<your-infura-project-id>
   ```

2. Deploy with:
   ```bash
   brownie run deploy.py --network rinkeby
   ```

### Contract Verification (`publish_source`)

#### The Manual Way (Flattening)
Flatten your contract before verifying it on **Etherscan**. Flattening combines all the Solidity files into one file, making it easier to verify.

#### The Programmatic Way (Using Etherscan API)
To verify your contract programmatically:

1. Get an [Etherscan API Key](https://etherscan.io/apis).
2. Set the API key in your environment variables:
   ```bash
   ETHERSCAN_TOKEN=<your-etherscan-api-key>
   ```

3. Use the following command to verify:
   ```bash
   brownie run publish.py --network rinkeby
   ```

### Deploying to Local Chains
For deploying to a **local Ganache network**, make sure you configure it in your `brownie-config.yaml`.

#### Introduction to Mocking
Mocking external contracts (like Chainlink oracles) is necessary when testing in a local development environment.

### Constructor Parameters
When deploying, you can pass constructor parameters like the fundraising goal and the minimum contribution to the contract. Ensure that these are set appropriately for each network.

#### `networks` in `brownie-config.yaml`
Ensure your networks are properly set up for each deployment scenario. Here's an example configuration:

```yaml
networks:
  default: development
  development:
    cmd: ganache-cli
    host: http://127.0.0.1
    fork: https://infura.io/v3/$WEB3_INFURA_PROJECT_ID
    accounts: 10
    mnemonic: brownie
    port: 8545
  rinkeby:
    host: https://rinkeby.infura.io/v3/$INFURA_PROJECT_ID
    accounts:
      - $PRIVATE_KEY
```

### Copying Mock Contracts from Chainlink-Mix
To use Chainlink oracles, copy mock contracts from the [chainlink-mix repository](https://github.com/smartcontractkit/chainlink-mix) and add them to your project.

---

## Funding and Withdrawing Python Scripts

### Fund Script
The `fund.py` script allows you to send Ether to the contract for the fundraising process. Ensure the contract address is correctly set.

```bash
brownie run fund.py --network rinkeby
```

### Withdraw Script
This script allows you to withdraw funds from the contract. It ensures that only the contract owner can withdraw.

```bash
brownie run withdraw.py --network rinkeby
```

---

## Testing Across Networks

### `test_can_fund_and_withdraw`
The `test_can_fund_and_withdraw` test checks if users can fund and withdraw from the contract on different networks.

### Default Networks
Brownie supports default networks like **development** (local), **rinkeby**, **mainnet**, and others. Configure them properly in `brownie-config.yaml`.

### pytest Setup

Install `pytest` for testing:

```bash
pip install pytest
```

Use `pytest.skip()` to conditionally skip tests.

### Custom Mainnet Fork
You can simulate transactions on the **mainnet** using a **fork** of the network. This allows you to test contracts as if they were deployed on the actual Ethereum mainnet.

#### Adding a Custom Mainnet Fork
You can add a custom mainnet fork network to your Brownie configuration like this:

```bash
brownie networks add development mainnet-fork-dev cmd=ganache-cli host=http://127.0.0.1 fork='https://infura.io/v3/$WEB3_INFURA_PROJECT_ID' accounts=10 mnemonic=brownie port=8545
```

### Running Tests on Mainnet Fork
Run tests on a mainnet fork with:

```bash
brownie test --network mainnet-fork
```

### Ganache vs Local Ganache vs Mainnet Fork vs Testnet
- **Ganache**: A local blockchain for fast development.
- **Mainnet Fork**: Simulates mainnet conditions using Infura.
- **Testnet (Rinkeby, Kovan)**: Ethereum testnets for realistic, low-cost testing.

---

## Compatibility with Ganache UI

If you're using **Ganache UI**, you might encounter issues with certain test cases. For example:

### Error: `ValueError: Execution reverted during call`
This error can occur when trying to interact with contracts deployed on Ganache UI due to specific transaction behavior.

For more details, check out this [GitHub issue](https://github.com/trufflesuite/ganache/issues/).

---

## Conclusion

This README provides an organized guide to deploying, testing, and interacting with smart contracts using **Brownie**. Be sure to check your configurations, set the correct network, and properly use mock contracts for testing. Happy coding!

--- 
