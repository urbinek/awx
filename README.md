# awx
Set of Ansible AWX templates for homelab

## RouterOS

### Generate self-signed certs

```RoS
{
    #/certificate/remove ssl-cert
    :local identity [/system identity get name];
    :local cn "$identity.mikrotik.local";
    /certificate add name=ssl-cert common-name=$cn key-size=2048 days-valid=3650;
    :local ip [/ip address get [find interface="vlan-infra"] address]
    :set ip [:pick $ip 0 [:find $ip "/" -1]]
    /certificate sign ssl-cert ca-crl-host="$ip";
    /ip service set www-ssl certificate=ssl-cert disabled=no;
    /ip service set api-ssl certificate=ssl-cert disabled=no;
    /log/info "$ip $cn"
}
```
