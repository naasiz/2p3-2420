# Setting Up a Reverse Proxy with Nginx and Firewall with UFW on Arch Linux

In this tutorial we will go you through setting up a reverse proxy using Nginx and configuring a firewall using UFW on an Arch Linux server. We will set up a backend service that runs on port 8080 and is accessible via the reverse proxy.


## Installing the Necessary Software

install Nginx and UFW.
`sudo pacman -Syu nginx ufw`


## Placing the Backend Binary on Your System and move it to a suitable folder 

1. we need to transfer the downloaded backup file from the host machine to the linux server 

2. Connect to your droplet through sftp: `sftp -i ~/.ssh/do-key you_username@ip`

3. move the file to your droplet(using put). 

4. SSH back into your droplet and move the file to this path: /usr/local/bin/server3`


## Creating a Service File for the Backend

1. Create a new systemd service file for your backend.

2. Open a vim file

3. Add this to the file:
```bash
[Unit]
Description= Backend Service
After=network.target

[Service]
ExecStart=/usr/local/bin/hello-server
Restart=always

[Install]
WantedBy=multi-user.target
```

Enable and start the service:

`sudo systemctl enable backend`
`sudo systemctl start backend`


## Configuring Nginx for Reverse Proxy

1. Create a new Nginx server block configuration file for your backend.

2. Open the vim file

3. Add the two location routes to the file:

It should look like this now:
```bash
listen 80;
listen [::]:80;

server_name -;
root /web/html/nginx-2420;

location / {
    index  index.html index.htm;
}

location /hey {
    proxy_pass http://127.0.0.1:8080;
}

location /echo {
    proxy_pass http://127.0.0.1:8080;
}
```

4. reload Nginx:
 `systemctl reload nginx`


## Configuring UFW Firewall

1. Allow HTTP and HTTPS traffic through the firewall:
`sudo ufw allow http`
`sudo ufw allow https`


2. Enable UFW:  `sudo ufw enable`

Press y to see this message "Firewall is active and enabled on system startup"


You have successfully set up a reverse proxy with Nginx and configured a firewall using UFW on your Arch Linux server. 
