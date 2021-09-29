# aspects_auditd
Install and configure AuditD.

# Requirements

Set ```hash_behaviour=merge``` in your ansible.cfg file.

# Role Variables
## aspects_auditd_enabled
If `False`, do not run tasks in this role.

If `True`, run tasks in this role.

# Dependencies
## aspects_packages
[aspects_packages](https://github.com/LaneCommunityCollege/aspects_packages) is used to install `cronie-anacron` on OracleLinux 6. The Vagrant box I used while testing did not have any crontab utility installed.

If you don't want to use `aspects_packages`, just set `aspects_packages_enabled: False`.

## aspects_auditd_override_auditd_conf
Whether, or not, to override the `/etc/audit/auditd.conf` file.

Default: `False`

## aspects_auditd_override_auditd_conf_values
The values with which to override the `/etc/audit/auditd.conf` file.

Default: `{}`

```yaml 
aspects_auditd_override_auditd_conf_values:
 itema:
   enabled: True
   block: |
     key = value
     key = value
     key = value
```

## aspects_auditd_override_audit_rules
Whether, or not, to override the `/etc/audit/audit.rules` file.

Default: `False`

## aspects_auditd_override_audit_rules_values
The values with which to override the `/etc/audit/audit.rules` file.

Default: `{}`

```yaml 
aspects_auditd_override_audit_rules_values:
 itema:
   enabled: True
   block: |
     - D
     # comment
```

## aspects_auditd_override_audit_stop_rules
Whether, or not, to override the `/etc/audit/auditd.conf` file.

Default: `False`

## aspects_auditd_override_audit_stop_rules_values
The values with which to override the `/etc/audit/audit-stop.rules` file.

Default: `{}`

```yaml 
aspects_auditd_override_audit_stop_rules_values:
 itema:
   enabled: True
   block: |
     - D
     # comment
```
## aspects_auditd_rules_d_99_custom_values
The values that go into the `/etc/audit/rules.d/99-custom.rules` file.

Default: 
```yaml
aspects_auditd_rules_d_99_custom_values:
  0000topcomment:
    enabled: True
    block: |
      # This file is where you can story custom
      # auditd rules.
```


# Example Playbook

```yaml
- hosts:
    - aspects_auditd
  vars:
    ansible_become: True
    aspects_packages_enabled: True
    aspects_auditd_enabled: True
    aspects_auditd_rules_d_99_custom_values:
      0001itema:
        enabled: True
        block: |
          ## First rule - delete all
          -D
          ## Increase the buffers to survive stress events.
          ## Make this bigger for busy systems
          -b 8192
          ## This determine how long to wait in burst of events
          --backlog_wait_time 0
          ## Set failure mode to syslog
          -f 1
  roles:
    - aspects_auditd
```

# License

MIT
