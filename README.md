# Stratum Server for LiteCoinCash Solo Mining

### OS Compatibility note.

This implementation has been tested in Ubuntu 16.04.01. It is a headless implementation of Mike Ghen's Litecoin Solo Mining Tutorial (https://github.com/mikeghen/litecoin-solo-mining-tutorial) and Matthew Little's Highly Efficient Stratum Server for Pools (https://github.com/zone117x/node-stratum-pool).

## Step 1: Installing LiteCoinCash Core

You will need to install and configure LiteCoinCash Core on your server. You will need to be running litecoincashd (LiteCoinCash Daemon) as well. Needless to say the LitecoinCash Daemon needs to run 24/7/365.

Create your litecoincash.conf file. After you are done with the .conf file, start the daemon with the ```-server``` switch.

```
rpcuser=rpcuser
rpcpassword=rpcpassword
rpcport=9332
daemon=1
server=1
gen=0
```

## Step 2: Creating your Stratum Server (aka Mining Pool)

1. Install Node.js, NPM, and NVM:

```
sudo apt-get install nodejs-legacy npm
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.6/install.sh | bash
```
After running `curl` to install NVM, you should see in your output something like:
```
=> Appending nvm source string to /home/ubuntu/.bashrc
```
To use NVM, you'll first need to restart your terminal or reload `.bashrc`:
```
source /home/<your username>/.bashrc
```
We want to use Node.js 0.10 so run:
```
nvm install v0.10.25
```

2. Install.

```
git clone https://github.com/qbanguije/lcc-solo.git lcc-solo
cd lcc-solo
git clone https://github.com/qbanguije/stratum-pool.git
cp stratum-pool/package.json package.json
npm install
mv stratum-pool node_modules/
```

4. Edit the server.js file to suit your configuration:

5. Run the server:

```
node server.js
```

If the server was started successfully, you will start getting log messages on the command line. If the blockchain hasbn't finished synching, you will see percentage messages:
```
error: [POOL] No rewardRecipients have been setup which means no fees will be taken
error: [POOL] Daemon is still syncing with network (download blockchain) - server will be started once synced
warning: [POOL] Downloaded 95.50% of blockchain from 8 peers
warning: [POOL] Downloaded 95.51% of blockchain from 8 peers
warning: [POOL] Downloaded 95.53% of blockchain from 8 peers
warning: [POOL] Downloaded 95.56% of blockchain from 8 peers
warning: [POOL] Downloaded 95.59% of blockchain from 8 peers
warning: [POOL] Downloaded 95.62% of blockchain from 8 peers
```

Eventually, you will see this log:
```
error: [No rewardRecipients have been setup which means no fees will be taken] undefined
special: [Stratum Pool Server Started for LitecoinCash [LCC] {sha256}
                                                Network Connected:      Mainnet
                                                Detected Reward Type:   POW
                                                Current Block Height:   1397736
                                                Current Connect Peers:  8
                                                Current Block Diff:     1521093148.9278824
                                                Network Difficulty:     1562215867.679909
                                                Network Hash Rate:      39.43 PH
                                                Stratum Port(s):        3256, 3333
                                                Pool Fee Percent:       0%
                                                Block polling every:    1000 ms] undefined
```
And then:
```
debug: [POOL] Block notification via RPC polling
debug: [POOL] No new blocks for 55 seconds - updating transactions & rebroadcasting work
debug: [POOL] Block notification via RPC polling
```
Then you're all set!

## Step 5: Connect your rigs

At this point, your pool is all set and you can start mining. Connect your miners to your pool as:

```
stratum+tcp://EXTERNAL_IP:3333
```

Where the EXTERNAL_IP is the external IP of your machine.

Read the `server.js` file for more information about how to change pool settings.
