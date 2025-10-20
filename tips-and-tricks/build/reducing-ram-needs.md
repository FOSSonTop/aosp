# Tips and Tricks

## 2. Working on Low-RAM Machines with zram

AOSP builds typically require 32GB+ of RAM.  You can use `zram` (compressed RAM) to build on systems with less physical RAM (e.g., 8GB).

1.  **Load zram Module:**

```bash
sudo modprobe zram
```

2.  **Configure zram:**

```bash
sudo swapoff /dev/zram0  # Only if you have existing zram
sudo zramctl /dev/zram0 -s 48G  # 48GB for 8GB RAM systems, 32GB for 16GB RAM
sudo mkswap /dev/zram0
sudo swapon /dev/zram0
```

Alternatively, use a systemd service:
  
1. **Create systemd service file:**

```bash
sudo tee /etc/systemd/system/swapf.service > /dev/null <<'EOF'
[Unit]
Description=swapf service
After=network-online.target
Wants=network-online.target

[Service]
ExecStart=/usr/bin/bash -c "sudo zramctl /dev/zram0 -s 48G && sudo mkswap /dev/zram0 && sudo swapon /dev/zram0"
WorkingDirectory=/usr/bin
Restart=never

[Install]
WantedBy=multi-user.target
EOF
```

2. **Enable the service:**
```bash
sudo systemctl daemon-reload 
sudo systemctl enable --now swapf.service
```


**Important:**  Using disk swap (a swap file or partition) is *not* recommended. It will be extremely slow. ZRAM is significantly faster because it uses compressed RAM.

