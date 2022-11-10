# s3mount

## Role installation

### create requirements file

``` bash
cd ANSIBLE_FOLDER
mkdir requirements
touch requirements/s3mount.yml
```

### file content

``` yaml
---

# ansible-galaxy install -p vss_galaxy_roles --force -r requirements/s3mount.yml
- src: "https://github.com/virsas/mods-ansible-rpm-s3mount"
  scm: git
  version: v1.0.0
  name: s3mount
  path: vss_galaxy_roles
```

## Install role

### Installation

``` bash
ansible-galaxy install -p galaxy_roles --force -r requirements/s3mount.yml
```

### Best practices

If you are using git for your playbooks and sites configuration, add vss_galaxy_roles to your .gitignore file. You are free to download the role directly to your roles directory, but it will be then pushed to your repo too.

## Role access

### Direct access

``` bash
$ cd vss_galaxy_roles
$ ls
s3mount
$ cd s3mount
$ ls -1
defaults
handlers
LICENSE
meta
README.md
tasks
templates
$
```

### Access from ansible (ansible.cfg)

``` bash
[defaults]
gathering = smart
roles_path=./roles:../roles:../../roles:./vss_galaxy_roles
log_path=./ansible_run.log
retry_files_enabled=False
[ssh_connection]
scp_if_ssh=True
host_key_checking=False
```

## Files

### Playbook (./playbooks/s3mount.yml)

``` yaml
---

- hosts: s3mount
  remote_user: ec2-user
  become: yes
  roles:
    - s3mount
```

### Inventory (./sites/NAME/inventory)

``` txt
[s3mount]
s3mount_01
```

### Host vars (./sites/NAME/host_vars/s3mount_01.yml)

``` yaml
---

ansible_ssh_host: 1.1.1.1
```

### Group vars (./sites/NAME/group_vars/s3mount.yml)

Please see the variables below to get the complete list of possible modifications. The YAML file below is just a basic example of a minimal configuration.

``` yaml
S3BUCKET_LIST:
  - { bucket: "mycompany-samba-bucket", mount_owner: "root", mount_group: "users", mount_mode: "0770", mount_point: "samba", iam_user: "AKIAXXXXXXXXXXXXXXXXXXX", iam_key: "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" }
```

## Usage

``` bash
ansible-playbook -i sites/NAME/inventory playbooks/s3mount.yml --diff
```

## Variables

``` yml
---
# Rewritable vars

S3FS_VERSION: "1.91"
S3BUCKET_LIST:
  - { bucket: bucket1, mount_owner: "root", mount_group: "root", mount_mode: "0770", mount_point: "mount1", iam_user: "AKAIXXXXXXXXX1", iam_key: "XXXXXXXXXXXXXXXXXXX" }
  - { bucket: bucket2, mount_owner: "root", mount_group: "root", mount_mode: "0770", mount_point: "mount2", iam_user: "AKAIXXXXXXXXX2", iam_key: "XXXXXXXXXXXXXXXXXXX" }
```
