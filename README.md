# Implementing Celo Blockchain Oracles for External Data Integration

## Introduction

In this tutorial, we will explore how to leverage Celo blockchain oracles to integrate external data into smart contracts and DApps. The ability to access off-chain data is crucial for real-world use cases, and decentralized oracles provide a reliable and secure way to fetch and verify this data within the Celo ecosystem.

We will be using Hardhat, a popular development environment for Ethereum and Celo, to build our project. Hardhat provides a robust framework and toolset for smart contract development, testing, and deployment.

## Prerequisites

Before we begin, make sure you have the following:

1. Basic knowledge of Solidity and smart contract development.
2. Node.js installed on your machine.
3. Familiarity with Hardhat. If you're new to Hardhat, you can refer to the [Hardhat documentation](https://hardhat.org/getting-started/) for installation and setup instructions.

## Setting up the Project

Let's start by setting up a new Hardhat project.

1. Create a new directory for your project:

   ```bash
   mkdir celo-oracles-tutorial
   cd celo-oracles-tutorial


2. Initialize a new Hardhat project:

```
npx hardhat init
```
3. Install the required dependencies:

```
npm install @celo/hardhat-celo hardhat dotenv
```

4. Create a new file named .env in the root directory of your project and add the following content:

```
MNEMONIC=<your-mnemonic-phrase>
```
Replace <your-mnemonic-phrase> with the mnemonic phrase of your Celo account. Make sure the account has some testnet CELO tokens for testing purposes.

## Configuring Hardhat
 Next, let's configure Hardhat to work with the Celo network and enable the required plugins.

1. Open the `hardhat.config.js` file generated by the  `npx hardhat init` command.

2. Replace the contents of the file with the following code:

```
   require('dotenv').config();

module.exports = {
  defaultNetwork: 'hardhat',
  networks: {
    hardhat: {},
    alfajores: {
      url: 'https://alfajores-forno.celo-testnet.org',
      accounts: { mnemonic: process.env.MNEMONIC },
    },
    mainnet: {
      url: 'https://forno.celo.org',
      accounts: { mnemonic: process.env.MNEMONIC },
    },
  },
  solidity: {
    version: '0.8.7',
    settings: {
      optimizer: {
        enabled: true,
        runs: 200,
      },
    },
  },
  paths: {
    sources: './contracts',
    tests: './test',
    cache: './cache',
    artifacts: './artifacts',
  },
  mocha: {
    timeout: 20000,
  },
};
```

This configuration sets up the networks for the Celo Alfajores testnet and the Celo mainnet. It also includes the necessary plugins and specifies the Solidity compiler version.

## Creating the Oracle Contract
Now, let's create a new Solidity contract that utilizes Celo oracles to fetch and store external data.

1. Create a new file named Oracle.sol in the contracts directory.

2. Add the following code to define the Oracle contract:

 ```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import '@celo/hardhat-celo';

contract Oracle {
    uint256 public data;

    constructor() {}

    function fetchExternalData(uint256 _requestId) external {
        require(msg.value >= 0.01 ether, 'Insufficient payment');
        data = address(this).balance;
    }
}

 ```
In this contract, we define a `data` variable to store the fetched external data. The `fetchExternalData` function is used to trigger the data retrieval process.

## Writing Tests
Let's write some tests to ensure our contract functions as expected.

1. Create a new file named oracle.test.js in the test directory.

2. Add the following code to define the tests:

```
  const { expect } = require('chai');
const { ethers } = require('hardhat');

describe('Oracle', function () {
  let oracle;

  beforeEach(async function () {
    const Oracle = await ethers.getContractFactory('Oracle');
    oracle = await Oracle.deploy();
    await oracle.deployed();
  });

  it('should fetch external data', async function () {
    await oracle.fetchExternalData();
    const data = await oracle.data();
    expect(data).to.be.equal(0);
  });
});
```

In this test, we deploy the `Oracle` contract and verify that the `fetchExternalData` function correctly initializes the `data` variable.

## Compiling and Running Tests

1. Compile the contracts and run the tests using Hardhat:

```
npx hardhat compile
npx hardhat test
```
If everything is set up correctly, you should see the test passing.

## Conclusion
Congratulations! You have learned how to implement Celo blockchain oracles for external data integration in your smart contracts and DApps. By leveraging Celo oracles, you can securely access off-chain data and enhance the functionality of your decentralized applications.

Remember to refer to the official Celo documentation and community resources to stay updated with the latest features and best practices.
   

