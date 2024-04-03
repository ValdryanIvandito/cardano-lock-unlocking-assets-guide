# Pendahuluan

Ini adalah dokumentasi yang memberikan panduan langkah demi langkah cara mengambil aset yang terkunci di Alamat Kontrak. Dokumentasi ini adalah lanjutan dari [dokumentasi](https://github.com/ValdryanIvandito/cardano-lock-unlocking-assets-guide/blob/main/lock-assets-at-contract-address-id.md) sebelumnya, yaitu mengunci aset di Alamat Kontrak. Tujuan dari percobaan di dokumentasi ini adalah mengirim kembali sejumlah ADA dari Alamat Kontrak yang terkunci ke Alamat Dompet.

# Langkah-Langkah

## Langkah-1 Inisiasi Parameter Input: UTxO Kontrak, UTxO Kolateral, File Plutus Skrip, dan Nilai Redeemer

**_Petunjuk: Asumsi Anda sudah memiliki Alamat Kontrak dan Alamat Dompet_**

### Menampilkan Informasi UTxO Alamat Kontrak

```bash
cardano-cli query utxo \
--address $contractAddress \
--$network
```

### Inisiasi TxHash dan TxIx dari UTxO Alamat Kontrak

```bash
contractUtxo="COPY TX-HASH DISINI#COPY TX-IX DISINI"
```

### Menampilkan Informasi UTxO Alamat Dompet

```bash
cardano-cli query utxo \
--address $myAddress \
--$network
```

### Inisiasi TxHash dan TxIx dari UTxO Alamat Dompet untuk Kolateral

```bash
collateralUtxo="COPY TX-HASH DISINI#COPY TX-IX DISINI"
```

### Inisiasi File Path Skrip Plutus

**_Petunjuk: Dalam contoh ini, Skrip Plutus dibuat di workspace demeter.run_**

Jika Anda menggunakan Skrip PlutusTx di [dokumentasi](https://github.com/ValdryanIvandito/cardano-script-compiling-guide/blob/main/compiling-plutustx-script-id.md) ini, gunakan:

```bash
plutusScript="/config/workspace/repo/ppbl2023-plutus-template/output/always-succeeds.plutus"
```

Jika Anda menggunakan Skrip Aiken di [dokumentasi](https://github.com/ValdryanIvandito/cardano-script-compiling-guide/blob/main/compiling-aiken-script-id.md) ini, gunakan:

```bash
plutusScript="/config/workspace/repo/aiken-template/output/always-succeeds.plutus"
```

### Inisiasi Nilai Redeemer

```bash
redeemerValue="1618"
```

**_Catatan: Nilai Redeemer bisa berapa saja karena kita menggunakan kontrak 'always-succeeds.plutus', yang outputnya selalu bernilai true._**

## Langkah-2 Membuat File Protocol JSON

```bash
cardano-cli query protocol-parameters \
--$network \
--out-file protocol.json
```

## Langkah-3 Membuat Transaksi dari Alamat Dompet (Pengirim)

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

## Langkah-4 Menandatangani Transaksi dari Alamat Dompet (Pengirim)

```bash
cardano-cli transaction sign \
--$network \
--tx-body-file unlock-always-succeeds.raw \
--signing-key-file payment.skey \
--out-file unlock-always-succeeds.signed
```

## Langkah-5 Mengirim Transaksi dari Alamat Dompet (Pengirim)

```bash
cardano-cli transaction submit \
--$network \
--tx-file unlock-always-succeeds.signed \
```

**_Petunjuk: Anda dapat melacak transaksi menggunakan penjelajah blockchain, seperti Cardano Explorer atau CardanoScan. Salin tautan di bawah ini._**

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

Berikut adalah video yang direkam oleh Komunitas Developer Cardano Indonesia di mana saya menjelaskan langkah-langkah di atas. Tonton video yang direkam pada timestamp **_1:27:27_** di [link](https://youtu.be/03hXLZ_07N0?list=PLUj8499OocHiL8gXPv8wMlLW-zIcyYdrQ) berikut ini.

# Referensi

[Gimbalabs PPBL Module 102.5: Unlock Tokens From a Contract Address](https://plutuspbl.io/modules/102/1025)

[Cardano Docs: Collateral Mechanism](https://docs.cardano.org/smart-contracts/plutus/collateral-mechanism/)
