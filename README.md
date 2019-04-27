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

## Warnings & Troubleshooting

The `python` library `urllib3` may need to be updated on each system in order to
install docker.

```
pip install urllib3 --upgrade
```
