# Keepalived Case Study - Healthcheck

> ⚠️ This content is based on the official Keepalived documentation.  
> Source: [Keepalived Case Study - Healthcheck](https://keepalived.readthedocs.io/en/latest/case_study_healthcheck.html)

---

## 🧩 Health Check Overview

In this case study, we walk through configuring Keepalived for a health check setup.

### Health Check Architecture

```text
This case study involves the setup of Keepalived in a high availability architecture where multiple web servers are monitored for health.
```

---

## 🔹 Main Architecture Components

The main components of the architecture in this case study include:

1. **Virtual Server**: The entry point for users.
2. **Real Servers**: The backend servers serving the actual content.
3. **Keepalived**: The software that manages the health checks and load balancing.

### Components Overview

| Component          | Description                                                |
|--------------------|------------------------------------------------------------|
| Virtual Server     | The IP address that clients access.                        |
| Real Servers       | Servers that provide the actual content for the clients.   |
| Keepalived         | Software that monitors the health of the real servers.     |

---

## 🔹 Server Pool Specifications

In this case study, the pool of servers consists of:

- **Server 1**: `192.168.1.101` with TCP health check.
- **Server 2**: `192.168.1.102` with HTTP-based health check.

Each server is monitored for availability and only healthy servers receive traffic.

---

## 🔹 Keepalived Configuration

The following is the configuration of Keepalived for this architecture:

```conf
global_defs {
    notification_email {
        admin@example.com
    }
    notification_email_from keepalived@example.com
    smtp_server 127.0.0.1
    smtp_connect_timeout 30
    lvs_id LVS_DEVEL
}

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

### Configuration Breakdown:

| Keyword              | Description                                                    | Type     |
|----------------------|----------------------------------------------------------------|----------|
| `virtual_server`     | Defines a virtual server block                                 | Block    |
| `delay_loop`         | Interval between health checks in seconds                      | Integer  |
| `lb_algo`            | Load balancing algorithm (`rr`, `wrr`, `lc`, etc.)             | String   |
| `lb_kind`            | Load balancing mode (`NAT`, `DR`, `TUN`)                       | String   |
| `persistence_timeout`| Persistence timeout in seconds                                 | Integer  |
| `protocol`           | Protocol used (`TCP` or `UDP`)                                 | String   |
| `sorry_server`       | Fallback server if all real servers are down                   | Address  |
| `real_server`        | Defines a real server                                          | Block    |
| `weight`             | Load balancing weight for the real server                      | Integer  |
| `TCP_CHECK`          | TCP-based health check                                         | Block    |
| `HTTP_GET`           | HTTP GET-based health check                                    | Block    |
| `url`                | URL path to request for health checking                        | Block    |
| `path`               | Path for the HTTP GET request                                  | String   |
| `digest`             | MD5 hash of the expected HTTP response                         | String   |
| `connect_port`       | Port to connect for the health check                           | Integer  |
| `connect_timeout`    | Timeout for connection attempts in seconds                     | Integer  |
| `retry`              | Number of retries before marking the server as down            | Integer  |
| `delay_before_retry` | Delay before retrying a failed health check in seconds         | Integer  |

---

## 📌 Notes

- **TCP_CHECK** and **HTTP_GET** are commonly used health check methods.
- The `delay_loop` keyword determines how frequently Keepalived checks the health of real servers.
- The `retry` parameter defines the number of retries before a server is considered down.

---

For the most up-to-date case study reference, visit:  
👉 [https://keepalived.readthedocs.io/en/latest/case_study_healthcheck.html](https://keepalived.readthedocs.io/en/latest/case_study_healthcheck.html)
