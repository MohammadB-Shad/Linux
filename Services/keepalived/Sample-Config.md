# Setup and configure Keepalived
We have 2 nginx Servers and we want setup keepalived.
We need healthcheck script in both Servers.
```
vim /opt/healthcheck.sh
```
```
#!/bin/bash

# Define the URL to check
URL="http://localhost/"

# Make an HTTP GET request and store the response
RESPONSE=$(curl -s -o /dev/null -w "%{http_code}" "$URL")

# Check the HTTP response code
if [ "$RESPONSE" == "200" ]; then
    # Server is healthy
    logger "its ok"
    exit 0
else
    # Server is unhealthy
    logger "DOWN"
    exit 1
fi
```

## In Server 1 (MASTER) with More priority No.
### Keepalived Config MASTER server (Priority 101)
vim /etc/keepalived/keepalived.conf
```
vrrp_script chk_http_port {
    script "/opt/healthcheck.sh"
    interval 5   # Check every 5 seconds
    fall 2       # Number of consecutive failures before considering the server as down
    rise 2       # Number of consecutive successes before considering the server as up
}


vrrp_instance VI_1 {
    state MASTER
    interface ens160
    virtual_router_id 51
    priority 101
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass mypassword
    }
    virtual_ipaddress {
        192.168.4.100
    }
    track_script {
        chk_http_port
    }
}
```
