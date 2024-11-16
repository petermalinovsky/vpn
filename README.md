# Wireguard VPN with Dynamic DNS

This docker compose file creates a VPN server that uses a DYNU Dynamic DNS client to allow for a stable connection environment when self hosted.

The services can be started with the command `docker compose up` given that Docker is already installed.

## Environment Variables
This compose file requires 3 varaibles to be defined:
```
DYN_HOSTNAME
DYN_USER
DYN_PASS
```
The hostname is used by both Wireguard and Dynu, whereas the username and password are just used by Dynu. 

I use Portainer to run my Docker stacks, so I define my environment variables in the UI, if you're running directly from the terminal you can create a `.env` file with the values.

## Wireguard
The Wireguard container hosts the VPN server and comes form `wg-easy` which exposes ports `51820` for UDP traffic and `51821` for TCP traffic. I have it set up to use `192.168.0.3` as a local DNS resolver which I run AdGuard on, if you don't have a local DNS resolver this should be commented out.

## Dynu DNS
The Dynu container updates the IP address associated with the hostname if it changes because of either a power loss or a new DHCP lease from your internet provider. This allows the Wireguard instance to reference a constant hostname that can be distributed to each client and allow for the server's IP address to change.
