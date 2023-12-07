---
author: Oleh Andrushko
title: "Smart contract solidity development with foundry"
date: "2023-12-01"
description: Discover the ease of smart contract solidity development with Foundry in this step-by-step guide. From initializing your project and using OpenZeppelin libraries to configuring continuous integration to streamline your blockchain development process.

categories: 
- Blockchain
  
tags: 
- foundry
- solidity
- programming
  
image: h2.png

readingTime: 15

draft: false
---

Developing smart contracts on EVM blockchains requires a robust development environment. While Hardhat is a popular choice, Foundry offers a streamlined, efficient alternative. This post guides you through setting up a Solidity development repository using Foundry.

## Prerequisites

- [GitHub](https://github.com/)
- Latest [Rust](https://www.rust-lang.org/) version
- [Solidity for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=JuanBlanco.solidity)
- latest [Solidity](https://docs.soliditylang.org/en/v0.8.23/installing-solidity.html) version

## Why Foundry?
> [Foundry](https://book.getfoundry.sh/) is a smart contract development toolchain. Foundry manages your dependencies, compiles your project, runs tests, deploys, and lets you interact with the chain from the command-line and via Solidity scripts. ..[read foundry annoncement article](https://www.paradigm.xyz/2021/12/introducing-the-foundry-ethereum-development-toolbox)..

Foundry was created by the team at [Paradigm](https://www.paradigm.xyz/), a cryptocurrency investment firm, to improve and streamline the experience of building on Ethereum. [Paradigm](https://www.paradigm.xyz/) is known for its contributions to the Ethereum ecosystem, with a team that includes researchers, engineers, and security experts.

Foundry is made up of four components:

- [**Forge**](https://book.getfoundry.sh/forge/): a testing framework (similar to Truffle, Hardhat, and DappTools).
- [**Cast**](https://book.getfoundry.sh/cast/): CLI for interacting (performing RPC calls) with EVM smart contracts, sending transactions, and getting chain data.
- [**Anvil**](https://book.getfoundry.sh/anvil/): local Ethereum node, similar to Ganache or Hardhat Network.
- [**Chisel**](https://book.getfoundry.sh/chisel/): advanced Solidity REPL. It can be used to quickly test the behavior of Solidity snippets on a local or forked network.

When weighing Foundry against other smart contract development frameworks (like Hardhat), it stands out for several key reasons:

- **Speed and Efficiency**: Foundry is written in Rust to be blazing fast, easy to install, and friendly to a wider set of contributors. It compiles Solidity code faster than Hardhat, which can be a significant advantage when iterating over smart contract development cycles.

- **Advanced Testing**: Foundry uses [Forge for testing](https://book.getfoundry.sh/forge/advanced-testing), which allows for [writing tests](https://www.paradigm.xyz/2021/12/introducing-the-foundry-ethereum-development-toolbox#you-should-be-writing-your-tests-in-solidity) in Solidity itself, unlike Hardhat which typically uses JavaScript. This can simplify the process as developers can write tests in the same language they're developing the smart contracts.

- **Integrated Debugging**: The Forge testing framework includes an [in-built debugger](https://book.getfoundry.sh/forge/debugger), which can be used without additional plugins or configurations, offering a streamlined debugging process.

- **Flexibility and Minimalism:** Foundry is minimalistic and flexible, it comes with fewer out-of-the-box features, which can be seen as a plus for developers who prefer a less opinionated framework that they can tailor to their specific needs.

- **Built-In Fuzz Testing**: Foundry supports [fuzz testing](https://www.paradigm.xyz/2021/12/introducing-the-foundry-ethereum-development-toolbox#you-should-be-fuzzing-your-functions), which tests contracts with huge amounts of random inputs to ensure robustness against unforeseen inputs.

- **First-Class Support for Solidity**: Being a solidity-first tool, Foundry provides first-class support for the language, with updates closely following Solidity's updates.

- **Gas Usage Reports:**: Forge generates [gas reports](https://book.getfoundry.sh/forge/gas-reports), a crucial feature for blockchain development, to help developers optimize smart contract efficiency.

- **Security Focus**: With tools like [Slither integrated](https://book.getfoundry.sh/config/static-analyzers), Foundry has a strong focus on security, making it easier to write safer smart contracts.


## Step 1: Install Foundry

Open your terminal and run the following command:
```bash
curl -L https://foundry.paradigm.xyz | bash
```
and then:
```bash
# if need to refresh the configs
source /path/to/.zshenv
# to actually install foundry
foundryup
```

## Step 2: Initiate the project

Create a new directory and run the following command inside:
```bash
forge init
```
The command will generate a complete project with the following structure:
- `.github/workflows`: a folder that contains a `test.yml` file with foundry-related GitHub actions
- `lib`: a directory for storing actual libraries/dependencies as git-submodules (similar to `node_modules` in hardhat)
- `script`: a directory for solidity scripts (similar to `tasks` in hardhat)
- `src`: a default directory for contracts code (similar to `contracts` in hardhat)
- `test`: a default directory for tests, where any contract with a function that starts with test is considered to be a test.
- `.gitmodules`: a file for mapping installed libraries/dependencies as [git-submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules)
- `foundry.toml`: a configuration file for Foundry behavior: e.g. contract folder location, output etc...

Now you can run the following [commands](https://book.getfoundry.sh/reference/forge/) to check if everything is working correctly inside the directory:
```bash
# 1. building the project
forge build
# 2. testing example contract
forge test
```

## Step 3: Smart Contract developing

### Install dependencies
Forge manages dependencies using [git submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules) by default, which means that it works with any GitHub repository that contains smart contracts.

In this case, we will use OpenZeppelin library and install it as a dependency in our project by simply running the command:
```bash
forge install OpenZeppelin/openzeppelin-contracts
```
It should add the `openzeppelin-contracts` library in `/lib` directory.
If you are using VS Code, you might see some errors like: "Source "forge-std/Test.sol" not found: File import callback not supported". In order to fix it simply run the [remappings](https://book.getfoundry.sh/config/vscode#1-remappings) command:
```bash
forge remappings > remappings.txt
```

### ERC20 Token
In this example, we will work with OpenZeppelin ERC20 smart contract:
1. Rename (or create new) the `src/Counter.sol` to `src/MyToken.sol` and replace it with the following code:
```Solidity
 // SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract MyToken is ERC20, Ownable {
    constructor(address initialOwner)
        ERC20("MyToken", "MTK")
        Ownable(initialOwner)
    {}

    function mint(address to, uint256 amount) public onlyOwner {
        _mint(to, amount);
    }
}
```

2. Rename (or create new) the `test/Counter.t.sol` to `test/MyToken.t.sol` and replace it with the following code:
```Solidity
 // SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import {Test, console2} from "forge-std/Test.sol";
import {MyToken} from "../src/MyToken.sol";

contract MyTokenTest is Test {
    MyToken public token;

    function setUp() public {
        token = new MyToken(0x636C16881D405cdE477f56546825c88862be5189);
    }
}
```

3. Rename (or create new) the `script/Counter.s.sol` to `script/MyToken.s.sol` and replace it with the following code:
```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import {Script, console2} from "forge-std/Script.sol";

contract MyTokenScript is Script {
    function setUp() public {}

    function run() public {
        vm.broadcast();
    }
}
```
> [Read more on solidity scripting...](https://book.getfoundry.sh/tutorials/solidity-scripting)

4. Run `forge build` to make sure everything is working as expected:
```bash
$ forge build
[⠒] Compiling...
[⠔] Compiling 30 files with 0.8.23
[⠑] Solc 0.8.23 finished in 3.40s
Compiler run successful!
```

### Write some tests
Let's add some more tests for our ERC20 token inside `MyToken.t.sol`.

Forge uses the following keywords in tests:
- **setUp**: an optional function invoked before each test case is run
- **test**: functions prefixed with test are run as a test case
- **testFailt**: the inverse of the test prefix - if the function does not revert, the test fails
> [Read more on writing tests in foundry...](https://book.getfoundry.sh/forge/writing-tests)

For now, let's just add a simple test for minting functionality, here's the complete content of `MyToken.t.sol`: 
```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import {Test, console2} from "forge-std/Test.sol";
import {MyToken} from "../src/MyToken.sol";

contract MyTokenTest is Test {
    MyToken public token;
    address constant contractOwner = 0x636C16881D405cdE477f56546825c88862be5189;
     address constant alice = 0x001C7CA24BC0B0b81b2F9Ee69dEDD76995861C39;

    function setUp() public {
        token = new MyToken(contractOwner);
    }

    function testMint() public { 
        // Impersonate the owner to call mint function
        vm.prank(contractOwner);
        // mint tokens to Alice
        token.mint(alice, 10000000);
        // assert the balance
        assertEq(10000000, token.balanceOf(alice));
    }
}
```

Run the tests:
```bash
forge test
[⠢] Compiling...
No files changed, compilation skipped

Running 1 test for test/MyToken.t.sol:MyTokenTest
[PASS] testMint() (gas: 58176)
Test result: ok. 1 passed; 0 failed; 0 skipped; finished in 528.25µs
 
Ran 1 test suites: 1 tests passed, 0 failed, 0 skipped (1 total tests)
```
Forge can produce [traces](https://book.getfoundry.sh/forge/traces) either for failing tests (-vvv) or all tests (-vvvv), what could be very useful for debugging the tests:
```bash
forge test -vvvv
[⠢] Compiling...
No files changed, compilation skipped

Running 1 test for test/MyToken.t.sol:MyTokenTest
[PASS] testMint() (gas: 58176)
Traces:
  [58176] MyTokenTest::testMint()
    ├─ [0] VM::prank(0x636C16881D405cdE477f56546825c88862be5189)
    │   └─ ← ()
    ├─ [49064] MyToken::mint(0x001C7CA24BC0B0b81b2F9Ee69dEDD76995861C39, 10000000 [1e7])
    │   ├─ emit Transfer(from: 0x0000000000000000000000000000000000000000, to: 0x001C7CA24BC0B0b81b2F9Ee69dEDD76995861C39, value: 10000000 [1e7])
    │   └─ ← ()
    ├─ [563] MyToken::balanceOf(0x001C7CA24BC0B0b81b2F9Ee69dEDD76995861C39) [staticcall]
    │   └─ ← 10000000 [1e7]
    └─ ← ()

Test result: ok. 1 passed; 0 failed; 0 skipped; finished in 1.09ms
 
Ran 1 test suites: 1 tests passed, 0 failed, 0 skipped (1 total tests)
```
You can also generate a pretty cool gas usage report with `--gas-report` suffix:
```bash
forge test --gas-report
[⠢] Compiling...
No files changed, compilation skipped

Running 1 test for test/MyToken.t.sol:MyTokenTest
[PASS] testMint() (gas: 58176)
Test result: ok. 1 passed; 0 failed; 0 skipped; finished in 956.46µs
| src/MyToken.sol:MyToken contract |                 |       |        |       |         |
|----------------------------------|-----------------|-------|--------|-------|---------|
| Deployment Cost                  | Deployment Size |       |        |       |         |
| 518572                           | 3010            |       |        |       |         |
| Function Name                    | min             | avg   | median | max   | # calls |
| balanceOf                        | 563             | 563   | 563    | 563   | 1       |
| mint                             | 49064           | 49064 | 49064  | 49064 | 1       |

Ran 1 test suites: 1 tests passed, 0 failed, 0 skipped (1 total tests)
```

> [Read more about test best practices...](https://book.getfoundry.sh/tutorials/best-practices#tests)

## Step 4: Configuring CI with GitHub actions

Foudry already created some basic ci steps inside `.github/workflows/test.yml`, but we will boost our pipeline with some more steps like:
- Solidity code **format check**: since to keep it simple, we do not have a linter in this setup, we will introduce this simple step just to be sure we are not commiting an ugly code :)
- **Coverage** report: committing a robust contract code is quite important so we will automatically check our test coverage regression in each PR.
- **Static analysis**: committing a secure contract code is also important, so we will use [slither](https://github.com/crytic/slither) to automatically analyze the code.
- Publish **documentation**: foundry has a nice way to automatically [generate smart contract documentation ](https://book.getfoundry.sh/reference/forge/forge-doc) so we will use that to publish a new version of documentation on [GitHub pages].

Let's start with renaming `.github/workflows/test.yml` file to `.github/workflows/ci.yml` and refactor the jobs.

### Build and test
This one is for building and testing our solidity code:
```yml
build-n-test:
  runs-on: ubuntu-latest
  steps:
    - uses: actions/checkout@v3

    - name: Install Foundry
      uses: foundry-rs/foundry-toolchain@v1

    - name: Build contracts
      run: |
        forge --version
        forge build --sizes

    - name: Run tests
      run: |
        forge test -vvv
```

### Format checks
This one is pretty simple, we just need to know if the code is formatted in the right way. 
A job to add to the ci should be the following:
```yml
format:
  runs-on: ubuntu-latest
  steps:
    - uses: actions/checkout@v3

    - name: Install Foundry
      uses: foundry-rs/foundry-toolchain@v1

    - name: Check formatting
      run: |
        forge --version
        forge fmt --check
```

### Coverage
In this job we:
- use [forge coverage](https://book.getfoundry.sh/reference/forge/forge-coverage) command to generate the report.
- publish a comprehensive coverage report as a comment on each push request using [romeovs/lcov-reporter-action](https://github.com/romeovs/lcov-reporter-action).
- check coverage threshold using [zgosalvez/github-actions-report-lcov](https://github.com/zgosalvez/github-actions-report-lcov).

The complete code for the job looks like this:
```yml
coverage:
  runs-on: ubuntu-latest
  steps:
    - uses: actions/checkout@v3

    - name: Install Foundry
      uses: foundry-rs/foundry-toolchain@v1
    # Generate coverage report
    - name: Run coverage
      run: forge coverage --report summary --report lcov

    # Adjust the paths in this step to exclude specific directories from coverage analysis.
    - name: Filter directories
      run: |
        sudo apt update && sudo apt install -y lcov
        lcov --remove lcov.info 'test/*' 'script/*' --output-file lcov.info --rc lcov_branch_coverage=1

    # This step automatically publishes a comprehensive coverage report as a comment on each push and
    # removes any previous comments. 
    - name: Post coverage report
      if: github.event_name == 'pull_request'
      uses: romeovs/lcov-reporter-action@v0.3.1
      with:
        delete-old-comments: true
        lcov-file: ./lcov.info
        github-token: ${{ secrets.GITHUB_TOKEN }} # Adds a coverage summary comment to the PR.

    # This step verifies that the minimum coverage threshold is met and fails if it is not.
    - name: Verify minimum coverage
      uses: zgosalvez/github-actions-report-lcov@v4
      with:
        coverage-files: ./lcov.info
        minimum-coverage: 100 # Set coverage threshold.
```

> Remember not to optimize for coverage, but to optimize for [well thought-out tests](https://book.getfoundry.sh/tutorials/best-practices?highlight=coverage#best-practices-1).

### Static analysis
For static analysis, we use [slither actions](https://github.com/crytic/slither-action) and the complete code for the job looks like this:
```yml
code-analysis:
  runs-on: ubuntu-latest
  permissions:
    # required for uploading slither sarif results
    security-events: write
  steps:
    - uses: actions/checkout@v2

    - name: Run static analysis with slither
      uses: crytic/slither-action@v0.3.0
      id: slither
      continue-on-error: true
      with:
        sarif: results.sarif
        slither-args: --filter-paths "./lib|./test" # exclude other contracts (open zeppelin ones) and test 
         
    - name: Upload static anaylisis SARIF output file # it should also add relevant comments on PR
      uses: github/codeql-action/upload-sarif@v2
      with:
        sarif_file: ${{ steps.slither.outputs.sarif }}
```

### Documentation
This will be a separate workflow and it will be executed only then we push to `main` branch. 
The flow will build the documentation and publish the generated static website to GitHub pages of the repository. 

To do so just create a new file `.github/workflows/docs.yml` with the following content:
```yml
# Simple workflow for deploying static content to GitHub Pages
name: Documentation Deploy

on:
  # Runs on pushes targeting the default branch
  push:
    branches: ["main"]
  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  # Single deploy job since we're just deploying
  build-n-deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Install Foundry
        uses: foundry-rs/foundry-toolchain@v1 

      - name: Build documentation
        run: |
          forge --version
          forge doc --build

      - name: Setup Pages
        uses: actions/configure-pages@v4

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v2
        with:
          # Upload final docs folder
          path: 'docs/book'
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v3

```
This will automatically generate documentation for your smart contract and publish it to GitHub pages like this one: https://olich97.github.io/solidity-foundry-template.

We are done with the CI configuration and you can view the complete code for this GitHub Actions setup under [olich97/solidity-foundry-template/.github/workflows](https://github.com/olich97/solidity-foundry-template/tree/main/.github/workflows).


![That was smooth!](smooth.gif)

## Step 5: Smart Contract deployment and verification

There are [different ways](https://book.getfoundry.sh/forge/deploying) to deploy and verify a smart contract with foundry. 

In this case, I choose to write my own deployment script and use a `.env` file for configuration. 
> **Note**: you must be careful when exposing private keys in a `.env` file and loading them into programs. This is only recommended for use with non-privileged deployers or for local / test setups. For production setups please review the various [wallet options](https://book.getfoundry.sh/reference/forge/forge-script#wallet-options---raw) that Foundry supports.

1. Rename (or create new) previously updated `script/MyToken.s.sol` to `script/DeployMyToken.sol` and replace the content with the following code: 
```Solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import {Script, console2} from "forge-std/Script.sol";
import {MyToken} from "../src/MyToken.sol";

contract DeployMyTokenScript is Script {
    function run() external {
        // load variables from envinronment
        uint256 deployerPrivateKey = vm.envUint("DEPLOYER_PRIVATE_KEY");
        address contractOwner = vm.envAddress("CONTRACT_OWNER_ADDRESS");
        // deploying the contract
        vm.startBroadcast(deployerPrivateKey);
        MyToken token = new MyToken(contractOwner);
        vm.stopBroadcast();
    }
}
```
2. Create the `.env` file with following content:
```env
# Get your api key from: https://etherscan.io/
ETHERSCAN_API_KEY=<your-etherscan-api-key>
# Get your api key from: https://www.alchemy.com/pricing
SEPOLIA_URL=https://eth-sepolia.alchemyapi.io/v2/<alchemy key>
# export from wallet
DEPLOYER_PRIVATE_KEY=<your-account-private-key>
# in order to transfer ownership after deploy
CONTRACT_OWNER_ADDRESS= <target address>
```
3. Run the following commands to deploy and verify the contract on Sepolia:
```bash
# To load the variables in the .env file
source .env
# To deploy and verify our contract
forge script script/DeployMyToken.sol:DeployMyTokenScript --rpc-url $SEPOLIA_URL --broadcast --verify -vvvv
```
This command will deploy the contract on sepolia testnet and verify it on etherscan, the output should be similar to the following (love the log details!):
```bash
forge script script/DeployMyToken.sol:DeployMyTokenScript --rpc-url $SEPOLIA_URL --broadcast --verify -vvvv
[⠢] Compiling...
No files changed, compilation skipped
Traces:
  [576533] DeployMyTokenScript::run()
    ├─ [0] VM::envUint("DEPLOYER_PRIVATE_KEY") [staticcall]
    │   └─ ← <env var value>
    ├─ [0] VM::envAddress("CONTRACT_OWNER_ADDRESS") [staticcall]
    │   └─ ← <env var value>
    ├─ [0] VM::startBroadcast(104482394105896740396624689231901044842727466658525734315239796502028651856635 [1.044e77])
    │   └─ ← ()
    ├─ [518572] → new MyToken@0x72ea9eB9019e52cF53fadA75eC32eC7ccf2d4996
    │   ├─ emit OwnershipTransferred(previousOwner: 0x0000000000000000000000000000000000000000, newOwner: 0xca4938B679aB6e6c9B7a19610C306A2256B7dF2a)
    │   └─ ← 2245 bytes of code
    ├─ [0] VM::stopBroadcast()
    │   └─ ← ()
    └─ ← ()
Script ran successfully.
## Setting up 1 EVM.
==========================
Simulated On-chain Traces:
  [618088] → new MyToken@0x72ea9eB9019e52cF53fadA75eC32eC7ccf2d4996
    ├─ emit OwnershipTransferred(previousOwner: 0x0000000000000000000000000000000000000000, newOwner: 0xca4938B679aB6e6c9B7a19610C306A2256B7dF2a)
    └─ ← 2245 bytes of code
==========================
Chain 11155111
Estimated gas price: 14.833583938 gwei
Estimated total gas used for script: 803514
Estimated amount required: 0.011918992364358132 ETH
==========================
###
Finding wallets for all the necessary addresses...
##
Sending transactions [0 - 0].
⠁ [00:00:00] [##################################################################################################################################] 1/1 txes (0.0s)
Transactions saved to: /Users/olich/tmp/solidity-foundry-template/broadcast/DeployMyToken.sol/11155111/run-latest.json
Sensitive values saved to: /Users/olich/tmp/solidity-foundry-template/cache/DeployMyToken.sol/11155111/run-latest.json
##
Waiting for receipts.
⠉ [00:00:06] [##############################################################################################################################] 1/1 receipts (0.0s)
##### sepolia
✅  [Success]Hash: 0xc693b84d7652efb6b2e5844dd7b0cc097d3d6002871d907798b60f9d32d2e7fd
Contract Address: 0x72ea9eB9019e52cF53fadA75eC32eC7ccf2d4996
Block: 4834242
Paid: 0.00645380857284286 ETH (618278 gas * 10.43836037 gwei)
##
Transactions saved to: /Users/olich/tmp/solidity-foundry-template/broadcast/DeployMyToken.sol/11155111/run-latest.json
Sensitive values saved to: /Users/olich/tmp/solidity-foundry-template/cache/DeployMyToken.sol/11155111/run-latest.json
==========================
ONCHAIN EXECUTION COMPLETE & SUCCESSFUL.
Total Paid: 0.00645380857284286 ETH (618278 gas * avg 10.43836037 gwei)
##
Start verification for (1) contracts
Start verifying contract `0x72ea9eB9019e52cF53fadA75eC32eC7ccf2d4996` deployed on sepolia
##
Submitting verification for [src/MyToken.sol:MyToken] 0x72ea9eB9019e52cF53fadA75eC32eC7ccf2d4996.
##
Submitted contract for verification:
        Response: `OK`
        GUID: `dnbux2hiruxytzwdylwuedk7dst7esbbwzdxggttqmwsa1rizr`
        URL:
        https://sepolia.etherscan.io/address/0x72ea9eb9019e52cf53fada75ec32ec7ccf2d4996
Contract verification status:
Response: `NOTOK`
Details: `Pending in queue`
Contract verification status:
Response: `OK`
Details: `Pass - Verified`
Contract successfully verified
All (1) contracts were verified!
##
Transactions saved to: /Users/olich/tmp/solidity-foundry-template/broadcast/DeployMyToken.sol/11155111/run-latest.json
Sensitive values saved to: /Users/olich/tmp/solidity-foundry-template/cache/DeployMyToken.sol/11155111/run-latest.json
```
![Pretty cool huh!!](ok.gif)

## Conclusion
As the blockchain landscape continues to evolve, tools like Foundry are crucial for developers seeking to build secure, efficient, and reliable smart contracts. This guide serves as an invaluable resource, whether you're a seasoned developer or just starting in the world of blockchain and smart contract development.

Complete code: [olich97/solidity-foundry-template](https://github.com/olich97/solidity-foundry-template).