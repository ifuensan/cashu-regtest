![TESTS](https://github.com/lnbits/legend-regtest-enviroment/actions/workflows/ci.yml/badge.svg)

# Nodes

Here is the complete list of nodes:
* lnd-1: for testing your software
* lnd-2: used for boltz backend
* lnd-3: used for lnbits inside docker
* clightning-1: for testing your software
* clightning-2: for testing your software
* clightning-2-rest: used for clightning-REST
* clightning-3: for tenting your software

# Additional services

* mempool (api|web|db): Mempool Explorer
* fulcrum: Electrum Server

# Installing regtest 
Get the regtest environment ready
```sh
# Install docker https://docs.docker.com/engine/install/
# Make sure your user has permission to use docker 'sudo usermod -aG docker ${USER}' then reboot
# Stop/start docker 'sudo systemctl stop docker' 'sudo systemctl start docker'

git clone https://github.com/callebtc/cashu-regtest.git
cd cashu-regtest
./start.sh  # start the regtest and also run tests
```
## Stopping regtest
Stop the regtest environment
```
./stop.sh # stop the regtest containers
```

# Running Nutshell on regtest
Add this ENV variables to your `.env` file (assuming that the `cashu-regtest` directory is in `../` from the `nutshell` directory)
```sh
# LND
MINT_BACKEND_BOLT11_SAT=LndRestWallet
MINT_LND_REST_ENDPOINT=https://localhost:8081
MINT_LND_REST_CERT="../cashu-regtest/data/lnd-3/tls.cert"
MINT_LND_REST_MACAROON="../cashu-regtest/data/lnd-3/data/chain/bitcoin/regtest/admin.macaroon"

# CLN
MINT_BACKEND_BOLT11_SAT=CoreLightningRestWallet
MINT_CORELIGHTNING_REST_URL=https://localhost:3001
MINT_CORELIGHTNING_REST_MACAROON=../cashu-regtest-enviroment/data/clightning-2-rest/access.macaroon
MINT_CORELIGHTNING_REST_CERT=../cashu-regtest-enviroment/data/clightning-2-rest/certificate.pem
```

# Regtest nodes

You can interact with the software running in the container using the `bitcoin-cli-sim`, `lightning-cli-sim` and `lncli-sim` aliases. You can bind these aliases by sourcing the `docker-scripts.sh` file:
```sh
source docker-scripts.sh

# LND
lncli-sim 1 addinvoice <amount> # create an invoice
lncli-sim 1 payinvoice -f <invoice> # pay an invoice

# use bitcoin core, mine a block
bitcoin-cli-sim -generate 1

# use c-lightning nodes
lightning-cli-sim 1 newaddr | jq -r '.bech32' # use node 1
lightning-cli-sim 2 getinfo # use node 2
lightning-cli-sim 3 getinfo # use node 3

# use lnd nodes
lncli-sim 1 newaddr p2wsh
lncli-sim 2 listpeers
```

# URLs

* Mempool Exploer: http://localhost:8080/
* Boltz API: http://localhost:9001/
* lnd-1 rest: http://localhost:8081/
* lnbits: http://localhost:5001/

# Debugging Docker logs
```sh
docker logs cashu-lnbits-1 -f
docker logs cashu-clightning-1-1 -f
docker logs cashu-clightning-2-1 -f
docker logs cashu-clightning-3-1 -f
docker logs cashu-lnd-1-1 -f
docker logs cashu-lnd-2-1 -f
docker logs cashu-lnd-3-1 -f
```
