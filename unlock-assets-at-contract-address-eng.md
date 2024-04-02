# Introduction

This is documentation that provides you with a step-by-step guide on how to unlock assets from a Contract Address. This documentation is a continuation of the previous [documentation](https://github.com/ValdryanIvandito/cardano-lock-unlocking-assets-guides/blob/main/lock-assets-at-contract-address-eng.md), which is about locking assets at the Contract Address. The goal of this practical session in this documentation is to unlock an amount of ADA from the Contract Address and then send it back to the Wallet Address.

# Step by step

## Step-1 Initiate the Input: Contract-UTxO, Collateral-UTxO, Plutus-Script-File, and Redeemer-Value

**_Hint: Assuming you already have a Contract Address and Wallet Address_**

### 1. Display Information About the Contract UTxO

```bash
cardano-cli query utxo \
--address $contractAddress \
--$network
```

### 2. Initiate TxHash and TxIx From Contract UTxO

```bash
contractUtxo="COPY THE TX-HASH HERE#COPY THE TX-IX NUMBER HERE"
```

### 3. Display Information About the Wallet UTxO

```bash
cardano-cli query utxo \
--address $myAddress \
--$network
```

### 4. Initiate TxHash and TxIx From Wallet UTxO for Collateral

```bash
collateralUtxo="COPY THE TX-HASH HERE#COPY THE TX-IX NUMBER HERE"
```

### 5. Initiate Plutus-Script-File Path

**_Hint: In this example, Plutus-Script-File generated at demeter.run workspace_**

If you use Plutus-Script in this [documentation](https://github.com/ValdryanIvandito/cardano-script-compiling-guides/blob/main/compiling-plutustx-script-eng.md), the file path is:

```bash
plutusScript="/config/workspace/repo/ppbl2023-plutus-template/output/always-succeeds.plutus"
```

If you use Aiken-Script in this [documentation](https://github.com/ValdryanIvandito/cardano-script-compiling-guides/blob/main/compiling-aiken-script-eng.md), the file path is:

```bash
plutusScript="/config/workspace/repo/aiken-template/output/always-succeeds.plutus"
```

### 6. Initiate Redeemer-Value

```bash
redeemerValue="1618"
```

**_Notes: The Redeemer Value can be any number because we use the 'always-succeeds.plutus' contract, which is the output always true._**

## Step-2 Build Protocol JSON File

```bash
cardano-cli query protocol-parameters \
--$network \
--out-file protocol.json
```

## Step-3 Build Transaction From the Wallet Address (Sender)

```bash
cardano-cli transaction build \
--babbage-era \
--$network \
--tx-in $contractUtxo \
--tx-in-collateral $collateralUtxo \
--tx-in-script-file $plutusScript \
--tx-in-inline-datum-present \
--tx-in-redeemer-value $redeemerValue \
--change-address $myAddress \
--protocol-params-file protocol.json \
--out-file unlock-always-succeeds.raw
```

## Step-4 Sign Transaction From the Wallet Address (Sender)

```bash
cardano-cli transaction sign \
--$network \
--tx-body-file unlock-always-succeeds.raw \
--signing-key-file payment.skey \
--out-file unlock-always-succeeds.signed
```

## Step-5 Submit Transaction From the Wallet Address (Sender)

```bash
cardano-cli transaction submit \
--$network \
--tx-file unlock-always-succeeds.signed \
```

**_Hint: You can track the transaction using a blockchain explorer, such as Cardano Explorer or CardanoScan. Copy the link below._**

Preprod:

```bash
https://preprod.cardanoscan.io/transaction/COPY-THE-TX-HASH-HERE
```

Preview:

```bash
https://preview.cardanoscan.io/transaction/COPY-THE-TX-HASH-HERE
```

Mainnet:

```bash
https://cardanoscan.io/transaction/COPY-THE-TX-HASH-HERE
```

# Demo

The following is a video recorded by the Indonesian Cardano Developers Community where I demonstrated the steps above. Watch the recorded video at timestamp **_1:27:27_**, here is the [link](https://youtu.be/03hXLZ_07N0?list=PLUj8499OocHiL8gXPv8wMlLW-zIcyYdrQ)

# References

[Gimbalabs PPBL Module 102.5: Unlock Tokens From a Contract Address](https://plutuspbl.io/modules/102/1025)

[Cardano Docs: Collateral Mechanism](https://docs.cardano.org/smart-contracts/plutus/collateral-mechanism/)

[Cardano Academy](https://academy.cardanofoundation.org/)
