# Prepare Miner

The basic steps to prepare miner:

1. Download miner program

2. Specify plot file paths

3. Enter Chia mnemonic

4. Configure timelord endpoint

5. Set reward address

6. Adjust network settings

## Download Miner

The mining program can be downloaded from the official open source library page:

[DePINC miner](https://github.com/DePINC/miner/releases/tag/v1.0.0rc2)

The pre-compiled package is the Windows version. If you use Linux, please refer to the instructions from [README.md](https://github.com/DePINC/miner) to download the source code and compile yourself.

## Configure Miner

### Initialize Configuration File

We use depinc-miner to carry out mining-related work, initializing an empty configuration file is one of its functions. You can also not use it to initialize the file, just copy the already written configuration file to the directory where you usually start mining, we will need to specify the location of the file when we start the mining program later.

Use the command below to initialize an empty configuration file.

```bash
> depinc-miner generate-config
```

Any editor can be used to edit this file. For how to modify this file, please refer to the content of the next subsection.

### Fill in Configuration

The configuration file format is Json text, named "config.json" by default. Below is an example, your configuration file will look similar to this. Note: The following configuration content points to mainnet. If you use the test network, please set testnet below to true, modify the content of host to http://127.0.0.1:18732, and modify the timelord connection string to timelord.depinc.org:29292

```json
{
    "testnet": false,
    "noproxy": true,
    "reward": "38CLnjuj31ifZMXZV8UhbyCo3fNP46Lszy",
    "seed": [
        "bird convince trend skin lumber escape crater describe ..."
    ],
    "plotPath": [
        "/home/matthew/data/plotfiles1",
        "/home/matthew/data/plotfiles2"
    ],
    "rpc": {
        "host": "http://127.0.0.1:8732",
    },
    "timelords": [
        "timelord.depinc.org:19191"
    ]
}
```

* Test Network "testnet"

When its value is true, use the test network, otherwise use the mainnet.

* Do not use network proxy "noproxy"

When this value is true, it means not to use a network proxy to connect to the wallet. If there is no special proxy requirement, please keep this option value as true.

* DePINC Reward Address "reward"

This is your address for receiving mining rewards in the DePINC network. Note that before you use this address, you need to make sure it has been bound to the Farmer public key related to your Chia initializing disk file. To learn how to perform the binding operation, please refer to the "Bind FarmerId" section.

* Chia Wallet Mnemonic "seed"

When you initialize the hard disk space, Chia will require you to provide a private key. This private key is composed of this string called "Seed", which is a combination of some words. When we generate DePINC blocks, we need to use this private key to sign the block to verify your identity. Please paste this "Seed" here. This private key is only used for signing and is only saved locally, it will not be uploaded to the DePINC chain or network.

* Plot File Directory "plotPath"

Here you need to specify your Plot file directory. This registration item is an array, you can register multiple Plot file directories at the same time.

* Local Wallet RPC Connection "rpc"

RPC connection with the local wallet. If you use login username and password, you can add "user" and "password" options here. By default, we use the `.cookie` file to complete the communication authentication with the wallet, so you only need to register the "host" item here, the "user" and "password" options do not need to be specified when using the cookie login method.

* Timelords

The endpoint string specifies the connection point for acquiring time-proofs. If a local timelord isn't configured, you can utilize the default timelord to obtain proofs. The default endpoint string of official timelord is `timelord.depinc.org:19191`.

## Start mining

### Understand the commands and parameters

All specifications for running the miner program are stored in a configuration file (e.g. config.json). The primary command to know is 'mining'. Use the '-c' argument to specify the config file location if it's not in the current directory. This streamlined approach allows for efficient setup and execution of the mining process.

For example, if you want to start mining with config.json in the current directory, simply run:

```bash
depinc-miner mining
```

The miner program will automatically locate and load settings from the `config.json` file once started.

And if you want to run the miner with a specified config.json file, for example if the config file path is `/etc/miner/config.json`, you can run the miner program as follows:

```bash
depinc-miner mining -c /etc/miner/config.json
```

### Bind FarmerId

Binding FarmerId with the miner's block generation address will allow the miner to use this address for mining. The signature related to this FarmerId needs to be verified in each block to maintain network security and prevent block forgery. Each binding requires an additional 0.1_DePINC in addition to the handling fee. This amount is staked on the chain and will be returned to the bound account when unbinding.

### Binding Command

Before mining starts, you need to bind your DePINC reward receiving address with FarmerId. Because a signature is needed, you need to write the correct configuration file first and fill in your correct "Seed" and DePINC reward address, then use the command below to bind.

```bash
> depinc-miner bind
```

- "bind" is the main command;

### Query Binding

After binding is completed, you can use `bind --check` to query the binding record.

```bash
> depinc-miner bind --check
  --> txid: b6037724221c1a21b183596f25cfcf46da90ce87
