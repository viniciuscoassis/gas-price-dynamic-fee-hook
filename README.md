# Gas Price Dynamic Fees Hook

A Uniswap V4 hook that dynamically adjusts pool fees based on the current gas price relative to a moving average. This hook incentivizes trading during high gas price periods and discourages trading during low gas price periods.

## Overview

The Gas Price Dynamic Fees Hook implements a dynamic fee mechanism for Uniswap V4 pools that adjusts the pool's fee based on the current gas price. The hook maintains a moving average of gas prices and adjusts the pool fee according to the following rules:

- If current gas price > 110% of moving average: Fee is halved (0.25%)
- If current gas price < 90% of moving average: Fee is doubled (1%)
- Otherwise: Base fee is used (0.5%)

This mechanism aims to:
1. Incentivize trading during high gas price periods by reducing fees
2. Discourage trading during low gas price periods by increasing fees
3. Maintain normal fees during stable gas price periods

## Features

- Dynamic fee adjustment based on gas prices
- Moving average calculation of gas prices
- Integration with Uniswap V4's hook system
- Automatic fee updates on each swap

## Technical Details

### Base Fee
- Default base fee: 0.5% (5000 basis points)

### Fee Adjustment Thresholds
- High gas price threshold: 110% of moving average
- Low gas price threshold: 90% of moving average

### Moving Average Calculation
The hook maintains a moving average of gas prices using the formula:
```
New Average = ((Old Average * # of txns tracked) + Current gas price) / (# of txns tracked + 1)
```

## Usage

1. Deploy the hook with a Uniswap V4 Pool Manager
2. Initialize a pool with the dynamic fee flag enabled
3. The hook will automatically adjust fees based on gas prices

### Example

```solidity
// Deploy the hook
GasPriceFeesHook hook = new GasPriceFeesHook(poolManager);

// Initialize pool with dynamic fee flag
PoolKey memory key = PoolKey({
    currency0: currency0,
    currency1: currency1,
    fee: LPFeeLibrary.DYNAMIC_FEE_FLAG,
    tickSpacing: 60,
    hooks: IHooks(address(hook))
});

// The hook will automatically adjust fees on swaps
```

## Testing

The project includes comprehensive tests that verify:
- Fee adjustments at different gas price levels
- Moving average calculations
- Integration with Uniswap V4's swap mechanism

Run tests using:
```bash
forge test
```

## Dependencies

- Uniswap V4 Core
- Uniswap V4 Periphery
- OpenZeppelin Contracts

## License

MIT License
