# 🚀 Altheia Protocol - Eliza agent setup guide

## 📋 Quick Setup

1. **Install Requirements**: Node.js 18+, Foundry, Git
2. **Clone & Install**: `git clone` repo, `npm install` in root and `eliza-agents/`
3. **Setup Environment**: Create `.env` files with your private key and API keys
4. **Deploy Contracts**: Run foundry deployment script
5. **Start System**: `cd eliza-agents && node elizaProduction.js`
6. **Test System**: `cd demo-scripts && node complete-asset-flow-demo.js`

---

## 🌐 Environment Setup

### Clone Repository
```bash
git clone https://github.com/your-org/altheia-contracts
cd altheia-contracts
npm install
cd eliza-agents && npm install && cd ..
cd demo-scripts && npm install && cd ..
```

### Environment Variables
Create `eliza-agents/.env`:
```env
# Blockchain
PRIVATE_KEY=your_private_key_here
RPC_URL=https://api.avax-test.network/ext/bc/C/rpc

# Contract Addresses (update after deployment)
ALTHEIA_TOKEN_ADDRESS=0x803af06f89394659aec110200e7afb8e716229cd
ALTHEIA_ISSUER_ADDRESS=0x4e53f7c5ff0cfe719c49e4bf531c60e3a4c2cf15
ALTHEIA_VALIDATOR_ADDRESS=0x8f3af42b92561b4395bed3c93088b99aa5210bd4
ALTHEIA_REGISTRY_ADDRESS=0x8dd27e6ba6cb60c46773fc83843d900a6066ce14

# Optional: External Data APIs
SEC_EDGAR_API_KEY=your_sec_api_key
FRED_API_KEY=your_fred_api_key
ALPHA_VANTAGE_API_KEY=your_alpha_vantage_key
```

---

## 📜 Deploy Contracts

```bash
# Deploy to Fuji testnet
forge script script/DeployAltheiaFujiProduction.s.sol --rpc-url $RPC_URL --private-key $PRIVATE_KEY --broadcast
```

---

## 🤖 Run Eliza AI Agents

```bash
cd eliza-agents
node elizaProduction.js
```

**That's it!** The script:
- Starts all 5 AI agents (SEC, Market, Property, Fraud, Consensus)
- Monitors blockchain for new assets
- Automatically validates and scores assets
- Provides health monitoring at http://localhost:3001/health

---

## 🎮 Test the System

```bash
cd demo-scripts
node complete-asset-flow-demo.js
```

This shows all assets, scores, and system status.

---

## 🐛 Troubleshooting

- **Connection issues**: Check your `.env` file has correct RPC_URL and PRIVATE_KEY
- **Contract errors**: Verify contract addresses in `.env` match deployed contracts
- **No assets found**: Deploy contracts first, then submit test assets
- **Logs**: Check console output from `elizaProduction.js` for errors

---

**Success**: When working, you'll see assets being validated by AI agents with risk/yield scores!
