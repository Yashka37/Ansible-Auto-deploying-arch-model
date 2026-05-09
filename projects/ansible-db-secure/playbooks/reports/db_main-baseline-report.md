# Baseline Audit Report

- Host: db_main
- Generated: 2026-03-23T13:05:34Z

## 1. Service State
- PostgreSQL wrapper service: active
- PostgreSQL cluster service: active
- auditd: active
- pg_isready:
192.168.56.20:5432 - accepting connections

## 2. SSH Hardening
- SSH drop-in exists: True

## 3. Password Policy
- pwquality baseline exists: True

## 4. Firewall
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22                         ALLOW IN    Anywhere                  
22/tcp                     ALLOW IN    Anywhere                  
5432/tcp                   ALLOW IN    192.168.56.10             
5432/tcp                   ALLOW IN    192.168.56.30             
22 (v6)                    ALLOW IN    Anywhere (v6)             
22/tcp (v6)                ALLOW IN    Anywhere (v6)             

## 5. Runtime sysctl
- kernel.kptr_restrict: 2
- kernel.yama.ptrace_scope: 3
- fs.protected_symlinks: 1

## 6. Audit
- audit rules file exists: True
- active audit rules:
-w /etc/ssh/sshd_config -p wa -k ssh_config
-w /etc/ssh/sshd_config.d/99-ansible-hardening.conf -p wa -k ssh_config
-w /etc/sudoers -p wa -k privileged-actions
-w /etc/sudoers.d -p wa -k privileged-actions
-w /etc/passwd -p wa -k identity
-w /etc/group -p wa -k identity
-w /etc/shadow -p wa -k identity

## 7. PostgreSQL
- App role exists: 1
- App database exists: 1
- listen_addresses:
192.168.56.20
- log_connections:
on

## 8. pg_hba baseline
- HBA marker block present: True
# Managed by Ansible

# local access
local   all             postgres                                peer
local   all             all                                     peer

# loopback
host    all             all             127.0.0.1/32            scram-sha-256
host    all             all             ::1/128                 scram-sha-256

# application access from allowed clients
# BEGIN ANSIBLE APP ACCESS
host    appdb    appuser    192.168.56.10/32    scram-sha-256
host    appdb    appuser    192.168.56.30/32    scram-sha-256
# END ANSIBLE APP ACCESS

## Conclusion
This report reflects the current configuration state of the host against the selected lab baseline.
