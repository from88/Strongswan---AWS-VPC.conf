# Strongswan Configuration

```conn net-awssg1
    authby=psk
    ike=aes-sha1-modp1024!
    ikelifetime=28800s
    esp=aes128gcm16-modp1024!
    lifetime=3600s
    type=tunnel
    dpddelay=10s
    dpdtimeout=30s
    keyexchange=ikev2
    reauth=no
    dpdaction=restart
    closeaction=restart
    keyingtries=%forever
    leftsubnet=0.0.0.0/0,::/0
    rightsubnet=0.0.0.0/0,::/0
    compress=no
    mobike=no
    left=localterminationip
    leftid=localterminationip
    right=awsvpcip
    rightid=awsvpcip
    auto=start
    mark=100
 ```

```cat /etc/strongswan/strongswan.d/charon.conf 
install routes = no
```

# tunnels configuration

```
/sbin/ip link add VTI_awssg1 type vti local localterminationip remote awsvpcip okey 100 ikey 100 
/sbin/sysctl -w net.ipv4.conf.VTI_awssg1.disable_policy=1
/sbin/sysctl -w net.ipv4.conf.VTI_awssg1.rp_filter=0
/sbin/ip addr add localaddr/30 remote localaddrpeer/30 dev VTI_awssg1
/sbin/ip link set VTI_awssg1 up mtu 1422
```

# sysctl

```
# Enable IPv4 forwarding
sysctl -w net.ipv4.ip_forward=1
sysctl -w net.ipv4.conf.VTI_awssg1.disable_xfrm=1
sysctl -w net.ipv4.conf.VTI_awssg1.disable_policy=1
```
