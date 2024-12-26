![Ethereum](https://img.shields.io/badge/Ethereum-3C3C3D?style=for-the-badge&logo=Ethereum&logoColor=white) ![NodeJS](https://img.shields.io/badge/node.js-6DA55F?style=for-the-badge&logo=node.js&logoColor=white) ![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black) ![macOS](https://img.shields.io/badge/mac%20os-000000?style=for-the-badge&logo=macos&logoColor=F0F0F0) ![Windows 11](https://img.shields.io/badge/Windows%2011-%230079d5.svg?style=for-the-badge&logo=Windows%2011&logoColor=white)



# User Guide for Script Configuration
![Version](https://img.shields.io/github/package-json/v/0xFillin/Eclipse-Relay-Bridge)

This guide explains how to configure the script for proper operation, focusing on configuration files and parameter settings.

---

## 1. **Configuration File (`config.yaml`)**

The configuration file is located in the `settings` folder alongside the script. It contains the core settings required for the script to function.

### Example `config.yaml` Structure:
```yaml
relayModule:
  rpc:
    ARBITRUM: "https://arbitrum.rpc.url"
    BASE: "https://base.rpc.url"
    BLAST: "https://blast.rpc.url"
    ETH: "https://eth.rpc.url"
    LINEA: "https://linea.rpc.url"
    OPTIMISM: "https://optimism.rpc.url"
    SCROLL: "https://scroll.rpc.url"
    ZKSYNC: "https://zksync.rpc.url"
    ZORA: "https://zora.rpc.url"
  gasLimit: 21000               # Gas limit for transactions. If `0`, the script estimates the gas.
  minAmount: false              # Minimum transaction amount (ETH). If `false`, value is taken from the CSV.
  maxAmount: false              # Maximum transaction amount (ETH). If `false`, value is taken from the CSV.
  minDelay: false               # Minimum delay between tasks (in seconds). If `false`, value is taken from the CSV.
  maxDelay: false               # Maximum delay between tasks (in seconds). If `false`, value is taken from the CSV.
  originChain: null             # Source chain (e.g., `ETH`). If `null`, value is taken from the CSV.
```

### Parameter Descriptions:

- **`rpc`**: Specifies RPC URLs for supported chains. Replace placeholder URLs with actual RPC endpoints.
- **`gasLimit`**: Sets the transaction gas limit. If set to `0`, the script will estimate the gas dynamically.
- **`minAmount`** and **`maxAmount`**: Define the range of transaction amounts in ETH. If set to `false`, values are derived from the `wallet.csv` file.
- **`minDelay`** and **`maxDelay`**: Specify the minimum and maximum delays between tasks in seconds. If `false`, values are taken from the CSV file.
- **`originChain`**: Defines the default source chain for transactions. If left `null`, the value must be provided in the CSV.

---

## 2. **CSV File (`wallet.csv`)**

The CSV file contains wallet data and specific parameters for each task. The script reads this file to process transactions.

### Example `wallet.csv` Structure:

| address        | privkey            | eclipsedeposit | minAmount | maxAmount | minDelay | maxDelay | originChain | proxy                         |
|----------------|--------------------|----------------|-----------|-----------|----------|----------|-------------|-------------------------------|
| 0x123...abc    | your_private_key_here | recipient_addr | 0.1       | 0.5       | 10       | 30       | ETH         | proxy_user:pass@host:port |

### Field Descriptions:

- **`address`**: The sender's wallet address.
- **`privkey`**: The private key for the wallet (keep this secure).
- **`eclipsedeposit`**: The recipient address for the transaction.
- **`minAmount`** and **`maxAmount`**: Transaction amount range in ETH for this specific wallet.
- **`minDelay`** and **`maxDelay`**: Delay range (in seconds) between tasks for this wallet.
- **`originChain`**: The source chain for the wallet (e.g., `ETH`, `ARBITRUM`).
- **`proxy`**: Proxy settings in the format `user:password@host:port`. Leave empty for no proxy.

---

## 3. **Logging**

The script logs all activity to a file in the `log` directory. Each run creates a new log file with a timestamp.

### Example Log Message:

```yaml
2024-12-26T12:00:00.000Z | Task #1 | Processing wallet: 0x123...abc | Chain: ETH
```

## 4. **Common Configuration Steps**

### Step 1: Update `config.yaml`

Ensure all RPC endpoints are valid and parameters are set correctly for your use case. Example RPC URL:

```yaml
relayModule:
  rpc:
    ETH: "https://mainnet.infura.io/v3/YOUR_INFURA_PROJECT_ID"
```
### Step 2: Prepare `wallet.csv`

Populate the file with wallet addresses, private keys, and task-specific parameters. Ensure each wallet is properly formatted and includes all necessary columns for operation.

---

### Step 3: Install Dependencies

Ensure all required Node.js modules are installed. Run the following command to install dependencies:

```bash
npm install fs yaml csv-parser path ethers colorette node-fetch https-proxy-agent
```
### Step 4: Run the Script

Execute the script using the following command:

```bash
node script.js
```
## 5. **Error Handling**

If an error occurs, check the log file in the `log` directory for detailed information. Common issues include:

- **Missing or invalid `rpc` URL**: Verify that all required RPC URLs are correctly configured in the `config.yaml` file.
- **Proxy format errors**: Ensure the proxy string adheres to the correct format (e.g., `user:password@host:port`).
- **Insufficient funds**: Check the wallet balance to ensure there are enough funds for gas fees and transaction values.
## 6. **FAQ**

### Q: How do I set a default chain for all wallets?

Set the `originChain` parameter in `config.yaml`. If a wallet specifies its own chain in `wallet.csv`, that will take precedence over the default.

---

### Q: Can I use proxies for requests?

Yes, proxies can be used. Simply include proxy details in the `proxy` column of the `wallet.csv` file. If no proxy is specified, requests will be sent without one.

---

### Q: What happens if parameters are missing?

- If a parameter, such as `minAmount`, is not included in `wallet.csv`, the script will use the value specified in `config.yaml`.
- If neither `wallet.csv` nor `config.yaml` provides a value, the task for the wallet will fail.
