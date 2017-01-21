# devsetup

Setup a development environment for apps around Bitcoin using VirtualBox and Vagrant - includes Rein client and server, bitcoind from ppa with pruning and testnet, JoinMarket develop branch.

# Installation

Install Vagrant from https://www.vagrantup.com/

Install VirtualBox from https://www.virtualbox.org/wiki/VirtualBox

Optionally fork, then clone this repo:

    git clone https://github.com/ReinProject/devsetup.git

Start your development environment:

    vagrant up --provider virtualbox

This will need to download the Ubuntu 16.04 box, then will provision the VM with:

  * Bitcoin Core via Ubuntu PPA
  * Rein's Causeway server from https://github.com/ReinProject/causeway
  * Python-rein from https://github.com/ReinProject/python-rein

Bitcoin Core is then configured to use the testnet and pruning to 2gb. The Causeway server
is auto-configured to talk to that Bitcoin Core instance via RPC.

# Running Rein

Each of the following need to run in a shell which you can invoke from your host machine with
`vagrant ssh` or by opening Terminator inside the VM. The password for the vagrant user is `vagrant`.
You may want to install screen to make the former option more robust.

Run Bitcoin Core:

    bitcoind &

Run Causeway:

    . c-venv/bin/activate
    python cserver.py

Run python-rein:

    . pr-venv/bin/activate
    rein start    

# Running JoinMarket

JoinMarket is a set of tools to make CoinJoins with others who are also running the software. This JoinMarket environment uses regtest. Since devsetup was built for Rein, it has testnet=1 in the default bitcoin.conf. JoinMarket has different needs so you have to do the following to run the JM tests.

      comment out testnet=1 line in /home/vagrant/.bitcoin/bitcoin.conf
      . envjm/bin/activate
      cd joinmarket
      make test
