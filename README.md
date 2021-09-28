# Preparing source

- Clone `goen-smart-contract`

```
git clone git@github.com:goen-finance/goen-smart-contract.git
cd goen-smart-contract
git checkout -b develop
yarn install
yarn compile
```

## Deploy Contracts

- run bash deployContract.sh
- Choose the task for contract you want deploy with hardhat-upgrades plugin + 1. For VenusVault + 2. For VenusBridge + 3. For SafeVenus + 4. For GovBNB, Treasury, GoenToken
- Hit enter and wating for moments contract deployed line appeares ...

## Verify Contracts

- Go to the BSC scan testnet and search address of contract was deployed to
- Current deploy is proxy upgrades -> go to the `Contract` tab -> choose `Code` -> `is this Proxy` -> verify button -> verify implementation of proxy -> make `readable` function on Contract(deploy using proxy)
- `run` command `bash deployContract.sh` -> choose any differences task number (from 1 to 4)-> `hit` enter and paste address of `implementation` need to verify.
- Back to the verify page of proxy -> hit `verify` button and see update verified

## Upgrade Contract

- You can see here for more informations : `https://docs.openzeppelin.com/upgrades-plugins/1.x/writing-upgradeable`
- deployContract.sh isn't complete to current point (or very simple, this is optional to use for)
- See list scripts of package.json file : some example of upgradeProxy from {upgrades} of hardhat tool. Change the address in example_upgrade.js file and run script `yarn`

## Deployment

- VaultVenus: 0xf7E71E5bf41BB3b5C0220bA8D505DF4b980D66AA
- VenusBridge: 0x082eB5f85E44AD0d8560157bf7236121fCCE222c
- SafeVenus: 0x37C0DC5Cf8aCE214fb1B8221201E1f9D478F989e
- Treasury: 0xc9D99de7523476C3928872334885f712507a8029
- GovBNB: 0x25701f1DC79CAD9FB20913CfBD5Cef3a0c73FcF9

- Unitroller: 0x94d1820b2D1c7c7452A163983Dc888CEC546b77D
- PancakeRouter01: 0x89e310DB3feB95cf8eFD1EB47d9e0f2261E0f6bb

**Tokens**

- GOENToken: 0x57d2584F977DdA3D94ee16b198eb02AE5464721A
- WBNB: 0x97c012Ef10eDc79510A17272CEE3ecBE1443177F
- XVS: 0xB9e0E753630434d7863528cc73CB7AC638a7c8ff
- BUSD: 0x8301F2213c0eeD49a7E28Ae4c3e91722919B8B47
- vBUSD: 0x08e0A5575De71037aE36AbfAfb516595fE68e5e4

**Setup the VaultVenus**

- All contracts and token is verified but some importants functions need the ownable requirements : harvest, setSafeVenus, setVenusBridge, addVault ...Therefore need for test (harvest function, simplest wway: `temporary` remove modifier `onlyOwner`; another way from `developer` can be used ex: set `WhiteList`)

- At contract VenusBridge on tab write, connect wallet and add 3 params to init the Vault and enter markets with these info to Venus protocol `@param1: address VaultVenus`, `@param2: address token BUSD` in this case, `vToken address` vBUSD in this case.

- At VenusVault contracts set library SafeVenus on tab write at `setSafeVenus` function.
- the same way with `setVenusBridge` but input is `VenusBridge` contract address
- now `updateVenusFactor` functions can run and interactive with Venus protocol. Every functions `deposit`, `withdrawUnderlying`, `withdrawAll` need run throught this function

**Notice**

- For per account wallet (EOA address) need to approve `vaultStakingToken` (BUSD) for transfer tokens with vault
- See the update the state of Contract on BSC after some functions modified need to refresh page.
- Issues:
  - For first deposit the balance from read tab of `VaultVenus` not update current value. Because updateVenusFactor() modify `state` not be use on balance() is view function.(dev fix)
  - BalanceOfUnderlying() the sames because the exchangeRateCurrent() update everytimes.
