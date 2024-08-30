# Setup-XION-Daemon

## Step-by-Step Guide for Setting Up Xion Validators

### 1. Installation of Xion Daemon (xiond)

Option 1: Manual Installation

Clone the Xion repository locally:

```
git clone https://github.com/burnt-labs/xion.git
cd xion
```
Install the xiond binary:

```
make install
```
This will install the xiond command into your $GOPATH/bin.

Ensure $GOPATH/bin is on your $PATH:

Add the following lines to your .bashrc or .zshrc file:

```
export GOPATH=$(go env GOPATH)
export PATH=$GOPATH/bin:$PATH
```
Then, reload your shell configuration:

```
source ~/.bashrc
# or
source ~/.zshrc
```
Option 2: Install via Homebrew

Tap the Xion Homebrew repository:

```
brew tap burnt-labs/xion
```
Install the xiond binary:

To install the latest version:

```
brew install xiond
```
To install a specific version:

```
brew install xiond@0.3.8
```
### 2. Confirm Installation
   
Run the following command in your terminal to verify the installation:

```
xiond
```
You should see an output similar to:

```
xion daemon (server)
```
Usage:
  xiond [command]

Available Commands:
  ...
### 3. Initialize the Validator Node
Initialize the configuration files and genesis file:

```
xiond init <moniker-name> --chain-id <chain-id>
Replace <moniker-name> with your validator's name and <chain-id> with the appropriate chain ID for your network (mainnet or testnet).
```
Create a new key for your validator:

```
xiond keys add <key-name>
```
Make sure to store the mnemonic phrase securely, as it is the only way to recover your key.

Add the validator key to the configuration file:

```
xiond add-genesis-account $(xiond keys show <key-name> -a) <amount><denom>
```
Replace <amount> and <denom> with the appropriate values, such as 1000000uxion.

Generate a transaction to create the validator:

```
xiond gentx <key-name> <amount><denom> --chain-id <chain-id>
```
This command creates a signed transaction that registers your node as a validator.

Collect all genesis transactions and start the node:

```
xiond collect-gentxs
xiond start
```
### 4. Validator Operations

Check Validator Status

To check the status of your validator node:

```
xiond status
```
Query List of Validators

To see the list of all validators in the network:

```
xiond query staking validators
```
Delegate Tokens to a Validator

To delegate tokens to a validator:

```
xiond tx staking delegate <validator-address> <amount><denom> --from <your-wallet-name>
```
Replace <validator-address> with the address of the validator you want to delegate to and <amount><denom> with the amount and token denomination (e.g., 1000uxion).

Unbond (Undelegate) Tokens from a Validator

To unbond tokens:

```
xiond tx staking unbond <validator-address> <amount><denom> --from <your-wallet-name>
```
Withdraw Rewards

To withdraw rewards from staking:

```
xiond tx distribution withdraw-rewards <validator-address> --from <your-wallet-name> --commission
```
Edit Validator Info

To edit your validator's information (description, commission rate, etc.):

```
xiond tx staking edit-validator --from <your-wallet-name> --moniker "<new-moniker>" --website "<new-website>" --details "<new-details>"
```
### 5. Monitoring and Security
   
Monitoring Validator Performance

Query Validator Info:

```
xiond query staking validator <validator-address>
```
Check Voting Power and Status:

Regularly monitor your voting power and ensure that your validator is in the active set.

Ensure Security of Your Validator Node

Set up a secure firewall and open only necessary ports.

Keep your keys secure and use a hardware security module (HSM) or a secure environment.

Use sentry nodes to protect your validator node from DDoS attacks.
