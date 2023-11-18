# PiHole on Android device with Linux Deploy
## Description 

This repository contains an Ansible Playbook to automatically install PiHole on an Android device running [Linux Deploy](https://github.com/meefik/linuxdeploy)

A while ago I decided to repurpose an old android phone laying down in one of my drawers and thanks to a [guide found on the internet](https://www.reddit.com/r/pihole/comments/nswh1r/successfully_installed_pihole_on_spare_android/) I was able to install PiHole inside of a linux container running in it. 

Briefly, the process to install PiHole with Linux Deploy is the following: 

1. Install Linux Deploy and Busy Box on a rooted phone 
2. Configure an environment (Debian in this case) as described [here](https://www.reddit.com/r/pihole/comments/nswh1r/successfully_installed_pihole_on_spare_android/)
3. Follow the steps in the guide to fix the issues with the Web UI

To make the whole process faster in case I need to wipe the linux machine or change phone, and to make practice with Ansible that I started studying a few days ago, I decided to create this ansible playbook to automate the whole process. 

The script has been initially tested on a Vagrant box created with `Vagrantfile` included in the repo, except for the *linux deploy* specific part which had to be tested on the phone itself (if you have suggestions on how to test also this part with Vagrant, highly appreciated!).

## How to use the Playbook 
The playbook is organized as follows:
1. **install_python.yml**: the debian distro in linux deploy comes without a python installation, therefore this playbook installs python to allow the subsequent playbooks to be executed by Ansible
2. Ansible Role [r_pufky.pihole](https://github.com/r-pufky/ansible_pihole): a very great ansible module that installs and configures PiHole
3. Ansible Role roles/pihole-linux-deploy that updates the permissions of the user groups and reboots the android device to fix the issues with the Web UI

Therefore, to run the playbook you need to install the role: 
```
ansible-galaxy install r_pufky.pihole
```

Create a variable file with the pihole password you want to set: 
```
touch host_vars/android-pihole/vars/secrets.yml
ansible-vault edit host_vars/android-pihole/vars/secrets.yml
```
The secrets.yml should contain:
```yaml
---
pihole_webpassword: <my_password_here>
```
Set the additional variables that you need in: 
```
host_vars/android-pihole/vars/pihole.yml
```
For Example: 
```yaml
pihole_local_backup: host_vars/android-pihole/pihole-backup-teleporter.tar.gz

pihole_webpassword: 'CLTmi5'

pihole_webtheme:           'default-light'
pihole_pihole_interface:   'wlan0'
pihole_pihole_dns_1:       '8.8.8.8'
pihole_pihole_dns_2:       '8.8.4.4'

pihole_query_logging:         'false'
pihole_install_web_server:    'true'
pihole_install_web_interface: 'true'
pihole_lighttpd_enabled:      'true'
pihole_cache_size:            '10000'


```
And run the playbook:

```
ansible-playbook playbook.yml --ask-vault-password
```


### Credits
- [@r-pufky](https://github.com/r-pufky) for PiHole Ansible Role[r_pufky.pihole](https://github.com/r-pufky/ansible_pihole)
- PiHole WebUI fix on Linux Deploy [here](https://www.reddit.com/r/pihole/comments/nswh1r/successfully_installed_pihole_on_spare_android/)
