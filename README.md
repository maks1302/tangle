#copy command and Change yourName_Telemetry bellow
```
yourname=<name>
```
```
sudo systemctl stop tangle
```
```
sudo systemctl disable tangle
```
```
cd $HOME
```
```
rm -rf /etc/systemd/system/tangle.service
```
```
rm -rf $HOME/.tangle/data/chains/tangle-testnet/db
```
```
tee /etc/systemd/system/tangle.service > /dev/null << EOF
[Unit]
Description=Tangle Validator Node
After=network-online.target
StartLimitIntervalSec=0
[Service]
User=$USER
Restart=always
RestartSec=3
LimitNOFILE=65535
ExecStart=/usr/bin/tangle \
  --base-path $HOME/.tangle/data/ \
  --name '$yourname' \
  --chain tangle-testnet \
  --node-key-file "$HOME/.tangle/node-key" \
  --port 30333 \
  --rpc-port 9933 \
  --prometheus-port 9615 \
  --auto-insert-keys \
  --validator \
  --pruning archive
  --telemetry-url "wss://telemetry.polkadot.io/submit/ 1" \
  --no-mdns
[Install]
WantedBy=multi-user.target
EOF
```
```
sudo systemctl daemon-reload
```
```
sudo systemctl enable tangle
```
```
sudo systemctl restart tangle && sudo journalctl -u tangle -f -o cat
```
