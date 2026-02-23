# StreamPay - Web3 Subscription Payments Platform   ( Drop To Bottom for Manual Smart Contract Testing ) 

A modern, minimal React frontend for managing trustless recurring payments on Web3.

**DEMO:** https://youtu.be/qLX9U08LpX0?si=dJzHV_hNQW5MHy3b

## Features

- **Landing Page**: Hero section with clear CTAs and wallet connection
- **Provider Onboarding**: Complete registration flow for service providers
- **Provider Dashboard**: Earnings tracking, plan management, and subscriber analytics
- **Marketplace**: Browse and subscribe to available plans
- **Wallet Management**: Escrow balance tracking and deposit/withdrawal functionality
- **Subscription Management**: View active subscriptions and payment history

## Tech Stack

- **Frontend**: React 18, TypeScript, Vite
- **Styling**: TailwindCSS with custom design system
- **UI Components**: Custom components with Headless UI
- **Charts**: Recharts for earnings visualization
- **Wallet**: Placeholder hooks ready for Wagmi/RainbowKit integration
- **Notifications**: React Hot Toast
- **Routing**: React Router DOM

## Project Structure

```
src/
├── components/          # Reusable UI components
│   ├── ui/             # Base UI components (Button, Card, Input, etc.)
│   ├── Layout.tsx      # Main layout with navigation
│   ├── PlanCard.tsx    # Plan display component
│   ├── StatsCard.tsx   # Statistics display component
│   └── EarningsChart.tsx # Earnings visualization
├── pages/              # Page components
│   ├── Landing.tsx     # Landing page
│   ├── ProviderOnboarding.tsx # Provider registration
│   ├── ProviderDashboard.tsx  # Provider dashboard
│   ├── Marketplace.tsx # Plan marketplace
│   ├── Wallet.tsx      # Wallet management
│   └── Subscriptions.tsx # Subscription management
├── hooks/              # Custom React hooks
│   ├── useContract.ts  # Contract interaction hooks
│   └── useWallet.ts    # Wallet connection hooks
├── services/           # API services
│   └── mockApi.ts      # Mock API for development
├── types/              # TypeScript type definitions
│   └── index.ts        # Core types
└── utils/              # Utility functions
```

## Integration Guide

### Smart Contract Integration

Replace the placeholder hooks in `src/hooks/useContract.ts` with real contract calls:

1. **Contract Address**: Add your deployed contract address
2. **ABI**: Import your contract ABI
3. **Provider**: Configure your Web3 provider (Wagmi recommended)

Example integration:
```typescript
// Replace placeholder in useStreamPayContract hook
const createPlan = async (price: string) => {
  const { writeContract } = useWriteContract();
  return writeContract({
    address: CONTRACT_ADDRESS,
    abi: CONTRACT_ABI,
    functionName: 'createPlan',
    args: [parseEther(price)],
  });
};
```

### Wallet Integration

Replace the mock wallet hook in `src/hooks/useWallet.ts` with Wagmi/RainbowKit:

```typescript
// Install and configure
npm install @rainbow-me/rainbowkit wagmi viem

// Use wagmi hooks
const { address, isConnected } = useAccount();
const { connect } = useConnect();
```

### API Integration

Replace `src/services/mockApi.ts` with real API calls:

1. **TheGraph**: For indexing on-chain events
2. **IPFS**: For storing plan metadata
3. **Backend API**: For user profiles and off-chain data

### Environment Variables

Create a `.env` file with:
```
VITE_CONTRACT_ADDRESS=0x...
VITE_CHAIN_ID=1
VITE_INFURA_PROJECT_ID=...
VITE_WALLET_CONNECT_PROJECT_ID=...
```

## Getting Started

1. **Install dependencies**:
   ```bash
   npm install
   ```

2. **Start development server**:
   ```bash
   npm run dev
   ```

3. **Connect your wallet** and explore the platform

## Production Deployment

1. **Build the project**:
   ```bash
   npm run build
   ```

2. **Deploy** the `dist` folder to your hosting provider

## Key Integration Points

- **Contract Hooks**: All contract interactions are centralized in `useStreamPayContract`
- **Mock API**: Replace with real API endpoints in `mockApi.ts`
- **Types**: Extend types in `src/types/index.ts` as needed
- **Toast Notifications**: Already configured for success/error feedback

## Design System

