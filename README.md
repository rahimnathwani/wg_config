This is a simple wireguard VPN user management script using on VPN server.
Client config file and qrcode are generated.

### usage
Run as `root`

### Install dependencies (wireguard and qrencode)
```bash
echo "deb http://deb.debian.org/debian/ unstable main" > /etc/apt/sources.list.d/unstable.list
printf 'Package: *\nPin: release a=unstable\nPin-Priority: 90\n' > /etc/apt/preferences.d/limit-unstable                       apt update
apt install wireguard
apt-get install qrencode
```

### config
```bash
touch /etc/wireguard/wg0.conf
git clone https://github.com/rahimnathwani/wg_config.git
cd wg_config
cp wg.def.sample wg.def
wg genkey | tee -a wg.def | wg pubkey >> wg.def
echo _SERVER_PRIVATE_KEY=`wg genkey | tee server_private_key` >> wg.def
echo _SERVER_PUBLIC_KEY=`cat server_private_key|wg pubkey` >> wg.def
nano wg.def
```
Put the server's public IP address in wg.def
#### start wireguard

```bash
systemctl enable wg-quick@wg0.service
systemctl stop wg-quick@wg0.service
systemctl start wg-quick@wg0.service
```

#### add a user

```bash
./user.sh -a alice
```

This will generate a client conf and qrcode in current directory whose name is alice
and add alice to the wg config.

#### delete a user

```bash
./user.sh -d alice
```
This will delete the alice directory and delete alice from the wg config.

#### view a user

```bash
./user.sh -v alice
```
This will show generated QR codes.

#### clear all

```bash
./user.sh -c
```
