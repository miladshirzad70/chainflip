# chainflip
chainflipe node
sudo apt update && sudo apt upgrade -y

apt install ufw -y 
ufw allow ssh 
ufw allow https 
ufw allow http 
ufw allow 30333
ufw allow 8078
ufw enable

sudo mkdir -p /etc/apt/keyrings
curl -fsSL repo.chainflip.io/keys/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/chainflip.gpg

gpg --show-keys /etc/apt/keyrings/chainflip.gpg

echo "deb [signed-by=/etc/apt/keyrings/chainflip.gpg] https://repo.chainflip.io/perseverance/ focal main" | sudo tee /etc/apt/sources.list.d/chainflip.list

sudo apt-get update
sudo apt-get install -y chainflip-cli chainflip-node chainflip-engine

sudo mkdir /etc/chainflip/keys

echo -n "xxxxxxxxxxxxxx" |  sudo tee /etc/chainflip/keys/ethereum_key_file

chainflip-node key generate   copy all to notepad

SECRET_SEED=xxxxxxxxxxxx

echo -n "${SECRET_SEED:2}" | sudo tee /etc/chainflip/keys/signing_key_file

sudo chainflip-node key generate-node-key --file /etc/chainflip/keys/node_key_file

cat /etc/chainflip/keys/node_key_file

sudo chmod 600 /etc/chainflip/keys/ethereum_key_file
sudo chmod 600 /etc/chainflip/keys/signing_key_file
sudo chmod 600 /etc/chainflip/keys/node_key_file
history -c


sudo mkdir -p /etc/chainflip/config
sudo nano /etc/chainflip/config/Default.toml


# Default configurations for the CFE
[node_p2p]
node_key_file = "/etc/chainflip/keys/node_key_file"
ip_address="82.115.21.18"
port = "8078"

[state_chain]
ws_endpoint = "ws://127.0.0.1:9944"
signing_key_file = "/etc/chainflip/keys/signing_key_file"

[eth]
# Ethereum RPC endpoints (websocket and http for redundancy).
ws_node_endpoint = "wss://eth-goerli.g.alchemy.com/v2/k1vo7Ptp8pPSxoE2hQhfZqgIXCsPDKVQ"
http_node_endpoint = "https://eth-goerli.g.alchemy.com/v2/k1vo7Ptp8pPSxoE2hQhfZqgIXCsPDKVQ"

# Ethereum private key file path. This file should contain a hex-encoded private key.
private_key_file = "/etc/chainflip/keys/ethereum_key_file"

[signing]
db_file = "/etc/chainflip/data.db"


sudo systemctl start chainflip-node

sudo systemctl status chainflip-node

tail -f /var/log/chainflip-node.log

tail -f /var/log/chainflip-node.log

sudo systemctl start chainflip-engine

sudo systemctl status chainflip-engine

sudo systemctl enable chainflip-node

sudo systemctl enable chainflip-engine

tail -f /var/log/chainflip-engine.log

logrotate

sudo nano /etc/logrotate.d/chainflip

/var/log/chainflip-*.log {
  rotate 7
  daily
  dateext
  dateformat -%Y-%m-%d
  missingok
  notifempty
  copytruncate
  nocompress
}

sudo chmod 644 /etc/logrotate.d/chainflip
sudo chown root.root /etc/logrotate.d/chainflip

Bidding & Staking

sudo systemctl restart chainflip-engine

tail -f /var/log/chainflip-engine.log

sudo chainflip-cli \
      --config-path /etc/chainflip/config/Default.toml \
      register-account-role Validator

sudo chainflip-cli \
    --config-path /etc/chainflip/config/Default.toml \
    activate

sudo chainflip-cli \
    --config-path /etc/chainflip/config/Default.toml rotate


sudo chainflip-cli \
    --config-path /etc/chainflip/config/Default.toml \
    vanity-name xxxxxxxxxxxx
