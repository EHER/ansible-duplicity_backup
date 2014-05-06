# EHER.duplicity_backup
A role to install, configure and schedule backups via duplicity and duplicity_backup script.

## variables

- eher_duplicity_backup_destination - Url to where the backup will be stored in [duplicity format](http://duplicity.nongnu.org/duplicity.1.html#sect8)
- eher_duplicity_backup_root - The commom root path of your backup to be ignored into archives
- eher_duplicity_backup_include_list - A list of paths to be included into backup
- eher_duplicity_backup_exclude_list - A list of paths to be excluded from backup
- eher_duplicity_backup_ftp_password - Password to be used if you will store into ftp
- eher_duplicity_backup_time - Time when the backup will run in [special_time format](http://docs.ansible.com/cron_module.html)
- eher_duplicity_backup_encryption - Enable backup encryption (yes/no)
- eher_duplicity_backup_passphrase - A passphrase to be used into encryption

## how it works

Just add a role with a list of paths to backup and destinations details (s3 on the example)
```yml
---
# site.yml
- hosts: duplicity_backup
  sudo: yes
  tags: duplicity_backup
  roles:
    - role: EHER.duplicity_backup
      eher_duplicity_backup_include_list: [ "/home/" ]
      eher_duplicity_backup_destination: "s3://s3.amazonaws.com/my_backup_bucket/"
      eher_duplicity_backup_aws_key: "{{ backup_aws_key }}"
      eher_duplicity_backup_aws_secret: "{{ backup_aws_secret }}"
```

A more complex example with all variables
```yml
---
# site.yml
- hosts: duplicity_backup
  sudo: yes
  tags: duplicity_backup
  roles:
    - role: EHER.duplicity_backup
      eher_duplicity_backup_root: /home
      eher_duplicity_backup_include_list: [ "/home/eher/", "/home/git/" ]
      eher_duplicity_backup_exclude_list: [ "/home/vagrant/" ]
      eher_duplicity_backup_destination: "s3://s3.amazonaws.com/my_backup_bucket/{{ ansible_hostname }}/"
      eher_duplicity_backup_aws_key: "{{ backup_aws_key }}"
      eher_duplicity_backup_aws_secret: "{{ backup_aws_secret }}"
      eher_duplicity_backup_time: hourly
      eher_duplicity_backup_encryption: yes
      eher_duplicity_backup_passphrase: "NotSecret"
```

## installation

I recommend you to use [Ansible Galaxy](https://galaxy.ansible.com/intro) to install roles.
```bash
ansible-galaxy install EHER.duplicity_backup
```
or 
```bash
ansible-galaxy install -p roles EHER.duplicity_backup
```
