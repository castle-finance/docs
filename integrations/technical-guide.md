---
description: >-
  Integrate the Castle ecosystem of products into your app to supercharge your
  UX
---

# Technical Guide

## Client-Side Integration

Client-side integrations are powered by our [SDK](https://github.com/castle-finance/castle-vault/tree/dev/sdk), which lives alongside the program code in the Vault repository. The primary actions a user takes is `deposit` and `withdraw`.&#x20;

The SDK exposes `deposit()` and `withdraw()`, both of abstract away intermediate instructions like creating ATAs, (un)wrapping sol, refreshing or reconciling the vault.&#x20;

If you run into transaction size limits (for example: sending transactions as part of governance proposals that increases the bytes to over 1232), then you can split up certain instructions into their own transactions. Each section will provide code for both **single SDK call** and **multi-SDK call** cases.

Here's the full list of instructions the vault contract expects:

1. `refresh`: Refreshes the vaults accounts
2. `reconcile`: Withdraws from the underlying lending markets to allow a user to withdraw
3. `deposit`: Sends reserve token (e.g. USDC) assets into the vault in exchange for LP tokens
4. `withdraw`: Sends LP tokens into the vault in exchange for reserve token assets

### Vault Client

The `VaultClient` contains static and member functions for loading information about a specific vault, along with all the necessary instructions for depositing and withdrawing.

```typescript
import { VaultClient, VaultConfig } from '@castlefinance/vault-sdk'

// Pull down the appropriate vault from the API.
const configResponse = await fetch('https://api.castle.finance/configs')
const vaults = (await response.json()) as VaultConfig[]
const vault = vaults.find(
  (v) => v.deploymentEnv == 'mainnet' && v.token_label == 'USDC'
)


// Create the vault client
const vaultClient = await VaultClient.load(
  new anchor.Provider(...),
  vault.vault_id,
  vault.deploymentEnv
)
```

Here is an example deployed vault along with the Anchor annotations: [https://explorer.solana.com/address/9n6ekjHHgkPB9fVuWHzH6iNuxBxN22hEBryZXYFg6cNk/anchor-account?cluster=devnet](https://explorer.solana.com/address/9n6ekjHHgkPB9fVuWHzH6iNuxBxN22hEBryZXYFg6cNk/anchor-account?cluster=devnet)

### Deposit

Here are the following instructions needed to make a deposit:

1. Create the user's LP token account if it does not exist
2. Refresh the Vault
3. Deposit into the Vault

_Note_: The refresh and deposit instructions need to be sent in the same transaction.

#### Deposit (single SDK call)

```typescript
// Get the users reserve token ATA
const userReserveToken = await splToken.Token.getAssociatedTokenAddress(
  ASSOCIATED_TOKEN_PROGRAM_ID,
  TOKEN_PROGRAM_ID,
  vaultClient.getVaultState().reserveTokenMint,
  reserveTokenOwner, // e.g. wallet.pubkey or DAO's account
  true
);

// Deposit into the vault
const sig = await vaultClient.deposit(wallet, amount, userReserveToken)
```

#### Deposit (multi-SDK call)

Create LP Token Account Transaction:

```typescript
let createLpAcctIx: TransactionInstruction | undefined = undefined

// Owner of the SPL Token account, e.g. the user or DAO. 
// In this example we assume a wallet logged-in user.
const reserveTokenOwner = wallet.publicKey

const userLpTokenAccount = await Token.getAssociatedTokenAddress(
  ASSOCIATED_TOKEN_PROGRAM_ID,
  TOKEN_PROGRAM_ID,
  vaultClient.getVaultState().lpTokenMint,
  reserveTokenOwner,
  true
)

const userLpTokenAccountInfo =
  await vaultClient.program.provider.connection.getAccountInfo(
    userLpTokenAccount
  )

if (userLpTokenAccountInfo == null) {
  createLpAcctIx = Token.createAssociatedTokenAccountInstruction(
    ASSOCIATED_TOKEN_PROGRAM_ID,
    TOKEN_PROGRAM_ID,
    vaultClient.getVaultState().lpTokenMint,
    userLpTokenAccount,
    reserveTokenOwner,
    wallet.publicKey // payer
  )
}

// Construct, sign, and send
const tx = new Transaction.add(createLpAcctIx)
const signedTx = await wallet.signTransaction(tx)
const sig = connection.send(signedTx)
```

Refresh and Deposit into the Vault:

```typescript
// Get the refresh instruction
const refreshIx = vaultClient.getRefreshIx()

// Get the user reserve ATA
const userReserveToken = await Token.getAssociatedTokenAddress(
  ASSOCIATED_TOKEN_PROGRAM_ID,
  TOKEN_PROGRAM_ID,
  vaultClient.getVaultState().reserveTokenMint,
  reserveTokenOwner,
  true
)

// Get the deposit instruction
const depositIx = vaultClient.program.instruction.deposit(
  new anchor.BN(amount),
  {
    accounts: {
      vault: vaultClient.vaultId,
      vaultAuthority: vaultClient.getVaultState().vaultAuthority,
      vaultReserveToken: vaultClient.getVaultState().vaultReserveToken,
      lpTokenMint: vaultClient.getVaultState().lpTokenMint,
      userReserveToken: userReserveToken,
      userLpToken: userLpTokenAccount,
      userAuthority: reserveTokenOwner,
      tokenProgram: TOKEN_PROGRAM_ID,
      clock: SYSVAR_CLOCK_PUBKEY,
    },
  }
)

// Construct, sign, and send
let tx = new Transaction();
tx.instructions = [refreshIx, depositIx];
const signedTx = await wallet.signTransaction(tx)
const sig = connection.send(signedTx)
```

### Withdraw

Withdrawing requires the following transactions to be sent:

1. Transaction #1
   1. Refresh the vault
   2. Reconcile the markets
2. Transaction #2
   1. Refresh the vault
   2. Withdraw from the vault

#### Withdraw (single SDK call)

```typescript
const sig = await vaultClient.withdraw(wallet, amount)
```

#### Withdraw (multi-SDK call)

Create the reconcile transactions:

```typescript
// Get reconcile transactions (note: each tx has a prepended refresh ix)
const txs = await vaultClient.getReconcileTxs(amount)

// Send all of them
txs.forEach(tx => {
  ...
})
```

Note: There may be up to 3 different reconcile transactions. Simply send off all of them.

Create the ATAs as needed and withdraw from the vault:

```typescript
// Create the user reserve ATA if it does not exist already
let createReserveAcctIx: TransactionInstruction | undefined = undefined
const userReserveTokenAccount = await Token.getAssociatedTokenAddress(
  ASSOCIATED_TOKEN_PROGRAM_ID,
  TOKEN_PROGRAM_ID,
  vaultClient.getVaultState().reserveTokenMint,
  lpTokenAccountOwner,
  true
)
const userReserveTokenAccountInfo =
  await vaultClient.program.provider.connection.getAccountInfo(
    userReserveTokenAccount
  )
if (userReserveTokenAccountInfo == null) {
  createReserveAcctIx = Token.createAssociatedTokenAccountInstruction(
    ASSOCIATED_TOKEN_PROGRAM_ID,
    TOKEN_PROGRAM_ID,
    vaultClient.getVaultState().reserveTokenMint,
    userReserveTokenAccount,
    lpTokenAccountOwner,
    wallet.publicKey
  )
}

// Get the refresh instruction
const refreshIx = vaultClient.getRefreshIx()

// Get the LP ATA
const userLpTokenAccount = await Token.getAssociatedTokenAddress(
  ASSOCIATED_TOKEN_PROGRAM_ID,
  TOKEN_PROGRAM_ID,
  vaultClient.getVaultState().lpTokenMint,
  lpTokenAccountOwner,
  true
)

// Get withdraw instruction. User selects the LP token to deposit back
// into the vault in exchange for the reserve token
const withdrawIx = vaultClient.program.instruction.withdraw(
  new anchor.BN(amount),
  {
    accounts: {
      vault: vaultClient.vaultId,
      vaultAuthority: vaultClient.getVaultState().vaultAuthority,
      userAuthority: lpTokenAccountOwner,
      userLpToken: userLpTokenAccount,
      userReserveToken: userReserveTokenAccount,
      vaultReserveToken: vaultClient.getVaultState().vaultReserveToken,
      lpTokenMint: vaultClient.getVaultState().lpTokenMint,
      tokenProgram: TOKEN_PROGRAM_ID,
      clock: SYSVAR_CLOCK_PUBKEY,
    },
  }
)

// Construct, sign, and send
let tx = new Transaction();
tx.instructions = [refreshIx, withdraw];
const signedTx = await wallet.signTransaction(tx)
const sig = connection.send(signedTx)
```

## Final Notes

These docs are still a work-in-progress. Please reach out to our Twitter or Discord if you have any questions or something is not working.
