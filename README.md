# SatoshiVault Protocol

![Stacks](https://img.shields.io/badge/Built%20on-Stacks-purple)
![Clarity](https://img.shields.io/badge/Language-Clarity-blue)
![DeFi](https://img.shields.io/badge/Category-DeFi-green)
![License](https://img.shields.io/badge/License-ISC-yellow)

## Overview

SatoshiVault is a revolutionary DeFi protocol that enables users to mint USD-pegged stablecoins (SATS tokens) by locking Bitcoin as collateral on the Stacks blockchain. The protocol creates a bridge between Bitcoin's store of value properties and DeFi utility, featuring automated liquidation mechanics and dynamic risk management for maximum capital efficiency.

## Key Features

- 🔒 **Over-collateralized Lending**: Customizable collateralization ratios (100-300%)
- 📊 **Real-time Price Oracle Integration**: Accurate BTC valuations with manipulation protection
- ⚡ **Automated Liquidation Engine**: Protects protocol solvency through threshold-based liquidations
- 🏛️ **Governance-driven Parameters**: Protocol adjustments for optimal stability
- ⛽ **Gas-efficient Vault Management**: Comprehensive error handling and validation
- 🛡️ **Advanced Security Measures**: Protection against common DeFi exploits

## System Architecture

### Core Components

```text
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Price Oracle  │    │  Vault Manager  │    │ SATS Stablecoin │
│                 │    │                 │    │                 │
│ - BTC Price Feed│◄──►│ - Vault Creation│◄──►│ - Mint/Redeem   │
│ - Validation    │    │ - Collateral    │    │ - Supply Track  │
│ - Anti-manip    │    │ - Liquidation   │    │ - SIP-010 Trait │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                        │                        │
         └────────────────────────┼────────────────────────┘
                                  │
                     ┌─────────────────┐
                     │   Governance    │
                     │                 │
                     │ - Risk Params   │
                     │ - Fee Management│
                     │ - Access Control│
                     └─────────────────┘
```

### Contract Architecture

The SatoshiVault protocol consists of a single, comprehensive smart contract with the following modules:

#### 1. **Oracle Management**

- Authorized price feed providers
- BTC price validation and storage
- Timestamp verification and anti-manipulation measures

#### 2. **Vault System**

- Individual vault creation and management
- Collateral tracking in satoshis
- Unique vault identification and ownership

#### 3. **Stablecoin Engine**

- SATS token minting against collateral
- Collateralization ratio enforcement
- Minting limits and fee collection

#### 4. **Liquidation Mechanism**

- Automated undercollateralized position detection
- Collateral seizure and debt burning
- Liquidator incentives

#### 5. **Governance Layer**

- Parameter adjustment capabilities
- Access control and authorization
- Protocol upgrade mechanisms

## Data Flow

### 1. Vault Creation Flow

```text
User → create_vault(collateral_amount) → Vault Storage → Return vault_id
```

### 2. Stablecoin Minting Flow

```text
User → mint_stablecoin() → Oracle Price Check → Collateral Validation → SATS Minting → Supply Update
```

### 3. Liquidation Flow

```text
Liquidator → liquidate_vault() → Price Check → Ratio Calculation → Collateral Seizure → Debt Burn
```

### 4. Redemption Flow

```text
User → redeem_stablecoin() → Vault Validation → Debt Reduction → Supply Decrease
```

## Technical Specifications

### Protocol Parameters

| Parameter | Default Value | Range | Description |
|-----------|---------------|-------|-------------|
| Collateralization Ratio | 150% | 100-300% | Minimum collateral required for minting |
| Liquidation Threshold | 125% | - | Trigger point for liquidation |
| Mint Fee | 0.5% | - | Fee charged on SATS minting |
| Redemption Fee | 0.5% | - | Fee charged on SATS redemption |
| Max Mint Limit | 1,000,000 SATS | - | Maximum mintable per vault |

### Error Codes

| Code | Error | Description |
|------|-------|-------------|
| u1000 | ERR-NOT-AUTHORIZED | Insufficient permissions |
| u1001 | ERR-INSUFFICIENT-BALANCE | Insufficient token balance |
| u1002 | ERR-INVALID-COLLATERAL | Invalid collateral amount |
| u1003 | ERR-UNDERCOLLATERALIZED | Below minimum collateral ratio |
| u1004 | ERR-ORACLE-PRICE-UNAVAILABLE | Oracle price not available |
| u1005 | ERR-LIQUIDATION-FAILED | Liquidation criteria not met |
| u1006 | ERR-MINT-LIMIT-EXCEEDED | Exceeds maximum mint limit |
| u1007 | ERR-INVALID-PARAMETERS | Invalid function parameters |
| u1008 | ERR-UNAUTHORIZED-VAULT-ACTION | Unauthorized vault operation |

## Getting Started

### Prerequisites

- Node.js (v16 or higher)
- Clarinet CLI
- Stacks wallet for testing

### Installation

1. **Clone the repository**

   ```bash
   git clone https://github.com/nicholas-source/satoshi-vault.git
   cd satoshi-vault
   ```

2. **Install dependencies**

   ```bash
   npm install
   ```

3. **Check contract syntax**

   ```bash
   clarinet check
   ```

4. **Run tests**

   ```bash
   npm test
   ```

### Development Commands

```bash
# Run tests with coverage
npm run test:report

# Watch mode for continuous testing
npm run test:watch

# Check contract integrity
clarinet check

# Start Clarinet console
clarinet console
```

## Usage Examples

### Creating a Vault

```clarity
;; Create a vault with 1 BTC (100,000,000 satoshis) as collateral
(contract-call? .satoshi-vault create-vault u100000000)
```

### Minting SATS Tokens

```clarity
;; Mint 50,000 SATS tokens from vault #1
(contract-call? .satoshi-vault mint-stablecoin 
  'ST1VAULT-OWNER-ADDRESS
  u1 
  u50000)
```

### Redeeming SATS Tokens

```clarity
;; Redeem 10,000 SATS tokens to reduce debt
(contract-call? .satoshi-vault redeem-stablecoin 
  'ST1VAULT-OWNER-ADDRESS
  u1 
  u10000)
```

### Liquidating an Undercollateralized Vault

```clarity
;; Liquidate vault #1 if undercollateralized
(contract-call? .satoshi-vault liquidate-vault 
  'ST1VAULT-OWNER-ADDRESS
  u1)
```

## Security Considerations

### Built-in Protections

1. **Price Manipulation Prevention**
   - Maximum BTC price ceiling ($10M)
   - Timestamp validation
   - Authorized oracle system

2. **Vault Safety Mechanisms**
   - Minimum collateralization ratios
   - Automatic liquidation triggers
   - Owner-only vault operations

3. **Economic Security**
   - Minting limits per vault
   - Fee-based sustainability model
   - Governance parameter controls

### Best Practices

- Always maintain collateralization above 150%
- Monitor BTC price movements for liquidation risk
- Use governance functions responsibly
- Regularly update oracle prices

## Testing

The protocol includes comprehensive test coverage:

```bash
# Run all tests
npm test

# Run with detailed reporting
npm run test:report

# Continuous testing during development
npm run test:watch
```

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Guidelines

- Follow Clarity best practices
- Include comprehensive tests for new features
- Update documentation for API changes
- Ensure all tests pass before submitting

## Roadmap

- [ ] **Phase 1**: Core protocol deployment
- [ ] **Phase 2**: Advanced liquidation strategies
- [ ] **Phase 3**: Multi-collateral support
- [ ] **Phase 4**: Governance token integration
- [ ] **Phase 5**: Cross-chain bridge implementation

## License

This project is licensed under the ISC License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Built on the Stacks blockchain
- Powered by Clarity smart contracts
- Inspired by Bitcoin's Layer 2 DeFi potential
