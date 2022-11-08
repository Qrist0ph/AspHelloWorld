# AspHelloWorld
Asp Test app for deployment and build testing

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

## Building the app
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
