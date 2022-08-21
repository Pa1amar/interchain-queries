```
cd ~
git clone https://github.com/Stride-Labs/interchain-queries.git && cd interchain-queries
go build -a
mv interchain-queries /usr/local/bin/interchain-queries
```
```
cd && mkdir .icq
```
```
echo "default_chain: stride
chains:
  gaia:
    key: wallet
    chain-id: GAIA
    rpc-addr: http://127.0.0.1:26657   # GAIA rpc
    grpc-addr: http://127.0.0.1:90090  # GAIA grpc
    account-prefix: cosmos
    keyring-backend: test
    gas-adjustment: 1.2
    gas-prices: 0.001uatom
    key-directory: /root/.icq/keys
    debug: false
    timeout: 20s
    block-timeout: ""
    output-format: json
    sign-mode: direct
  stride:
    key: wallet
    chain-id: STRIDE-TESTNET-4
    rpc-addr: http://127.0.0.1:26657  # STRIDE rpc 
    grpc-addr: http://127.0.0.1:9090  # STRIDE grpc
    account-prefix: stride
    keyring-backend: test
    gas-adjustment: 1.2
    gas-prices: 0.001ustrd
    key-directory: /root/.icq/keys
    debug: false
    timeout: 20s
    block-timeout: ""
    output-format: json
    sign-mode: direct
cl: {}" > ~/.icq/config.yaml
```

```
icq keys restore --chain stride wallet
icq keys restore --chain gaia wallet
```

```echo "[Unit]
Description=Relayer
After=network.target

[Service]
User=$USER
Type=simple
ExecStart=/usr/local/bin/interchain-queries run
Restart=on-failure
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target" > $HOME/relayerd.service
sudo mv $HOME/relayerd.service /etc/systemd/system
sudo systemctl daemon-reload
sudo systemctl enable relayerd
sudo systemctl restart relayerd && journalctl -u relayerd -f -o cat```

