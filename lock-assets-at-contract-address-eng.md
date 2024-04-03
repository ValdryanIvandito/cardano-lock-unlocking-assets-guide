# Introduction

This is documentation that provides you with a step-by-step guide on how to lock assets to a Contract Address. First, You need the **Contract Address** and **the Wallet Address**. The goal of this practical session in this documentation is to send an amount of ADA from the Wallet Address to the Contract Address, and then lock the assets.

# Step by step

## Step-1 Generate Contract Address

If you haven't generated a Contract Address, follow the previous [documentation](https://github.com/ValdryanIvandito/cardano-lock-unlocking-assets-guide/blob/main/generate-contract-address-eng.md).

## Step-2 Generate Wallet Address (Sender)

If you haven't generated a Wallet Address, follow the previous [documentation](https://github.com/ValdryanIvandito/cardano-basic-transaction-guide/blob/main/generate-wallet-address-eng.md).

## Step-3 Initiate the Input: Transaction Hash (TxHash) and Transaction Index (TxIx) from Wallet Address (Sender)

**_Hint: Assuming you already Wallet Address_**

### Display Information About the Wallet Address UTxO

```bash
cardano-cli query utxo \
--address $myAddress \
--$network
```

### Initiate TxHash and TxIx

```bash
utxo="COPY THE TX-HASH HERE#COPY THE TX-IX NUMBER HERE"
```

## Step-4 Initiate the Output: Amount to Lock, Datum Value, and Contract Address

**_Hint: Assuming you already Contract Address_**

```bash
lockAmount="AMOUNT IN LOVELACE"
datumValue="1618"
```

**_Notes: A UTxO at a Contract Address must include Datum. In this example, we'll simply use an inline datum value. The Datum Value can be any number because we use the 'always-succeeds.plutus' contract, which is the output always true._**

## Step-5 Build Transaction From the Wallet Address (Sender)

```bash
cardano-cli transaction build \
--babbage-era \
--$network \
--tx-in $utxo \
--tx-out $contractAddress+$lockAmount \
--tx-out-inline-datum-value $datumValue \
--change-address $myAddress \
--out-file lock-always-succeeds.raw
```

## Step-6 Sign Transaction From the Wallet Address (Sender)

```bash
cardano-cli transaction sign \
--$network \
--tx-body-file lock-always-succeeds.raw \
--signing-key-file payment.skey \
--out-file lock-always-succeeds.signed
```

## Step-7 Submit Transaction From the Wallet Address (Sender)

```bash
cardano-cli transaction submit \
--$network \
--tx-file lock-always-succeeds.signed \
```

### Display Information About the Contract Address UTxO

```bash
cardano-cli query utxo \
--address $contractAddress \
--$network
```

**Example Result:**

```bash
                           TxHash                                 TxIx        Amount
--------------------------------------------------------------------------------------
07b7300894f2b175d322ea24e18b2a210ef8d8141d22ce6ce9f37efb108d5544     0        500000000 lovelace + TxOutDatumInline ReferenceTxInsScriptsInlineDatumsInBabbageEra (ScriptDataNumber 1618)
```

**_Notes: Look at the UTxO; there is a TxOutDatumInline which has ScriptDataNumber 1618, in accordance with the datum value provided during the transaction._**

# Demo

The following is a video recorded by the Indonesian Cardano Developers Community where I demonstrated the steps above. Watch the recorded video at timestamp **_1:27:27_**, here is the [link](https://youtu.be/03hXLZ_07N0?list=PLUj8499OocHiL8gXPv8wMlLW-zIcyYdrQ)

# References

[Gimbalabs PPBL2023 Module 102.4: Lock Tokens at a Contract Address](https://plutuspbl.io/modules/102/1024)
