# zabbix-client-installation-on-ubuntu-server
zabbix client

# Zabbix Agent Installation & Configuration Guide (Ubuntu 24.04)

Install Zabbix repository
```

 wget https://repo.zabbix.com/zabbix/5.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_5.0+ubuntu24.04_all.deb
 dpkg -i zabbix-release_latest_5.0+ubuntu24.04_all.deb
 apt update
```
Install Zabbix Agent
Install the Zabbix Agent package:
```
apt install zabbix-agent -y
```
Start and Enable Zabbix Agent

Start the agent immediately and enable it to auto-start on boot:
```
 systemctl restart zabbix-agent
systemctl enable zabbix-agent
```
Check the status:
```
systemctl status zabbix-agent
```
Configure Zabbix Agent

Edit the agent configuration file:
```
sudo nano /etc/zabbix/zabbix_agentd.conf
```
Update the following lines (replace placeholders with your actual values):
```
# Zabbix Server IP (allow passive checks)
Server=ZABBIX_SERVER_IP

# Zabbix Server IP (allow active checks)
ServerActive=ZABBIX_SERVER_IP

# Hostname of this machine (should match what you configure in Zabbix frontend)
Hostname=NAME_OF_THIS_SERVER

# Optional metadata (useful for auto-registration)
HostMetadata=Linux
```
Restart the agent to apply changes:
```
sudo systemctl restart zabbix-agent

```
Firewall Configuration (if enabled)

Allow Zabbix Agent traffic (default port 10050/tcp):
```
sudo ufw allow 10050/tcp
sudo ufw reload
```
Verify Installation
a) Check agent service status
```
systemctl status zabbix-agent
```
Test connectivity from Zabbix Server

Run this command on the Zabbix Server to test agent connectivity:
```
zabbix_get -s CLIENT_IP_ADDRESS -k agent.ping
```
Expected output:
```
1
```
c) Check logs on the client
```
tail -f /var/log/zabbix/zabbix_agentd.log

```
Configure in Zabbix Frontend

Log in to the Zabbix Web Interface.

Go to Configuration → Hosts → Create host.

Enter:

Hostname = NAME_OF_THIS_SERVER (must match the agent config).

Groups = Select appropriate group (e.g., Linux servers).

Agent interfaces = Enter client IP.

Assign templates (e.g., Linux by Zabbix agent).

Save.

