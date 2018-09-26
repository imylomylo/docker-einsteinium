**tl;dr;** pass in 2 --mount settings.  First, what you would find in .komodo directory.  It can be empty except for komodo.conf like the repo's _node_ dir.  Second zcash-params.

put your chain data in the node directory, alternatively change the --mount settings when you run it, also --mount setting needs zcash-params passed in.

```
git clone https://github.com/imylomylo/docker-einsteinium.git
cd docker-einsteinium/src
make build # to build the docker image
make run # to run it
```
it will drop you into a shell
```
cd
ls -la
einsteiniumd &
tail ~/.einsteinium/debug.log
einsteinium-cli getblockchaininfo
```


**Tips:**
 - The node directory has the blockchain data and config.
 - curl should work, something like `curl --user rpcuser:passworddrowssap --data '{"method": "getinfo"}' http://127.0.0.1:RPCPORT`
