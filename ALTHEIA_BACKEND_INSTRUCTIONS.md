# Altheia Backend

A comprehensive real estate asset validation platform with AI validators, IPFS integration, and blockchain asset management.

## 🏗️ Architecture

### Development (Local)
```
┌─────────────────┐  ┌─────────────────┐
│    Node.js      │  │   PostgreSQL    │
│  Express API    │  │    Database     │
│   + Prisma      │  │                 │
└─────────────────┘  └─────────────────┘
        │                       │
        └───────────────────────┘
                │
    ┌─────────────────┐
    │      IPFS       │
    │   (Pinata)      │
    └─────────────────┘
```

### Production
```
┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐
│      Nginx      │  │    Node.js      │  │   PostgreSQL    │
│   (Reverse      │  │  Express API    │  │    Database     │
│    Proxy)       │  │   + Prisma      │  │                 │
└─────────────────┘  └─────────────────┘  └─────────────────┘
        │                       │                       │
        └───────────────────────┼───────────────────────┘
                                │
                    ┌─────────────────┐
                    │      IPFS       │
                    │   (Pinata)      │
                    └─────────────────┘
```

## 🚀 Quick Start (Local Development)

### Prerequisites
- Docker & Docker Compose
- Node.js 18+ (for local development)

### 1. Clone & Setup
```bash
git clone <repository-url>
cd altheia-backend
```

### 2. Environment Configuration
Create `.env` file in the root directory:

```env
# Database
DATABASE_URL=postgresql://postgres:password@localhost:5432/altheia

# Server
NODE_ENV=development
PORT=3001

# Admin Configuration  
ADMIN_WALLET_ADDRESS=0x0F6913362855d81b5236A398c9dc05566228f11a

# Development Modes
DEMO_MODE=true
SKIP_SIGNATURE_VERIFICATION=true

# IPFS/Pinata Configuration (Get from https://pinata.cloud)
PINATA_API_KEY=your_pinata_api_key
PINATA_SECRET_API_KEY=your_pinata_secret_key  
PINATA_JWT=your_pinata_jwt_token
PINATA_GATEWAY_URL=your_pinata_gateway_url
```

#### How to Generate/Obtain Required Values:

