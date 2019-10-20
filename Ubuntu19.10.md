Ubuntu19.10:

Configure network:

`sudo vim /etc/netplan/50-cloud-init.yaml`

add contents below to the file:

```yaml
network:
        version: 2
        renderer: networkd
        ethernets:
                enp0s3:
                        dhcp4: yes
```

Save and quit. 

Run `sudo netplan try`, nework now should works fine, if everything is ok, runing `sudo netplan apply` will apply the configuration above persistant.