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

## Roles

### deploy_site

`deploy_site` is a base level role that is to be used for all sites within the
`metacpan-docker` environments. The `site` variable is a required setting and
must match the name of the service to be started, and the configuration values
in the `var/settings.yml` file.

The `deploy_site` role is comprised of 3 parts:

1. configure
2. compose_pull
3. compose_up

#### configure

The configure phase integrates the values for the site in `vars/settings.yml`
into the `{{ site }}/environment.json` file. Site settings keys are used to
search the environment file for matching keys, and the values from settings are
substituted. This allows for separation of development/test values and those
used in production.

#### compose_pull

The compose pull phase issues the `docker-compose pull {{ site }}` command in
order to make sure that the local images are up to date with the latest releases
before starting the containers.

#### compose_up

The compose up phase starts the containers as necessary by issuing the
`docker-compose up -d {{ site }}` command.

### env_files

The `env_files` role writes settings specified in the `env` variable to the environment file
named for the value in the `container` variable. If none is specified the `.env` file is
updated instead.

```yaml
    - name: Set logging environment
      include_role:
        name: env_files
      vars:
        container: logging
        env: "{{ logging.environment }}"
```

## Warnings & Troubleshooting

The `python` library `urllib3` may need to be updated on each system in order to
install docker.

```
pip install urllib3 --upgrade
```
