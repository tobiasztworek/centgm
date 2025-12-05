# 🪙 Cent GM - Multi-Chain dApp

Send "Good Morning" on multiple blockchains for a symbolic 1 cent (0.01 USD). Modern dApp with support for 12 networks, dark mode, and real price oracle integration.

**Live Demo**: _[Vercel deployment URL - coming soon]_

[![Version](https://img.shields.io/badge/version-2.0.3-blue.svg)](https://github.com/tobiasztworek/centgm)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

## ✨ Features

- 🌐 **12 different networks** - 9 mainnets + 3 testnets (more coming soon)
- 📱 **Mobile first** - WalletConnect v2, works great with MetaMask Mobile
- 🎨 **Dark mode** - light/dark theme toggle with localStorage persistence
- 💰 **Fixed $0.01 USD price** - Pyth Network for realtime conversion
- 🔗 **Explorer links** - every transaction is a clickable link
- 🔄 **Smart retry** - automatic connection retries, fallback RPC
- 🎯 **Responsive design** - Bootstrap 5 + custom CSS with animations

## 🚀 Live Demo

**Deployment**: _[Vercel URL - coming soon]_

## 🔗 Supported Networks

### Mainnets (9)
- **Arbitrum One** - chainId `0xa4b1` | Contract: `0x9369D60FF784cC31311536baC9C7cd98A66cE235` | Oracle: Pyth ETH/USD
- **Base** - chainId `0x2105` | Contract: `0xf47e08cf3513e2D3fc0625e106271004102C92c6` | Oracle: Pyth ETH/USD
- **BNB Smart Chain** - chainId `0x38` | Contract: `0xf98CbC628bF7Af83928efBE26AfC45BDc0c196E5` | Oracle: Pyth BNB/USD
- **Celo** - chainId `0xa4ec` | Contract: `0xaAC09b017F6E263f98F74699162d5921232856A7` | Oracle: Pyth CELO/USD
- **Ink** - chainId `0xdef1` | Contract: `0x480d9f49Be81d64b2A9F2c036Dd6d352753129FD` | Oracle: Pyth ETH/USD
- **Mantle** - chainId `0x1388` | Contract: `0x362DDD2e7515306b3F22f453D4bbCF3369853857` | Oracle: Pyth MNT/USD
- **Optimism** - chainId `0xa` | Contract: `0x8c20C048a4e2D3d6d9A25DF7db23b4F0CBac52da` | Oracle: Pyth ETH/USD
- **Soneium** - chainId `0x74c` | Contract: `0xD6db4F5BBd1F83f5218bf2E4A78E7c4C609Ece9A` | Oracle: Pyth ETH/USD
- **Unichain** - chainId `0x82` (130) | Contract: `0xad273a4384015be5a18345b595A9e6BBA2D9803C` | Oracle: Pyth ETH/USD

### Testnets (3)
- **Base Sepolia** - `0x14a34` | Contract: `0xCdB9e8B10525B0c58b7ECCb3D2512EfB690c5f06`
- **Ethereum Sepolia** - `0xaa36a7` | Contract: `0x43ef985e0A520A7331bf93319CE3e676c9FAEbc9`
- **Optimism Sepolia** - `0xaa37dc` | Contract: `0x0a56E2E236547575b2db6EF7e872cd49bC91A556`

## 🛠️ Technology Stack

### Frontend
- **JavaScript (ES6 modules)** - no frameworks, pure vanilla JS
- **Bootstrap 5.3.2** - UI framework with custom CSS variables for dark mode
- **esbuild** - production bundler (minify + sourcemap)

### Web3
- **ethers.js v6.15.0** - blockchain interaction
- **@reown/appkit v1.8.10** - Web3Modal with WalletConnect v2
- **@reown/appkit-adapter-ethers** - Ethers adapter for AppKit
- **@reown/appkit/networks** - network imports (viem/chains)

### Smart Contracts (Solidity ^0.8.20)
- **Pyth Network** - IPyth interface for all networks (ETH/USD, BNB/USD, CELO/USD, MNT/USD)
- **OpenZeppelin** - ERC721, Ownable, ReentrancyGuard
- **Fixed USD fee** - always 0.01 USD regardless of native token
- **Hermes price updates** - realtime prices from Pyth Network

### Project Structure

```
├── gm.html                # Main HTML file
├── gm.js                  # Main application logic
├── gm.css                 # Styling
├── dist/
│   ├── gm.bundle.js           # Production bundle
│   └── gm.bundle.js.map       # Source map
└── img/                   # Network logos
```

## 🔧 Configuration

### Debug Mode

Toggle debug mode by changing the `DEBUG_MODE` constant in `gm.js`:

```javascript
const DEBUG_MODE = false; // Set to true for development
```

When enabled, debug mode provides:
- 📝 Detailed console logging
- 🔘 Developer buttons (Dump logs, Refresh Provider, Emergency Reset)
- 💬 Info banners for all operations

### Adding New Networks

To add a new network, update the `NETWORKS` array in `gm.js`:

```javascript
{
  name: 'Network Name',
  chainId: '0xHEX_CHAIN_ID',
  contractAddress: '0xCONTRACT_ADDRESS',
  rpcUrl: 'https://rpc.url',
  explorer: 'https://explorer.url/',
  buttonColor: '#COLOR',
  logoUrl: 'img/logo.jpg',
  nativeCurrency: { 
    name: 'Token Name', 
    symbol: 'SYMBOL', 
    decimals: 18 
  },
  feeFunction: 'getGmFeeInEth', // or 'getGmFeeInCelo' for Celo
}
```

## 🎯 Key Solutions

### Dynamic Fee Calculation
- Automatic network detection and calling the correct fee function:
  - `getGmFeeInEth()` - for ETH networks (Arbitrum, Base, Ink, Optimism, Soneium, Unichain)
  - `getGmFeeInCelo()` - for Celo
  - `getGmFeeInBnb()` - for BNB Smart Chain
  - `getGmFeeInMnt()` - for Mantle
- **Pyth Network** for all networks - IPyth interface with expo normalization
- Realtime prices from Hermes API (Pyth price service)
- Retry logic with fallback RPC (up to 3 attempts, 30s timeout each)

### Dark Mode
- Toggle in header (icon 🌙/☀️)
- Persist in localStorage (`theme` key)
- Automatic system preference detection (`prefers-color-scheme`)
- CSS variables for all colors (`:root` + `[data-theme="dark"]`)
- Bootstrap overrides: `.text-muted`, `.alert-*`, all buttons

### Mobile Optimization
- WalletConnect v2 for all mobile wallets
- Nonce monitoring (race condition with receipt polling)
- 10-15s buffer info for user (MetaMask Mobile returns immediately)
- Circuit breaker for failed connections
- Network change detection with transaction counter reset

### UI/UX
- Minimalist h2 headers (uppercase, letter-spacing, animated underline on hover)
- Glassmorphism header with gradient overlay
- Status cards with hover effects (translateY, shadow)
- Automatic text color detection for light buttons (luminance > 0.5 → dark text)
- Footer matching header design (Resources + Community links, copyright year)
- Transaction hash as clickable link to explorer

## 🔐 Smart Contract Interface

All contracts use Pyth Network:

```solidity
// Pyth Network version (all networks)
contract GMFixedPyth is ERC721, Ownable, ReentrancyGuard {
    IPyth internal pyth;
    bytes32 internal priceId; // ETH/USD, BNB/USD, CELO/USD, MNT/USD
    
    // Different fee functions depending on native token
    function getGmFeeInEth() external view returns (uint256);   // Arbitrum, Base, Ink, Optimism, Soneium, Unichain
    function getGmFeeInCelo() external view returns (uint256);  // Celo
    function getGmFeeInBnb() external view returns (uint256);   // BNB Smart Chain
    function getGmFeeInMnt() external view returns (uint256);   // Mantle
    
    function sayGM() external payable;
}
```

Price IDs (Pyth Network):
- ETH/USD: `0xff61491a931112ddf1bd8147cd1b641375f79f5825126d665480874634fd0ace`
- BNB/USD: `0x2f95862b045670cd22bee3114c39763a4a08beeb663b145d283c31d7d1101c4f`
- CELO/USD: `0x7d669ddcdd23d9ef1fa9a9cc022ba055ec900e91c4cb960f3c20429d4447a411`
- MNT/USD: `0xf42c6e49a6d4ba0ac22e0b5b1e5c8d382b95ebfe66e5edcad70fd0c6e5c4dc1c`

All contracts have a dynamic fee fixed to 0.01 USD calculated by Pyth oracle.

## 🐛 Troubleshooting

### Common Issues

**MetaMask Mobile not connecting:**
- Check your internet connection
- Ensure relay.walletconnect.org is not blocked by firewall/VPN
- Try switching between WiFi and mobile data
- Clear MetaMask cache: Settings → Advanced → Clear browser data

**Transaction failing:**
- Ensure you have sufficient native tokens (ETH/CELO) for gas + fee
- Check you're on the correct network
- Try disconnecting and reconnecting your wallet

**Network not switching:**
- Manually add the network to your wallet first
- Check RPC endpoint is accessible
- Try the "Emergency Reset" button (in DEBUG_MODE)

## 📊 Version History

- **v2.0.3** (Current) - Rebrand to "Cent GM" with icon 🪙, footer with links, fixed Unichain mainnet (chain ID 130), transaction hash as clickable explorer link
- **v2.0.2** - Dark mode toggle with localStorage + system preference detection, fix text visibility, button styling, modern header redesign, cohesive h1/h2/cards
- **v2.0.1** - Added 6 mainnets (Arbitrum, Ink, Mantle, BNB, Optimism, Unichain), Pyth Network for Ink/Unichain, automatic light button text detection, fixed recursion bug in debugLog/debugWarn
- **v2.0.0** - Soneium mainnet added, DEBUG_MODE system
- **v1.9.x** - Celo mainnet support, mobile optimizations, WalletConnect fixes

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

## 👨‍💻 Author

**Tobiasz Tworek**
- GitHub: [@tobiasztworek](https://github.com/tobiasztworek)
- Repository: [centgm](https://github.com/tobiasztworek/centgm)

## 🙏 Acknowledgments

- [Pyth Network](https://pyth.network/) - realtime price feeds for all networks
- [Reown (WalletConnect)](https://reown.com/) - AppKit and WalletConnect v2
- [ethers.js](https://docs.ethers.org/) - the best Ethereum library
- [OpenZeppelin](https://openzeppelin.com/) - secure smart contract templates
- All L2s: Base, Optimism, Arbitrum, Unichain, Mantle, Ink, Soneium
- [Bootstrap](https://getbootstrap.com/) - foundation for UI

---

**For a symbolic 1 cent you can send GM on 12 different blockchains 🪙**
