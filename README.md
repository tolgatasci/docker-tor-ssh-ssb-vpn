# docker-tor-ssh

Run a tor container that publishes a hidden service that lets you ssh to the underlying docker-machine host or VM parent machine.

# Usage:

Using `docker-compose`, merely:

    docker-compose build tor
    docker-compose up -d tor

The first time you run this, a new hidden service identity will be created.
To show the `.onion` address, restart the service and look at the logs:

    docker-compose restart tor
    docker-compose logs --tail=100 -f tor

For better anonymity, this attemps to use the longer SHA-3/ed25519/curve25519 `.onion` addresses.

# Connecting:

To ssh to the box in question, make sure your client has `tor` running, then use `torify`/`torsocks` to run your ssh client connection:

    torify ssh sshgxqz2vwpbgnmckeybbzsuuozpzdh6nwbcgtk7er6voqxklrhzh7qd.onion

which is effecively doing:

    torsocks ssh sshgxqz2vwpbgnmckeybbzsuuozpzdh6nwbcgtk7er6voqxklrhzh7qd.onion

Alternatively, add some magic to your `~/.ssh/config` to allow you to ssh in with a simple netcat socks5 proxy:

    Host *.onion
      ProxyCommand /usr/bin/nc -xlocalhost:9050 -X5 %h %p

Then you can directly:

    ssh sshgxqz2vwpbgnmckeybbzsuuozpzdh6nwbcgtk7er6voqxklrhzh7qd.onion

If you wish to create your own tor v3 vanity ssh address, you can use (cathugger/mkp224o)[https://github.com/cathugger/mkp224o] to generate your own vanity private/public keys, and replace the autogenerated ones under `/var/lib/tor/ssh/`

If you're doing this though, you're probably defeating the point of having the more secure v3 address to begin with.

