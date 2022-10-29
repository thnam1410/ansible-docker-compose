# Getting started
This ansible module was use for update tag docker service was declared in docker-compose file on remote
## Example
if you have an already docker-compose.yml file like that

```yaml
version: 3.1
services:
    backend:
        image: cr.github.io/my-image:my-tag
        container_name: my-server
```

if you have an new image 'my-tag-latest'
it will replace your docker-compose.yml like that and recreate your container

```yaml
version: 3.1
services:
    backend:
        image: cr.github.io/my-image:my-tag-latest
        container_name: my-server
```

## Create variable file
```shell
cat << EOF > ~/deploy_var.yml
---
all:
  vars:
    remote_user: root
    working_time: "{{ ansible_date_time.iso8601_basic_short }}"
    service_stacks:
      - docker_repository: cr.github.io
        image_name: my-image
        image_version: my-tag
        working_directory: /var/www/econsul/econsul_deployment
    timezone: "Asia/Ho_Chi_Minh"
EOF
```

## Generate inventory file
```shell
cat << EOF > ~/inventory.ini
[web_hosts]
web_host ansible_host=your_remote_server ansible_user=root
EOF
```

## Run ansible
```shell
read ansible_ssh_pass; # input your pass
export ANSIBLE_HOST_KEY_CHECKING=False && /usr/bin/ansible-playbook playbook.yml \
-i inventory.ini \
-i deploy_var.yml \
--extra-vars "ansible_ssh_password=$ansible_ssh_pass"
```
