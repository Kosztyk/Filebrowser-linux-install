# Filebrowser-linux-install

Steps to install Filebrowser linux native

# 1. Install the app
```
curl -fsSL https://raw.githubusercontent.com/filebrowser/get/master/get.sh | bash
filebrowser -r /path/to/your/files  #Change to your root directory here in my case /root
```

a password will be generated for admin user - save it 


# 2. Setup

# a) create /etc/filebrowser.env that will contain:

FILEBROWSER_USERNAME=admin
FILEBROWSER_PASSWORD=password saved at step 1

EXECUTE:
```
sudo tee /etc/filebrowser.env > /dev/null <<EOF
FILEBROWSER_USERNAME=admin
FILEBROWSER_PASSWORD=password saved at step 1
EOF
sudo chmod 600 /etc/filebrowser.env
sudo chown root:root /etc/filebrowser.env
```

# b) create /etc/filebrowser.json that will contain:

{
  "address": "0.0.0.0",
  "port": 8080,
  "database": "/root/filebrowser.db",
  "root": "/mnt"
}

EXECUTE:
```
sudo tee /etc/filebrowser.json > /dev/null <<EOF
{
  "address": "0.0.0.0",
  "port": 8080,
  "database": "/root/filebrowser.db",
  "root": "/mnt"
}
EOF
sudo chown root:root /etc/filebrowser.json
```
# 3. create Configure Systemctl File
```
  nano /etc/systemd/system/filebrowser.service
```
  File will contain:
```
  [Unit]
Description=Filebrowser
After=network-online.target

[Service]
EnvironmentFile=/etc/filebrowser.env
ExecStart=/usr/local/bin/filebrowser \
  -c /etc/filebrowser.json \
  --username ${FILEBROWSER_USERNAME} \
  --password ${FILEBROWSER_PASSWORD}
Restart=on-failure
User=root
Group=root

[Install]
WantedBy=multi-user.target
```
# 4. Enable the service:
```
    sudo systemctl enable filebrowser.service
```
# 5. Start the service:
```
   sudo systemctl start filebrowser.service
```   
# 6. Check the service:
```   
 systemctl status filebrowser.service
 
```
# 7. Access teh service: http://your_server_ip:8080
