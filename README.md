# **A Guide for Dunder-LSP operators**

If you are a LND node runner and want to commit to scaling the Lightning Network, then this guide is for you.

### There are four points to keep in mind: 
1) Dunder LSP works only for LND nodes in combination with [Blixt Wallet](https://blixtwallet.github.io/) at the moment.
2) You have to have some solid knowledge of how to use Terminal and Linux, as well as IP networks and proxy servers.
3) I am far from being a developer, so this guide is built solely on my experience setting a Dunder LSP Server up.
4) I link to guides from experienced Bitcoiners and developers I used myself. But still -> do your own research.


### What is a Dunder LSP server ?
Who else could put it in better words than [DarthCoin](https://darthcoin.substack.com/) ? Here is his detailled [Dunder LSP Guide](https://darthcoin.substack.com/p/dunder-lsp-and-lightning-box-provider). And there is also a short description on the [Blixt Wallet](https://blixtwallet.github.io/) homepage.

### Where to start ?
Log in to your LND node with Terminal and then head over to [Hampus Sjöberg's](https://github.com/hsjoberg/dunder-lsp) Github Repository to get the code and start building the server. You may need to hop back and forth between his repository and my guide, here.

To check if your Dunder LSP server is up and running on your machine, type ` 127.0.0.1:8080 ` or ` 0.0.0.0:8080 ` in your browser. If all is good you will see ` hello, world ` and you are ready for the next step.

### Admin Dashboard on clearnet
To be able to create an admin and have access to it, you first have to make your Dunder LSP server visible in clearnet. You will need a domain of your choice and a proxy server plus a valid SSL/TLS certificate for your domain.
Without that step you will not be able to access your Admin Dashboard. Start with adding your domain by `cd dunder-lsp/config ` and  `nano default.json `
like this:

```"env": "development",
"serverHost": "127.0.0.1:8080",
"serverDomain": "https://<your Domain>",
```

#### There are several ways you can safely expose your node to clearnet:
1)  Use Caddy or Nginx, if you are familiar with them. This is the easiest way.
2)  Use Cloudflare (creating an account is easy, but setting the tunnel up in terminal is a bit tricky). For this setup you also need to have ` cloudflared ` installed on your machine.
4)  ?? Darth ?? any others ?

Proceed in Terminal with [Hampus Sjöberg's](https://github.com/hsjoberg/dunder-lsp) to create a temporary HTTP server. This will present a QR code you have to scan with your wallet of joice, preferable Blixt. Keep in mind that your Dunder LSP server must be stopped for that process. If you use another wallet that other wallet will be irreversable connected to your Dunder LSP server.  

If you have successfully created your /admin you will be able to access it in your browser with ` <yourDomain>/admin ` . A QR code will be crated which you have to scan with your LNURL-auth compatible wallet (Blixt). I recommend Blixt for that, as it makes sense to be in the same workflow as your Dunder LSP server. Be aware that Brave browser has issues (the generated QR code was unresponsive and it took me a while to figure that out). I had to use Safari on Mac. Chrome works fine, too.

This is what you will see after you typed ` <yourDomain>/admin ` into your browser:

![dunder_lsp_admin_login_qr](https://github.com/Satoshi-Samourai/dunder-lsp-operators-guide/assets/130285815/fec34e4d-ff42-4184-a1ab-d3a7dd77e697)

...and here your Admin Dashboard (in this case from my Dunder LSP server). Nice and slick.

![dunder-lsp_admin_dsahboard](https://github.com/Satoshi-Samourai/dunder-lsp-operators-guide/assets/130285815/acb1e903-5dd6-4c0c-a9f8-bcf81bc0258c)


### Setting channel sizes and fees for your services
At the moment, there is no other way to set your fees than in Terminal. You have to ` cd dunder-lsp/config ` and then ` nano default.json `
Here is how I have mine set up at the moment:

 ```"minimumPaymentMultiplier": 5,
  "maximumPaymentSat": 410000,
  "fee": {
    "maxSat": 40000,
    "maxSatPerVByte": 100,
    "subsidyFactor": 1
```

Of course you can set the channel size and fees as you see fit.

?? Darth ?? HERE SHOULD BE AN EXPLANATION ABOUT HOW TOSE PARAMETERS ARE WORKING.

### Keep your Dunder LSP Server running
It is absolutely necessary to keep your Dunder LSP server up and running at any time if you want to provide a reliable service for other Lightning Network users. Create a ` systemd ` ` dunder.service ` file to do so:

` cd /etc/systemd/system ` and then ` nano dunder.service ` Here is how i set up mine. You can copy paste it and fill in your personal data.


```[Unit]
Description=Dunder LSP

[Service]
WorkingDirectory=/<your path to>/<your path to>/dunder-lsp/
ExecStart=npm start
Type=simple
KillMode=control-group
TimeoutSec=180
Restart=always
RestartSec=60
User=<your username>

[Install]
WantedBy=multi-user.target
```

### Get Blixt Wallet for your Android / IOS phone
It is anyways a good idea to install [Blixt Wallet](https://blixtwallet.github.io/) on your smartphone to do some testing with your own Dunder LSP server.

 
### Anything missing ? Suggestions ? Add-ons ?
Feel free to comment here or get in contact on Telegram [@SatoshiSamourai](https://t.me/SatoshiSamourai) or via email: satoshi_samourai@protonmail.com
