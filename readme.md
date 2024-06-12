
# Introduction to Deploying Rust Contracts on Solana: A Step-by-Step Example

## Contract Deployment Step
### 1. Setting Up Your Project

#### Create a new project:

```sh
anchor init my_solana_project
cd my_solana_project
```

#### Navigate to the project directory:

```sh
cd my_solana_project
```

### 2. Writing Your Smart Contract

#### Edit the Rust smart contract:

Open `programs/my_solana_project/src/lib.rs` and write your smart contract logic in Rust. For example:

```rust
use anchor_lang::prelude::*;

declare_id!("Fg6PaFpoGXkYsidMpWxTW6WXYG8T2HNyzeHU1Du8y4ny");

#[program]
pub mod my_solana_project {
    use super::*;
    pub fn initialize(ctx: Context<Initialize>) -> Result<()> {
        Ok(())
    }
}

#[derive(Accounts)]
pub struct Initialize {}
```

### 3. Building and Deploying

#### Build the project:

```sh
anchor build
```

#### Set up a Solana cluster (choose between devnet, testnet, or mainnet-beta):

```sh
solana config set --url https://api.devnet.solana.com
```

#### Create a new Solana keypair (if you don't have one):

```sh
solana-keygen new --outfile ~/.config/solana/id.json
```

#### Airdrop some SOL for deploying the contract (devnet only):

```sh
solana airdrop 2
```

#### Deploy the smart contract:

```sh
anchor deploy
```

### 4. Interacting with Your Smart Contract

#### Write tests or client interaction code:

Create test scripts in the `tests` directory using JavaScript or TypeScript. For example:

```javascript
const anchor = require('@project-serum/anchor');
const { SystemProgram } = anchor.web3;

describe('my_solana_project', () => {
  const provider = anchor.Provider.env();
  anchor.setProvider(provider);

  it('Is initialized!', async () => {
    const program = anchor.workspace.MySolanaProject;
    const tx = await program.rpc.initialize({});
    console.log("Your transaction signature", tx);
  });
});
```

#### Run the tests:

```sh
anchor test
```

### 5. Additional Resources

- [Solana Documentation](https://docs.solana.com/)
- [Anchor Documentation](https://project-serum.github.io/anchor/)
- [Rust Documentation](https://www.rust-lang.org/)

## About This Contract

This README file provides an overview of the Solana smart contract functions for managing NFT sales and offers. The smart contract includes five main functions: `list_for_sale`, `accept_offer`, `make_offer`, `cancel_offer`, and `cancel_list_for_sale`. Each function has specific parameters, accounts, and logic to facilitate the process of listing NFTs for sale, making offers, accepting offers, and canceling listings or offers.

## Functions

### 1. list_for_sale

#### Parameters
- **NFT List Price**: The price at which the NFT is listed for sale.

#### Accounts
- **Seller Account (Signer)**: The account of the user listing the NFT for sale.
- **ProductInfo Account (PDA)**: The account storing the product information.
- **NFT Token Account (Seller’s)**: The seller's account holding the NFT.
- **NFT Mint Account**: The mint account of the NFT being listed.
- **Vault NFT Token Account (PDA)**: The PDA token account with the same mint as the NFT Mint Account.

#### Logic
- Add information to the ProductInfo Account.
- Transfer the NFT to the Vault Account.
- Set the authority of the Vault Account to PDA.

### 2. accept_offer

#### Parameters
- **Offer_Id**: The identifier of the offer being accepted.

#### Accounts
- **Seller Account (Signer)**: The account of the user accepting the offer.
- **Site Wallet Account**: The account holding the SOL.
- **Wallet Authority Account (PDA)**: The PDA authority account.
- **ProductInfo Account (PDA)**: The account storing the product information.
- **Offerer NFT Token Account**: The account of the user who made the offer.
- **NFT Mint Account**: The mint account of the NFT.
- **Vault NFT Token Account (PDA)**: The PDA token account with the same mint as the NFT Mint Account.
- **Vault Authority Account**: The PDA authority account for the Vault Token Account.

#### Logic
- Transfer SOL from the Site Wallet Account to the Seller Account.
- Transfer the NFT from the Vault NFT Token Account to the Offerer NFT Token Account.
- Close the ProductInfo Account.

### 3. make_offer

#### Parameters
- **Offer Price**: The price of the offer being made.

#### Accounts
- **Offerer Account (Signer)**: The account of the user making the offer.
- **Site Wallet Account**: The account holding the SOL.
- **NFT Mint Account**: The mint account of the NFT.
- **Offerer NFT Token Account**: The account of the user making the offer.
- **ProductInfo Account (PDA)**: The account storing the product information.

#### Logic
- Add offering information to the ProductInfo Account.
- Transfer SOL, equal to the Offer Price, to the Site Wallet Account.

### 4. cancel_offer

#### Parameters
- **Offer Id**: The identifier of the offer being canceled.

#### Accounts
- **Offerer Account (Signer)**: The account of the user canceling the offer.
- **Site Wallet Account**: The account holding the SOL.
- **Wallet Authority Account (PDA)**: The PDA authority account.
- **NFT Mint Account**: The mint account of the NFT.
- **ProductInfo Account (PDA)**: The account storing the product information.

#### Logic
- Remove offering information from the ProductInfo Account.
- Withdraw SOL from the Site Wallet Account to the Offerer Account.

### 5. cancel_list_for_sale

#### Parameters
- None

#### Accounts
- **Seller Account (Signer)**: The account of the user canceling the listing.
- **ProductInfo Account (PDA)**: The account storing the product information.
- **NFT Token Account (Seller’s)**: The seller's account holding the NFT.
- **NFT Mint Account**: The mint account of the NFT.
- **Vault Token Account (PDA)**: The PDA token account with the same mint as the NFT Mint Account.
- **Vault Authority Account**: The PDA authority account for the Vault Token Account.

#### Logic
- Close the ProductInfo Account.
- Transfer the NFT from the Vault Token Account to the Seller's NFT Token Account.

### Conclusion

This smart contract provides a comprehensive mechanism for managing the listing, selling, and offering of NFTs on the Solana blockchain. By utilizing the specified parameters and accounts, users can seamlessly list NFTs for sale, make and accept offers, and cancel listings or offers as needed.
