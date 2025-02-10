# 🚀 PIPE Devnet2 Node Setup  

This guide explains how to install and configure a **PIPE Devnet2** node.  

📌 **Official Guide:** [Pipe Network Documentation](https://docs.pipe.network/devnet-2)  
📊 **Node Dashboard:** [Pipe Network Dashboard](https://dashboard.pipenetwork.com/node-lookup)  

---

## ⚡ Installation Steps  

### **1️⃣ Update System Packages**  
Run the following command to update the system:  

```bash
sudo apt update && sudo apt upgrade -y
```

### **2️⃣ Open Required Ports**  
Allow necessary ports through the firewall:  

```bash
sudo ufw allow 8003/tcp
```

### **3️⃣ Create Required Directories**  
```bash
mkdir -p $HOME/download_cache
sudo mkdir -p $HOME/pipe
```

### **4️⃣ Download & Authorize the POP Binary**  
```bash
POP="PASTE_THE_LINK_FROM_YOUR_EMAIL"
cd $HOME/pipe
wget -O pop "$POP"
chmod +x pop
```

### **5️⃣ Create the Service File**  
Modify the service file with your **pubKey**, RAM, and disk limits:  

```bash
sudo tee /etc/systemd/system/popd.service > /dev/null << EOF
[Unit]
Description=Pipe POP Node Service
After=network.target
Wants=network-online.target

[Service]
User=$USER
ExecStart=$(echo $HOME)/pipe/pop --ram=12 --pubKey YOUR_PUBKEY --max-disk 175 --cache-dir $(echo $HOME)/download_cache
Restart=always
RestartSec=5
LimitNOFILE=65536
LimitNPROC=4096
StandardOutput=journal
StandardError=journal
SyslogIdentifier=dcdn-node
WorkingDirectory=$HOME/pipe

[Install]
WantedBy=multi-user.target
EOF
```

### **6️⃣ Set Permissions**  
```bash
sudo chown -R $USER:$USER $HOME/pipe
sudo chown -R $USER:$USER $HOME/download_cache
```

### **7️⃣ Start the Service**  
```bash
sudo systemctl daemon-reload
sudo systemctl enable popd
sudo systemctl start popd
```

### **8️⃣ Check Service Status**  
```bash
sudo systemctl status popd
```

### **9️⃣ Monitor Logs**  
```bash
sudo journalctl -u popd -fo cat
```

---

This completes the PIPE Devnet2 node installation. 🚀  
