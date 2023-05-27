# Timpi


sudo apt update
sudo apt upgrade -y

sudo apt-get install git curl build-essential make jq gcc snapd chrony lz4 tmux unzip bc -y

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
go version

cd $HOME

rm -rf Timpi-ChainTN

git clone https://github.com/Timpi-official/Timpi-ChainTN.git

cd Timpi-ChainTN

cd cmd/TimpiChain

go build
