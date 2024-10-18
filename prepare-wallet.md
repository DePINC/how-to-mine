# Prepare DePINC wallet

DePINC wallet is a program that synchronizes blockchain data from a peer-to-peer network. It allows you to manage your private keys and mnemonic words, as well as send coins to other users. This versatile tool serves as your gateway to the DePINC ecosystem. For miners, the DePINC wallet is essential as it allows you to submit newly created blocks to the P2P network, facilitating the mining process and ensuring your contributions are properly recorded on the blockchain.

In this guide, you'll learn how to:

1. Download and install the DePINC wallet

2. Configure the wallet for optimal performance

3. Synchronize your wallet with the blockchain

4. Set up the wallet for mining (if applicable)

5. Secure your wallet and manage your funds

## Official website

Download the latest version from the official website. The current release is v3.0.0.9:

[Release v3.0.0.9](https://github.com/DePINC/depinc/releases/tag/v3.0.0.9)

The official release supports both Windows and Linux platforms. Download the appropriate installation package for your operating system from the table below:

|Platform|Download Link|Installation Method|
|-|-|-|
|Windows|[Download Windows Version](https://github.com/DePINC/depinc/releases/download/v3.0.0.9/depinc-v3.0.0.9-b5d623b-win64.zip)|Double-click to install|
|Linux|[Download Linux Version](https://github.com/DePINC/depinc/releases/download/v3.0.0.9/depinc-v3.0.0.9-b5d623b-x86_64-linux-gnu.tar.gz)|Extract manually|

**Note: The wallet and related mining programs are now open-source. For more detailed information and the latest updates, visit the official [DePINC GitHub repository](https://github.com/depinc). There you can access the source code, contribute to development, report issues, and stay informed about new releases and features.**

## Download and installation

**For Windows users:**

1. Visit the official DePINC website (https://depinc.org) to download the latest DePINC node wallet installer.

2. Double-click the installer to begin installation.

3. After installation, launch the wallet program to start syncing blockchain data.

4. For testnet:
   - Click the green application icon to run the testnet GUI wallet.
   - Do not run the yellow icon program, which is for the mainnet.

5. For mainnet:
   - Click the yellow application icon to run the mainnet GUI wallet.

This ensures you're using the correct network version during setup and testing.

You can also run the console version of the node wallet program. Execute `depincd -testnet -server` in the command line to start the console node wallet. The parameter `-testnet` indicates starting the node and connecting to the test network.

After installation, you need to add the `daemon` folder of DePINC to the system's path search list. The specific steps are as follows:

1. Copy the subdirectory "daemon" of DePINC's installation directory in File Explorer, usually `C:\Program Files\DePINC\daemon`;
2. Open in sequence: Windows Start Menu - Settings - System - About - Advanced System Settings - Environment Variables;
3. In the opened window, there are two lists. Find the "path" item in the first list at the top, double-click it to open the path editing window;
4. Click the "New" button, paste the copied directory into the list, then click OK to save;
5. Reopen your command line window, now if you enter `depincd --help`, you should see the command being correctly called;

By default, if you use the GUI version of the wallet, it cannot directly communicate with the mining program. If you need to use the GUI version of the wallet for mining, you need to modify its configuration:

1. Open the wallet, Note: If using the test network, please double-click the green icon;
2. Open in sequence: Settings - Options - Configuration File, and click "OK" in the pop-up prompt box;
3. In the opened text editor, enter "server=1", then save;
4. Close the wallet and restart, now the mining program will be able to connect correctly;

**For Linux users:**

Download the Linux version of the program package to local storage device, then use `tar xf [program package path]` to unpack all the files in the package. Then copy these files to the `/usr/local/bin` directory or you concat the path of the binaries to env `PATH`. Note: If you are not a root user, you need to use the `sudo` command to perform the copy work. As shown below.

Once completed, you can verify the installation by running `depincd --help` in the terminal. This command will execute the wallet program and display the help documentation, confirming that the installation was successful and the program is accessible from any directory.

## Syncing the blockchain

Once the wallet program is downloaded and prepared, you can initiate the blockchain synchronization process. The wallet will automatically connect to available nodes in the P2P network.

For Windows users:
1. Start the wallet program by clicking the appropriate icon (green for testnet, yellow for mainnet).
2. Alternatively, for command-line enthusiasts, use the same instructions as Linux users.

For Linux users:
Run the following command in the terminal:
```
depincd -txindex -server
```

This command starts the wallet program with transaction indexing enabled and in server mode, optimizing it for full node functionality and mining operations.

Regardless of the platform, the wallet will begin syncing with the blockchain upon startup. This process may take some time depending on your internet connection and the current size of the blockchain.

## Specify data dir

By default, the wallet program stores synchronized blockchain data in your home directory at `$HOME/.depinc`. However, you can optimize storage by specifying a custom path for the blockchain data. This is particularly useful if you want to store the data on a separate drive with more space or faster read/write speeds. To set a custom data directory, use the `-datadir` parameter when launching the wallet program. For example:

```
depincd -datadir=/path/to/custom/directory
```

Ensure the specified directory has sufficient storage capacity and appropriate read/write permissions for the wallet program. Currently, ensure the target device has at least 10 GB of free disk space to accommodate the blockchain and allow for future growth. It's recommended to allocate more space if possible, as the blockchain size tends to increase over time. Regularly monitor your available disk space and consider upgrading storage capacity if needed for optimal performance.

## Other Important Parameters

- `-testnet` parameter, start the test network node program. If this parameter is not included, it means starting the official network node program;
- `-server` parameter, start the RPC server, used to receive connections and commands from other clients, including mining clients;
- `-rpcport` parameter, used to specify the port number on which RPC runs. The default port number is 18732 (testnet3), 8732 (mainnet). If you have a requirement, you can specify this number here;
- `-rpcuser` parameter, used to specify the name to log in to the RPC service;
- `-rpcpassword` parameter, used to specify the password to log in to the RPC service;
- `-rpcbind` parameter, used to specify the address and port number that the RPC service listens to. The default value is "127.0.0.1:18732" (testnet3). If you need to remotely connect to this RPC service, you need to use this parameter to set the binding address to "0.0.0.0:18732". If not needed, you don't need to bring in this parameter;
- `-rpcallowip` parameter, used to specify the allowed address range. For example, using "172.16.0.0/24" means that all host addresses that satisfy "172.16.0.*" are allowed to connect to this RPC service. Note that this parameter can be used repeatedly to specify different address ranges;

## About '.cookie'

DePINC extends Bitcoin's RPC service code, maintaining similar parameter settings. For single-computer mining setups, use default parameters and simply add "server=1" to depinc.conf. Upon startup, the DePINC node wallet generates a username/password pair, storing it in a `.cookie` file for local programs (e.g., depinc-cli, depinc-miner) to use for authentication.

When using a non-default data directory:
1. For depinc-miner: Use `--cookie` to specify the `.cookie` file path.
2. For depinc-cli: Use `--rpccookiefile` to specify the `.cookie` file path.

Failure to specify these paths when using custom data directories will result in authentication failures, as clients won't have the necessary credentials to connect to the RPC service.

## Custom Network Configuration

To optimize your network settings for complex scenarios, follow these steps:

1. Bind the listening address:
   Use `-rpcbind` to specify the address and port for remote connections.
   Example: `-rpcbind=0.0.0.0:18732`

2. Set allowed IP ranges:
   Use `-rpcallowip` to define permitted IP addresses or ranges.
   Example: `-rpcallowip=192.168.1.0/24`

3. Specify login credentials:
   Use `-rpcuser` and `-rpcpassword` to set authentication details.
   Example: `-rpcuser=myusername -rpcpassword=mypassword`

Important notes:
- Always use `-rpcallowip` when enabling remote connections to prevent unauthorized access.
- Set a strong username and password to secure your RPC service.
- Combine these parameters when starting the node wallet for optimized remote access.

Example command:
```
depincd -rpcbind=0.0.0.0:18732 -rpcallowip=192.168.1.0/24 -rpcuser=myusername -rpcpassword=mypassword
```

This configuration allows for secure, efficient remote connections to your DePINC node wallet.


## Wallet Password

If your wallet has a password set, you need to unlock it before using the miner tool. Otherwise, the miner tool will encounter an error when trying to use the wallet to sign transaction records. You can use the "depinc-cli" command to unlock the wallet. Here's an example:

```bash
depinc-cli walletpassphrase "your_wallet_password" 100000000
```

Replace "your_wallet_password" with your actual wallet password. The number after it (100000000 in this example) represents how many seconds the wallet will remain unlocked. Setting a larger value will keep your wallet in an available state for a longer period of time.

Note: Be cautious when using this command, especially in unsecured environments. Always ensure you're using a secure connection when entering your wallet password.
