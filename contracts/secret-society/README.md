# Secret Society NFT (TSSOTBS) Contract

A soulbound NFT contract deployed on Base mainnet that implements a one-per-wallet minting system with upgradeable functionality.

## Contract Details

- **Network**: Base Mainnet
- **Implementation Address**: `0xcd257e6010978d59069c2dc030151052c016b6a5`
- **Proxy Address**: `0xaD3fBBb0Ca46ee86E9C1178ce3D631e25a99Dee0`
- **Standard**: ERC721 (Upgradeable)
- **Version**: 1.0.0

## Features

### 🔒 Soulbound Tokens

- NFTs cannot be transferred between wallets (except to burn address)
- Prevents secondary market trading
- Ensures one-to-one relationship between wallet and NFT

### 🎯 One-Per-Wallet Minting

- Each wallet address can only mint one NFT
- Prevents multiple mints from the same address
- Fair distribution mechanism

### 🔧 Upgradeable Contract

- Uses UUPS (Universal Upgradeable Proxy Standard) pattern
- Allows for future contract upgrades
- Maintains state and token ownership during upgrades

### 🛡️ Security Features

- Reentrancy protection
- Pausable functionality
- Owner-only administrative functions
- Input validation and error handling

## Contract Functions

### Public Functions

#### `mint()`

- Mints one NFT to the caller
- Requirements:
  - Caller must not have already minted
  - Contract must not be paused
  - Max supply not reached
  - Caller cannot be zero address

#### `burn(uint256 tokenId)`

- Burns an NFT by transferring it to the burn address
- Only the token owner can burn their NFT

#### `hasAccountMinted(address account)`

- Checks if an address has already minted an NFT
- Returns boolean value

#### `totalSupply()`

- Returns the total number of tokens minted

#### `tokenURI(uint256 tokenId)`

- Returns the metadata URI for a token
- All tokens share the same metadata

### Owner-Only Functions

#### `setBaseURI(string memory newBaseURI)`

- Updates the base URI for token metadata
- Emits `BaseURIUpdated` event

#### `setMaxSupply(uint256 _maxSupply)`

- Updates the maximum supply limit
- Cannot be set below current total supply
- Emits `MaxSupplyUpdated` event

#### `pause()` / `unpause()`

- Pauses or unpauses the contract
- When paused, minting is disabled

#### `setMintingStatus(address account, bool status)`

- Emergency function to reset minting status
- Allows owner to modify if an address has minted

## Contract State

- **Max Supply**: 1000 (configurable by owner)
- **Token ID Start**: 1
- **Burn Address**: `0x000000000000000000000000000000000000dEaD`

## Events

- `NFTMinted(address indexed to, uint256 indexed tokenId)` - Emitted when an NFT is minted
- `NFTBurned(address indexed from, uint256 indexed tokenId)` - Emitted when an NFT is burned
- `BaseURIUpdated(string newBaseURI)` - Emitted when base URI is updated
- `MaxSupplyUpdated(uint256 newMaxSupply)` - Emitted when max supply is updated

## Technical Implementation

### Inheritance Chain

```
NFTContract
├── Initializable
├── ERC721Upgradeable
├── OwnableUpgradeable
├── PausableUpgradeable
├── ReentrancyGuardUpgradeable
└── UUPSUpgradeable
```

### Key Overrides

- `_update()` - Prevents transfers except to burn address
- `approve()` - Disabled (reverts)
- `setApprovalForAll()` - Disabled (reverts)
- `getApproved()` - Always returns zero address
- `isApprovedForAll()` - Always returns false

## Usage Examples

### Minting an NFT

```solidity
// Call the mint function (no parameters needed)
contract.mint();
```

### Checking if an address has minted

```solidity
bool hasMinted = contract.hasAccountMinted(userAddress);
```

### Getting total supply

```solidity
uint256 supply = contract.totalSupply();
```

## Security Considerations

1. **Soulbound Nature**: Tokens cannot be transferred, ensuring they remain with the original minter
2. **One-Per-Wallet**: Prevents multiple mints from the same address
3. **Upgradeable**: Contract can be upgraded while maintaining state
4. **Pausable**: Contract can be paused in case of emergencies
5. **Reentrancy Protection**: Prevents reentrancy attacks during minting

## Deployment Information

The contract is deployed using a proxy pattern:

- **Proxy Contract**: `0xaD3fBBb0Ca46ee86E9C1178ce3D631e25a99Dee0`
- **Implementation Contract**: `0xcd257e6010978d59069c2dc030151052c016b6a5`

Users interact with the proxy address, which forwards calls to the implementation contract.

## License

MIT License - See contract header for details.

## Support

For technical support or questions about this contract, please refer to the contract source code or contact the development team.
