# How to setup a Hello World Static Website Using a Virtual Machine from Digital Ocean

## Step1: Create a VM on Digital Ocean


Virtual Machines on Digital Ocean, as you may have guessed it are called droplets. Unline other cloud providers,
the instructions for DropLet creation are much simpler.

1. Choose a region. It should be closest to where your customers are like London, Sydney or Singapore.
2. Choose a datacenter within a region. You can leave it as the default selected.
3. Leave the VPC network as the default
4. Choose an OS for your Virtual Machine. The default is multiple distributions of linux but there are more in their marketplace.
5. Leave the Version of the OS as default or whatever you need.
6. Choose the Droplet type as Basic
7. Chose disk type as SSD
8. Choose the cheapest option for pricing with 4$/month which includes 
   - 512 MB Ram/1 vCPU
   - 10Gb SSD
   - 500Gb Transfer
9. Not needed to add additional volumes for now
10. Choose password for connecting with VM using console. Better option is to go with SSH.
11. Created the root password.
12. Ignored all other extras and pressed Create DropLet.


## Step2: Install nginx

Now navigaete from the left and find Manage-->Droplets page. Click on Access->Launch Droplet Console, from where you will be 
dropped into a console.

Run the following commands to instal nginx first
```
sudo apt update
sudo apt install nginx
```
Verify nginx is running by looking for nginx process using
```
ps -eaf | grep nginx
root        2742       1  0 07:13 ?        00:00:00 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
www-data    2844    2742  0 07:15 ?        00:00:00 nginx: worker process
root        3488    1876  0 08:07 pts/0    00:00:00 grep --color=auto nginx
```

You can also verify as follows :-
```
systemctl status nginx
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; preset: enabled)
     Active: active (running) since Sat 2024-12-28 07:13:12 UTC; 56min ago
 Invocation: c35d05233a0c4b5497619af9f8ffb275
       Docs: man:nginx(8)
    Process: 2739 ExecStartPre=/usr/sbin/nginx -t -q -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
    Process: 2741 ExecStart=/usr/sbin/nginx -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
   Main PID: 2742 (nginx)
      Tasks: 2 (limit: 506)
     Memory: 2.7M (peak: 3.5M)
        CPU: 55ms
     CGroup: /system.slice/nginx.service
             ├─2742 "nginx: master process /usr/sbin/nginx -g daemon on; master_process on;"
             └─2844 "nginx: worker process"
```

Verify curl localhost returns a valid response like this
```
curl localhost
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```
## Step4: Verify VM connectivity from Internet

Verify you can reach this vm from a broswer. In DigitalOcean, my droplet already had a public IP assigned. You can find it in your DropLet tab, listed as ipv4:<ip>. So all i had to 
do was type http://<ip> on my browser and it will show the same output as the curl localhost, but a bit nicer, without the html tags like:-

```
Welcome to Mrinali's nginx!
If you see this page, the nginx web server is successfully installed and working. Further configuration is required.

For online documentation and support please refer to nginx.org.
Commercial support is available at nginx.com.

Thank you for using nginx.
```


## Step5: Customize nginx index.html

Now verify the root for your nginx by looking at the file cat /etc/nginx/sites-enabled/default

In my case it was  root /var/www/html;


Now open the index.html in your editor and modify the welcome message to return something custom.
```
 ls -al /var/www/html/
total 12
drwxr-xr-x 2 root root 4096 Dec 28 07:15 .
drwxr-xr-x 3 root root 4096 Dec 28 07:01 ..
-rw-r--r-- 1 root root  625 Dec 28 07:15 index.nginx-debian.html
```


You need to also restart nginx. You can do that using the command 
```
sudo nginx -s reload
```
Now verify both curl localhost and the browser return your updated index.html as follows:-

```
Welcome to Mrinali's nginx!
If you see this page, the nginx web server is successfully installed and working. Further configuration is required.

For online documentation and support please refer to nginx.org.
Commercial support is available at nginx.com.

Thank you for using nginx.
```

## Step6: Cleanup and destroy the Virtual Machine
Ensure you cleanup the droplet, by first turning it off and then destroying it to prevent charges from incurring.