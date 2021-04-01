## Introduction 

Please refer to https://github.com/w3f/Open-Grants-Program/pull/212 for descriptions. 



## How to Run Tests

This guide is tested on macOS only. Feel free to comment if you would like me to help setting it up on other platforms as well. 

#### Install Node.js

please reference to [Node.js Website](https://nodejs.org/en/download/) 

- We recommend you to install [yarn](https://classic.yarnpkg.com/en/docs/install/#mac-stable) as an alternative to `npm` . Simple run `npm install --global yarn` 

- The repo is tested with nodejs version `14.6.0` , to check on your nodejs version `node -v` , to switch version of node, I recommend using [n](https://github.com/tj/n) by TJ. 

    

#### Setup the Substrate smart contract development environment

A good general guide to setup the environment for Substrate environment can be founded [here](https://substrate.dev/docs/en/knowledgebase/getting-started/). 



1. Install Rust for help: refer to [Rust Website](https://www.rust-lang.org/tools/install)

    ```bash
    curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
    source $HOME/.cargo/env
    ```

    Check your installed version
    ```bash
    rustc --version
    cargo --version
    ```
    This guides is tested with `rustc 1.50.0 (cb75ad5db 2021-02-10)` and `cargo 1.50.0 (f04e7fab7 2021-02-04)`

2. Install [Binaryen](https://github.com/WebAssembly/binaryen). You can simply install with [Homebrew](https://brew.sh/) on macOS

    ```bash
    brew install binaryen
    ```

    To install `Homebrew` use

    ```bash
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
    ```

3. Install [cargo-contract](https://github.com/paritytech/cargo-contract) 

    ```bash
    cargo install --force cargo-contract
    ```

4. Grab a local Substrate blockchain node with `pallet-contract` included. There are many options: [jupiter](https://github.com/patractlabs/jupiter) is the one we choose. Alternatively, you can get [canvas](https://github.com/paritytech/canvas-node) by Parity. `Rust` is known for compiling slowly. It took me an hour to compile [jupiter](https://github.com/patractlabs/jupiter). 

    - To use [jupiter](https://github.com/patractlabs/jupiter), follow this [guide](https://github.com/patractlabs/jupiter#compile-and-run).  



    - To use [canvas](https://github.com/paritytech/canvas-node), follow this [guide](https://substrate.dev/substrate-contracts-workshop/#/0/setup?id=installing-the-canvas-node). 
    
    output of `canvas --version` is `canvas 0.1.0-385c4cc-x86_64-macos`
    
    - Lastly, fire up the local blockchain 

        ```
        path-to-jupiter-repo/target/release/jupiter-prep --dev
        # OR with Canvas
        canvas --dev --tmp
        ```

        You can visit https://ipfs.io/ipns/dotapps.io and choose to connect to `ws://127.0.0.1:9944` to have a visual portal to interact with the blockchain. 

5. clone this repo and initialize it by

    ```bash
    git clone https://github.com/skyekiwi/skyepass-vault.git
    cd skyepass-vault
    yarn # or npm install
    ```

    

#### Run Tests

Simply type in `yarn test` to start testing. The process can take somewhere between 10 - 30 minutes. 



## Navigate the Code

```
├── LICENSE.txt        --> Apache 2 License
├── (artifacts)        --> to be generated by Redspot that contains the compiled contract
│   ├── skyepassvault.contract
│   └── skyepassvault.json
├── client        --> Client side code
│   ├── blockchain.ts - UNFINISHED blockchain adapter 
│   ├── db.ts - DB is used to interact with the local password database
│   ├── index.ts
│   ├── ipfs.ts - IPFS is used to itneract with IPFS
│   ├── local.json - is the database to store caches of blockchain data, empty by now
│   ├── metadata.ts - is used to maintain the metadata to be stored on IPFS and on blockchain
│   └── passwords.json - the password database file
├── contracts
│   ├── Cargo.lock 
│   ├── Cargo.toml
│   ├── lib.rs - source code for the smart contract
│   └── (target)
│       .......
├── (node_modules) - generated by npm or yarn
|       .......
├── package.json
├── redspot.config.ts - Reference: https://redspot.patract.io/en/documentation/#configuration
├── scripts
│   ├── deploy.ts - run by `yarn deploy`
├── tests
│   └── skyepassvault.test.ts
├── tsconfig.json
└── yarn.lock
```

There are 5 parts in `skyepassvault.test.ts` and the first part is a end to end walkthrough of the process. Test names and descriptions should be very self-explanatory.


