# Raspberry Pi Nodejs Web Server w/ PM2

> This guide assumes you have already completed the first boot configuration
> of your raspberry pi and are logged in via SSH or terminal in X.

1. Create a safe user account for serving the nodejs app
```
sudo useradd -s /bin/bash -m -d /home/myuser -c "safe myuser account" myuser