Role Name
=========

Ansible role to install weblogic and required patches.

Requirements
------------

 - botocore
 - boto
 - boto3

Role Variables
--------------

```yaml
mount_point: "{{ alternate_mount_point|default('/srv') }}"

mw_home: "{{ mount_point }}/app/oracle/middleware"
wls_home: "{{ mw_home }}/wlserver"
java_home: "{{ lookup('env', 'JAVA_HOME') or '/usr/java/latest' }}"

system_users:
  # List of system users for ansible to create
  - name: user_name
    uid: user_id
    group: user_group
    gid: user_group_id
    profile: |
          alias ll='ls -lah'
          alias cp='cp -iv'
          export MW_HOME={{ mw_home }}
          export WLS_HOME={{ wls_home }}
          export JAVA_HOME={{ java_home }}
          PATH=$JAVA_HOME/bin:$PATH
          export PATH

mount_owner: "{{ system_users.0.name }}"
mount_group: "{{ system_users.0.group }}"

artefact_path: "path/to/our/s3/artifacts"
weblogic_artefacts:
  - name: 'artifact.jar'
  - name: 'patch.zip'

weblogic_patches:
  - name: "patch" #Excludes the extension
    command: "cli install command"
    creates_file: "file_name_created_during_extraction"
nodemgr_port: "xxxx"
```

License
-------

MIT

Author Information
------------------

HMPPS Digital Studio