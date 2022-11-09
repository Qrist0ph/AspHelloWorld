# AspHelloWorld
Asp Test app for deployment and build testing

# Setting up the machine
## Preparing nginx
Setup nginx
```
sudo nano /etc/nginx/sites-available/default
```

Paste
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

Restart nginx
```
sudo systemctl restart nginx
```


## Setting up the Service
```
sudo nano /etc/systemd/system/asphelloworld.service
```

Paste
```
[Unit]
Description=AspHelloWorld
 
[Service]
WorkingDirectory=/root/locator
ExecStart=/home/pi/dotnet/dotnet /home/pi/apps/asphelloworld AspHelloWorldSln.dll
Restart=always
RestartSec=10
SyslogIdentifier=AF
User=root
Environment=ASPNETCORE_ENVIRONMENT=Production
 
[Install]
WantedBy=multi-user.target

```
Restarting the Service
```
systemctl daemon-reload && systemctl restart locator.service && systemctl status locator.service
```

# Building the app
Very first time
```
mkdir  ~/apps && mkdir  ~/apps/asphelloworld/
```

Build
```
cd ~/src/AspHelloWorldSln/AspHelloWorldSln
~/dotnet/dotnet  restore && ~/dotnet/dotnet  publish
rsync -mirror ~/src/AspHelloWorldSln/AspHelloWorldSln/bin/Debug/netcoreapp3.1/publish/ ~/apps/asphelloworld/
```
## manual start
```
cd ~/apps/asphelloworld/ &&  ~/dotnet/dotnet AspHelloWorldSln.dll
```

Goto [app](http://192.168.17.211/).
