# Drosera Time Window Trap – Hoodi Testnet

## 📌 Description
**Time Window Trap** is a custom trap that triggers a response only within a specific time range in every minute.
Example: active only between the 10th and 20th second of each minute.
Its purpose is to detect bots or users that are unaware of this time limitation.

---

## 🖥️ System Requirements
- 2 CPU Cores
- 4 GB RAM
- 100 GB NVME
- OS: **Ubuntu 24.04 LTS**

## ✅ Tested On
🟢 **Ubuntu 24.04 LTS**

---

## ⚙️ Smart Contract

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import {ITrap} from "drosera-contracts/interfaces/ITrap.sol";

interface ITimeWindowTrap {
    function isActive() external view returns (bool);
}

contract Trap is ITrap {
    // Replace with your TimeWindowTrap contract address
    address public constant RESPONSE_CONTRACT = 0xYourContractAddress;

    // Replace with your Discord username
    string constant discordName = "prex1703";

    function collect() external view returns (bytes memory) {
        bool active = ITimeWindowTrap(RESPONSE_CONTRACT).isActive();
        return abi.encode(active, discordName);
    }

    function shouldRespond(bytes[] calldata data) external pure returns (bool, bytes memory) {
        (bool active, string memory name) = abi.decode(data[0], (bool, string));
        if (!active || bytes(name).length == 0) {
            return (false, bytes(""));
        }
        return (true, abi.encode(name));
    }
}
```

---

## 📥 Installation & Setup on Server

> **Requirements:**
> - Already migrated your Drosera node from Holesky to Hoodi
> - Installed `forge` and `drosera` CLI

### 1️⃣ Stop Drosera service
```bash
sudo systemctl stop drosera
```

### 2️⃣ Clone this repository
```bash
git clone https://github.com/USERNAME/REPO-NAME.git
cd REPO-NAME
```

### 3️⃣ Edit `drosera.toml`
```bash
nano drosera.toml
```
Update the following:
```
path = "out/Trap.sol/Trap.json"
response_contract = "0xYourContractAddress"
response_function = "isActive()"
```
> Press **CTRL+X**, then **Y**, and Enter to save.

### 4️⃣ Edit `src/Trap.sol`
Paste the **smart contract code** above and replace:
- `0xYourContractAddress` → your contract address
- `prex1703` → your Discord username

---

## 🚀 Deploy & Apply Trap

```bash
droseraup
forge build
DROSERA_PRIVATE_KEY=YOURPRIVATEKEY drosera apply
drosera dryrun
sudo systemctl restart drosera
sudo systemctl status drosera
```

> Make sure the logs run normally without errors.

---

## ✅ Verify Trap on Dashboard

1. Go to [Drosera Dashboard](https://app.drosera.io)
2. Connect your wallet
3. Check your trap → it should be **Green**

---

## 🏅 Claim Sergeant Role

Prepare the following 4 links:
1. **Dashboard Link:**
```
https://app.drosera.io/trap?trapId=YOURTRAPADDRESS
```
2. **GitHub Setup:**
```
https://github.com/USERNAME/REPO-NAME
```
3. **Post on X (Twitter)** → share your setup & contract link
4. **Hoodi Etherscan:** transaction link of your contract deployment

📩 Send the 4 links to Drosera Discord in this format:
```
Claim Sergeant Roles sir
```
Then tag **@Bjorn Agnesi**

Wait 1–7 days for the role to be assigned.

---

## 📚 References
- [Drosera Docs](https://docs.drosera.io)
- [Remix IDE](https://remix.ethereum.org)
- [Hoodi Etherscan](https://hoodi.etherscan.io)
