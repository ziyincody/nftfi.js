# NFTfi.js

A JavaScript SDK for interacting with the NFTfi Protocol.

NFTfi is a smart contract platform for P2P (Peer-2-Peer) loans using NFTs as collateral. P2P loans
are directly between a Borrower and Lender — the Borrower uses an NFT as collateral to borrow ETH
or DAI, and the Lender provides liquidity. The NFT is then held in an escrow contract until the loan is
repaid. If the Borrower fails to repay in time, the Lender can claim the NFT.

Please note that this SDK is in **closed beta**, and is constantly under development. **USE AT YOUR OWN RISK**.

## Table of Contents

- [Install](#install)
- [Getting Started](#getting-started)
- [SDK Reference](#sdk-reference)
  - [Listings](#listings)
  - [Offers](#offers)
  - [Bundles](#bundles)
  - [Immutables](#immutables)
  - [Loans](#loans)
  - [Utils](#utils)
  - [Erc20](#erc20)
  - [Erc721](#erc721)
- [Examples](#examples)
  - [SDK using an EOA](#sdk-using-an-eoa-externally-owned-account)
  - [SDK using a Multisig](#sdk-using-a-multisig-gnosis-safe)

## Install

```shell
yarn install
```

## Getting Started

To begin experimenting, please ensure that the following are available:

- a NFTfi API key (contact the team)
- an Ethereum RPC Provider URL
- a Private Key of an Ethereum wallet (funded with some ETH, wETH, and DAI (optional))

You will need the values above when initialising the SDK. We recommend that you start by using the SDK on the Goerli network, to get a feeling for the various functionality. Then once you are ready, transitioning over to Mainnet.

Please note that if the SDK is configured to use Goerli, it will use the dApp located at [https://goerli-integration.nftfi.com](https://goerli-integration.nftfi.com). If a Mainnet configuration is used, the SDK will use the dApp located at [https://app.nftfi.com](https://app.nftfi.com).

After you've set up and configured the environment, you need to initialise NFTfi.

```javascript
// Import SDK
import NFTfi from '@nftfi/js';

// Initialise
const nftfi = await NFTfi.init({
  config: { 
    api: { key: <nftfi-sdk-api-key> }
  },
  ethereum: {
    account: { privateKey: <ethereum-account-private-key> },
    provider: { url: <ethereum-provider-url> }
  }
});
```

Upon initialisation the NFTfi SDK is bound to the account that will be interacting with the NFTfi protocol. The account address is computed by using the private key specified.

We recommend that you **don't hardcode your credentials** into `NFTfi.init(...)`, instead you could use environment vars, or a more secure mechanism.

Once the SDK is initialised, you can use all the methods documented below.

## SDK Reference

<a name="Bundles"></a>

### Bundles
Class for working with bundles.

**Kind**: global class  

* [Bundles](#Bundles)
    * [`.mint()`](#Bundles+mint) ⇒ <code>Object</code>
    * [`.add(options)`](#Bundles+add) ⇒ <code>Object</code>
    * [`.remove(options)`](#Bundles+remove) ⇒ <code>Object</code>
    * [`.seal(options)`](#Bundles+seal) ⇒ <code>Object</code>
    * [`.empty(options)`](#Bundles+empty) ⇒ <code>Object</code>
    * [`.elements(options)`](#Bundles+elements) ⇒ <code>Object</code>
    * [`.migrate(options)`](#Bundles+migrate) ⇒ <code>Object</code>


* * *

<a name="Bundles+mint"></a>

#### `bundles.mint()` ⇒ <code>Object</code>
Mint a new bundle.

**Kind**: instance method of [<code>Bundles</code>](#Bundles)  
**Returns**: <code>Object</code> - An object containing information about the minted bundle.  
**Example**  
```js
// Mint a new v1.1 bundle.
// NOTE: v1 bundles have been deprecated, therefore this method wont mint a v1 bundle anymore.
const bundle = await nftfi.bundles.mint();
```

* * *

<a name="Bundles+add"></a>

#### `bundles.add(options)` ⇒ <code>Object</code>
Adds elements to a bundle.

**Kind**: instance method of [<code>Bundles</code>](#Bundles)  
**Returns**: <code>Object</code> - An object containing information about the updated bundle.  

| Param | Type | Description |
| --- | --- | --- |
| options | <code>Object</code> | An object containing options for the add operation. |
| options.bundle.id | <code>string</code> | The ID of the bundle to which elements will be added. |
| options.nftfi.contract.name | <code>string</code> | Name of the contract used for adding elements to the bundle. |
| options.elements | <code>Array.&lt;Object&gt;</code> | An array of objects representing the elements to be added. |
| options.elements[].token | <code>Object</code> | An object containing information about the token associated with the element. |
| options.elements[].token.address | <code>string</code> | The address of the token contract associated with the element. |
| options.elements[].token.ids | <code>Array.&lt;string&gt;</code> | An array of token IDs associated with the element. |

**Example**  
```js
// Add elements to a v1.1 bundle.
// NOTE: v1 bundles have been deprecated. You can migrate your v1 bundle to a v1.1 bundle using `bundles.migrate()`, then add elements afterwards.
const bundle = await nftfi.bundles.add({
  bundle: { id: '42' },
  elements: [
    { token: { address: '0xabc', ids: ['1', '2'] } },
    { token: { address: '0xdef', ids: ['3'] } }
  ],
  nftfi: {
    contract: {
      name: 'v1-1.bundler'
    }
  }
});
```

* * *

<a name="Bundles+remove"></a>

#### `bundles.remove(options)` ⇒ <code>Object</code>
Removes elements from a bundle.

**Kind**: instance method of [<code>Bundles</code>](#Bundles)  
**Returns**: <code>Object</code> - An object containing information about the updated bundle.  

| Param | Type | Description |
| --- | --- | --- |
| options | <code>Object</code> | An object containing options for the remove operation. |
| options.bundle.id | <code>string</code> | The ID of the bundle from which elements will be removed. |
| options.nftfi.contract.name | <code>string</code> | Name of the contract used for removing elements from the bundle. |
| options.elements | <code>Array.&lt;Object&gt;</code> | An array of objects representing the elements to be removed. |
| options.elements[].token | <code>Object</code> | An object containing information about the token associated with the element. |
| options.elements[].token.address | <code>string</code> | The address of the token contract associated with the element. |
| options.elements[].token.ids | <code>Array.&lt;string&gt;</code> | An array of token IDs associated with the element. |

**Example**  
```js
// Remove elements from a v1.1 bundle.
const bundle = await nftfi.bundles.remove({
  bundle: { id: '42' },
  elements: [
    {
      token: {
        address: '0xabc',
        ids: ['1', '2', '3']
      }
    }
  ],
  nftfi: {
    contract: {
      name: 'v1-1.bundler'
    }
  }
});
```

* * *

<a name="Bundles+seal"></a>

#### `bundles.seal(options)` ⇒ <code>Object</code>
Seals a bundle, transferring it to an immutable contract, and mints a new immutable.

**Kind**: instance method of [<code>Bundles</code>](#Bundles)  
**Returns**: <code>Object</code> - A promise that resolves to an object containing information about the newly minted immutable object.  

| Param | Type | Description |
| --- | --- | --- |
| options | <code>Object</code> | An object containing options for the seal operation. |
| options.bundle.id | <code>string</code> | The ID of the bundle to be sealed. |
| options.nftfi.contract.name | <code>string</code> | Name of the contract used for sealing the bundle. |

**Example**  
```js
// Seal a v1.1 bundle and mint a new v1.1 immutable.
// NOTE: v1 bundles have been deprecated. You can migrate your v1 bundle to a v1.1 bundle using `bundles.migrate()`, then seal afterwards.
const immutable = await nftfi.bundles.seal({
  bundle: { id: '42' },
  nftfi: {
    contract: {
      name: 'v1-1.bundler'
    }
  }
});
```

* * *

<a name="Bundles+empty"></a>

#### `bundles.empty(options)` ⇒ <code>Object</code>
Empties a bundle, transferring its contents to your account.

**Kind**: instance method of [<code>Bundles</code>](#Bundles)  
**Returns**: <code>Object</code> - An object containing the status of the empty operation.  

| Param | Type | Description |
| --- | --- | --- |
| options | <code>Object</code> | An object containing options for the empty operation. |
| options.bundle.id | <code>string</code> | The ID of the bundle to be emptied. |
| options.nftfi.contract.name | <code>string</code> | Name of the contract used for emptying the bundle. |

**Example**  
```js
// NOTE: v1 bundles are deprecated, after emptying one it will be destroyed and you will not be able to use it anymore.
// Approve the migration contract to handle your v1 bundle.
const approvalResult = await nftfi.erc721.setApprovalForAll({
  token: { address: nftfi.config.bundler.v1.address },
  nftfi: { contract: { name: 'v1.bundler.migrate' } }
});
// Empty the v1 bundle and transfer its contents to your account.
const response = await nftfi.bundles.empty({
  bundle: { id: '42' },
  nftfi: {
    contract: {
      name: 'v1.bundler'
    }
  }
});
```
**Example**  
```js
// Empty a v1.1 bundle and transfer its contents to your account.
const response = await nftfi.bundles.empty({
  bundle: { id: '42' },
  nftfi: {
    contract: {
      name: 'v1-1.bundler'
    }
  }
});
```

* * *

<a name="Bundles+elements"></a>

#### `bundles.elements(options)` ⇒ <code>Object</code>
Retrieves the elements in a bundle.

**Kind**: instance method of [<code>Bundles</code>](#Bundles)  
**Returns**: <code>Object</code> - An object containing information about the bundle and its elements.  

| Param | Type | Description |
| --- | --- | --- |
| options | <code>Object</code> | An object containing options for retrieving the elements. |
| options.bundle.id | <code>string</code> | The ID of the bundle whose elements are to be retrieved. |
| options.nftfi.contract | <code>Object</code> | An object containing information about the contract. |
| options.nftfi.contract.name | <code>string</code> | Name of the contract used for retrieving the elements. |

**Example**  
```js
// Get the elements of a bundle.
const elements = await nftfi.bundles.elements({
  bundle: { id: '42' },
  nftfi: {
    contract: {
      name: 'v1-1.bundler'
    }
  }
});
```

* * *

<a name="Bundles+migrate"></a>

#### `bundles.migrate(options)` ⇒ <code>Object</code>
Migrates a bundle from one bundler contract to another.

**Kind**: instance method of [<code>Bundles</code>](#Bundles)  
**Returns**: <code>Object</code> - An object containing information about the migrated bundle.  

| Param | Type | Description |
| --- | --- | --- |
| options | <code>Object</code> | An object containing options for migrating the bundle. |
| options.bundle.id | <code>string</code> | The ID of the bundle to be migrated. |
| options.from.nftfi.contract.name | <code>string</code> | Name of the source contract. |
| options.to.nftfi.contract.name | <code>string</code> | Name of the destination contract. |

**Example**  
```js
// Approve the v1 bundler contract with the v1 migration contract.
const approvalResult = await nftfi.erc721.setApprovalForAll({
  token: { address: nftfi.config.bundler.v1.address },
  nftfi: { contract: { name: 'v1.bundler.migrate' } }
});
// Migrate a bundle from v1 bundle to v1.1 bundle.
const migrateResult = await nftfi.bundles.migrate({
  bundle: { id: '42' },
  from: {
    nftfi: {
      contract: {
        name: 'v1.bundler'
      }
    }
  },
  to: {
    nftfi: {
      contract: {
        name: 'v1-1.bundler'
      }
    }
  }
});
```

* * *

<a name="Erc20"></a>

### Erc20
Class for working with ERC20 tokens.

**Kind**: global class  

* [Erc20](#Erc20)
    * [`.allowance(options)`](#Erc20+allowance) ⇒ <code>number</code>
    * [`.approve(options)`](#Erc20+approve) ⇒ <code>boolean</code>
    * [`.approveMax(options)`](#Erc20+approveMax) ⇒ <code>boolean</code>
    * [`.balanceOf(options)`](#Erc20+balanceOf) ⇒ <code>number</code>


* * *

<a name="Erc20+allowance"></a>

#### `erc20.allowance(options)` ⇒ <code>number</code>
Returns the ERC20 allowance, for v1 & v2 NFTfi contracts, for your account (by default), or a specified account.

**Kind**: instance method of [<code>Erc20</code>](#Erc20)  
**Returns**: <code>number</code> - The user account's token allowance for that contract, in base units (eg. 1000000000000000000 wei)  

| Param | Type | Description |
| --- | --- | --- |
| options | <code>object</code> | Hashmap of config options for this method |
| [options.account.address] | <code>object</code> | The account address to get the allowance of (optional) |
| options.token.address | <code>string</code> | The ERC20 token address |
| options.nftfi.contract.name | <code>string</code> | The name of the contract NFTfi contract (eg. `v1.loan.fixed`, `v2.loan.fixed`, `v2-1.loan.fixed`) |

**Example**  
```js
const balance = await nftfi.erc20.allowance({
 token: { address: '0x00000000' },
 nftfi: { contract: { name: 'v2-1.loan.fixed' } }
});
```

* * *

<a name="Erc20+approve"></a>

#### `erc20.approve(options)` ⇒ <code>boolean</code>
Approves your account's ERC20 spending amount, if not already approved, for v1 & v2 NFTfi contracts.

**Kind**: instance method of [<code>Erc20</code>](#Erc20)  
**Returns**: <code>boolean</code> - Boolean value indicating whether the operation succeeded  

| Param | Type | Description |
| --- | --- | --- |
| options | <code>object</code> | Hashmap of config options for this method |
| options.token.address | <code>string</code> | The ERC20 token address |
| options.nftfi.contract.name | <code>string</code> | The name of the contract NFTfi contract (eg. `v1.loan.fixed`, `v2.loan.fixed`, `v2-1.loan.fixed`) |
| options.amount | <code>number</code> | The token amount to approve, in base units (eg. 1000000000000000000 wei) |

**Example**  
```js
const results = await nftfi.erc20.approve({
  amount: 1000000000000000000,
  token: { address: '0x00000000' },
  nftfi: { contract: { name: 'v2-1.loan.fixed' } }
});
```

* * *

<a name="Erc20+approveMax"></a>

#### `erc20.approveMax(options)` ⇒ <code>boolean</code>
Approves your account's ERC20 maximum amount, if not already approved, for v1 & v2 NFTfi contracts.

**Kind**: instance method of [<code>Erc20</code>](#Erc20)  
**Returns**: <code>boolean</code> - Boolean value indicating whether the operation succeeded  

| Param | Type | Description |
| --- | --- | --- |
| options | <code>object</code> | Hashmap of config options for this method |
| options.token.address | <code>string</code> | The ERC20 token address |
| options.nftfi.contract.name | <code>string</code> | The name of the contract NFTfi contract (eg. `v1.loan.fixed`, `v2.loan.fixed`, `v2-1.loan.fixed`) |

**Example**  
```js
const results = await nftfi.erc20.approveMax({
  token: { address: '0x00000000' },
  nftfi: { contract: { name: 'v2-1.loan.fixed' } }
});
```

* * *

<a name="Erc20+balanceOf"></a>

#### `erc20.balanceOf(options)` ⇒ <code>number</code>
Returns the balance of a given ERC20 token for your account (by default), or a specified account.

**Kind**: instance method of [<code>Erc20</code>](#Erc20)  
**Returns**: <code>number</code> - The user account's token balance, in base units (eg. 1000000000000000000 wei)  

| Param | Type | Description |
| --- | --- | --- |
| options | <code>object</code> | Options |
| [options.account.address] | <code>object</code> | The account address to get the balance of (optional) |
| options.token.address | <code>string</code> | The ERC20 token address |

**Example**  
```js
const balance = await nftfi.erc20.balanceOf({
  token: { address: '0x00000000' }
});
```

* * *

<a name="Erc721"></a>

### Erc721
Class for working with ERC721 non-fungible tokens.

**Kind**: global class  

* [Erc721](#Erc721)
    * [`.ownerOf(options)`](#Erc721+ownerOf) ⇒ <code>string</code>
    * [`.setApprovalForAll(options)`](#Erc721+setApprovalForAll) ⇒ <code>boolean</code>
    * [`.isApprovedForAll(options)`](#Erc721+isApprovedForAll) ⇒ <code>boolean</code>


* * *

<a name="Erc721+ownerOf"></a>

#### `erc721.ownerOf(options)` ⇒ <code>string</code>
Returns the owner of the specified NFT.

**Kind**: instance method of [<code>Erc721</code>](#Erc721)  
**Returns**: <code>string</code> - The NFT's owner address  

| Param | Type | Description |
| --- | --- | --- |
| options | <code>object</code> | Options |
| options.token.address | <code>string</code> | The ERC721 token address |
| options.token.id | <code>string</code> | The ERC721 token ID |

**Example**  
```js
const address = await nftfi.erc721.ownerOf({
  token: {
   address: '0x00000000',
   id: '0'
  }
});
```

* * *

<a name="Erc721+setApprovalForAll"></a>

#### `erc721.setApprovalForAll(options)` ⇒ <code>boolean</code>
Sets or unsets the approval of a given NFTfi contract.
The NFTfi contract is allowed to transfer all tokens of the sender on their behalf.

**Kind**: instance method of [<code>Erc721</code>](#Erc721)  
**Returns**: <code>boolean</code> - Boolean value indicating whether the operation succeeded  

| Param | Type | Description |
| --- | --- | --- |
| options | <code>object</code> | Options |
| options.token.address | <code>string</code> | The ERC721 token address |
| options.nftfi.contract.name | <code>string</code> | The name of the NFTfi contract (eg. `v1.loan.fixed`, `v2.loan.fixed`, `v2-1.loan.fixed`) |

**Example**  
```js
const address = await nftfi.erc721.setApprovalForAll({
  token: {
   address: '0x00000000'
  },
  nftfi: { contract: { name: 'v2-1.loan.fixed' } }
});
```

* * *

<a name="Erc721+isApprovedForAll"></a>

#### `erc721.isApprovedForAll(options)` ⇒ <code>boolean</code>
Retruns the approval of a given NFTfi contract.
The NFTfi contract is allowed to transfer all tokens of the sender on their behalf.

**Kind**: instance method of [<code>Erc721</code>](#Erc721)  
**Returns**: <code>boolean</code> - Boolean value indicating whether permission has been granted or not  

| Param | Type | Description |
| --- | --- | --- |
| options | <code>object</code> | Options |
| options.token.address | <code>string</code> | The ERC721 token address |
| options.nftfi.contract.name | <code>string</code> | The name of the NFTfi contract (eg. `v1.loan.fixed`, `v2.loan.fixed`, `v2-1.loan.fixed`) |

**Example**  
```js
const address = await nftfi.erc721.isApprovalForAll({
  token: {
   address: '0x00000000'
  },
  nftfi: { contract: { name: 'v2-1.loan.fixed' } }
});
```

* * *

<a name="Immutables"></a>

### Immutables
Class for working with immutables.

**Kind**: global class  

* [Immutables](#Immutables)
    * [`.unseal(options)`](#Immutables+unseal) ⇒ <code>Object</code>
    * [`.getBundle(options)`](#Immutables+getBundle) ⇒ <code>Object</code>
    * [`.empty(options)`](#Immutables+empty) ⇒ <code>Object</code>
    * [`.migrate(options)`](#Immutables+migrate) ⇒ <code>Object</code>


* * *

<a name="Immutables+unseal"></a>

#### `immutables.unseal(options)` ⇒ <code>Object</code>
Unseals an immutable bundle.

**Kind**: instance method of [<code>Immutables</code>](#Immutables)  
**Returns**: <code>Object</code> - An object containing information about the bundle that was released from the immutable.  

| Param | Type | Description |
| --- | --- | --- |
| options | <code>Object</code> | An object containing options for the unseal operation. |
| options.immutable.id | <code>string</code> | The ID of the immutable bundle to unseal. |
| options.nftfi.contract | <code>Object</code> | An object containing information about the contract used to facilitate the bundle. |
| options.nftfi.contract.name | <code>string</code> | Name of the contract used to facilitate the bundle: `v1.immutable.bundle` (deprecated), `v1-1.immutable.bundle`. |

**Example**  
```js
// Unseal a v1.1 immutable bundle.
// NOTE: v1 immutables have been deprecated. You must call `nftfi.immutables.empty()` instead, or you can migrate your v1 immutable to a v1.1 immutable using `nftfi.immutables.migrate()`, then unseal it.
const bundle = await nftfi.immutables.unseal({
  immutable: { id: '42' },
  nftfi: {
    contract: {
      name: 'v1-1.immutable.bundle'
    }
  }
});
```

* * *

<a name="Immutables+getBundle"></a>

#### `immutables.getBundle(options)` ⇒ <code>Object</code>
Retrieves a bundle of an immutable.

**Kind**: instance method of [<code>Immutables</code>](#Immutables)  
**Returns**: <code>Object</code> - An object containing information about an bundle.  

| Param | Type | Description |
| --- | --- | --- |
| options | <code>Object</code> | An object containing options for the getBundle operation. |
| options.immutable.id | <code>string</code> | The ID of the immutable object. |
| options.nftfi.contract | <code>Object</code> | An object containing information about the contract used to facilitate the bundle. |
| options.nftfi.contract.name | <code>string</code> | Name of the contract used to facilitate the bundle: `v1.immutable.bundle` (deprecated), `v1-1.immutable.bundle`. |

**Example**  
```js
// Get a bundle of a v1 immutable.
const bundle = await nftfi.immutables.getBundle({
  immutable: { id: '42' },
  nftfi: {
    contract: {
      name: 'v1.immutable.bundle'
    }
  }
});
```
**Example**  
```js
// Get a bundle of a v1.1 immutable.
const bundle = await nftfi.immutables.getBundle({
  immutable: { id: '42' },
  nftfi: {
    contract: {
      name: 'v1-1.immutable.bundle'
    }
  }
});
```

* * *

<a name="Immutables+empty"></a>

#### `immutables.empty(options)` ⇒ <code>Object</code>
Empties an immutable according to the specified contract.

**Kind**: instance method of [<code>Immutables</code>](#Immutables)  
**Returns**: <code>Object</code> - An object containing the success status of the empty operation.  

| Param | Type | Description |
| --- | --- | --- |
| options | <code>Object</code> | An object containing options for the empty operation. |
| options.immutable.id | <code>string</code> | The ID of the immutable object to be emptied. |
| options.nftfi.contract.name | <code>string</code> | Name of the contract used for emptying the immutable object: `v1.immutable.bundle`, `v1-1.immutable.bundle`. |

**Example**  
```js
// NOTE: v1 immutables have been deprecated. If you empty it you will also burn the bundle after the NFTs have been transferred back into your account. You could optionally migrate your v1 immutable to a v1.1 immutable using `nftfi.immutables.migrate()`, then empty it.
// Approve the migration contract to handle your v1 immutable.
const approvalResult = await nftfi.erc721.setApprovalForAll({
  token: { address: nftfi.config.immutable.v1.address },
  nftfi: { contract: { name: 'v1.bundler.migrate' } }
});
// Empty the v1 immutable and transfer its contents to your account.
const result = await nftfi.immutables.empty({
  immutable: { id: '42' },
  nftfi: {
    contract: {
      name: 'v1.immutable.bundle'
    }
  }
});
```
**Example**  
```js
// Empty an v1.1 immutable and transfer its contents to your account.
const result = await nftfi.immutables.empty({
  immutable: { id: '42' },
  nftfi: {
    contract: {
      name: 'v1-1.immutable.bundle'
    }
  }
});
```

* * *

<a name="Immutables+migrate"></a>

#### `immutables.migrate(options)` ⇒ <code>Object</code>
Migrates an immutable from one contract to another.

**Kind**: instance method of [<code>Immutables</code>](#Immutables)  
**Returns**: <code>Object</code> - An object containing information about the migrated immutable object.  

| Param | Type | Description |
| --- | --- | --- |
| options | <code>Object</code> | An object containing options for the migration operation. |
| options.immutable.id | <code>string</code> | The ID of the immutable object to be migrated. |
| options.from.nftfi.contract.name | <code>string</code> | Name of the source immutable contract. |
| options.to.nftfi.contract.name | <code>string</code> | Name of the destination immutable contract. |

**Example**  
```js
// Approve the v1 immutable contract with the v1 migration contract.
const approvalResult = await nftfi.erc721.setApprovalForAll({
  token: {
    address: nftfi.config.immutable.v1.address
  },
  nftfi: { contract: { name: 'v1.bundler.migrate' } }
});
// Migrate an immutable from a v1 contract to a v1.1 contract.
const migrateResult = await nftfi.immutables.migrate({
  immutable: { id: '42' },
  from: {
    nftfi: {
      contract: {
        name: 'v1.immutable.bundle'
      }
    }
  },
  to: {
    nftfi: {
      contract: {
        name: 'v1-1.immutable.bundle'
      }
    }
  }
});
```

* * *

<a name="Listings"></a>

### Listings
Class for working with listings.

**Kind**: global class  

* * *

<a name="Listings+get"></a>

#### `listings.get([options])` ⇒ <code>Array.&lt;object&gt;</code>
Gets all current listings.

**Kind**: instance method of [<code>Listings</code>](#Listings)  
**Returns**: <code>Array.&lt;object&gt;</code> - Array of listings hashmaps  

| Param | Type | Description |
| --- | --- | --- |
| [options] | <code>object</code> | Hashmap of config options for this method |
| [options.filters.nftAddresses] | <code>Array.&lt;string&gt;</code> | NFT contract addresses (optional) |
| [options.pagination.page] | <code>number</code> | Pagination page (optional) |
| [options.pagination.limit] | <code>number</code> | Pagination limit (optional) |

**Example**  
```js
// get listings without specifying pagination or filters
const listings = await nftfi.listings.get();
```
**Example**  
```js
// get the first `page` of listings, filtered by `nftAddresses`
const listings = await nftfi.listings.get({
  filters: {
    nftAddresses: ['0x11111111', '0x22222222']
  },
  pagination: {
    page: 1,
    limit: 20
  }
});
```

* * *

<a name="Loans"></a>

### Loans
Class for working with loans.

**Kind**: global class  

* [Loans](#Loans)
    * [`.get(options)`](#Loans+get) ⇒ <code>Array.&lt;object&gt;</code>
    * [`.begin(options)`](#Loans+begin) ⇒ <code>object</code>
    * [`.liquidate(options)`](#Loans+liquidate) ⇒ <code>object</code>
    * [`.repay(options)`](#Loans+repay) ⇒ <code>object</code>
    * [`.revokeOffer(options)`](#Loans+revokeOffer) ⇒ <code>object</code>


* * *

<a name="Loans+get"></a>

#### `loans.get(options)` ⇒ <code>Array.&lt;object&gt;</code>
Gets loans in which your account is a participant.

**Kind**: instance method of [<code>Loans</code>](#Loans)  
**Returns**: <code>Array.&lt;object&gt;</code> - Array of listing objects  

| Param | Type | Description |
| --- | --- | --- |
| options | <code>object</code> | Hashmap of config options for this method |
| options.filters.counterparty | <code>string</code> | Loans where the counterparty is: `lender` or `borrower` |
| options.filters.status | <code>string</code> | Loan status: `escrow`, `defaulted`, `repaid` or `liquidated` |

**Example**  
```js
// Get loans in `escrow` where your account is the `lender`
const loans = await nftfi.loans.get({
  filters: {
    counterparty: 'lender',
    status: 'escrow'
  }
});
```

* * *

<a name="Loans+begin"></a>

#### `loans.begin(options)` ⇒ <code>object</code>
Begin a loan. Called by the borrower when accepting a lender's offer.

**Kind**: instance method of [<code>Loans</code>](#Loans)  
**Returns**: <code>object</code> - Response object  

| Param | Type | Description |
| --- | --- | --- |
| options | <code>object</code> | Hashmap of config options for this method |
| options.offer.nft.address | <code>string</code> | Address of the NFT being used as collateral |
| options.offer.nft.id | <code>string</code> | ID of NFT being used as collateral |
| options.offer.terms.loan.currency | <code>string</code> | Address of the ERC20 contract being used as principal/interest |
| options.offer.terms.loan.principal | <code>number</code> | Sum of money transferred from lender to borrower at the beginning of the loan |
| options.offer.terms.loan.repayment | <code>number</code> | Maximum amount of money that the borrower would be required to retrieve their collateral |
| options.offer.terms.loan.duration | <code>number</code> | Amount of time (measured in seconds) that may elapse before the lender can liquidate the loan |
| options.offer.terms.loan.expiry | <code>number</code> | Timestamp (in seconds) of when the signature expires |
| options.offer.lender.address | <code>string</code> | Address of the lender that signed the offer |
| options.offer.lender.nonce | <code>string</code> | Nonce used by the lender when they signed the offer |
| options.offer.signature | <code>string</code> | ECDSA signature of the lender |
| options.offer.nftfi.fee.bps | <code>number</code> | Percent (measured in basis points) of the interest earned that will be taken as a fee by the contract admins when the loan is repaid |
| options.offer.nftfi.contract.name | <code>string</code> | Name of contract used to facilitate the loan: `v2-1.loan.fixed`, `v2.loan.fixed.collection` |

**Example**  
```js
// Begin a loan on a lender's offer.
const result = await nftfi.loans.begin({
  offer: {
    nft: {
      id: '42',
      address: '0x00000000',
    },
    lender: {
      address: '0x00000000',
      nonce: '314159265359'
    },
    terms: {
      loan: {
        principal: 1000000000000000000,
        repayment: 1100000000000000000,
        duration: 86400 * 7, // 7 days (in seconds)
        currency: "0x00000000",
        expiry: 1690548548 // Friday, 28 July 2023 14:49:08 GMT+02:00
      }
    },
    signature: '0x000000000000000000000000000000000000000000000000000',
    nftfi: {
      fee: { bps: 500 },
      contract: { name: 'v2-1.loan.fixed' }
    }
  }
});
```

* * *

<a name="Loans+liquidate"></a>

#### `loans.liquidate(options)` ⇒ <code>object</code>
Liquidate `defaulted` loans in which your account is a participant.
Can be called once a loan has finished its duration and the borrower still has not repaid.

**Kind**: instance method of [<code>Loans</code>](#Loans)  
**Returns**: <code>object</code> - Response object  

| Param | Type | Description |
| --- | --- | --- |
| options | <code>object</code> | Hashmap of config options for this method |
| options.loan.id | <code>string</code> | The ID of the loan being liquidated |
| options.nftfi.contract.name | <code>string</code> | Name of contract used to facilitate the liquidation: `v1.loan.fixed`, `v2.loan.fixed`, `v2-1.loan.fixed` |

**Example**  
```js
// Liquidate a v1 fixed loan
const result = await nftfi.loans.liquidate({
  loan: { id: 1 },
  nftfi: {
    contract: {
      name: 'v1.loan.fixed'
    }
  }
});
```
**Example**  
```js
// Liquidate a v2 fixed loan
const result = await nftfi.loans.liquidate({
  loan: { id: 2 },
  nftfi: {
    contract: {
      name: 'v2.loan.fixed'
    }
  }
});
```
**Example**  
```js
// Liquidate a v2 fixed collection loan
const result = await nftfi.loans.liquidate({
  loan: { id: 3 },
  nftfi: {
    contract: {
      name: 'v2.loan.fixed.collection'
    }
  }
});
```
**Example**  
```js
// Liquidate a v2.1 fixed loan
const result = await nftfi.loans.liquidate({
  loan: { id: 2 },
  nftfi: {
    contract: {
      name: 'v2-1.loan.fixed'
    }
  }
});
```

* * *

<a name="Loans+repay"></a>

#### `loans.repay(options)` ⇒ <code>object</code>
Repay a loan. Can be called at any time after the loan has begun and before loan expiry.

**Kind**: instance method of [<code>Loans</code>](#Loans)  
**Returns**: <code>object</code> - Response object  

| Param | Type | Description |
| --- | --- | --- |
| options | <code>object</code> | Hashmap of config options for this method |
| options.loan.id | <code>string</code> | The ID of the loan being repaid |
| options.nftfi.contract.name | <code>string</code> | Name of contract used to facilitate the repayment: `v1.loan.fixed`, `v2.loan.fixed`, `v2-1.loan.fixed`, `v2.loan.fixed.collection` |

**Example**  
```js
// Repay a v1 fixed loan
const result = await nftfi.loans.repay({
  loan: { id: 1 },
  nftfi: {
    contract: {
      name: 'v1.loan.fixed'
    }
  }
});
```
**Example**  
```js
// Repay a v2 fixed loan
const result = await nftfi.loans.repay({
  loan: { id: 2 },
  nftfi: {
    contract: {
      name: 'v2.loan.fixed'
    }
  }
});
```
**Example**  
```js
// Repay a v2.1 fixed loan
const result = await nftfi.loans.repay({
  loan: { id: 2 },
  nftfi: {
    contract: {
      name: 'v2-1.loan.fixed'
    }
  }
});
```
**Example**  
```js
// Repay a v2 fixed collection loan
const result = await nftfi.loans.repay({
  loan: { id: 3 },
  nftfi: {
    contract: {
      name: 'v2.loan.fixed.collection'
    }
  }
});
```

* * *

<a name="Loans+revokeOffer"></a>

#### `loans.revokeOffer(options)` ⇒ <code>object</code>
Revokes an active offer made by your account.

**Kind**: instance method of [<code>Loans</code>](#Loans)  
**Returns**: <code>object</code> - Response object  

| Param | Type | Description |
| --- | --- | --- |
| options | <code>object</code> | Hashmap of config options for this method |
| options.offer.nonce | <code>object</code> | The nonce of the offer to be deleted |
| options.nftfi.contract.name | <code>string</code> | Name of contract which the offer was created for: `v1.loan.fixed`, `v2.loan.fixed`, `v2-1.loan.fixed` |

**Example**  
```js
// Revoke a v1 fixed loan offer
const revoked = await nftfi.loans.revoke({
  offer: {
    nonce: '42'
  },
  nftfi: {
    contract: {
      name: 'v1.loan.fixed'
    }
  }
});
```
**Example**  
```js
// Revoke a v2 fixed loan offer
const revoked = await nftfi.loans.revoke({
  offer: {
    nonce: '42'
  },
  nftfi: {
    contract: {
      name: 'v2.loan.fixed'
    }
  }
});
```
**Example**  
```js
// Revoke a v2.1 fixed loan offer
const revoked = await nftfi.loans.revoke({
  offer: {
    nonce: '42'
  },
  nftfi: {
    contract: {
      name: 'v2-1.loan.fixed'
    }
  }
});
```

* * *

<a name="Offers"></a>

### Offers
Class for working with offers.

**Kind**: global class  

* [Offers](#Offers)
    * [`.get([options])`](#Offers+get) ⇒ <code>Array.&lt;object&gt;</code>
    * [`.create(options)`](#Offers+create) ⇒ <code>object</code>
    * [`.delete(options)`](#Offers+delete) ⇒ <code>object</code>
    * [`.revoke(options)`](#Offers+revoke) ⇒ <code>object</code>


* * *

<a name="Offers+get"></a>

#### `offers.get([options])` ⇒ <code>Array.&lt;object&gt;</code>
When called with no argument, gets all offers made by your account.
When provided with filters, gets all offers by specified filters.

**Kind**: instance method of [<code>Offers</code>](#Offers)  
**Returns**: <code>Array.&lt;object&gt;</code> - Array of offers  

| Param | Type | Default | Description |
| --- | --- | --- | --- |
| [options] | <code>object</code> |  | Hashmap of config options for this method |
| [options.filters.nft.address] | <code>string</code> |  | NFT contract address to filter by (optional) |
| [options.filters.nft.id] | <code>string</code> |  | NFT id of the asset to filter by (optional) |
| [options.filters.lender.address.eq] | <code>string</code> |  | Lender wallet address to filter by (optional) |
| [options.filters.lender.address.ne] | <code>string</code> |  | Lender wallet address to exclude (optional) |
| [options.filters.nftfi.contract.name] | <code>string</code> |  | Contract name to filter by (optional) |
| [options.pagination.page] | <code>number</code> |  | Pagination page (optional) |
| [options.pagination.limit] | <code>number</code> |  | Pagination limit (optional) |
| [options.pagination.sort] | <code>string</code> |  | Field to sort by (optional) |
| [options.pagination.direction] | <code>&#x27;asc&#x27;</code> \| <code>&#x27;desc&#x27;</code> |  | Direction to sort by (optional) |
| [options.validation.check] | <code>boolean</code> | <code>true</code> | Validate offers and append error info (optional) |

**Example**  
```js
// Get all offers made by your account
const offers = await nftfi.offers.get();
```
**Example**  
```js
// Get the first page of offers made by your account, for a given NFT
const offers = await nftfi.offers.get({
  filters: {
    nft: {
      address: "0x00000000",
      id: "42"
    }
  },
  pagination:{
    page: 1,
    limit: 10
  }
});
```
**Example**  
```js
// Get all offers made by your account, for multiple NFTs in a collection
const offers = await nftfi.offers.get({
  filters: {
    nft: {
      address: "0x00000000"
    }
  }
});
```
**Example**  
```js
// Get the first page of collection offers made by a specific lender
const offers = await nftfi.offers.get({
  filters: {
    nft: {
      address: "0x00000000",
    },
    lender:{
      address: {
        eq: "0x12345567"
      }
    },
    nftfi: {
      contract: {
        name: "v2.loan.fixed.collection"
      }
    }
  },
  pagination:{
    page: 1,
    limit: 10
  }
});
```
**Example**  
```js
// Get all offers made by your account, and dont perform validation checks.
const offers = await nftfi.offers.get({
  validation: {
    check: false
  }
});
```

* * *

<a name="Offers+create"></a>

#### `offers.create(options)` ⇒ <code>object</code>
Creates a new offer on a NFT or collection.

**Kind**: instance method of [<code>Offers</code>](#Offers)  
**Returns**: <code>object</code> - Response object  

| Param | Type | Description |
| --- | --- | --- |
| options | <code>object</code> | Config options for this method |
| options.terms | <code>object</code> | Terms of the offer |
| options.nft | <code>object</code> | NFT to place an offer on |
| options.borrower | <code>object</code> | Owner of the NFT |
| options.nftfi | <code>object</code> | NFTfi options |

**Example**  
```js
// Create an offer on a NFT
const offer = await nftfi.offers.create({
  terms: {
    principal: 1000000000000000000,
    repayment: 1100000000000000000,
    duration: 86400 * 7, // 7 days (in seconds)
    currency: "0x00000000",
    expiry: 21600 // 6 hours (in seconds)
  },
  nft: {
    address: "0x00000000",
    id: "42"
  },
  borrower: {
    address: "0x00000000"
  },
  nftfi: {
    contract: {
      name: "v2-1.loan.fixed"
    }
  }
});
```

* * *

<a name="Offers+delete"></a>

#### `offers.delete(options)` ⇒ <code>object</code>
Deletes an active offer made by your account.

**Kind**: instance method of [<code>Offers</code>](#Offers)  
**Returns**: <code>object</code> - Response object  

| Param | Type | Description |
| --- | --- | --- |
| options | <code>object</code> | Hashmap of config options for this method |
| options.offer.id | <code>object</code> | The Id of the offer to be deleted |

**Example**  
```js
// Get first avilable offer made by your account
const offers = await nftfi.offers.get();
const offerId = offers[0]['id'];
// Delete the offer by Id
const deleted = await nftfi.offers.delete({
  offer: {
    id: offerId
  }
});
```

* * *

<a name="Offers+revoke"></a>

#### `offers.revoke(options)` ⇒ <code>object</code>
Revokes an active offer made by your account.

**Kind**: instance method of [<code>Offers</code>](#Offers)  
**Returns**: <code>object</code> - Response object  

| Param | Type | Description |
| --- | --- | --- |
| options | <code>object</code> | Hashmap of config options for this method |
| options.offer.nonce | <code>object</code> | The nonce of the offer to be deleted |
| options.nftfi.contract.name | <code>string</code> | Name of contract which the offer was created for: `v1.loan.fixed`, `v2.loan.fixed`, `v2-1.loan.fixed` |

**Example**  
```js
// Get first avilable offer made by your account
const offers = await nftfi.offers.get();
const nonce = offers[0]['lender']['nonce'];
const contractName = offers[0]['nftfi']['contract']['name']
// Revoke offer
const revoked = await nftfi.offers.revoke({
  offer: { nonce },
  nftfi: { contract: { name: contractName } }
});
```

* * *

<a name="Rewards"></a>

### Rewards
Class for working with rewards.

**Kind**: global class  

* * *

<a name="Utils"></a>

### Utils
Class with utility methods.

**Kind**: global class  

* [Utils](#Utils)
    * [`.getNonce()`](#Utils+getNonce) ⇒ <code>string</code>
    * [`.getExpiry()`](#Utils+getExpiry) ⇒ <code>number</code>
    * [`.formatEther(wei)`](#Utils+formatEther) ⇒ <code>string</code>
    * [`.formatUnits(wei, unit)`](#Utils+formatUnits) ⇒ <code>string</code>
    * [`.formatWei(value, unit)`](#Utils+formatWei) ⇒ <code>BigNumber</code>
    * [`.calcRepaymentAmount(principal, apr, duration)`](#Utils+calcRepaymentAmount) ⇒ <code>number</code>
    * [`.calcApr(principal, repayment, duration)`](#Utils+calcApr) ⇒ <code>number</code>


* * *

<a name="Utils+getNonce"></a>

#### `utils.getNonce()` ⇒ <code>string</code>
Gets random nonce.

**Kind**: instance method of [<code>Utils</code>](#Utils)  
**Returns**: <code>string</code> - Nonce  
**Example**  
```js
// Get a random nonce
const nonce = nftfi.utils.getNonce();
```

* * *

<a name="Utils+getExpiry"></a>

#### `utils.getExpiry()` ⇒ <code>number</code>
Gets an expiry timestamp.

**Kind**: instance method of [<code>Utils</code>](#Utils)  
**Returns**: <code>number</code> - Expiry  
**Example**  
```js
// Get an expiry timestamp into the future
const expiry = nftfi.utils.getExpiry();
```

* * *

<a name="Utils+formatEther"></a>

#### `utils.formatEther(wei)` ⇒ <code>string</code>
Formats an amount of wei into a decimal string representing the amount of ether.

**Kind**: instance method of [<code>Utils</code>](#Utils)  
**Returns**: <code>string</code> - Ether denomination of the amount  

| Param | Type | Description |
| --- | --- | --- |
| wei | <code>number</code> | Wei denomination of the amount |

**Example**  
```js
// Format wei into the amount of ether
const wei = 100;
const ether = nftfi.utils.formatEther(wei);
```

* * *

<a name="Utils+formatUnits"></a>

#### `utils.formatUnits(wei, unit)` ⇒ <code>string</code>
Formats an amount of wei into a decimal string representing the amount of unit.

**Kind**: instance method of [<code>Utils</code>](#Utils)  
**Returns**: <code>string</code> - String representation of value formatted with unit digits  

| Param | Type | Description |
| --- | --- | --- |
| wei | <code>BigNumber</code> | Wei denomination of the amount |
| unit | <code>string</code> | Unit denomination to format value |

**Example**  
```js
// Format usdc wei amount into the amount of unit
const wei = '1000000';
const usdc = nftfi.utils.formatUnits(wei, 'mwei'); // 1 usdc
```
**Example**  
```js
// Format wei into the amount of unit
const wei = '1000000000000000000';
const ether = nftfi.utils.formatUnits(wei, 'ether'); // 1 ether
```

* * *

<a name="Utils+formatWei"></a>

#### `utils.formatWei(value, unit)` ⇒ <code>BigNumber</code>
Formats value into a BigNumber representing the value in wei from the unit specified.

**Kind**: instance method of [<code>Utils</code>](#Utils)  
**Returns**: <code>BigNumber</code> - BigNumber representation of value parsed with unit digits  

| Param | Type | Description |
| --- | --- | --- |
| value | <code>number</code> | Value |
| unit | <code>string</code> | Unit denomination to format from |

**Example**  
```js
// Format usdc amount into the amount of wei
const value = 1;
const usdcWei = nftfi.utils.formatWei(value, 'mwei'); // 1000000
```
**Example**  
```js
// Format ether amount into the amount of wei
const value = 100;
const wei = nftfi.utils.formatWei(value, 'ether'); // 100000000000000000000
```

* * *

<a name="Utils+calcRepaymentAmount"></a>

#### `utils.calcRepaymentAmount(principal, apr, duration)` ⇒ <code>number</code>
Calculates the loan repayment amount given its other parameters.

**Kind**: instance method of [<code>Utils</code>](#Utils)  
**Returns**: <code>number</code> - The result maximum repayment amount, in base units (eg. 1250000000000000000 wei)  

| Param | Type | Description |
| --- | --- | --- |
| principal | <code>number</code> | The loan's principal amount, in base units (eg. 1000000000000000000 wei) |
| apr | <code>number</code> | The APR (yearly percentage rate) |
| duration | <code>number</code> | The duration of the loan denominated in days |

**Example**  
```js
// Calculate the loan repayment amount
const principal = 1000000000000000000;
const apr = 32;
const duration = 30;
const amount = nftfi.utils.calcRepaymentAmount(principal, apr, duration);
```

* * *

<a name="Utils+calcApr"></a>

#### `utils.calcApr(principal, repayment, duration)` ⇒ <code>number</code>
Calculates the loan APR (yearly percentage rate) given its other parameters

**Kind**: instance method of [<code>Utils</code>](#Utils)  
**Returns**: <code>number</code> - The result APR  

| Param | Type | Description |
| --- | --- | --- |
| principal | <code>number</code> | The loan's principal amount in base units (eg. 1000000000000000000 wei) |
| repayment | <code>number</code> | The maximum repayment amount to be paid by the borrower, in base units (eg. 1230000000000000000 wei) |
| duration | <code>number</code> | The duration of the loan denominated in days |

**Example**  
```js
// Calculate the APR
const principal = 1000000000000000000;
const repayment = 1500000000000000000;
const duration = 30;
const apr = nftfi.utils.calcApr(principal, repayment, duration);
```

* * *


## Examples

To experiment with common NFTfi SDK use cases, run the following scripts from the root directory:

### SDK using an EOA (Externally Owned Account)

```shell
node examples/get-listings.js
node examples/make-offer-on-listing.js
node examples/make-offer-on-nft.js
node examples/make-offer-on-collection.js
node examples/get-my-offers.js
node examples/get-offers-on-listing.js
node examples/get-offers-on-collection.js
node examples/delete-my-offers.js
node examples/revoke-and-delete-my-offers.js
node examples/begin-loan.js
node examples/get-my-active-loans.js
node examples/repay-loan.js
node examples/liquidate-my-defaulted-loans.js
node examples/bundles/basics.js
node examples/bundles/get-nfts-in-bundle-listing.js
```

### SDK using a Multisig (Gnosis Safe)

```shell
node examples/multisig/gnosis-safe/make-offer-on-listing.js
node examples/multisig/gnosis-safe/make-offer-on-nft.js
node examples/multisig/gnosis-safe/get-offers.js
node examples/multisig/gnosis-safe/delete-offers.js
node examples/multisig/gnosis-safe/get-active-loans.js
node examples/multisig/gnosis-safe/liquidate-defaulted-loans.js
```
