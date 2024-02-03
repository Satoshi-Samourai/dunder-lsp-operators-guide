# **A Guide for Dunder-LSP operators**

If you are a LND node runner and want to commit to scaling the Lightning Network, then this guide is for you.

&nbsp;

### There are four points to keep in mind: 
1) Dunder LSP works only for LND nodes in combination with [Blixt Wallet](https://blixtwallet.github.io/) at the moment.
2) You have to have some solid knowledge of how to use Terminal and Linux, as well as IP networks and proxy servers.
3) I am far from being a developer, so this guide is built solely on my experience of setting up a Dunder LSP Server.
4) I link to guides from experienced Bitcoiners and developers I used myself. But still -> do your own research.

&nbsp;

### What is a Dunder LSP server ?
Who else could put it in better words than [DarthCoin](https://darthcoin.substack.com/) ? Here is his detailled [Dunder LSP Guide](https://darthcoin.substack.com/p/dunder-lsp-and-lightning-box-provider). There is also a short description on the [Blixt Wallet](https://blixtwallet.github.io/) homepage.

&nbsp;

### Where to start ?
Log in to your LND node with Terminal and then head over to [Hampus Sjöberg's](https://github.com/hsjoberg/dunder-lsp) Github Repository to get the code and start building the server. You may need to hop back and forth between his repository and my guide, here.

To check if your Dunder LSP server is up and running on your machine, type ` 127.0.0.1:8080 ` or ` 0.0.0.0:8080 ` in your browser (use the port you gave to your instance). If all is good you will see ` hello, world ` and you are ready for the next step.

&nbsp;

### Admin Dashboard on clearnet
To be able to create an admin and have access to it, you first have to make your Dunder LSP server visible in clearnet. You will need a domain of your choice and a proxy server plus a valid SSL/TLS certificate for your domain.
Without that step you will not be able to create and access your Admin Dashboard. Start with adding your domain by `cd dunder-lsp/config ` and  `nano default.json `
like this:

```
"env": "development",
"serverHost": "127.0.0.1:8080",
"serverDomain": "https://<your Domain>",
```

#### There are several ways you can safely expose your node to clearnet:
1)  Use Caddy or Nginx, if you are familiar with them. This is the easiest way.
2)  Use Cloudflare (creating an account is easy, but setting the tunnel up in terminal is a bit tricky). For this setup you also need to have ` cloudflared ` installed on your machine.
4)  ?? Darth ?? any others ?

&nbsp;

Proceed in Terminal with [Hampus Sjöberg's](https://github.com/hsjoberg/dunder-lsp) guide to create a temporary HTTP server. This will present a QR code you have to scan with your wallet of joice, preferable Blixt. Keep in mind that your Dunder LSP server must be stopped for that process. If you use another wallet that other wallet will be irreversable connected to your Dunder LSP server. The only chance you have is to create another admin with another wallet later in your Admin Dashboard. 

If you have successfully created your /admin you will be able to access your Dashboard in your browser with ` <yourDomain>/admin ` . A QR code will be crated which you have to scan with the same wallet you used to create the admin (Blixt). I recommend Blixt as it makes sense to be in the same workflow as your Dunder LSP server. Be aware that Brave browser has issues (the generated QR code was unresponsive and it took me a while to figure that out). I had to use Safari on Mac. Chrome works fine, too.

&nbsp;

This is what you will see after you typed ` <yourDomain>/admin ` into your browser:

&nbsp;

![dunder_lsp_admin_login_qr](https://github.com/Satoshi-Samourai/dunder-lsp-operators-guide/assets/130285815/fec34e4d-ff42-4184-a1ab-d3a7dd77e697)

&nbsp;

...and after login your Admin Dashboard (in this case from my Dunder LSP server). Nice and slick.

&nbsp;

![dunder-lsp_admin_dsahboard](https://github.com/Satoshi-Samourai/dunder-lsp-operators-guide/assets/130285815/acb1e903-5dd6-4c0c-a9f8-bcf81bc0258c)

&nbsp;

### Setting channel sizes and fees for your services
At the moment, there is no other way to set your fees than in Terminal. You have to ` cd dunder-lsp/config ` and then ` nano default.json `
Here is how I have mine set up for now:

 ```
"minimumPaymentMultiplier": 5,
  "maximumPaymentSat": 80000,
  "fee": {
    "maxSat": 40000,
    "maxSatPerVByte": 100,
    "subsidyFactor": 1
```

Of course, you can set the channel size and fees as you see fit.

#### Here is an explanation what the parameters in the ` default.json ` mean:

#### minimumPaymentMultiplier: 5 ####

This parameter sets a rule for the minimum allowed payment amount in a transaction. The minimum payment amount is calculated by multiplying this multiplier with some base value. For example, if the base value is 10,000 satoshis, then the minimum payment amount would be 5 times that, which is 50,000 satoshis.

#### maximumPaymentSat: 80,000 ####

This parameter establishes the maximum amount of satoshis that can be sent in a single payment or transaction (in this case 80,000 Stats). If a user attempts to send more than this specified amount, the transaction may be rejected or adjusted to comply with this limit. If a user wants to have a 400,000 Sats channel opened, he needs to send the maximum amount of 80,000 Sats to do so. This basically means that channels are restricted to a maximum size of 400,000 Sats each from the Dunder LSP server side.  

&nbsp;

### Fees: ###

#### maxSat: 40,000 ####

This sets the maximum allowable transaction fee in satoshis. Users creating transactions are restricted from setting a fee higher than this value.

#### maxSatPerVByte: 100 ####

This parameter sets the maximum fee rate per virtual byte in satoshis. Transaction fees are often calculated based on the size (in virtual bytes) of the transaction. This parameter imposes an upper limit on the fee rate.

#### subsidyFactor: 1 ####

The subsidy factor is related to how transaction fees are subsidized. The subsidy factor of 1 sets that the on-chain fees are paid by the channel opening peer (user) and not the Dunder LSP server.

&nbsp;

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

Make sure you ` sudo systemctl reload <service-name> ` or ` sudo systemctl daemon-reload ` after saving the file.

&nbsp;

### Congrats, you are now a Dunder LSP !
[DarthCoin](https://darthcoin.substack.com/) made a nice graphic how your node is now connected to the Lightning Netork. You can also see how Blixt users will connect to you if they choose your server.

&nbsp;

![dunder_lsp_node_in_the_LN-Network](https://github.com/Satoshi-Samourai/dunder-lsp-operators-guide/assets/130285815/e2120d38-4e40-427f-a5dc-119b8ff8ce49)

&nbsp;

### Liquidity of your Dunder LSP node
It is absolutely important to keep in mind that you are providing liquidity for others out of your own pocket. You must have enough liquidity on-chain on your LND node.

&nbsp;

### Get Blixt Wallet for your Android / IOS phone
It is a good idea to install [Blixt Wallet](https://blixtwallet.github.io/) on your smartphone to do some testing with your own Dunder LSP server. Go to "Settings" and "Enable Dunder LSP". Then "Set Dunder LSP" to your servers URL and test if you are up and running by "Diagnose Dunder problems" further down in the debug section. If you have not enough funds on your node you will see an error.

&nbsp;

### Anything missing ? Suggestions ? Add-ons ?
Feel free to comment here or get in contact on Telegram [@SatoshiSamourai](https://t.me/SatoshiSamourai) or via email: satoshi_samourai@protonmail.com

&nbsp;

#### If you want to connect to my Dunder LSP node add this URL in Blixt settings as described above:  https://lsp.btc-payserver.eu
