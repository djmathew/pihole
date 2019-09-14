# pihole
Docker commands to run pihole as a container

#### Commands:
```bash
# Pull the latest pihole image
docker pull pihole/pihole

# Create directory on host to save data
mkdir -p /mnt/pihole/pihole
mkdir -p /mnt/pihole/dnsmasq.d
chown 775 -R /mnt/pihole

# Command to start pihole
docker run -d \
  --name pihole \
  -p 53:53/tcp \
  -p 53:53/udp \
  -p 67:67/udp \
  -p 80:80 \
  -p 43:443 \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Asia/Dubai \
  -v /mnt/pihole/pihole/:/etc/pihole/ \
  -v /mnt/pihole/dnsmasq.d/:/etc/dnsmasq.d/ \
  -e 127.0.0.1 \
  -e 1.1.1.1 \
  --restart=unless-stopped \
  pihole/pihole:latest

# Changes when ports and mount points are different
docker run -d \
  --name pihole \
  -p 53:53/tcp \
  -p 53:53/udp \
  -p 67:67/udp \
  -p 8080:80 \
  -p 8443:443 \
  -e PUID=1000 \
  -e PGID=1000 \
  -e TZ=Asia/Dubai \
  -v /mnt/omv/pihole/pihole/:/etc/pihole/ \
  -v /mnt/omv/pihole/dnsmasq.d/:/etc/dnsmasq.d/ \
  -e 127.0.0.1 \
  -e 1.1.1.1 \
  --restart=unless-stopped \
  pihole/pihole:latest


# By default password is set automatically unless specified by a '-e WEBPASSWORD:<secure-password>' option, so, to retrieve it
docker logs pihole 2> /dev/null | grep 'password'

# To reset the password
docker exec -it pihole pihole -a -p 
```

### Now force clients to use the new DNS by changing the Router Primary DNS to ip address of pihole
For DLINK - Settings > Internet > ivp4
