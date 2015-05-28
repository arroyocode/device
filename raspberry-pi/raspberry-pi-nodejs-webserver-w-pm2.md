# Raspberry Pi Nodejs Web Server w/ PM2

Turn your Raspberry Pi into a development nodejs server configured to auto start your node
app, watch and reload your node app when files change automatically.

> PLEASE NOTE: This guide assumes you have already completed the first boot configuration
> of your raspberry pi and are logged in via SSH or terminal in X and are performing each
> step below in the order they're written.
  
##### Create a safe user account for serving the nodejs app
```
sudo useradd -s /bin/bash -m -d /home/myuser -c "safe myuser account" myuser
```

##### Set password for safe user account (you will be prompted twice to enter new password)
```
sudo passwd myuser
```

##### Give safe user account permission to root level commands
```
sudo usermod -aG sudo myuser
```

##### Login as the safe user account
```
ssh myuser@192.168.xxx.xxx (replace with ip address of your pi)
```

##### Download and install latest nodejs ARM package
```
wget http://node-arm.herokuapp.com/node_latest_armhf.deb
sudo dpkg -i node_latest_armhf.deb
```

##### Verify node installation and version
```
node -v
```

##### Give safe user account permission to use port 80
```
sudo apt-get install libcap2-bin
sudo setcap cap_net_bind_service=+ep /usr/local/bin/node
```

##### Install PM2 NPM package
```
sudo npm install -g pm2
```

##### Create basic node app.js file (file in /home/myuser)
```
nano app.js
```
Add javascript syntax, press ctl-x to exit and Y to save
```
var http = require('http');
var server = http.createServer(function (request, response) {
  response.writeHead(200, {"Content-Type": "text/plain"});
  response.end("Hello via nodejs running on Rasbian!\n");
});
server.listen(80);
console.log("Server running at http://127.0.0.1:80/");
```

##### Load new node app, view in browser
```
node app.js
```
View the app in a browser (i.e.: http://localhost, http://127.0.0.1)
and press ctl-c to close the node app.

##### Configure node app to run as a service
```
sudo env PATH=$PATH:/usr/local/bin pm2 startup -u myuser
pm2 save
```

##### Start the node app with PM2
```
pm2 start app.js --watch
This will preserve last known state of the node app and add it to PM2's
list of apps for later auto load. The --watch flag ensures the app is
automatically restarted when files are changed.

##### IMPORTANT bug correction
You must edit the `/etc/init.d/pm2-init.sh` script and replace the `/root`
with `/home/myuser` (or whateve account you've configured) to correctly
configure the auto loading of your node app on Pi reboot.
```
export PM2_HOME="/root/.pm2"
```
should be changed to
```
export PM2_HOME="/home/myuser/.pm2"
```

###### Reboot and enjoy
Reboot the raspberry pi and browse to it's localhost to see the new node app!
