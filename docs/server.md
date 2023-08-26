# Server setup documentation

Here we are going to setup an ubuntu server with ssh keys and UFW as our firewall with a samba fileserver

## Configuration for some simple things

`sudo hostnamectl set-hostname nmdammer.com`

`sudo nano /etc/hosts`

`sudo apt update && sudo apt upgrade -y`

`sudo apt install ufw -y`

## Installing docker and docker-compose

Installing docker first

`curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`

`sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"`

`sudo apt install docker-ce`

Installing docker-compose

`sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`

`sudo chmod +x /usr/local/bin/docker-compose`

## Setting up SSH keys

Generate keypair
`ssh-keygen -t rsa -b 4096 -C "nicolaydammer@gmail.com"`

`sudo mv id_rsa.pub authorized_keys`

`sudo chmod 700 ~/.ssh &&
sudo chmod 600 ~/.ssh/authorized_keys &&
sudo chown $USER:$USER ~/.ssh -R`

add private key to own pc (convert key with puttygen) and ssh-agent

## configuring SSH server

`sudo nano /etc/ssh/sshd_config`

* change port to 6666
* uncomment logingracetime
* uncomment strictmode
* uncomment maxauthtries
* uncomment maxsessions
* uncomment authorizedKeysFile

## open up used ports in the firewall and enable them

`sudo ufw allow 6666 &&
sudo ufw allow 6667 &&
sudo ufw allow 6661 &&
sudo ufw allow 25565 &&
sudo ufw allow http &&
sudo ufw allow https &&
sudo ufw allow allow 53 &&
sudo ufw enable`

## Quick reboot

`sudo reboot`

## Check for working keypair and disable password login
`sudo nano /etc/ssh/sshd_config`

* PasswordAuthentication no
* sudo systemctl restart ssh

## mounting the raid 1 disks
`sudo mkdir /mnt/storage`

`sudo mount /dev/md0 /mnt/storage/`

mounting on startup.

`sudo nano /etc/fstab`

* /dev/md0   /mnt/storage   ext4   defaults   0   0

## making a network share with user authentication
`sudo nano /etc/samba/smb.conf`

```
[global]
admin users = root, smbadmin, nicolay

[storage]
path = /mnt/storage
valid users = @nicolay
browsable = yes
read only = no
writable = yes
guests = no
force user = nicolay
force group = nicolay
create mask = 775
force create mode = 775
security mask = 775
force security mode = 775
directory mask = 2775
force directory mode = 2775
directory security mask = 2775
force directory security mode = 2775
```


add user to smb

`sudo smbpasswd -a nicolay`

`sudo smbpasswd -e nicolay`

`sudo systemctl restart smbd`

`sudo ufw allow Samba`