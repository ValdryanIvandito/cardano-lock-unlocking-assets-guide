# Pendahuluan

Ini adalah dokumentasi yang memberikan panduan langkah demi langkah cara mengunci aset di alamat kontrak. Pertama-tama, Anda membutuhkan **Alamat Kontrak** dan **Alamat Dompet**. Tujuan dari percobaan di dokumentasi ini untuk mengirim sejumlah ADA dari Alamat Dompet ke Alamat Kontrak, lalu menguncinya.

# Langkah-Langkah

## Langkah-1 Membuat Alamat Kontrak

Jika Anda belum memiliki Alamat Kontrak, ikuti langkah-langkah di [dokumentasi](https://github.com/ValdryanIvandito/cardano-lock-unlocking-assets-guides/blob/main/generate-contract-address-id.md) berikut ini.

## Langkah-2 Membuat Alamat Dompet (Pengirim)

Jika Anda belum memiliki Alamat Dompet, ikuti langkah-langkah di [dokumentasi](https://github.com/ValdryanIvandito/cardano-cli-simplified/blob/main/1-generate-wallet-address.md) berikut ini.

## Langkah-3 Inisiasi Parameter Input: Alamat Dompet (Pengirim), Hash Transaksi (TxHash), Indeks Transaksi (TxIx)

### Menampilkan Informasi UTxO di Alamat Dompet

```bash
cardano-cli query utxo \
--address $myAddress \
--$network
```

**Contoh Hasil:**

```bash
                           TxHash                                 TxIx        Amount
--------------------------------------------------------------------------------------
62c0ce8d6e0b584e9e263e3ba076f53c23095ebd0a9198305819cfa5ecef8e81     0        1000000000 lovelace + TxOutDatumNone
```

### Inisiasi TxHash dan TxIx

```bash
utxo="COPY TX-HASH DISINI#COPY TX-IX DISINI"
```

## Langkah-4 Inisiasi Parameter Output: Alamat Kontrak, Jumlah ADA yang Akan Dikunci, Nilai Datum

```bash
contractAddress="COPY ALAMAT KONTRAK DISINI"
lockAmount="JUMLAH DALAM LOVELACE"
datumValue="1618"
```

**_Catatan: UTxO di Alamat Kontrak harus menyertakan Datum. Dalam contoh ini, kita hanya akan menggunakan inline datum value. Nilai Datumnya bisa berapa saja karena kita menggunakan kontrak 'always-succeeds.plutus', yang outputnya selalu bernilai true._**

## Langkah-5 Membuat Transaksi Dari Alamat Dompet (Pengirim)

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

**_Estimasi Fee Transaksi: Lovelace 167349_**

## Langkah-6 Menandatangani Transaksi Dari Alamat Dompet (Pengirim)

```bash
cardano-cli transaction sign \
--$network \
--tx-body-file lock-always-succeeds.raw \
--signing-key-file payment.skey \
--out-file lock-always-succeeds.signed
```

## Langkah-7 Kirim Transaksi Dari Alamat Dompet (Pengirim)

```bash
cardano-cli transaction submit \
--$network \
--tx-file lock-always-succeeds.signed \
```

**_Hasil: Transaction successfully submitted_**

### Menampilkan Informasi UTxO di Alamat Kontrak

```bash
cardano-cli query utxo \
--address $contractAddress \
--$network
```

**Contoh Hasil:**

```bash
                           TxHash                                 TxIx        Amount
--------------------------------------------------------------------------------------
07b7300894f2b175d322ea24e18b2a210ef8d8141d22ce6ce9f37efb108d5544     0        500000000 lovelace + TxOutDatumInline ReferenceTxInsScriptsInlineDatumsInBabbageEra (ScriptDataNumber 1618)
```

**_Catatan: Lihatlah UTxO; terdapat TxOutDatumInline yang memiliki ScriptDataNumber 1618, sesuai dengan nilai datum yang diberikan saat transaksi._**

# Demo

Berikut adalah video yang direkam oleh Komunitas Developer Cardano Indonesia di mana saya menjelaskan langkah-langkah di atas. Tonton video yang direkam pada timestamp **_1:27:27_** di [link](https://youtu.be/03hXLZ_07N0?list=PLUj8499OocHiL8gXPv8wMlLW-zIcyYdrQ) berikut ini.

# Referensi

[Gimbalabs PPBL Module 102.4: Lock Tokens at a Contract Address](https://plutuspbl.io/modules/102/1024)

[Cardano Academy](https://academy.cardanofoundation.org/)
