# Dockerized OpenVPN server

## Starting the docker container

    $ docker compose up -d

Then stop the container:

    $ docker compose down

For a quick documentation see:
https://github.com/kylemanna/docker-openvpn/blob/master/docs/docker-compose.md

## Creating Configuration

Best is always to create a new config from scratch. As root remove all templates and backups in the bind volume:

    # rm openvpn-data/conf/openvpn.conf
    # rm openvpn-data/conf/ovpn_env.sh

Create server config anew as non root:

    $ docker compose run --rm openvpn ovpn_genconfig -d -N -u udp://<server-ip> -p "route <subnet> <mask>" -n 1.1.1.1 -n 8.8.8.8

The -p indicates that the client gets a push with a specific split tunnel route for an IP or a subnet.

F.I for a subnet:

    -p "route 142.250.74.0 255.255.255.0"

or

    -p "route 142.250.74.3 255.255.255.255"

for a specific IP.

## Adding a client

    $ docker compose run -rm openvpn easyrsa build-client-full CLIENTNAME

Get the client configuration and certificates:

    $ docker compose run -rm openvpn ovpn_getclient CLIENTNAME > CLIENTNAME.ovpn

## Deployment

No deployment available.
