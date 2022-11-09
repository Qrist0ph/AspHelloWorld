# AspHelloWorld
Asp Test app for deployment and build testing

# Setting up the machine
## Preparing nginx
### Option 1: As nginx default site
```
sudo nano /etc/nginx/sites-available/default
```

**Paste**
```
server {
         location /  {
                        proxy_set_header Host            $host;
                        proxy_set_header  X-Real-IP          $remote_addr;
                        proxy_set_header  X-Forwarded-For    $proxy_add_x_forwarded_for;
                        proxy_pass http://localhost:5000;
                        proxy_http_version 1.1;
                        proxy_set_header Connection "";
                }
}

```

**Restart nginx**
```
sudo systemctl restart nginx
```

### Option 2: As different site
```
sudo nano /etc/nginx/sites-available/asphelloworld.yaico.de
```
**Paste**
```
server {

        server_name asphelloworld.yaico.de;
        location /  {
                        proxy_set_header Host            $host;
                        proxy_set_header  X-Real-IP          $remote_addr;
                        proxy_set_header  X-Forwarded-For    $proxy_add_x_forwarded_for;
                        proxy_pass http://localhost:5000;
                        proxy_http_version 1.1;
                        proxy_set_header Connection "";
                }
}

```
**Restart nginx**
```
sudo systemctl restart nginx
```
## Setting up the Service
```
sudo nano /etc/systemd/system/asphelloworld.service
```

**Paste**
```
[Unit]
Description=AspHelloWorld
 
[Service]
WorkingDirectory=/home/pi/apps/asphelloworld
ExecStart=/home/pi/dotnet/dotnet /home/pi/apps/asphelloworld/AspHelloWorldSln.dll
Restart=always
RestartSec=10
SyslogIdentifier=AF
User=root
Environment=ASPNETCORE_ENVIRONMENT=Production
 
[Install]
WantedBy=multi-user.target

```
**Restarting the Service**
```
sudo systemctl daemon-reload && sudo systemctl enable asphelloworld.service && sudo systemctl restart asphelloworld.service && sudo systemctl status asphelloworld.service
```

## Other initial tasks
```
mkdir  ~/apps && mkdir  ~/apps/asphelloworld/
```
# Copy src code from dev machine
```
WinSCP.com /ini=nul /script=D:\repos\github\AspHelloWorld\copysrc.txt
```
See WinSCP [Script](https://github.com/Qrist0ph/AspHelloWorld/blob/main/copysrc.txt).
# Building the app


**Build**
```
cd ~/src/AspHelloWorldSln/AspHelloWorldSln
~/dotnet/dotnet restore && ~/dotnet/dotnet publish
rsync -mirror ~/src/AspHelloWorldSln/AspHelloWorldSln/bin/Debug/netcoreapp3.1/publish/ ~/apps/asphelloworld/
```
## manual start
```
cd ~/apps/asphelloworld/ &&  ~/dotnet/dotnet AspHelloWorldSln.dll
```

Goto [app](http://192.168.17.211/).
