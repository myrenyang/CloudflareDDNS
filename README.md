# Synology Cloudflare DDNS Script 📜

The is a script to be used to add [Cloudflare](https://www.cloudflare.com/) as a DDNS to [Synology](https://www.synology.com/) NAS. The script used an updated API, Cloudflare API v4.

## How to use `dsmCloudflareDdnsModule.sh`

### Access Synology via SSH

1. Login to your DSM
2. Go to Control Panel > Terminal & SNMP > Enable SSH service
3. Use your client to access Synology via SSH.
4. Use your Synology admin account to connect.

### Run commands in Synology

1. Download `dsmCloudflareDdnsModule.sh` from this repository to `/sbin/cloudflareddns.sh`, or any folder as you like

```
wget https://raw.githubusercontent.com/myrenyang/SynologyDsmDdns/master/dsmCloudflareDdnsModule.sh -O /sbin/cloudflareddns.sh
```

2. Make it executable in `/sbin/cloudflareddns.sh`

```
chmod +x /sbin/cloudflareddns.sh
```

If you put the script file in another folder, just make a link

```
ln -s /whatever-path-of-the-folder/cloudflareddns.sh /sbin/cloudflareddns.sh
```

3. Add `cloudflareddns.sh` to Synology

Append following config to `/etc.defaults/ddns_provider.conf`.

```
[Cloudflare]
        modulepath=/sbin/cloudflareddns.sh
        queryurl=https://www.cloudflare.com
        website=https://dash.cloudflare.com
```

You can use VI editor or following commands
```
echo "[Cloudflare]" >> /etc.defaults/ddns_provider.conf
echo "        modulepath=/sbin/cloudflareddns.sh" >> /etc.defaults/ddns_provider.conf
echo "        queryurl=https://www.cloudflare.com" >> /etc.defaults/ddns_provider.conf
echo "        website=https://dash.cloudflare.com" >> /etc.defaults/ddns_provider.conf
echo "" >> /etc.defaults/ddns_provider.conf
```

Note, `queryurl` does not matter because we are going to use our script but it is needed.


## Setup DDNS

1. Login to your DSM
2. Go to Control Panel > External Access > DDNS > Add
3. Enter the following:
   - Service provider: `Cloudflare`
   - Hostname: `www.example.com`
   - Username/Email: `example.com`
   - Password Key: `<password>`


## How to use `cloudflareDdnsWorker.js`

### Get Cloudflare parameters

1. Go to your domain overview page and copy your zone ID.
2. Go to your profile > **API Tokens** > **Create Token**. It should have the permissions of `Zone > DNS > Edit`. Copy the api token.