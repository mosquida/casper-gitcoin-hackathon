1. ## Create and deploy a simple, smart contract [with cargo casper and cargo test](https://docs.casperlabs.io/en/latest/dapp-dev-guide/setup-of-rust-contract-sdk.html)

   Error occurs when using the cargo test command

   ![](https://github.com/mosquida/casper-gitcoin-hackathon/blob/master/get-started-with-casper/screenshots/1/1-test-error.png)

   

   Placing contract\target\wasm32-unknown-unknown\release\contract.wasm to test/wasm will make the test work

   ![](https://github.com/mosquida/casper-gitcoin-hackathon/blob/master/get-started-with-casper/screenshots/1/2-wasm-copied-to-wasm-folder-succeed.png)

   

   Spinning up the local testnet for deployment

   ![](https://github.com/mosquida/casper-gitcoin-hackathon/blob/master/get-started-with-casper/screenshots/1/3-localnet-started.png)

   

   Successful contract deployment

   ![](https://github.com/mosquida/casper-gitcoin-hackathon/blob/master/get-started-with-casper/screenshots/1/4-deploy-contract-locally.png)

   

   Querying deployed contract details

   ![](https://github.com/mosquida/casper-gitcoin-hackathon/blob/master/get-started-with-casper/screenshots/1/5-contract-deployed%20status.png)

   

2. ## Complete one of the [existing tutorials](https://docs.casperlabs.io/en/latest/dapp-dev-guide/tutorials/index.html) for writing smart contracts

   * [Multi-Signature Tutorial](https://docs.casperlabs.io/en/latest/dapp-dev-guide/tutorials/multi-sig/index.html) was selected from existing tutorial

     

   Building the contract

   ![](https://github.com/mosquida/casper-gitcoin-hackathon/blob/master/get-started-with-casper/screenshots/2/1-contract-build-multi-%20sig-existing-tut.png)

   

   Building the client

   ![](https://github.com/mosquida/casper-gitcoin-hackathon/blob/master/get-started-with-casper/screenshots/2/2-client-build-multi-%20sig-existing-tut.png)

   

   **Testing the client**

   * Start running client

   ![](https://github.com/mosquida/casper-gitcoin-hackathon/blob/master/get-started-with-casper/screenshots/2/3-atomic-start-pt1-multi-%20sig-existing-tut.png)

   

   * Output after running client

   ![](https://github.com/mosquida/casper-gitcoin-hackathon/blob/master/get-started-with-casper/screenshots/2/4-atomic-start-pt2-multi-%20sig-existing-tut.png.png)

   

3. ## Demonstrate [key management concepts](https://docs.casperlabs.io/en/latest/dapp-dev-guide/tutorials/multi-sig/index.html) by modifying the client in the Multi-Sig tutorial to address one of the [additional scenarios](https://docs.casperlabs.io/en/latest/dapp-dev-guide/tutorials/multi-sig/examples.html)

   * [Scenario 2: Deploying with special keys ](https://docs.casperlabs.io/en/latest/dapp-dev-guide/tutorials/multi-sig/examples.html#scenario-2-deploying-with-special-keys) was selected from scenarios

   

   Modified Code from scenario-atomic.js, (2 keys, Address a1(mainAccount) can manage key and deploy, Address b2(deployAccount) can only deploy)

   ````javascript
   const keyManager = require('./key-manager');
   const TRANSFER_AMOUNT = process.env.TRANSFER_AMOUNT || 2500000000;
   
   (async function () {
       // SCENARIO 2 
       // 1. Set Address a1(mainAccount) weight to 2.
       // 2. Set Address a1(mainAccount) Keys Management Threshold to 2.
       // 3. Set Address a1(mainAccount) Deploy Threshold to 1.
       // 4. Add Address b2(deployAccount) first new key with weight 1 .
   
       let deploy;
   
       // 0. Initial state of the account.
       // There should be only one associated key (facuet) with weight 1.
       // Deployment Threshold should be set to 1.
       // Key Management Threshold should be set to 1.
       let masterKey = keyManager.randomMasterKey();
       let mainAccount = masterKey.deriveIndex(1);    // deployment and management
       let deployAccount = masterKey.deriveIndex(2);    // only for deployment
   
       console.log("\n0.1 Fund main account.\n");
       await keyManager.fundAccount(mainAccount);
       await keyManager.printAccount(mainAccount);
       
       console.log("\n[x]0.2 Install Keys Manager contract");
       deploy = keyManager.keys.buildContractInstallDeploy(mainAccount);
       await keyManager.sendDeploy(deploy, [mainAccount]);
       await keyManager.printAccount(mainAccount);
   
       // 1. Set Address a1(mainAccount) weight to 2.
       console.log("\n1. Set faucet's weight to 2\n");
       deploy = keyManager.keys.setKeyWeightDeploy(mainAccount, mainAccount, 2);
       await keyManager.sendDeploy(deploy, [mainAccount]);
       await keyManager.printAccount(mainAccount);
       
       // 2. Set Address a1(mainAccount) Keys Management Threshold to 2.
       console.log("\n2. Set Keys Management Threshold to 2\n");
       deploy = keyManager.keys.setKeyManagementThresholdDeploy(mainAccount, 2);
       await keyManager.sendDeploy(deploy, [mainAccount]);
       await keyManager.printAccount(mainAccount);
       
       // 3. Set Address a1(mainAccount) Deploy Threshold to 1.
       console.log("\n3. Set Deploy Threshold to 1.\n");
       deploy = keyManager.keys.setDeploymentThresholdDeploy(mainAccount, 1);
       await keyManager.sendDeploy(deploy, [mainAccount]);
       await keyManager.printAccount(mainAccount);
       
       // 4. Add Address b2(deployAccount) first new key with weight 1.
       console.log("\n4. Add first new key with weight 1.\n");
       deploy = keyManager.keys.setKeyWeightDeploy(mainAccount, deployAccount, 1);
       await keyManager.sendDeploy(deploy, [mainAccount]);
       await keyManager.printAccount(mainAccount);
       
   })();
   ````

   

   **Testing the client**

   * Start running client

     ![](https://github.com/mosquida/casper-gitcoin-hackathon/blob/master/get-started-with-casper/screenshots/3/1-atomic-start-pt1-multi-sig-existing-tut-modified.png)

     

   * Output after running client

     ![](https://github.com/mosquida/casper-gitcoin-hackathon/blob/master/get-started-with-casper/screenshots/3/2-atomic-start-pt2-multi-sig-existing-tut-modified.png.png)

   

3. ## Learn to transfer tokens to an account on the [Casper Testnet](https://testnet.cspr.live/). Check out [this documentation](https://docs.casperlabs.io/en/latest/workflow/transfer-workflow.html).

   Transferring Token to Account 2

   ![](https://github.com/mosquida/casper-gitcoin-hackathon/blob/master/get-started-with-casper/screenshots/3/2-atomic-start-pt2-multi-sig-existing-tut-modified.png.png)

   

   Get state root hash - need for next step

   ![](https://github.com/mosquida/casper-gitcoin-hackathon/blob/master/get-started-with-casper/screenshots/4/2-get-state-root-hash.png)

   

   Query target account details - main_purse need for next step

   ![](https://github.com/mosquida/casper-gitcoin-hackathon/blob/master/get-started-with-casper/screenshots/4/3-query-target-account-details-purse-need.png)

   

   Get target account amount

   ![](https://github.com/mosquida/casper-gitcoin-hackathon/blob/master/get-started-with-casper/screenshots/4/4-get%20target-account-amount-recieved.png)

   

5. ## Learn to Delegate and Undelegate on the [Casper Testnet](https://testnet.cspr.live/). Check out [these instructions](https://docs.casperlabs.io/en/latest/workflow/staking.html#delegating-tokens).

   * Testnet Account used: https://testnet.cspr.live/account/02030f2950062c51bfbb9ff5039fd94a43d5b8ab6e5143ae9416531b9ea75bb28d8d

   * Delegated 10 CSPR, Undelegated 5 CSPR for testing

   

   Delegation Success

   ![](https://github.com/mosquida/casper-gitcoin-hackathon/blob/master/get-started-with-casper/screenshots/5/delegate.png)

   

   Undelegation Success

   ![](https://github.com/mosquida/casper-gitcoin-hackathon/blob/master/get-started-with-casper/screenshots/5/undelegate.png)

