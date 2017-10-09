Role Name
=========

This role install Rabbitmq client, Rabbitmq Server and configure optionnally a 3-nodes cluster

Requirements
------------

- Ubuntu Xenial

Role Variables
--------------

    rabbitmq_port: 5672
    rabbitmq_bind_address: 127.0.0.1

    rabbitmq_use_lvm: "true"
    rabbitmq_vg_name: vg_vroot
    rabbitmq_lv_name: lv_rabbit
    rabbitmq_lv_size: 3g
    rabbitmq_fs_type: "xfs"

    rabbitmq_env_file: /etc/rabbitmq/rabbitmq-env.conf
    rabbitmq_conf_file: /etc/rabbitmq/rabbitmq.config
    rabbitmq_datadir: /var/lib/rabbitmq

    rabbitmq_use_fqdn: "true"


    ### Secret variables
    rabbitmq_erlang_cookie: "RHFMUPCIIAMHLDJNZGNG"
    rabbitmq_admin_password: "passwd"
    rabbitmq_app_password: "passwd"
    rabbitmq_monitoring_password: "passwd"


Example Playbook hosts
----------------------


     [rabbitmq_cluster]
     jphqueuefr01l.btsys.local rabbitmq_hostname="jphqueuefr01l.btsys.local" rabbitmq_firstnode=True
     jphqueuefr02l.btsys.local rabbitmq_hostname="jphqueuefr02l.btsys.local" rabbitmq_firstnode=False
     jphqueuefr03l.btsys.local rabbitmq_hostname="jphqueuefr03l.btsys.local" rabbitmq_firstnode=False


Playbook Example
----------------

    - name: Install Rabbitmq role
      hosts: rabbit
      become: yes
      vars:
        - rabbitmq_bind_address: "0.0.0.0"
        - rabbitmq_admin_password: "passwd"
        - rabbitmq_app_username: "app_user"
        - rabbitmq_app_password: "passwd"
        - rabbitmq_app_vhost: "/application"
        - rabbitmq_monitoring_username: "collectd"
        - rabbitmq_monitoring_password: "passwd"
        - rabbitmq_clustering: True
        - rabbitmq_erlang_cookie: "RHFMUPCIIAMHLDJNZGNG"
        - rabbitmq_nodes_list:
          - "jphqueuefr01l.btsys.local"
          - "jphqueuefr02l.btsys.local"
          - "jphqueuefr03l.btsys.local"
      roles:
        - ansible-rabbitmq
      tags: [installation]


Playbook launch
---------------

      ANSIBLE_HOST_KEY_CHECKING=False; ansible-playbook -i hosts playbook.yml --ask-vault-pass --ask-pass --ask-become-pass --tags installation

Tags
----

- [installation] : Rabbitmq server or cluster installation
- [testing] : Unit testing for a server

License
-------

BSD

Author Information
------------------

BSO ISL
