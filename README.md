<h1 align="center"> Timpi </h1>

> Timpi Eylül'de mainnete geçecek, o zamana kadarda testnet devam edecek.

> Ödül token olarak bilgi yok, NFT verileceği yazıyor ama bunun işlevi var mı belirsiz.

> Timpi her ne kadar `2 CPU 2 RAM` ile çalışsada bedava sunucu değilse para vermeye değer mi bilmem.

> Çalışan nodelarımın arasına sıkıştırdım Timpi'yi.

> Topluluk kanallarım, [Duyuru Kanalım](https://t.me/RuesAnnouncement) - [Sorular için sohbet](https://t.me/RuesChat) - [Timpi Discord](https://discord.gg/timpi-the-new-way-946982023245992006)

```sh
# Sunucumuzu güncelleyelim
sudo apt update
sudo apt upgrade -y
sudo apt-get install git curl build-essential make jq gcc snapd chrony lz4 tmux unzip bc -y
```
```sh
# Go'yu indirelim.
rm -rf $HOME/go
sudo rm -rf /usr/local/go
cd $HOME
curl https://dl.google.com/go/go1.18.1.linux-amd64.tar.gz | sudo tar -C/usr/local -zxvf -
cat <<'EOF' >>$HOME/.profile
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export GO111MODULE=on
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
EOF
source $HOME/.profile
```
```sh
# Timpi'yi kuralım
cd $HOME
rm -rf Timpi-ChainTN
git clone https://github.com/Timpi-official/Timpi-ChainTN.git

cd Timpi-ChainTN
cd cmd/TimpiChain
go build
```
```sh
# İnitalize
./TimpiChain init TimpiChain --chain-id WhatYouWant

# Cüzdan oluşturalım, rues'i değiştirin. Cüzdan bilgileriniz kaydedin.
./TimpiChain keys add rues --keyring-backend test

# Burada da rues'i değiştirin.
./TimpiChain add-genesis-account rues 4000000000stake --keyring-backend test

# Aynı şekilde.
./TimpiChain gentx rues 1000000stake --keyring-backend test --chain-id WhatYouWant
./TimpiChain collect-gentxs

# Benzer cosmos değil, screen dışında çalıştırmayalım.
apt install screen
screen -S timpi
./TimpiChain start
```

> Cüzdan oluşturduktan sonra bu linkin sonuna cüzdanını yazıp explorer'da aratman yeterli, token gelecek. http://173.249.54.208:1337/cüzdanadresi

> 2'den fazla search edip API'yı zorlamayın, gereksiz yere olumsuzluk yaratmayalım ve tokenleri bitirmeyelim.

![image](https://github.com/ruesandora/Timpi/assets/101149671/8369a6cb-0163-4581-bfbe-0b4a6795d0fd)

<h1 align="center"> Sync olmak uzun sürüyorsa </h1>

```
SNAP_NAME=$(curl -s https://ss-t.timpi.nodestake.top/ | egrep -o ">20.*\.tar.lz4" | tr -d ">")
curl -o - -L https://ss-t.timpi.nodestake.top/${SNAP_NAME}  | lz4 -c -d - | tar -x -C $HOME/.TimpiChain
```

> Node'u durdurun ve bu snapshot ile kurun, denemedim ama çalışıyordur.

> Çalışmazsa node kuran kişilerin peerlerini toplar burada peer listesi hazırlarım.

<h1 align="center"> Validatör oluşturma </h1>

> Sync olduğunuzdan eminseniz, bu komutta `moniker` ve `from` flaglarını düzenleyin.

```sh
# sync kontrol, false alırsanız validatör oluşturabilirsiniz
./TimpiChain status 2>&1 | jq .SyncInfo

./TimpiChain tx staking create-validator --amount=1000000utimpiTN --pubkey=$(./TimpiChain tendermint show-validator)  --moniker=rues --chain-id=TimpiChainTN --from cüzdanadresiveyaismi --keyring-backend test --commission-rate="0.10" --commission-max-rate="0.20" --commission-max-change-rate="0.01" --min-self-delegation="1000000" --gas="auto" --gas-prices="0.0025utimpiTN" --gas-adjustment="1.5"
```

> Önemli: Bu projenin testneti dışında hiç bir şeye katılmayacağım.





