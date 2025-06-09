# BitPredict Protocol

## Overview

BitPredict is a sophisticated decentralized prediction market built on the Stacks Layer 2 blockchain, enabling trustless Bitcoin price forecasting with automated settlements, proportional rewards, and community-driven oracle validation. The protocol transforms Bitcoin price speculation into a transparent, fair, and profitable DeFi experience while maintaining the security and decentralization principles of the Bitcoin network.

## Key Features

- **Trustless Oracle Integration**: Accurate price settlement through authorized oracle feeds
- **Proportional Reward Distribution**: Fair compensation based on stake and market participation
- **Dynamic Market Creation**: Customizable time windows and market parameters
- **Anti-Manipulation Safeguards**: Comprehensive protection with minimum stake requirements
- **Real-time Analytics**: User performance tracking and comprehensive statistics
- **Administrative Controls**: Platform optimization and governance capabilities
- **Bitcoin Layer 2 Integration**: Full Stacks compatibility with Bitcoin security inheritance

## System Architecture

### Core Components

```
┌─────────────────────────────────────────────────────────────┐
│                    BitPredict Protocol                       │
├─────────────────────────────────────────────────────────────┤
│  Market Management    │  User Participation │  Oracle System │
│  ┌─────────────────┐  │  ┌───────────────┐  │  ┌───────────┐ │
│  │ Create Markets  │  │  │ Make Prediction│  │  │ Price Feed│ │
│  │ Resolve Markets │  │  │ Claim Winnings │  │  │ Resolution│ │
│  │ Track State     │  │  │ View Stats     │  │  │ Validation│ │
│  └─────────────────┘  │  └───────────────┘  │  └───────────┘ │
├─────────────────────────────────────────────────────────────┤
│              Administrative & Governance Layer               │
│  ┌─────────────────────────────────────────────────────────┐ │
│  │ Fee Management │ Oracle Updates │ Platform Configuration│ │
│  └─────────────────────────────────────────────────────────┘ │
├─────────────────────────────────────────────────────────────┤
│                     Data Storage Layer                       │
│  ┌─────────┐  ┌──────────────┐  ┌──────────────────────────┐ │
│  │ Markets │  │ Predictions  │  │ User Statistics          │ │
│  └─────────┘  └──────────────┘  └──────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
```

### Contract Architecture

The BitPredict protocol is structured around three primary data structures:

#### 1. Market Management

- **Markets Map**: Core market data including price points, stake totals, and timing
- **Market Lifecycle**: Creation → Active → Resolution → Settlement
- **State Validation**: Comprehensive checks for market integrity

#### 2. User Participation System

- **Prediction Tracking**: Individual user predictions with stake amounts
- **Reward Calculation**: Proportional distribution based on pool ratios
- **Claim Mechanism**: Secure payout system with anti-double-spend protection

#### 3. Statistics & Analytics

- **User Performance**: Win rates, total stakes, lifetime earnings
- **Platform Metrics**: Volume tracking, market counts, fee collection
- **Real-time Data**: Active market status and potential payouts

## Data Flow

### Market Creation Flow

```
Owner/Admin → Create Market → Validate Parameters → Initialize Market Data → Update Counter
```

### Prediction Flow

```
User → Make Prediction → Validate Market Status → Transfer STX → Record Prediction → Update Statistics
```

### Resolution Flow

```
Oracle → Submit Price → Validate Authority → Resolve Market → Enable Claims
```

### Claim Flow

```
User → Claim Winnings → Validate Win Condition → Calculate Payout → Transfer Rewards → Update Stats
```

## Technical Specifications

### Constants & Limits

- **Maximum Platform Fee**: 10%
- **Minimum Market Duration**: 10 blocks
- **Maximum Stake Limit**: 100 STX
- **Minimum Stake**: 1 STX (configurable)

### Error Handling

Comprehensive error code system covering:

- Authorization failures (ERR-OWNER-ONLY)
- Resource management (ERR-NOT-FOUND)
- Market validation (ERR-MARKET-CLOSED)
- Financial operations (ERR-INSUFFICIENT-BALANCE)

