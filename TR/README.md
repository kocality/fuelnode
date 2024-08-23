# Fuel Testnet Node Kurulum Rehberi

![image](https://github.com/user-attachments/assets/e44ea8a4-4bea-488f-a0e6-388e170f4ae2)

## Fuel Hakkında
Fuel, Ethereum rollupları için özel olarak tasarlanmış bir işletim sistemidir ve geliştiricilerin ölçeklenebilir merkeziyetsiz ekonomiler oluşturmasına yardımcı olur.
* [Twitter / X](https://x.com/fuel_network)
* [Websitesi](https://fuel.network/)
* [Discord](https://discord.com/invite/xfpK4Pe)
* [Dokümantasyon](https://docs.fuel.network/)
* [GitHub](https://github.com/FuelLabs)
* [Explorer](https://app.fuel.network/)
* [Forum](https://forum.fuel.network/)
* [Fuel Türkiye](https://x.com/fuelnetworktr)

## Fuel Node Hakkında

Fuel blockchain'inde bir node—genellikle client olarak da bilinir—blokzincirin yerel bir kopyasını indiren, depolayan ve sürekli olarak güncelleyen temel bir yazılımdır. Bu yazılım, her bloğun ve işlemin doğruluğunu doğrulamada kritik bir rol oynar ve node'unuzun ağ ile tam olarak senkronize olmasını sağlar. Fuel node'ları, güvenilir doğrulayıcıların blok oluşturma ve işlem doğrulama görevini üstlendiği Proof of Authority (PoA) konsensüs mekanizması ile çalışır. Bu yöntem, ağa diğer konsensüs modelleri olan Proof of Work veya Proof of Stake'e kıyasla daha hızlı işlem süreleri ve daha yüksek enerji verimliliği sağlar.

### Fuel Üzerinde Bir Node Çalıştırmanın Avantajları

Kendi Fuel node'unuzu çalıştırmanın önemli avantajları vardır:

- **Tam Kontrol:** Kendi node'unuzu barındırarak, Fuel blockchain ile olan etkileşimleriniz üzerinde tam kontrol sağlarsınız ve hız sınırlamaları ile üçüncü taraf bağımlılıklarından kaçınırsınız.
- **Ağ Bağımsızlığı:** Kendi node'unuzu çalıştırmak, dış hizmetlere bağımlı olmamanızı sağlar ve blokzincir ile doğrudan etkileşim kurmanıza ve sorguları sınırsız bir şekilde çalıştırmanıza olanak tanır.

## Sistem Gereksinimleri

| Donanım    | Minimum | Önerilen   |
|------------|---------|------------|
| İşlemci    | 2 Çekirdek | 8 Çekirdek  |
| Bellek     | 4 GB    | 12 GB       |
| Depolama   | 30 GB   | 100 GB      |


## Adım 1: Sistem Güncellemeleri ve Gerekli Araçların Kurulumu

### Sistem Paketlerini Güncelleyin
```bash
sudo apt-get update && sudo apt-get upgrade -y
sudo apt install wget curl
```

### Rust ve Cargo Kurulumu
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

source $HOME/.cargo/env
```

#### Rust Versiyonunu Kontrol Edin:
```bash
rustup --version
```

## Adım 2: Fuel Araç Takımı

### Fuel İndirin ve Kurun
```bash
curl https://install.fuel.network | sh
source /root/.bashrc
```
<img width="349" alt="Ekran Resmi 2024-08-23 14 02 04" src="https://github.com/user-attachments/assets/24ecc340-5378-439a-a75e-5752da1ee6d9">

#### Fuelup'ın doğru kurulduğundan emin olmak için kontrol edin:
```bash
fuelup --version
```
<img width="349" alt="Ekran Resmi 2024-08-23 14 03 28" src="https://github.com/user-attachments/assets/c6519335-bdb3-42f5-8c54-a92121966566">

### P2P Anahtar Çifti Oluşturma
#### Not: Komutu girdikten sonra size önemli bilgiler verecektir. Bu bilgileri bir yere not alın ve kimseyle paylaşmayın.
```bash
fuel-core-keygen new --key-type peering
```
<img width="1258" alt="Ekran Resmi 2024-08-23 14 05 12" src="https://github.com/user-attachments/assets/d5bcb155-95a5-4144-a9ba-a78e0abefce6">


### Chain Configuration'ı Klonlama ve Dosyaları Kopyalama
```bash
git clone https://github.com/FuelLabs/chain-configuration chain-configuration
mkdir ~/.fuel-testnet
cp -r chain-configuration/ignition/* ~/.fuel-testnet/
```

#### `Sepolia Ethereum` için bir RPC endpoint'i almak üzere [Alchemy](https://www.alchemy.com/) sitesini ziyaret edin ve Apps > Endpoints > Networks adımlarını izleyin. Ethereum bölümünün altında `Sepolia` seçeneğini seçerek RPC URL'nizi alın. Bu URL'yi node konfigürasyonunda `Sepolia-ETH-RPC-ENDPOINT` olarak kullanacağız.

<img width="1201" alt="Ekran Resmi 2024-08-23 14 17 43" src="https://github.com/user-attachments/assets/c1fcaf63-417d-444c-b1f5-be01d2c6ee2c">

## Adım 3: Node'u Çalıştırma

### Screen Oluşturma
```bash
screen -S fuel-node
```

### Node'u Çalıştırma
#### Not: `NODENAME`, `P2P-SECRET`, `Sepolia-ETH-RPC-ENDPOINT` değerlerini kendi bilgilerinizle değiştirin.
```bash
fuel-core run \
--service-name=`NODENAME` \
--keypair `P2P-SECRET` \
--relayer `Sepolia-ETH-RPC-ENDPOINT` \
--ip=0.0.0.0 --port=4000 --peering-port=30333 \
--db-path ~/.fuel-testnet \
--snapshot ~/.fuel-testnet \
--utxo-validation --poa-instant false --enable-p2p \
--reserved-nodes /dns4/p2p-testnet.fuel.network/tcp/30333/p2p/16Uiu2HAmDxoChB7AheKNvCVpD4PHJwuDGn8rifMBEHmEynGHvHrf \
--sync-header-batch-size 100 \
--enable-relayer \
--relayer-v2-listening-contracts=0x01855B78C1f8868DE70e84507ec735983bf262dA \
--relayer-da-deploy-height=5827607 \
--relayer-log-page-size=500 \
--sync-block-stream-buffer-size 30
```

<img width="900" alt="Ekran Resmi 2024-08-23 14 24 53" src="https://github.com/user-attachments/assets/dc9e6317-e6c4-49e0-b7af-0fa992f87206">

#### Loglar bu şekilde görünmelidir. Ekrandan çıkmak için `CTRL A+D` tuş kombinasyonunu kullanabilirsiniz. Bu şekilde çıktığınızda, node'unuz ekran oturumunda çalışmaya devam edecektir. 


### Node'u Durdurma

Node'u durdurmak için, screen oturumuna geri dönmek için şu komutu kullanın:

```bash
screen -r fuel-node
```
#### Screen oturumuna girdikten sonra node'u durdurmak için `CTRL + C` tuşlarına basın.
