# STEAM STREAMING
steam streaming -> steaming

## INTRODUCTION
[When reading this blogpost about running a gaming PC in the cloud](https://lg.io/2015/07/05/revised-and-much-faster-run-your-own-highend-cloud-gaming-service-on-ec2.html)  
I wanted to do that myself, just not on AWS, since I already own a gaming rig.  
I did more-or-less the same setup, connectivity-wise, and am now able to connect with my PC!

This is essentially the same setup as the blogpost, but you'll use the `steaming`  
script instead of `up.sh` and `down.sh`. It will probably work for the AWS setup as well.

## REQUIREMENTS
- server: a windows PC, reachable via port-forwarding
- client: a linux pc (mac might work, if you can get the dependencies)

## SETUP
### Server
Install openvpn on your PC; use the 64 bit version, unless your PC is 32 bits.  
Please install all components

Copy `server.ovpn` from this repo into `C:\Program Files\OpenVPN\config\`

Open a powershell window and run the following commands:
```powershell
cd C:\Program Files\OpenVPN\easy-rsa
init-config
vars
clean-all
build-ca			(leave all answers blank)
build-key-server server	(leave all answers blank except Common Name "server", yes to Sign and yes to Commit)
build-key client		(leave all answers blank except Common Name "client", yes to Sign and yes to Commit)
build-dh
robocopy keys ../config ca.crt dh1024.pem server.crt server.key
```

### Client
then copy to following files from `C:\Program Files\OpenVPN\easy-rsa\keys\`
to `/etc/openvpn/` on your client:
- `ca.crt`
- `client.crt`
- `client.key`

Change the following in `client.ovpn`:
- `MY_HOSTNAME` into the IP or domain name of the router/windows pc

Copy `client.ovpn` from this repository into `/etc/openvpn/`

Open port `1194/UDP` on your router, and point it to your windows PC  
Look up "port forwarding YOUR_ROUTER_MODEL_HERE" if you don't know how to do this

Copy the client script (`steaming`) to `/usr/local/bin/`

Install these tools on the client:
- `route`
- `grep`
- `awk`
- `head`
- `kill`
- `pkill`
- `openvpn`
- `ifconfig`
- `tshark`
- `script`
- `xxd`
- `socat`

For debian/ubuntu:
```sh
sudo apt install net-tools grep coreutils procps openvpn tshark bsdutils vim-common socat
```

## CONNECT
start openvpn server on your PC (tray icon->connect)
you may have to import the configuration for the first time

Run this script  (`sudo steaming --debug`)

## TODO
- [x] add server config
