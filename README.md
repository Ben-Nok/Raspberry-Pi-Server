# Raspberry-Pi-Server

A simple Server running following servies:

- Nginx
- PHP-FPM (ver. 8.1)
- MariaDB
- Adminer
- A FTP Server

For configuration check the docker-compose.yml.\
Configuration files for the webserver and ftp server are in the docker-ftp-server and docker-webserver folders.

# Usage

Use `docker compose up` to start the docker services and `docker compose down` to stop the docker services.

If you change one of the config files, you may have to use docker `compose up --build`

# Installing and configuration of Bind9


1. Installing bind9: \
`sudo apt install bind9`

2. Update named.conf and named.conf.options in /etc/bind/

3. create the zone file and reverse zone file.

4. Add `supersede domain-name-servers <static-server-ip>;` to the /etc/dhcp/dhclientconf

5. rastart the service with `sudo systemctl restart named`

# Adding Portainer to the server:

1. create a Docker Volume for portainer: \
`docker volume create portainer_data` 

2. Start the Container:\
`docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest` 

More Information: https://docs.portainer.io/

# Adding Netdata to the Server

Use following command to build the container, note that no volumes are created.\
Using Portainer is recommended.\
`docker run -d --name=netdata -p YOUR_HOST_PORT:19999 --cap-add SYS_PTRACE --security-opt apparmor=unconfined netdata/netdata`

More Information: https://learn.netdata.cloud/docs/installing/docker

# Installing Samba

1. Install Samba:\
 `sudo nano /etc/samba/smb.conf`

2. Setup Users:\
`sudo adduser samba1`\
`sudo adduser samba2`\
Followed by:
`sudo smbpasswd -a samba1`\
`sudo smbpasswd -a samba2`

3. Create public directory and set rights:\
`sudo mkdir /path/to/public`\
`sudo chmod -R 777 /path/to/public`

4. add following to the smb.conf to setup the public directory:\
[public]\
  comment = Public Download Directory\
  path = /samba/public\
  browseable = yes\
  writeable = yes\
  create mask = 0664\
  directory mask = 0775\

5. restart samba:\
`sudo service smbd restart`
