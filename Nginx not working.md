Sometimes Nginx just randomly stops working and you are wondering what the hell happened. One of the main issues that causes this issue is if appache is running in the same server.

To check if Apache is running use:

```sudo service apache2 status```

and if you get the following it means appache is running:
```
● apache2.service - The Apache HTTP Server
     Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor prese>
     Active: active (running) since Wed 2024-05-15 13:11:36 IST; 14min ago
       Docs: https://httpd.apache.org/docs/2.4/
   Main PID: 771 (apache2)
      Tasks: 55 (limit: 4647)
     Memory: 8.5M
        CPU: 122ms
     CGroup: /system.slice/apache2.service
             ├─771 /usr/sbin/apache2 -k start
             ├─774 /usr/sbin/apache2 -k start
             └─775 /usr/sbin/apache2 -k start

May 15 13:11:36 skipper apachectl[751]: AH00558: apache2: Could not reliably de>
May 15 13:11:36 skipper systemd[1]: Starting The Apache HTTP Server...
May 15 13:11:36 skipper systemd[1]: Started The Apache HTTP Server.
```

you can stop the Apache2 service by using:

```sudo service apache2 stop```

and the restart nginx:

```sudo service nginx restart```

if you are facing issues during nginx restart you can do. Note that this will reset the Nginx:

```bench setup nginx```

In my case I lost SSL certificate of my site. I just setup the certificate again and changed the site name in "config/nginx.conf".

If this is not the case for you, you can try this solution: https://github.com/frappe/bench/wiki/Regenerate-Production-Config-Files-%28ERPNext%29
