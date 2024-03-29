# Pendahuluan

Ini adalah dokumentasi yang memberikan panduan langkah demi langkah cara menghasilkan alamat kontrak.

# Langkah-langkah

## Langkah-1 Kompilasi Skrip Plutus / UPLC

Pertama-tama Anda harus mengkompilasi Skrip Validator menjadi Plutus Skrip atau UPLC. Ikuti [dokumentasi](https://github.com/ValdryanIvandito/cardano-script-compiling-guides) berikut ini. Terdapat Skrip Validator PlutusTX dan Aiken; pilih salah satu. Setelah Anda mengkompilasi Skrip Validator, kemudian pergi ke direktori output.

## Langkah-2 Inisiasi Jaringan Cardano

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

**_Catatan: Dalam contoh ini, kita menggunakan Skrip 'always-succeeds.plutus' berdasarkan [dokumentasi](https://github.com/ValdryanIvandito/cardano-script-compiling-guides) berikut. Namun nama file skrip bisa apa saja._**

### Menampilkan Alamat

```bash
contractAddress=$(cat contract.addr)
echo $contractAddress
```

**Contoh Alamat:**

```bash
addr_test1wzxwjsv8ktwprjjnvx23xqp68qk282ks604hm37k75kekgqsl59k0
```

### Menampilkan informasi UTxO (Cek Saldo)

```bash
cardano-cli query utxo \
--address $myAddress \
--$network
```

# Demo

Berikut adalah video yang direkam oleh Komunitas Developer Cardano Indonesia di mana saya menjelaskan langkah-langkah di atas. Tonton video yang direkam pada timestamp **_1:27:27_** di [link](https://youtu.be/03hXLZ_07N0?list=PLUj8499OocHiL8gXPv8wMlLW-zIcyYdrQ) berikut ini.

# Referensi

[Gimbalabs PPBL Module 102.2: Build an Address](https://plutuspbl.io/modules/102/1022)

[Cardano Academy](https://academy.cardanofoundation.org/)
