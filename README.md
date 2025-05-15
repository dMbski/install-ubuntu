# Installation of Ubuntu

## Download

### Desktop
> https://ubuntu.com/download/desktop

### Server
> https://ubuntu.com/download/server#manual-install

# Create boot usb (need >=16GB)
> https://etcher.balena.io/#download-etcher
or with
> https://rufus.ie/


# 
# Add APT repository
> https://github.com/webmin/webmin/blob/master/webmin-setup-repo.sh

## repository script
```
sudo curl -o setup-repos.sh https://raw.githubusercontent.com/webmin/webmin/master/setup-repos.sh
sudo bash setup-repos.sh
```
## install webmin from apt
> sudo apt install --install-recommends webmin -y

## check if is it running
> sudo systemctl status webmin

... connect to http://localhost:10000

### open connectivity to webmin
```
sudo ufw allow 10000
sudo ufw allow https > this opens 443 port
sudo ufw reload
sudo ufw status
```
