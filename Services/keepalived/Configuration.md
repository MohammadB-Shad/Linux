# Keepalived Configuration Synopsis

This document summarizes the configuration options available in Keepalived.

> 📖 **Source**: [Keepalived Official Documentation](https://keepalived.readthedocs.io/en/latest/configuration_synopsis.html)

---

## Global Definitions

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

