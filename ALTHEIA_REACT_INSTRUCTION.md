# Altheia Protocol - Local Development Setup

## Prerequisites

- **Node.js** (v18 or higher)
- **npm** or **yarn**
- **Git**
- **MetaMask** or compatible Web3 wallet
- **Backend API** running (if testing full functionality)

## Quick Start

### 1. Clone and Install

```bash
# Clone the repository
git clone <repository-url>
cd altheia-web-react

# Install dependencies
npm install
```

### 2. Environment Setup

```bash
# Copy the demo environment file
cp .env.demo .env

# Edit the .env file with your configuration
nano .env
```

### 3. Run Development Server

```bash
# Start the development server
npm run dev

# The app will be available at:
# http://localhost:5173
```

## Environment Configuration

### Required Environment Variables

The project requires these environment variables to be set in your `.env` file:

#### **API Configuration**
```bash
VITE_API_BASE_URL=http://localhost:3001
```

#### **Wallet Configuration**
```bash
VITE_ADMIN_WALLET_ADDRESS=0x0F6913362855d81b5236A398c9dc05566228f11a
```

#### **IPFS Configuration (Optional for Demo)**
```bash
# For production, get these from https://app.pinata.cloud/keys
VITE_PINATA_API_KEY=your_pinata_api_key_here
VITE_PINATA_SECRET_KEY=your_pinata_secret_key_here
```

#### **Development Flags**
```bash
VITE_ENABLE_MOCK_DATA=true
VITE_DEBUG_MODE=true
VITE_SHOW_TEST_CONTROLS=true
```

### Demo Mode Features

The project includes several demo mode features for local development:

- **Hardcoded IPFS uploads** - Uses demo IPFS hash instead of real uploads
- **Mock blockchain interactions** - Can work without live blockchain
- **Demo asset data** - Pre-filled forms for quick testing
- **Admin access simulation** - Bypass real authentication for testing

## Backend API Setup

### Option 1: Use Mock Data (Recommended for Frontend Development)
Set `VITE_ENABLE_MOCK_DATA=true` in your `.env` file to use mock data without a backend.

### Option 2: Run Local Backend
If you have access to the backend API:

```bash
# Ensure your backend is running on port 3001
# Or update VITE_API_BASE_URL to match your backend port
```

## Wallet Setup

### MetaMask Configuration
1. Install MetaMask browser extension
2. Create or import a wallet
3. Add Avalanche Fuji Testnet (if testing with real blockchain)
4. Get test AVAX from faucet (if needed)

### Supported Networks
- **Avalanche Fuji Testnet** (recommended for testing)
- **Local development** (with mock data)

## Features Testing

### 1. Issuer Flow
1. Connect your wallet
2. Navigate to "Submit Asset"
3. Fill out the asset submission form
4. Use "Quick Fill" button for demo data
5. Submit and track progress through IPFS → Blockchain → API

### 2. Validator Flow
1. Connect your wallet
2. Navigate to validator dashboard
3. View available assets for validation
4. Submit scores and validations

### 3. Admin Flow
1. Use the admin wallet address (configured in env)
2. Access admin panel for approvals and analytics

## Available Scripts

```bash
# Development
npm run dev          # Start development server
npm run build        # Build for production
npm run preview      # Preview production build

# Code Quality
npm run lint         # Run ESLint
npm run type-check   # Run TypeScript checking (if available)
```

## Project Structure

```
src/
├── components/          # React components
│   ├── auth/           # Authentication components
│   ├── dashboard/      # Dashboard layouts
│   ├── forms/          # Form components
│   ├── issuer/         # Issuer-specific components
│   ├── validator/      # Validator-specific components
│   └── shared/         # Shared/common components
├── hooks/              # Custom React hooks
│   ├── api/           # API-related hooks
│   └── contracts/     # Blockchain contract hooks
├── pages/              # Page components
├── services/           # API and external services
├── types/              # TypeScript type definitions
├── utils/              # Utility functions
└── constants/          # App constants
```

## Demo Data and Testing

### Quick Fill Feature
Most forms include a "Quick Fill" button that populates demo data:
- Asset submission forms
- Registration forms
- Validation forms

### Mock Asset Data
The app includes realistic mock data for:
- Real estate assets (REITs)
- Bond securities
- Financial metrics
- Validation scores

### Test Wallets
Use any wallet address for testing. The admin wallet configured in `.env` gets special permissions.

## Common Issues and Solutions

### 1. Environment Variables Not Loading
- Ensure `.env` file is in the project root
- Restart the development server after changing `.env`
- Check that variable names start with `VITE_`

### 2. API Connection Issues
- Verify `VITE_API_BASE_URL` is correct
- Check if backend is running (if not using mock data)
- Enable `VITE_ENABLE_MOCK_DATA=true` for frontend-only testing

### 3. Wallet Connection Issues
- Ensure MetaMask is installed and unlocked
- Check network configuration
- Clear browser cache if needed

### 4. Build Issues
- Clear node_modules and reinstall: `rm -rf node_modules && npm install`
- Check Node.js version compatibility
- Verify all environment variables are set

## Production Deployment

### Environment Configuration
1. Create production `.env` file with real values:
   ```bash
   VITE_API_BASE_URL=https://your-production-api.com
   VITE_ENABLE_MOCK_DATA=false
   VITE_DEBUG_MODE=false
   ```

2. Configure real IPFS credentials:
   ```bash
   VITE_PINATA_API_KEY=your_real_api_key
   VITE_PINATA_SECRET_KEY=your_real_secret_key
   ```

### Build and Deploy
```bash
# Build for production
npm run build

# Deploy the dist/ folder to your hosting provider
```

## Support

For development issues:
1. Check the browser console for errors
2. Verify environment variable configuration
3. Test with mock data first
4. Check network connectivity for API calls

## Security Notes

- Never commit real API keys or secrets to version control
- Use environment variables for all sensitive configuration
- Keep your wallet private keys secure
- Use test networks for development

---

**Happy coding!** 🚀