### Security Features

- **Oracle Authorization**: Only authorized oracles can resolve markets
- **Stake Validation**: Minimum and maximum stake enforcement
- **Double-Spend Prevention**: Claim status tracking
- **Parameter Validation**: Comprehensive input sanitization

## Public Functions

### Market Management

- `create-market(start-price, start-block, end-block)`: Initialize new prediction market
- `resolve-market(market-id, final-price)`: Oracle-driven market resolution

### User Operations

- `make-prediction(market-id, direction, stake-amount)`: Submit price prediction with STX stake
- `claim-winnings(market-id)`: Claim proportional rewards from won predictions

### Administrative Functions

- `update-oracle-address(new-oracle)`: Change authorized oracle
- `update-minimum-stake(amount)`: Modify minimum stake requirements
- `update-platform-fee(rate)`: Adjust platform fee structure
- `withdraw-platform-fees(amount)`: Withdraw accumulated fees

## Read-Only Functions

### Data Queries

- `get-market-details(market-id)`: Complete market information
- `get-user-prediction-details(market-id, user)`: Individual prediction data
- `calculate-potential-winnings(market-id, user)`: Estimated payout calculation
- `get-platform-stats()`: Platform metrics and configuration
- `get-user-performance(user)`: User statistics and performance
- `get-market-status(market-id)`: Current market state and eligibility

## Economic Model

### Fee Structure

- **Platform Fee**: 2.5% (250 basis points) - configurable up to 10%
- **Fee Distribution**: Collected from winnings and transferred to contract owner
- **Proportional Rewards**: Winners receive share based on stake proportion in winning pool

### Reward Calculation

```clarity
gross-winnings = (user-stake × total-pool) ÷ winning-pool
platform-fee = (gross-winnings × fee-rate) ÷ 10000
net-payout = gross-winnings - platform-fee
```

## Deployment & Configuration

### Prerequisites

- Stacks blockchain testnet/mainnet access
- STX tokens for contract deployment
- Oracle service for price feeds

### Initial Configuration

1. Deploy contract with owner credentials
2. Configure oracle address
3. Set minimum stake and platform fee
4. Initialize first prediction market

### Oracle Integration

The protocol requires integration with a reliable Bitcoin price oracle service. The oracle address can be updated by the contract owner for flexibility and reliability improvements.

## Security Considerations

### Access Control

- **Owner-only functions**: Market creation, oracle updates, fee management
- **Oracle authorization**: Only designated oracle can resolve markets
- **User validation**: Balance checks and stake limits

### Anti-Manipulation

- **Minimum stake requirements**: Prevents spam predictions
- **Maximum stake limits**: Prevents market manipulation
- **Time-based windows**: Clear market boundaries

### Fund Security

- **Contract escrow**: STX tokens held securely in contract
- **Atomic operations**: All transfers use try/unwrap patterns
- **Claim protection**: Double-spend prevention mechanisms

## Usage Examples

### Creating a Market (Owner Only)

```clarity
(contract-call? .bitpredict create-market u60000000 u1000 u1100)
;; Creates market with $60k start price, active from block 1000-1100
```

### Making a Prediction

```clarity
(contract-call? .bitpredict make-prediction u1 "up" u5000000)
;; Predicts price will go up in market 1 with 5 STX stake
```

### Claiming Winnings

```clarity
(contract-call? .bitpredict claim-winnings u1)
;; Claims rewards from market 1 if prediction was correct
```

## Development Roadmap

### Phase 1: Core Protocol ✅

- Basic market creation and resolution
- User prediction system
- Proportional reward distribution

### Phase 2: Enhanced Features

- Multiple prediction types (price ranges, volatility)
- Automated market makers
- Cross-chain oracle integration

### Phase 3: Advanced Analytics

- Machine learning insights
- Social prediction features
- Governance token integration

## Contributing

BitPredict is designed as a foundational DeFi protocol for the Stacks ecosystem. Contributions and improvements are welcome through:

- Security audits and vulnerability reports
- Oracle service integrations
- Frontend interface development
- Documentation improvements
