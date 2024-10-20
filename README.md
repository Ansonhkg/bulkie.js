# Bulkie SDK API Documentation

The Bulkie SDK provides a simplified interface for interacting with the Lit Protocol. It covers basic development flows for various use cases, such as those found in developer guides and hackathon code.

## API Doc
https://bulkiejs.vercel.app/

## Table of Contents

1. [Constructor](#constructor)
2. [Connection Methods](#connection-methods)
3. [Minting Methods](#minting-methods)
4. [Token Creation Methods](#token-creation-methods)
5. [Permission Granting Methods](#permission-granting-methods)
6. [Getter Methods](#getter-methods)
7. [Action Methods](#action-methods)
8. [Utility Methods](#utility-methods)
9. [API Overview](#api-overview)

## API Overview

| Method Name                          | Description                                           | Parameters                                                                                      | Return Type         |
|--------------------------------------|-------------------------------------------------------|-------------------------------------------------------------------------------------------------|----------------------|
| `new Bulkie(params)`                 | Creates a new instance of the Bulkie SDK.            | `network`, `debug?`, `litDebug?`, `guides?`, `signer?`, `rpc?`                               | `Bulkie`             |
| `connectToLitNodeClient(params?)`   | Connects to the Lit Node Client.                     | `params.outputId?`                                                                              | `Promise<this>`      |
| `connectToLitContracts(params?)`     | Connects to the Lit Contracts.                        | `params.outputId?`                                                                              | `Promise<this>`      |
| `mintPKP(params?)`                   | Mints a Programmable Key Pair (PKP).                 | `selfFund?`, `amountInEth?`, `params.outputId?`                                              | `Promise<this>`      |
| `mintCreditsNFT(params)`             | Mints a Credits NFT.                                 | `requestsPerKilosecond`, `daysUntilUTCMidnightExpiration`, `params.outputId?`                | `Promise<this>`      |
| `createCreditsDelegationToken(params)`| Creates a Credits Delegation Token.                  | `expiry?`, `creditsTokenId`, `delegatees?`, `params.outputId?`                               | `Promise<this>`      |
| `createAccessToken(params)`          | Creates an Access Token.                             | `expiration?`, `pkpPublicKey`, `type`, `resources`, `creditsDelegationToken?`, `params.outputId?` | `Promise<this>`      |
| `grantAuthMethodToUsePKP(params)`   | Grants an auth method permission to use a PKP.      | `pkpTokenId`, `authMethodId`, `authMethodType`, `scopes`, `params.outputId?`                | `Promise<this>`      |
| `grantIPFSCIDtoUsePKP(params)`      | Grants an IPFS CID permission to use a PKP.         | `pkpTokenId`, `ipfsCid`, `scopes`, `params.outputId?`                                       | `Promise<this>`      |
| `getPkps()`                          | Retrieves all PKPs associated with the signer's address.| None                                                                                           | `Promise<this>`      |
| `use(accessToken)`                   | Provides methods to use an access token for various actions.| `accessToken`                                                                                  | Object with methods   |
| `getOutput(fnName, outputId?)`      | Retrieves the output of a specific function.         | `fnName`, `outputId?`                                                                          | `FunctionReturnTypes[T] | undefined` |
| `getAllOutputs()`                    | Retrieves all outputs.                               | None                                                                                           | `Map<BulkieSupportedFunctions, any>` |
| `getTotalExecutionTime()`            | Retrieves the total execution time of all operations. | None                                                                                           | `{ ms: number, s: number }` |

## Constructor

```typescript
new Bulkie(params: {
  network: LIT_NETWORKS_KEYS,
  debug?: boolean,
  litDebug?: boolean,
  guides?: boolean,
  signer?: Signer,
  rpc?: string,
})
```

Creates a new instance of the Bulkie SDK.

- `network`: (Required) The Lit network to connect to.
- `debug`: (Optional) Enable debug logging.
- `litDebug`: (Optional) Enable Lit Protocol debug logging.
- `guides`: (Optional) Enable guide messages.
- `signer`: (Optional) Ethereum signer.
- `rpc`: (Optional) Custom RPC URL.

## Connection Methods

### connectToLitNodeClient

```typescript
async connectToLitNodeClient(params?: OutputHandler): Promise<this>
```

Connects to the Lit Node Client.

- `params.outputId`: (Optional) Custom output identifier.

### connectToLitContracts

```typescript
async connectToLitContracts(params?: OutputHandler): Promise<this>
```

Connects to the Lit Contracts.

- `params.outputId`: (Optional) Custom output identifier.

## Minting Methods

### mintPKP

```typescript
async mintPKP(params?: {
  selfFund?: boolean,
  amountInEth?: string
} & OutputHandler): Promise<this>
```

Mints a Programmable Key Pair (PKP).

- `params.selfFund`: (Optional) Whether to self-fund the PKP.
- `params.amountInEth`: (Optional) Amount of ETH to fund the PKP with.
- `params.outputId`: (Optional) Custom output identifier.

### mintCreditsNFT

```typescript
async mintCreditsNFT(params: {
  requestsPerKilosecond: number;
  daysUntilUTCMidnightExpiration: number;
} & OutputHandler): Promise<this>
```

Mints a Credits NFT.

- `params.requestsPerKilosecond`: Number of requests per kilosecond.
- `params.daysUntilUTCMidnightExpiration`: Number of days until expiration.
- `params.outputId`: (Optional) Custom output identifier.

## Token Creation Methods

### createCreditsDelegationToken

```typescript
async createCreditsDelegationToken(params: {
  expiry?: string,
  creditsTokenId: string,
  delegatees?: HexAddress[],
} & OutputHandler): Promise<this>
```

Creates a Credits Delegation Token.

- `params.expiry`: (Optional) Expiration date in ISO format.
- `params.creditsTokenId`: Credits Token ID.
- `params.delegatees`: (Optional) Array of delegatee addresses.
- `params.outputId`: (Optional) Custom output identifier.

### createAccessToken

```typescript
async createAccessToken(params: {
  expiration?: string;
  pkpPublicKey: HexAddress;
  type: 'custom_auth' | 'native_auth' | 'eoa';
  resources: Array<{
    type: 'pkp-signing' | 'lit-action-execution' | 'rate-limit-increase-auth' | 'access-control-condition-decryption' | 'access-control-condition-signing';
    request: string | '*';
  }>;
  creditsDelegationToken?: AuthSig;
  jsParams?: any;
  code?: string;
  ipfsCid?: IPFSCIDv0;
  permissions?: {
    grantIPFSCIDtoUsePKP?: {
      scopes: AuthMethodScopes;
    }
  };
} & OutputHandler): Promise<this>
```

Creates an Access Token.

- `params.expiration`: (Optional) Expiration date in ISO format.
- `params.pkpPublicKey`: PKP public key.
- `params.type`: Token type ('custom_auth', 'native_auth', or 'eoa').
- `params.resources`: Array of resources and their types.
- `params.creditsDelegationToken`: (Optional) Credits Delegation Token.
- `params.jsParams`: (Optional) JavaScript parameters.
- `params.code`: (Optional) JavaScript code.
- `params.ipfsCid`: (Optional) IPFS CID.
- `params.permissions`: (Optional) Additional permissions.
- `params.outputId`: (Optional) Custom output identifier.

## Permission Granting Methods

### grantAuthMethodToUsePKP

```typescript
async grantAuthMethodToUsePKP(params: {
  pkpTokenId: HexAddress;
  authMethodId: `${string}:${string}` | string;
  authMethodType: number;
  scopes: AuthMethodScopes
} & OutputHandler): Promise<this>
```

Grants an auth method permission to use a PKP.

- `params.pkpTokenId`: PKP Token ID.
- `params.authMethodId`: Auth method ID.
- `params.authMethodType`: Auth method type.
- `params.scopes`: Array of auth method scopes.
- `params.outputId`: (Optional) Custom output identifier.

### grantIPFSCIDtoUsePKP

```typescript
async grantIPFSCIDtoUsePKP(params: {
  pkpTokenId: HexAddress,
  ipfsCid: IPFSCIDv0,
  scopes: AuthMethodScopes
} & OutputHandler): Promise<this>
```

Grants an IPFS CID permission to use a PKP.

- `params.pkpTokenId`: PKP Token ID.
- `params.ipfsCid`: IPFS CID.
- `params.scopes`: Array of auth method scopes.
- `params.outputId`: (Optional) Custom output identifier.

## Getter Methods

### getPkps

```typescript
async getPkps(): Promise<this>
```

Retrieves all PKPs associated with the signer's address.

## Action Methods

### use

```typescript
use(accessToken: SessionSigsMap): {
  toRun: <T extends PKG_TYPES>(repo: T, params: PkgParams[T] & OutputHandler) => Promise<this>;
  toExecuteJs: (params: {
    code?: string,
    ipfsId?: IPFSCIDv0,
    authMethod?: AuthMethod[],
    jsParams?: { [key: string]: any }
  } & OutputHandler) => Promise<this>;
  toPkpSign: (params: {
    publicKey: HexAddress,
    message: string | Uint8Array,
  } & OutputHandler) => Promise<this>;
}
```

Provides methods to use an access token for various actions.

#### toRun

Runs a specific package or IPFS CID.

#### toExecuteJs

Executes JavaScript code on Lit nodes.

#### toPkpSign

Signs a message using a PKP.

## Utility Methods

### getOutput

```typescript
getOutput<T extends BulkieSupportedFunctions>(fnName: T, outputId?: string): FunctionReturnTypes[T] | undefined
```

Retrieves the output of a specific function.

### getAllOutputs

```typescript
getAllOutputs(): Map<BulkieSupportedFunctions, any>
```

Retrieves all outputs.

### getTotalExecutionTime

```typescript
getTotalExecutionTime(): { ms: number, s: number }
```

Retrieves the total execution time of all operations.
