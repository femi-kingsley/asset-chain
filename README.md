# AssetChain - Tokenized Real-World Asset Platform

![Stacks](https://img.shields.io/badge/Stacks-Blockchain-5546FF?style=for-the-badge&logo=stacks&logoColor=white)
![Clarity](https://img.shields.io/badge/Clarity-Smart%20Contract-FF4136?style=for-the-badge)
![Bitcoin](https://img.shields.io/badge/Bitcoin-L2-F7931E?style=for-the-badge&logo=bitcoin&logoColor=white)

A comprehensive smart contract platform for fractionalized ownership of real-world assets on Bitcoin L2, featuring dividend distribution, governance proposals, and KYC compliance.

## 🚀 Overview

AssetChain enables the tokenization of high-value assets into semi-fungible tokens (SFTs), allowing for fractional ownership, governance voting, and dividend distribution. The platform incorporates regulatory compliance through configurable KYC requirements, price oracles for accurate valuations, and an on-chain governance system that empowers token holders to participate in key decisions about the underlying assets.

Built on Stacks for Bitcoin L2 compatibility and security.

## 📋 Table of Contents

- [Features](#-features)
- [Architecture](#-architecture)
- [Getting Started](#-getting-started)
- [Smart Contract Functions](#-smart-contract-functions)
- [Testing](#-testing)
- [Deployment](#-deployment)
- [Usage Examples](#-usage-examples)
- [Security Considerations](#-security-considerations)
- [Contributing](#-contributing)
- [License](#-license)

## ✨ Features

### Core Functionality

- **Asset Tokenization**: Convert real-world assets into fractionalized SFTs
- **Fractional Ownership**: 100,000 tokens per asset for granular ownership
- **Dividend Distribution**: Automated dividend claims based on token holdings
- **Governance System**: Token-weighted voting on asset-related proposals
- **KYC Compliance**: Configurable compliance levels for regulatory requirements
- **Price Oracles**: Real-time asset valuation through oracle integration

### Key Capabilities

- ✅ Semi-fungible token (SFT) standard compliance
- ✅ Owner-controlled asset registration
- ✅ Proportional dividend distribution
- ✅ Democratic governance proposals
- ✅ Time-bound voting mechanisms
- ✅ KYC verification levels (1-5)
- ✅ Oracle-based price feeds
- ✅ Comprehensive error handling

## 🏗️ Architecture

### Smart Contract Structure

```
AssetChain Contract
├── Constants & Configuration
│   ├── Error codes (100-117)
│   ├── Value limits (1K - 1T)
│   └── Time constraints
├── Data Maps
│   ├── assets: Asset registry
│   ├── token-balances: Ownership records
│   ├── kyc-status: Compliance tracking
│   ├── proposals: Governance proposals
│   ├── votes: Voting records
│   ├── dividend-claims: Claim tracking
│   └── price-feeds: Oracle data
└── Functions
    ├── Asset Management
    ├── Dividend Distribution
    ├── Governance System
    └── Read-Only Queries
```

### Data Models

#### Asset Registry

```clarity
{
  owner: principal,
  metadata-uri: (string-ascii 256),
  asset-value: uint,
  is-locked: bool,
  creation-height: uint,
  last-price-update: uint,
  total-dividends: uint
}
```

#### Governance Proposals

```clarity
{
  title: (string-ascii 256),
  asset-id: uint,
  start-height: uint,
  end-height: uint,
  executed: bool,
  votes-for: uint,
  votes-against: uint,
  minimum-votes: uint
}
```

## 🚀 Getting Started

### Prerequisites

- [Clarinet](https://github.com/hirosystems/clarinet) - Stacks development environment
- [Node.js](https://nodejs.org/) v18+ - For running tests
- [Git](https://git-scm.com/) - Version control

### Installation

1. **Clone the repository**

   ```bash
   git clone https://github.com/femi-kingsley/asset-chain.git
   cd asset-chain
   ```

2. **Install dependencies**

   ```bash
   npm install
   ```

3. **Initialize Clarinet**

   ```bash
   clarinet check
   ```

### Quick Start

1. **Check contract syntax**

   ```bash
   clarinet check
   ```

2. **Run tests**

   ```bash
   npm test
   ```

3. **Start development console**

   ```bash
   clarinet console
   ```

## 📚 Smart Contract Functions

### Public Functions

#### Asset Management

##### `register-asset`

Registers a new real-world asset for tokenization.

```clarity
(register-asset 
  (metadata-uri (string-ascii 256))
  (asset-value uint))
```

**Parameters:**

- `metadata-uri`: IPFS or web URL containing asset metadata
- `asset-value`: Asset value in micro-STX (1,000 - 1,000,000,000,000)

**Returns:** `(response uint uint)` - Asset ID on success

**Access:** Contract owner only

---

##### `claim-dividends`

Claims accumulated dividends for token holders.

```clarity
(claim-dividends (asset-id uint))
```

**Parameters:**

- `asset-id`: ID of the asset to claim dividends from

**Returns:** `(response bool uint)` - Success confirmation

**Access:** Any token holder

---

#### Governance Functions

##### `create-proposal`

Creates a new governance proposal for an asset.

```clarity
(create-proposal
  (asset-id uint)
  (title (string-ascii 256))
  (duration uint)
  (minimum-votes uint))
```

**Parameters:**

- `asset-id`: Target asset for the proposal
- `title`: Proposal description (max 256 characters)
- `duration`: Voting period in blocks (12-144)
- `minimum-votes`: Minimum votes required for validity

**Returns:** `(response bool uint)` - Proposal creation confirmation

**Access:** Token holders with ≥10% ownership

---

##### `vote`

Cast a vote on an active proposal.

```clarity
(vote
  (proposal-id uint)
  (vote-for bool)
  (amount uint))
```

**Parameters:**

- `proposal-id`: ID of the proposal to vote on
- `vote-for`: `true` for support, `false` for opposition
- `amount`: Number of tokens to vote with

**Returns:** `(response bool uint)` - Vote recording confirmation

**Access:** Token holders only

### Read-Only Functions

#### `get-asset-info`

Retrieves complete asset information.

```clarity
(get-asset-info (asset-id uint))
```

#### `get-balance`

Gets token balance for a specific owner and asset.

```clarity
(get-balance (owner principal) (asset-id uint))
```

#### `get-proposal`

Retrieves proposal details and voting results.

```clarity
(get-proposal (proposal-id uint))
```

#### `get-price-feed`

Gets current oracle price data for an asset.

```clarity
(get-price-feed (asset-id uint))
```

## 🧪 Testing

### Running Tests

```bash
# Run all tests
npm test

# Run tests with coverage
npm run test:report

# Watch mode for development
npm run test:watch
```

### Test Structure

```typescript
describe("AssetChain Tests", () => {
  it("should register new assets", () => {
    // Test asset registration
  });
  
  it("should handle dividend distribution", () => {
    // Test dividend claims
  });
  
  it("should manage governance proposals", () => {
    // Test proposal creation and voting
  });
});
```

### Contract Validation

```bash
# Check contract syntax and types
clarinet check

# Run contract analysis
clarinet analyze
```

## 🚀 Deployment

### Local Deployment

1. **Start local devnet**

   ```bash
   clarinet integrate
   ```

2. **Deploy contract**

   ```bash
   clarinet deploy --local
   ```

### Testnet Deployment

1. **Configure testnet settings**

   ```bash
   # Edit settings/Testnet.toml
   ```

2. **Deploy to testnet**

   ```bash
   clarinet deploy --testnet
   ```

### Mainnet Deployment

1. **Configure mainnet settings**

   ```bash
   # Edit settings/Mainnet.toml
   ```

2. **Deploy to mainnet**

   ```bash
   clarinet deploy --mainnet
   ```

## 💡 Usage Examples

### Basic Asset Registration

```clarity
;; Register a real estate property
(contract-call? .asset-chain register-asset
  "https://ipfs.io/ipfs/QmProperty123"
  u50000000000) ;; 50,000 STX value
```

### Creating a Governance Proposal

```clarity
;; Propose asset maintenance
(contract-call? .asset-chain create-proposal
  u1 ;; asset-id
  "Approve property maintenance budget"
  u72 ;; 12 hour voting period
  u10000) ;; 10% minimum participation
```

### Voting on Proposals

```clarity
;; Vote in favor with 1000 tokens
(contract-call? .asset-chain vote
  u1 ;; proposal-id
  true ;; vote in favor
  u1000) ;; voting power
```

### Claiming Dividends

```clarity
;; Claim accumulated dividends
(contract-call? .asset-chain claim-dividends u1)
```

## 🔒 Security Considerations

### Access Controls

- **Owner-only functions**: Asset registration, price updates
- **Token-holder verification**: Voting, dividend claims
- **Balance validation**: Prevents over-voting and invalid claims

### Input Validation

- Asset values: 1,000 - 1,000,000,000,000 micro-STX
- Voting duration: 12-144 blocks (1-24 hours)
- KYC levels: 1-5 compliance tiers
- URI validation: Non-empty, ≤256 characters

### Economic Security

- Minimum ownership for proposals: 10% of total tokens
- Time-locked voting periods
- Oracle price expiration checks
- Dividend calculation safeguards

### Best Practices

- Always validate user inputs
- Check authorization before state changes
- Use `unwrap!` for critical operations
- Implement proper error handling

## 📊 Error Codes Reference

| Code | Constant | Description |
|------|----------|-------------|
| 100 | `err-owner-only` | Function restricted to contract owner |
| 101 | `err-not-found` | Requested resource not found |
| 102 | `err-already-listed` | Asset already registered |
| 103 | `err-invalid-amount` | Amount outside valid range |
| 104 | `err-not-authorized` | Insufficient permissions |
| 105 | `err-kyc-required` | KYC verification required |
| 106 | `err-vote-exists` | User has already voted |
| 107 | `err-vote-ended` | Voting period has expired |
| 108 | `err-price-expired` | Oracle price data expired |
| 110 | `err-invalid-uri` | Invalid metadata URI |
| 111 | `err-invalid-value` | Asset value out of range |
| 112 | `err-invalid-duration` | Voting duration invalid |
| 113 | `err-invalid-kyc-level` | KYC level out of range |
| 114 | `err-invalid-expiry` | Expiration date invalid |
| 115 | `err-invalid-votes` | Vote count invalid |
| 116 | `err-invalid-address` | Principal address invalid |
| 117 | `err-invalid-title` | Proposal title invalid |

## 🤝 Contributing

We welcome contributions to AssetChain! Please follow these guidelines:

### Development Process

1. **Fork the repository**
2. **Create a feature branch**

   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Make your changes**
4. **Add tests for new functionality**
5. **Run the test suite**

   ```bash
   npm test
   ```

6. **Submit a pull request**

### Code Standards

- Follow Clarity best practices
- Include comprehensive tests
- Document all public functions
- Use descriptive variable names
- Add error handling for edge cases

### Pull Request Process

1. Ensure all tests pass
2. Update documentation if needed
3. Add a clear description of changes
4. Link any related issues
5. Request review from maintainers

## 📄 License

This project is licensed under the ISC License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **Stacks Foundation** - For the Stacks blockchain platform
- **Hiro Systems** - For Clarinet development tools
- **Bitcoin Community** - For the foundational security model

## 📞 Support

For questions, issues, or contributions:

- **Documentation**: [Stacks Documentation](https://docs.stacks.co/)
- **Community**: [Stacks Discord](https://discord.gg/stacks)
