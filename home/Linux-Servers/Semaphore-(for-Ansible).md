# Semaphore (for Ansible)

# Installing Semaphore

<aside>
💡 Python, Ansible and Git should be installed on your system.

</aside>

**Debian / Ubuntu (x64)Debian / Ubuntu (ARM64)CentOS (x64)CentOS (ARM64)**

```
wget https://github.com/semaphoreui/semaphore/releases/\
download/v2.9.58/semaphore_2.9.44_linux_amd64.deb

sudo dpkg -i semaphore_2.9.44_linux_amd64.deb
```

Setup Semaphore by using the following command:

```
semaphore setup
```

Now you can run Semaphore:

```
semaphore server --config=./config.json
```

Semaphore will be available via this URL [https://localhost:3000](https://localhost:3000/).

# Run Semaphore as a Serivice

If you installed Semaphore via a package manager, or by downloading a binary file, you should create the Semaphore service manually.

Create the systemd service file:

<aside>
💡 Replace `/path/to/semaphore` and `/path/to/config.json` to your semaphore and config file path

</aside>

```bash
sudo cat > /etc/systemd/system/semaphore.service <<EOF
[Unit]
Description=Semaphore Ansible
Documentation=https://github.com/semaphoreui/semaphore
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
ExecReload=/bin/kill -HUP $MAINPID
ExecStart=/path/to/semaphore server --config=/path/to/config.json
SyslogIdentifier=semaphore
Restart=always
RestartSec=10s

[Install]
WantedBy=multi-user.target
EOF
```

Start the Semaphore service:

```bash
sudo systemctl daemon-reload
sudo systemctl start semaphore
```

Check the Semaphore service status:

```bash
sudo systemctl status semaphore
```

To make the Semaphore service auto start:

```bash
sudo systemctl enable semaphore
```