**Pinata IPFS Credentials:**
1. Sign up at [pinata.cloud](https://pinata.cloud)
2. Go to API Keys section
3. Create new API key with admin permissions
4. Copy the API Key, Secret, and JWT
5. Get your gateway URL from the Gateways section

**Admin Wallet Address:**
- Use any Ethereum wallet address that will serve as the platform admin
- This address can approve issuers and validators

**Database Password:**
- For local development, `password` is fine
- For production, generate a secure password: `openssl rand -base64 32`

### 3. Start Services
```bash
# Start all services (API + Database)
docker-compose up -d

# View logs
docker-compose logs -f

# Check status
docker-compose ps
```

### 4. Database Setup
```bash
# Run migrations
docker exec altheia-backend-api-1 npx prisma migrate dev

# Seed with sample data (optional)
docker exec altheia-backend-api-1 npm run seed
```

### 5. Verify Installation
```bash
# Check API health
curl http://localhost:3001/health

# List assets (should return empty array initially)
curl http://localhost:3001/assets
```

## 📊 Sample Data Population

### Populate with Demo Data
```bash
# Run the blockchain data population script
docker exec altheia-backend-api-1 npx ts-node scripts/populate-blockchain-data.ts
```

This creates:
- ✅ 1 Approved Issuer (American REIT Holdings LLC)
- ✅ 4 AI Validators (Property, SEC, Fraud, Market Analysis)
- ✅ 2 REIT Assets (Realty Income Corp, Prologis Inc)
- ✅ Risk Scores & Validator Assignments
- ✅ IPFS Metadata & Documents

### Clear Database (if needed)
```bash
docker exec altheia-backend-api-1 npx ts-node scripts/clear-database.ts --force
```

## 🔌 API Endpoints

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/health` | Health check | No |
| GET | `/assets` | List all assets | No |
| GET | `/assets/:id` | Get asset details | No |
| POST | `/assets` | Create asset | Yes (Approved Issuer) |
| POST | `/assets/:id/score` | Submit risk score | Yes (Approved Validator) |
| GET | `/admin/validators` | List validators | Yes (Admin) |
| POST | `/issuers/register` | Register issuer | Yes |
| POST | `/validators/register` | Register validator | Yes |

### Authentication Headers
```bash
# For admin operations
-H "X-Wallet-Address: 0x0F6913362855d81b5236A398c9dc05566228f11a"

# For issuer operations  
-H "X-Wallet-Address: 0x52d38b975E19B0C2d3F8393F58839F7e028596f7"

# For validator operations
-H "X-Wallet-Address: <validator_wallet_address>"
```

## 🛠️ Development

### Local Development (without Docker)
```bash
# Install dependencies
npm install

# Generate Prisma client
npx prisma generate

# Start PostgreSQL (via Docker)
docker run --name postgres -e POSTGRES_PASSWORD=password -e POSTGRES_DB=altheia -p 5432:5432 -d postgres:15-alpine

# Run migrations
npx prisma migrate dev

# Start development server
npm run dev
```

### Database Management
```bash
# View database in Prisma Studio
npx prisma studio

# Reset database
npx prisma migrate reset

# Generate new migration
npx prisma migrate dev --name migration_name
```

### Scripts
- `npm run dev` - Start development server with hot reload
- `npm run build` - Build for production
- `npm start` - Start production server
- `npm run seed` - Populate with sample data

## 🐳 Docker Configuration

### Development (`docker-compose.yml`)
- **API**: Hot reload enabled, development mode
- **Database**: PostgreSQL with persistent volume  
- **Ports**: API (3001), Database (5432)
- **No reverse proxy**: Direct API access

### Production (`docker-compose.prod.yml`)
- **Nginx**: Reverse proxy with SSL termination
- **API**: Optimized production build
- **Database**: Secure configuration with restricted access
- **Environment**: Production-ready settings with domain restrictions

### Environment Variables Explained

| Variable | Development | Production | Description |
|----------|-------------|------------|-------------|
| `NODE_ENV` | `development` | `production` | Application environment |
| `DATABASE_URL` | Local PostgreSQL | Secure PostgreSQL | Database connection string |
| `DEMO_MODE` | `true` | `false` | Skip authentication for demos |
| `SKIP_SIGNATURE_VERIFICATION` | `true` | `false` | Skip wallet signature checks |
| `ADMIN_WALLET_ADDRESS` | Admin wallet | Admin wallet | Authorized admin address |
| `PINATA_*` | Your credentials | Your credentials | IPFS service credentials |

#### Production-Only Variables
```bash
# Generate secure JWT secret
openssl rand -hex 32

# Generate secure database password  
openssl rand -base64 32

# Domain restrictions (automatically set)
ALLOWED_ORIGINS=https://altheia.xyz,https://www.altheia.xyz,https://app.altheia.xyz
```

## 🗃️ Database Schema

### Key Tables
- **issuer_registrations**: Real estate issuers (REIT companies)
- **validator_registrations**: AI/Human validators 
- **assets**: Tokenized real estate assets
- **asset_risk_scores**: Validator risk assessments
- **validator_assignments**: Validator-to-asset assignments
- **asset_documents**: IPFS document metadata

### Validator Types
- **AI_AGENT**: Automated AI validators
- **HUMAN**: Human expert validators

## 🚀 Production Deployment

See `deploy.sh` for automated DigitalOcean deployment with:
- SSL certificates (Let's Encrypt)
- Domain restrictions (altheia.xyz only)
- Secure database configuration
- Data migration from development

## 🔧 Troubleshooting

### Common Issues

**Database Connection Failed**
```bash
# Check if PostgreSQL container is running
docker ps | grep postgres

# Check logs
docker logs altheia-backend-db-1
```

**API Not Responding**
```bash
# Check API container logs
docker logs altheia-backend-api-1

# Restart API container
docker-compose restart api
```

**Permission Denied**
```bash
# Fix script permissions
chmod +x deploy.sh scripts/*.sh
```

### Health Checks
```bash
# Database health
docker exec altheia-backend-db-1 pg_isready -U postgres

# API health  
curl http://localhost:3001/health
```

## 📝 License

Private - Altheia Platform
