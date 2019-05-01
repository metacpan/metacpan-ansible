## Vault Utilization

This project makes use of the `ansible-vault` for encrypting settings. To work
with the vault files use the `--vault-id` parameter to pass the location of the
vault id file.

To access/edit the contents of the vault file use:

```
ansible-vault --vault-id <path/to/vault/id> edit vars/settings.yml
```

## Galaxy Roles

These roles are listed in the `requirements.yml` file and added to the
workstation by issuing:

```
ansible-galaxy install --roles-path ./galaxy -r requirements.yml
```

The `ansible.cfg` file includes the directive to include the `./galaxy`
directory when looking for roles.

## Playbooks

### provision_docker.yml

Adds the docker installation repository and then installs `docker` and
`docker-compose`.

### deploy_docker_mgmt.yml

This role clones the `metacpan-docker-production` repository onto the servers
that are part of the inventory group `container_hosts`.

This is the lowest level of the docker deployment playbooks and is required to
be run before deploying any other docker based sites.

Variables used by this playbook can be defined in the inventory `group_vars/container_hosts.yml` file

```yaml
---

docker_mgmt:
  directory: /home/metacpan/docker-production
  git:
    repo: https://github.com/metacpan/metacpan-docker-production
    version: master
    user: metacpan
```

These values are used by the Ansible `git` task to perform the actual cloning.

## Warnings & Troubleshooting

The `python` library `urllib3` may need to be updated on each system in order to
install docker.

```
pip install urllib3 --upgrade
```
