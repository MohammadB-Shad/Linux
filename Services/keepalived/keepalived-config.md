# Keepalived Configuration Synopsis

> ⚠️ This file is based on the official Keepalived documentation.  
> Source: [Keepalived Documentation](https://keepalived.readthedocs.io/en/latest/configuration_synopsis.html)

---

## 🧩 Global Definitions

```
conf
global_defs {
    notification_email {
        admin@example.com
    }
    notification_email_from keepalived@example.com
    smtp_server 127.0.0.1
    smtp_connect_timeout 30
    lvs_id LVS_DEVEL
}
```
- Keyword Descriptions:
```
| Keyword                   | Description                                         | Type    |
| ------------------------- | --------------------------------------------------- | ------- |
| `global_defs`             | Block that contains global settings                 | Block   |
| `notification_email`      | List of email addresses to receive notifications    | List    |
| `notification_email_from` | Email address used in the "MAIL FROM:" SMTP command | String  |
| `smtp_server`             | SMTP server used to send notification emails        | String  |
| `smtp_connect_timeout`    | SMTP connection timeout in seconds                  | Integer |
| `lvs_id`                  | Identifier for the LVS director                     | String  |
```

# Virtual Server Definitions
```
virtual_server 192.168.1.100 80 {
    delay_loop 6
    lb_algo rr
    lb_kind NAT
    persistence_timeout 50
    protocol TCP

    sorry_server 192.168.1.254 80

    real_server 192.168.1.101 80 {
        weight 1
        TCP_CHECK {
            connect_port 80
            connect_timeout 3
        }
    }

    real_server 192.168.1.102 80 {
        weight 2
        HTTP_GET {
            url {
                path /health
                digest 9e107d9d372bb6826bd81d3542a419d6
            }
            connect_port 80
            connect_timeout 3
            retry 3
            delay_before_retry 2
        }
    }
}
```
- Keyword Descriptions:
| Keyword               | Description                                            | Type    |
| --------------------- | ------------------------------------------------------ | ------- |
| `virtual_server`      | Defines a virtual server block                         | Block   |
| `delay_loop`          | Interval between health checks in seconds              | Integer |
| `lb_algo`             | Load balancing algorithm (`rr`, `wrr`, `lc`, etc.)     | String  |
| `lb_kind`             | Load balancing mode (`NAT`, `DR`, `TUN`)               | String  |
| `persistence_timeout` | Persistence timeout in seconds                         | Integer |
| `protocol`            | Protocol used (`TCP` or `UDP`)                         | String  |
| `sorry_server`        | Fallback server if all real servers are down           | Address |
| `real_server`         | Defines a real server                                  | Block   |
| `weight`              | Load balancing weight for the real server              | Integer |
| `TCP_CHECK`           | TCP-based health check                                 | Block   |
| `HTTP_GET`            | HTTP GET-based health check                            | Block   |
| `url`                 | URL path to request for health checking                | Block   |
| `path`                | Path for the HTTP GET request                          | String  |
| `digest`              | MD5 hash of the expected HTTP response                 | String  |
| `connect_port`        | Port to connect for the health check                   | Integer |
| `connect_timeout`     | Timeout for connection attempts in seconds             | Integer |
| `retry`               | Number of retries before marking the server as down    | Integer |
| `delay_before_retry`  | Delay before retrying a failed health check in seconds | Integer |

# VRRP Instance Example
```
vrrp_instance VI_1 {
    state MASTER
    interface eth0
    virtual_router_id 51
    priority 100
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1111
    }
    virtual_ipaddress {
        192.168.1.100
    }
}
```
- Keyword Descriptions:
| Keyword             | Description                            | Type    |
| ------------------- | -------------------------------------- | ------- |
| `vrrp_instance`     | Defines a VRRP instance block          | Block   |
| `state`             | Initial state (`MASTER` or `BACKUP`)   | String  |
| `interface`         | Network interface used by the instance | String  |
| `virtual_router_id` | VRID to identify the virtual router    | Integer |
| `priority`          | Priority of the VRRP instance          | Integer |
| `advert_int`        | Advertisement interval in seconds      | Integer |
| `authentication`    | Authentication settings block          | Block   |
| `auth_type`         | Authentication type (`PASS`)           | String  |
| `auth_pass`         | Password used for authentication       | String  |
| `virtual_ipaddress` | List of virtual IP addresses           | List    |


