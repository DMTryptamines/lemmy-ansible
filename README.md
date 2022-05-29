# Lemmy-Ansible-Dual-Frontend-Jaeger-Prom

(**Does not include Prometheus yet**)

This provides an easy way to install a dual front [Lemmy](https://github.com/LemmyNet/lemmy) on any server along with a fully functioning Jaeger-all-in-one. It automatically sets up an nginx server, letsencrypt certificates, and email for both lemmy front ends and jaeger.

## Requirements

To run this ansible playbook, you need to:

- Have a server / VPS where lemmy will run.
- Configure 3 DNS `A` Records to point at your jaeger and lemmy frontends.
- Make sure you can ssh to it, with a sudo user: `ssh <your-user>@<your-domain>`
- Install [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) on your **local** machine (do not install it on your destination server).

## Install

Clone this repo: 

```
git clone https://github.com/DMTryptamines/lemmy-ansible-dual-frontend-jaeger-prom.git
```

Make a directory to hold your config: 

`mkdir -p inventory/host_vars/<your-domain>`

Copy the sample configuration file:

`cp examples/config.hjson inventory/host_vars/<your-domain>/config.hjson`

Edit that file (inventory/host_vars/<your-domain>/config.hjson) and change the config to your liking. Note: **Do not edit anything inside the {{ }} braces.**You can install a fully functioning instance without changing anything.

`nano inventory/host_vars/<your-domain>/config.hjson`

[Here are all the config options.](https://join-lemmy.org/docs/en/administration/configuration.html#full-config-with-default-values)

Copy the sample inventory hosts file:

`cp examples/hosts inventory/hosts`

**Mandatory you must edit the defaults in the inventory hosts file (inventory/hosts) as described in the file comments

`nano inventory/hosts`

Run the playbook: 

`ansible-playbook -i inventory/hosts lemmy.yml`

*Note*: if you are not the root user or don't have password-less sudo, use this command:

`ansible-playbook -i inventory/hosts lemmy.yml --become --ask-become-pass`

If the command above fails, you may need to comment out this line In the ansible.cfg file:

`interpreter_python=/usr/bin/python3`

## Upgrading **NOT FOR THIS REPO TBU**

- Run `git pull`
- Check out the [Lemmy Releases Changelog](https://github.com/LemmyNet/lemmy/blob/main/RELEASES.md) to see if there are any config changes with the releases since your last. 
- Run `ansible-playbook -i inventory/hosts lemmy.yml --become`

## Migrating your existing install to use this deploy **NOT FOR THIS REPO TBU**

- [Follow this guide](https://join-lemmy.org/docs/en/administration/backup_and_restore.html) to backup your existing install.
- Run `docker-compose stop` to stop lemmy.
- Move your docker folders on the server to `<lemmy_base_dir>/<your-domain>`.
- Copy your postgres password to `inventory/host_vars/<your-domain>/passwords/postgres`.
- Follow the install guide above, making sure your `config.hjson` is the same as your backup.

## Uninstall 

`ansible-playbook -i inventory/hosts uninstall.yml --become`

## License

- [AGPL License](/LICENSE)
