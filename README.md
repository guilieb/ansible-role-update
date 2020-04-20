# `ansible-role-update`: update system and configure automatic updates

Update the system (all) and install an automatic package updater (`yum-cron` or `dnf-automatic`) depending on the target OS.

### Role Variables

If the target system is CentOS 7 or RHEL 7, `yum-cron` will be installed, configuration below:

```yml
yum_automatic:
  base:
    debuglevel: -2
    mdpolicy: 'group:main'
  commands:
    apply_updates: false
    download_updates: true
    random_sleep: 0
    update_cmd: default
    update_messages: true
  email:
    email_from: root@localhost
    email_host: localhost
    email_to: root
  emitters:
    emit_via: stdio
    output_width: 80
    system_name: None
  groups:
    group_list: None
    package_types:
      - mandatory
      - default
```

If the target system is CentOS or RHEL > 7 or Fedora, `dnf-automatic` will be used instead, configuration below:
```yaml
dnf_automatic:
  commands:
    apply_updates: false
    download_updates: false
    random_sleep: 0
    upgrade_type: default
  command:
    command_format: cat
    stdin_format: '"{body}"'
  command_email:
    command_format: '"mail -s {subject} -r {email_from} {email_to}"'
    stdin_format: '"{body}"'
    email_from: root
    email_to: root
  emitters:
    emit_via: stdio
    system_name: None
  email:
    email_from: root
    email_host: localhost
    email_to: root
```

**Note**: to only override the parameters you wish, please see the [`hash_behaviour`](https://docs.ansible.com/ansible/2.3/intro_configuration.html#hash-behaviour) settings.

### Example playbook

```yml
---
- hosts: localhost
  become: true
  connection: local

  vars:
    dnf_automatic:
      commands:
        download_updates: true
      command_email:
        email_to: test@example.com

  roles:
    - guilieb.update
```

### Author Information

- [Guillaume Bernard](https://www.guillaume-bernard.fr)
