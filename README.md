# EIP-7702 Revoke Tool

Web app untuk menghapus delegasi EIP-7702 (drainer) dari wallet Anda di semua chain sekaligus.

**Live:** [https://yowanda.github.io/revoke-eip7702/](https://yowanda.github.io/revoke-eip7702/)

## Fitur

- **100% Client-Side** — Private key tidak pernah meninggalkan browser Anda. Semua proses berjalan di browser, tidak ada server backend.
- **Multi-Chain** — Support 7 chain: Ethereum, Base, Arbitrum, Optimism, Polygon, BNB Chain, Avalanche
- **Flashbots Protect** — Transaksi di Ethereum mainnet dikirim melalui Flashbots untuk mencegah frontrunning
- **Sponsored Mode** — Wallet lain bisa bayar gas fee (untuk wallet yang sudah 0 balance)
- **Smart Gas Estimation** — Estimasi gas cost real-time dari network, bukan threshold hardcoded. Bekerja optimal di L2 chains (Optimism, Base, Arbitrum) yang gas fee-nya sangat murah
- **Estimasi Gas Sponsor** — Panel real-time gas fee per chain + total yang perlu dikirim ke sponsor, lengkap dengan tombol copy
- **Quick Actions** — Tombol cek delegasi dan revoke per chain untuk kemudahan
- **Real-time Status** — Lihat progress revoke per chain secara langsung

## Cara Pakai

1. Buka [https://yowanda.github.io/revoke-eip7702/](https://yowanda.github.io/revoke-eip7702/)
2. Masukkan **Private Key** wallet yang terkena drain
3. Masukkan **Private Key Sponsor** (wallet lain yang punya ETH/gas token untuk bayar gas fee)
4. Klik **"Cek Delegasi Semua Chain"** untuk scan chain mana yang ada delegasi aktif
5. Klik **"Estimasi Gas Fee Semua Chain"** untuk lihat berapa gas yang dibutuhkan per chain
6. Kirim jumlah yang tertera ke wallet sponsor (jumlah pasti, tidak lebih)
7. Pilih chain yang mau di-revoke, atau klik **"Revoke All Chains"**
8. Tunggu konfirmasi — setiap chain akan menampilkan status dan link ke block explorer

## Apa itu EIP-7702?

EIP-7702 memungkinkan EOA (Externally Owned Account) mendelegasikan eksekusi kode ke smart contract. Jika seseorang menandatangani authorization yang mendelegasikan wallet-nya ke kontrak drainer, maka drainer bisa mengeksekusi transaksi atas nama wallet tersebut — termasuk mentransfer semua aset keluar secara otomatis.

### Bagaimana drainer bisa menambahkan delegasi di semua chain?

Jika authorization ditandatangani dengan `chainId = 0`, maka delegasi berlaku di **semua EVM chain** sekaligus. Dengan satu signature saja, drainer bisa menerapkan delegasi di Ethereum, Optimism, Base, Arbitrum, dan chain lainnya.

### Cara revoke bekerja

Tool ini mengirim transaksi EIP-7702 baru yang mendelegasikan wallet ke `address(0)` (zero address). Ini menghapus kode delegasi dari wallet dan mengembalikannya ke EOA biasa.

## Keamanan

- **Jangan bagikan private key ke siapapun**
- Jalankan tool ini di **komputer pribadi** yang aman
- Jangan gunakan di komputer publik atau jaringan WiFi publik
- Buka **Developer Tools (F12)** untuk verifikasi tidak ada request ke server selain RPC blockchain
- Jika private key Anda juga bocor (bukan hanya delegasi EIP-7702), **buat wallet baru** karena drainer bisa re-delegasi lagi

## Chain yang Didukung

| Chain | Chain ID | Gas Token |
|-------|----------|-----------|
| Ethereum Mainnet | 1 | ETH |
| Base | 8453 | ETH |
| Arbitrum One | 42161 | ETH |
| Optimism | 10 | ETH |
| Polygon | 137 | POL |
| BNB Smart Chain | 56 | BNB |
| Avalanche C-Chain | 43114 | AVAX |

## Gas Estimation

Tool ini menggunakan estimasi gas cost real-time dari network:

1. Mengambil `feeData` (gas price terkini) dari RPC setiap chain
2. Menghitung estimasi biaya: `gasLimit (60000) × maxFeePerGas`
3. Menambah 30% buffer untuk safety margin
4. Membandingkan balance dengan estimasi biaya yang sebenarnya

Dengan cara ini, revoke tetap bisa dilakukan di L2 chains seperti Optimism, Base, dan Arbitrum meskipun balance sponsor sangat kecil (misalnya 0.00002 ETH), selama masih cukup untuk gas fee.

## Tech Stack

- HTML/CSS/JavaScript (vanilla, tanpa framework)
- [ethers.js v6](https://docs.ethers.org/v6/) — library Ethereum
- GitHub Pages — hosting

## License

MIT
