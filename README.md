# **A Guide for Dunder-LSP operators**

If you are a LND node runner and want to commit to scaling the Lightning Network, this guide is for you.

### There are four points to keep in mind: 
1) Dunder LSP works only for LND nodes in combination with [Blixt Wallet](https://blixtwallet.github.io/) at the moment.
2) You have to have some solid knowledge of how to use Terminal and Linux, as well as IP networks and proxy servers.
3) I am far from being a developer, so this guide is built solely on my experience setting a Dunder LSP Server up.
4) I link to guides from experienced Bitcoiners and developers I used myself. But still -> do your own research.


### What is a Dunder LSP server ?
Who else could put it in better words than DarthCoin ? Here is his detailled [description](https://darthcoin.substack.com/p/dunder-lsp-and-lightning-box-provider). And there is also a short description on the [Blixt Wallet](https://blixtwallet.github.io/) homepage.

### Where to start
Log in to your node with Terminal and then head over to [Hampus Sjöberg's](https://github.com/hsjoberg/dunder-lsp) Github Repository to get the code and start building the server.

### Admin Dashboard on clearnet
You have to make your Dunder LSP server visible in clearnet with a domain of your choice and a proxy server plus a valid SSL/TLS certificate for it.
Without that step you will not be able to access your Admin Dashboard. Start with adding your domain in cd dunder-lsp/config and nano default.json
like this:

"env": "development",
  "serverHost": "127.0.0.1:8080",
  "serverDomain": "https://<your Domain>",
  

#### There are several ways you can do so:
1)  Use Caddy or Nginx, if you are familiar with them. This is the easiest way.
2)  Use Cloudflare (creating an account is easy if you don't have one). For this setup you also need to have cloudflared installed on your machine.
4)  ?? Darth ?? any others ?

### Setting channel sizes and fees for your services
At the moment, there is no other way to set your fees than in Terminal. You have to cd dunder-lsp/config and then nano default.json
Here is how I have mine set up for the moment:

 "minimumPaymentMultiplier": 5,
  "maximumPaymentSat": 410000,
  "fee": {
    "maxSat": 40000,
    "maxSatPerVByte": 100,
    "subsidyFactor": 1

Of course you can set the channel size and fees as you see fit.

### Get Blixt Wallet for your Android / IOS phone
It is a good idea to install [Blixt Wallet](https://blixtwallet.github.io/) on your smartphone to do some testing with your own Dunder LSP server.

 
