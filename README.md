# Ogar4
An open source Agar.io server implementation, written in Node.js. Forked from [Ogar3](https://github.com/Faris90/Ogar3)

## Why fork?
Ogar3 works great, but it has some security flaws that make it unable to function in HTTPS. Most notably, it uses WebSocket (ws) instead of Secure WebSocket (wss), which most modern browsers will not allow to run on an HTTPS site. Furthermore, this fork also hopes to make a more polished version of the Ogar3, which has missing files, broken links, and worst of all, php. While this fork may occasionally update from upstream, this difference in philosophy will probably lead it to detach from the parent project altogether. For more information about Ogar3, please see the link above.

## A Note on HTTPS: Reverse Proxying
It is possible to run the Node server as a `screen` process, detach, and set up [nginx](https://nginx.org) to serve as a lightweight webserver as a reverse proxy to NodeJS. What this means is that one could have their server running behind a secure, internal-only port (3000 is the default for this project) and use nginx to accept connections on port 80 (HTTP) and 443 (HTTPS) that internally manages connections with NodeJS. The benefits of this include not having to worry (mostly) about what is exposed on NodeJS and making it easy to distinguish between external and internal API functions. Furthermore, there's no need to go through the hassle of setting up website solely based on NodeJS nor to manage certificates and private keys in JavaScript.

Setting up this project behind a web server is beyond the scope of this tutorial, but note that if you do so, you should be mindful about your proxy; on nginx, your config may look something like this:

```
listen 443

location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
}
```

Anonther hassle of php is that it must be managed in the nginx config. For me, I used fastcgi with the following config to deal with them until they are eventually removed:

```
index index.html index.php

location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass localhost:9000;
        include fastcgi_params;
}
```

Your setup might require your own unique tweaks/hacks like mine I had to use.

## Installation process
Installing is simply a matter of cloning the repo and installing npm packages.

```sh
git clone https://github.com/ben9583/Ogar4 Ogar4
cd Ogar4
npm install
npm start
```

You may also wish to have the server as a background process so you can disconnect from your terminal safely with it running:

```sh
touch start-server.sh
echo "screen -dmS ogar npm start" >> start-server.sh
chmod 700 start-server.sh

./start-server.sh
```

## Configuring your server
As on Ogar3, Ogar4 can be modified using the `gameserver.ini` file located on the root of the project. This is where you should change configs for your server, map, players, bots, and rules.

## Have an issue?
Although this is a fork of Ogar3, since this project is expected to seperate eventually, you should submit any issues you have to this project's [issues](https://github.com/ben9583/Ogar4/issues) section.


