
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
