<!-- PROJECT LOGO -->
<br />
<div align="center">
  <a href="https://github.com/othneildrew/Best-README-Template">
    <img src="images/logo.png" alt="Logo" width="80" height="80">
  </a>

  <h3 align="center">CXP Lending Protocol</h3>

  <p align="center">
    An awesome defi protocol!
    <br />
    <a href="https://github.com/othneildrew/Best-README-Template"><strong>Explore the docs »</strong></a>
    <br />
  </p>
</div>

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#core-contracts">Core Contracts</a>
      <ul>
        <li><a href="#lendingpooladdressesprovider">LendingPoolAddressesProviderRegistry</a></li>
      </ul>
      <ul>
        <li><a href="#built-with">LendingPoolAddressesProvider</a></li>
      </ul>
      <ul>
        <li><a href="#built-with">ReserveLogic</a></li>
      </ul>
      <ul>
        <li><a href="#built-with">GenericLogic</a></li>
      </ul>
      <ul>
        <li><a href="#built-with">ValidationLogic</a></li>
      </ul>
      <ul>
        <li><a href="#built-with">LendingPoolImpl</a></li>
      </ul>
      <ul>
        <li><a href="#built-with">LendingPool</a></li>
      </ul>
      <ul>
        <li><a href="#built-with">LendingPoolConfiguratorImpl</a></li>
      </ul>
      <ul>
        <li><a href="#built-with">LendingPoolConfigurator</a></li>
      </ul>
      <ul>
        <li><a href="#built-with">StableAndVariableTokensHelper</a></li>
      </ul>
      <ul>
        <li><a href="#built-with">ATokensAndRatesHelper</a></li>
      </ul>
      <ul>
        <li><a href="#built-with">AToken</a></li>
      </ul>
      <ul>
        <li><a href="#built-with">StableDebtToken</a></li>
      </ul>
      <ul>
        <li><a href="#built-with">AaveOracle</a></li>
      </ul>
      <ul>
        <li><a href="#built-with">LendingRateOracle</a></li>
      </ul>
      <ul>
        <li><a href="#built-with">AaveProtocolDataProvider</a></li>
      </ul>
      <ul>
        <li><a href="#built-with">WETHMocked</a></li>
      </ul>
      <ul>
        <li><a href="#built-with">WETHGateway</a></li>
      </ul>
      <ul>
        <li><a href="#built-with">DefaultReserveInterestRateStrategy</a></li>
      </ul>
      <ul>
        <li><a href="#built-with">rateStrategyStableOne</a></li>
      </ul>
      <ul>
        <li><a href="#built-with">LendingPoolCollateralManagerImpl</a></li>
      </ul>
      <ul>
        <li><a href="#built-with">LendingPoolCollateralManager</a></li>
      </ul>
      <ul>
        <li><a href="#built-with">WalletBalanceProvider</a></li>
      </ul>
    </li>
    <li>
      <a href="#configurations">Configurations</a>
    </li>
    <li>
      <a href="#deployment">Deployment</a>
    </li>
  </ol>
</details>

<!-- CORE CONTRACTS -->

## Core Contracts

<!-- CONFIGURATIONS -->

## Configurations

### Npm scripts

1. Declare a new network in `package.json` file. The network's name should match with that declared in `hardhat.config.ts`

   ```json
   {
     ...
     "scripts": {
       ...
       "hardhat:vdev": "hardhat --network vdev",
     }
   }
   ```

2. Add a new script to fully migrate contracts to the network declared above

   ```json
   {
     ...
     "scripts": {
       ...
       "vchain:vdev:full:migration": "npm run compile && npm run hardhat:vdev vchain:mainnet -- --pool Vchain --verify",
     }
   }
   ```

### Hardhat configurations

1. Add type definitions in `helpers/types.ts`

   ```typescript
   export type eNetwork = ... | eVchainNetwork;
   ...
   export enum eVchainNetwork {
     vchain = "vchain",
     vdev = "vdev",
   }
   ...
   export type iVchainPoolAssets<T> = Pick<iAssetBase<T>, "VUSD">;
   ...
   export type ... | iVchainParamsPerNetwork<T>;
   ...
   export interface iVchainParamsPerNetwork<T> {
     [eVchainNetwork.vchain]: T;
     [eVchainNetwork.vdev]: T;
   }
   ...
   export interface IVchainConfiguration extends ICommonConfiguration {
     ReservesConfig: iVchainPoolAssets<IReserveParams>;
   }

   // add new asset
   export interface iAssetBase<T> {
     VUSD: T;
     ...
   }
   ```

2. Add URL for the new network in `helper-hardhat-config.ts`

   ```typescript
   export const NETWORKS_RPC_URL: iParamsPerNetwork<string> = {
     ...
     [eVchainNetwork.vdev]: `https://staging.rpc.vcex.xyz`,
   }
   ```

3. Add the new network in `hardhat.config.ts`

   ```typescript
   const builderConfig = {
     ...
     etherscan: {
       apiKey: {
         vdev: // ADD KEY HERE
       },
       customChains: [
         {
           network: "vdev",
           chainId: 15000,
           urls: {
             apiURL: "https://staging.explorer.vcex.xyz/api",
             browserURL: "https://staging.explorer.vcex.xyz",
           },
         }
       ]
     },
     ...
     networks: {
       ...
       vdev: {
         ...getCommonNetworkConfig(eVchainNetwork.vdev, 15000),
         accounts: { mnemonic: process.env.MNEMONIC },
       }
     }
   }
   ```

### Migrations

1. Create new migration files for the new network in `markets/vchain`

   ```bash
   ├──...
   ├── markets
   │   ├── vchain
   │   │   ├── common.js
   │   │   ├── index.js
   │   │   ├── rateStrategies.js
   │   │   └── reservesConfigs.js
   │   └── ...
   ├── ...
   ```

<!-- Deployment -->

## Deployment
