# Highly Available Pihole utilizing keepalived

We will see if this works
- Three VM's across three separate nodes in Proxmox VE
- Three keepalived nodes across three separate nodes in Proxmox VE
- Each keepalived server will have all three real Pihole Servers in their load balance configuration

> Example block here

/etc/keepalive/keepalived.conf file:
```
vrrp_instance GeeknetDNS {
        state MASTER
        interface eth0
        virtual_router_id 10
        priority 100
        advert_int 1
        authentication {
                auth_type PASS
                auth_pass passw123
        }
        virtual_ipaddress {
        192.168.201.53
        }
}

virtual_server 192.168.201.53 53 {
        delay_loop 10
        lb_algo rr
        lb_kind NAT
        persistence_timeout 9600
        protocol UDP

        real_server 192.168.100.23 {
                weight 1
        }
}
```
