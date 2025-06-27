# aztec-service
öncelikle docker durduruyoruz ve siliyoruz
```bash
docker stop id
docker rm id
```

```bash
sudo tee /etc/systemd/system/aztec.service > /dev/null <<EOF
[Unit]
Description=Aztec Node Service
After=network.target

[Service]
User=root
WorkingDirectory=/root
ExecStart=$(which aztec) start --node --archiver --sequencer \
  --network alpha-testnet \
  --l1-rpc-urls RPC_URL \
  --l1-consensus-host-urls BEACON_URL \
  --sequencer.validatorPrivateKey 0xYourPrivateKey \
  --sequencer.coinbase 0xYourAddress \
  --p2p.p2pIp $(wget -qO- eth0.me)
Restart=on-failure
RestartSec=10
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```
