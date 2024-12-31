#

## Backend

1. Install dependencies
```
sudo apt-get update
```
```
sudo apt install -y git curl mysql-server unzip
```

2. Setup MySQL
- Connect via command line
```
sudo mysql
```
- Setup database
```
CREATE DATABASE `nomor-surat`;
```
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'root';
```

3. Setup NVM
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
```
```
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
```
```
nvm install 22
```

4. Clone Repo
```
git clone https://github.com/kerja-praktik-bapenda-jatim/nomor-surat-be ~/bapenda/be
```
```
cd bapenda/be && nano config/config.json
```
Add ```JWT_SECRET``` and ```PORT```
```
nano .env.development
```
```
curl -L -o data.zip "https://drive.google.com/uc?export=download&id=1ywEKUhXWtfh7M4MtaTAYiY9MOP9F6-La"
```
```
unzip data.zip
```
```
npm install
```

5. Seed database
```
npx sequelize-cli db:seed:all
```

6. Create Service
```
which node
```
```/etc/systemd/system/nomor-surat-be.service```
```
[Unit]
Description=Penomoran Surat Backend
After=network.target

[Service]
ExecStart=/path/to/node server.js
Restart=always
Environment=NODE_ENV=production
WorkingDirectory=/path/to/backend/repository
```

7. Enable and start service
```
sudo systemctl enable nomor-surat-be.service
```
```
sudo systemctl start nomor-surat-be.service
```
Check the log:
```
sudo journalctl -u nomor-surat-be.service -f
```

8. Test
check ip
```
curl ifconfig.me
```
```
curl http://<ip>:<port>/api
```


## Frontend
1. Clone Repo
```
cd ~
```
```
git clone https://github.com/kerja-praktik-bapenda-jatim/nomor-surat-fe ~/bapenda/fe
```
```
sudo bapenda/fe && npm install
```
change ```API_BASE_URL``` to the ip
```
nano next.config.js
```
change the start command to ```next start -H 0.0.0.0 -p 80```
```
nano package.json
```
Build
```
npm run build
```

2. Create Service
```
which node
```
```/etc/systemd/system/nomor-surat-be.service```
```
[Unit]
Description=Penomoran Surat Frontend
After=network.target

[Service]
Environment="PATH=/path/to/node/bin:/usr/bin:/bin"
ExecStart=/path/to/npm run start
Restart=always
WorkingDirectory=/path/to/frontend/repository

[Install]
WantedBy=multi-user.target

```

3. Enable and start service
```
sudo systemctl enable nomor-surat-fe.service
```
```
sudo systemctl start nomor-surat-fe.service
```
