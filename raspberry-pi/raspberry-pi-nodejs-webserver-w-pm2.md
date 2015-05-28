# Raspberry Pi Nodejs Web Server w/ PM2

Turn your Raspberry Pi into a development nodejs server configured to auto start your node
app, watch and reload your node app when files change automatically.

> PLEASE NOTE: This guide assumes you have already completed the first boot configuration
> of your raspberry pi and are logged in via SSH or terminal in X.

1. Create a safe user account for serving the nodejs app
```
sudo useradd -s /bin/bash -m -d /home/myuser -c "safe myuser account" myuser
```

2. Set password for safe user account (you will be prompted twice to enter new password)
```
sudo passwd myuser
```

3. Give safe user account permission to root level commands
```
sudo usermod -aG sudo myuser
```

4. Login as the safe user account
```
ssh myuser@192.168.xxx.xxx (replace with ip address of your pi)
```

5. Download and install latest nodejs ARM package
```
wget http://node-arm.herokuapp.com/node_latest_armhf.deb
sudo kpkg -i node_latest_armhf.deb
```