- **Colors**: Blue primary (#3B82F6), Purple secondary (#8B5CF6), Green accent (#10B981)
- **Typography**: System fonts with proper hierarchy
- **Spacing**: 8px base grid system
- **Components**: Accessible, consistent UI components
- **Responsive**: Mobile-first design with proper breakpoints

The platform is designed to be production-ready with minimal additional configuration once integrated with your smart contracts and backend services.

# Automation Script using smart contract :

### Step 1: Initialize Contract (Already Done ✅)
```bash
# Initialize the contract - only needed once
cast send 0xe65365Ea1cfb28dafD5fF6246a2E2A124A13093B "initialize()" --private-key YOUR_PRIVATE_KEY --rpc-url https://sepolia-rollup.arbitrum.io/rpc
```

### Step 2: Register as Provider (Already Done ✅)
```bash
# Register yourself as a service provider
cast send 0xe65365Ea1cfb28dafD5fF6246a2E2A124A13093B "registerProvider(string)" "MyProvider" --private-key YOUR_PRIVATE_KEY --rpc-url https://sepolia-rollup.arbitrum.io/rpc
```

### Step 3: Create Plans (Already Done ✅)
```bash
# Plan 1: 0.001 ETH every 60 seconds (for testing)
cast send 0xe65365Ea1cfb28dafD5fF6246a2E2A124A13093B "createPlan(uint256,uint256,string)" 1000000000000000 60 "test-plan-60sec" --private-key YOUR_PRIVATE_KEY --rpc-url https://sepolia-rollup.arbitrum.io/rpc

# Plan 2: 0.01 ETH every 30 days (monthly)
cast send 0xe65365Ea1cfb28dafD5fF6246a2E2A124A13093B "createPlan(uint256,uint256,string)" 10000000000000000 2592000 "monthly-plan" --private-key YOUR_PRIVATE_KEY --rpc-url https://sepolia-rollup.arbitrum.io/rpc
```

### Step 4: Check Available Plans
```bash
# See all available subscription plans
cast call 0xe65365Ea1cfb28dafD5fF6246a2E2A124A13093B "getPlans()" --rpc-url https://sepolia-rollup.arbitrum.io/rpc
```

### Step 5: Deposit Funds to Escrow
```bash
# Deposit 0.015 ETH to your escrow balance (enough for 15 payments of Plan 1)
cast send 0xe65365Ea1cfb28dafD5fF6246a2E2A124A13093B "deposit()" --value 15000000000000000 --private-key YOUR_PRIVATE_KEY --rpc-url https://sepolia-rollup.arbitrum.io/rpc
```

### Step 6: Check Your Balance
```bash
# Check how much you have in escrow
cast call 0xe65365Ea1cfb28dafD5fF6246a2E2A124A13093B "getUserBalance(address)" YOUR_WALLET_ADDRESS --rpc-url https://sepolia-rollup.arbitrum.io/rpc
```

### Step 7: Subscribe to Plan 1 (60-second interval)
```bash
# Subscribe to Plan 1 - this auto-pays the first payment
cast send 0xe65365Ea1cfb28dafD5fF6246a2E2A124A13093B "subscribe(uint256)" 1 --private-key YOUR_PRIVATE_KEY --rpc-url https://sepolia-rollup.arbitrum.io/rpc
```
### Step 8 : Automation -  Go to Gelato UI https://app.gelato.cloud/dashboard 
```bash
Connect Wallet 
Go to More -> Functions -> New -> Time Interval -> 30 seconds -> Transactions -> Contract Adress:0xe65365Ea1cfb28dafD5fF6246a2E2A124A13093B -> CustomABI json file(in our repo) -> ProcessSubscriptionPayment(uint256) -> Subscription_id = 3 (because 1 and 2 already registered in our contract ) -> enter task name and ->create -> Automation started :> 

```

### Step 9: Check Balance After Payments
```bash
# See your remaining escrow balance after payments
cast call 0xe65365Ea1cfb28dafD5fF6246a2E2A124A13093B "getUserBalance(address)" YOUR_WALLET_ADDRESS --rpc-url https://sepolia-rollup.arbitrum.io/rpc
```

### Step 10: Withdraw Remaining Funds
```bash
# Withdraw 0.005 ETH from your escrow balance
cast send 0xe65365Ea1cfb28dafD5fF6246a2E2A124A13093B "withdraw(uint256)" 5000000000000000 --private-key YOUR_PRIVATE_KEY --rpc-url https://sepolia-rollup.arbitrum.io/rpc
```

## 📊 Current Test Status

✅ **Active Subscription**: ID `2` (60-second plan)  
✅ **Test Wallet**: `0xB6041EF165C65260f03C1D114fCc36e37971958C`  
✅ **Plan 1**: 0.001 ETH every 60 seconds  
✅ **Next Payment**: Every 60 seconds after last payment  

## 🔄 Testing Recurring Payments

To test the recurring payment system:

1. **Subscribe** (Step 7) - First payment happens automatically
2. **Wait 60+ seconds**
3. **Process Payment** (Step 8) - Second payment  
4. **Wait 60+ seconds**
5. **Process Payment** (Step 8) - Third payment
6. **Repeat** until balance runs out
