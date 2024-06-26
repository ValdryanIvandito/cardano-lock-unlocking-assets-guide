# Pendahuluan

Ini adalah dokumentasi yang memberikan panduan langkah demi langkah cara membuat Alamat Kontrak.

# Langkah-langkah

## Langkah-1 Kompilasi Skrip Plutus / UPLC

Pertama-tama Anda harus mengkompilasi Skrip Validator menjadi Plutus Skrip atau UPLC. Ikuti [dokumentasi](https://github.com/ValdryanIvandito/cardano-script-compiling-guide) berikut ini. Terdapat Skrip Validator PlutusTX dan Aiken; pilih salah satu. Setelah Anda mengkompilasi Skrip Validator, kemudian pergi ke direktori output.

## Langkah-2 Inisiasi Jaringan Cardano (Opsional)

**_Petunjuk: Jika Anda sudah memilih jaringan, Anda dapat melewati langkah ini_**

```bash
network="testnet-magic 1"
```

_or_

```bash
network="testnet-magic 2"
```

_or_

```bash
network="mainnet"
```

**Berikut adalah tabel mengenai jaringan di Cardano**
| cli Parameter | Network Name |
|---------|---------|
| testnet-magic 1 | Preprod |
| testnet-magic 2 | Preview |
| mainnet | Mainnet |

## Langkah-3 Membuat Alamat Kontrak

```bash
cardano-cli address build \
--payment-script-file always-succeeds.plutus \
--out-file contract.addr \
--$network
```

**_Catatan: Dalam contoh ini, kita menggunakan Skrip 'always-succeeds.plutus' berdasarkan [dokumentasi](https://github.com/ValdryanIvandito/cardano-script-compiling-guide) berikut. Namun nama file skrip bisa apa saja._**

### Menampilkan Alamat Kontrak

```bash
contractAddress=$(cat contract.addr)
echo $contractAddress
```

### Menampilkan informasi UTxO Alamat Kontrak (Cek Saldo)

```bash
cardano-cli query utxo \
--address $contractAddress \
--$network
```

# Demo

Berikut adalah video yang direkam oleh Komunitas Developer Cardano Indonesia di mana saya menjelaskan langkah-langkah di atas. Tonton video yang direkam pada timestamp **_1:27:27_** di [link](https://youtu.be/03hXLZ_07N0?list=PLUj8499OocHiL8gXPv8wMlLW-zIcyYdrQ) berikut ini.

# Referensi

[Gimbalabs PPBL2023 Module 102.2: Build an Address](https://plutuspbl.io/modules/102/1022)
