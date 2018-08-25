Machines
========
Collection of Ansible roles

## 1. Coding conventions
* Jinja2: [Chromium's style guide](https://chromium.org/developers/jinja)
* Commons var:
    ```ini
    ansible_user=dungdm93
    ansible_password=foobar
    ansible_become_pass=abcxyz
    ansible_python_interpreter=/usr/bin/python3
    ```

* List Ansible pre-defined variables
    ```bash
    $ ansible -i inventory.ini -m setup <node>
    ```

## 2. Awesome Ansible
* [ANXS](http://anxs.io/) : [MySQL](https://github.com/ANXS/mysql) | [Postgres](https://github.com/ANXS/postgresql)
* [Cloud Alchemy](https://github.com/cloudalchemy) : [Grafana](https://github.com/cloudalchemy/ansible-grafana) | [Prometheus](https://github.com/cloudalchemy/ansible-prometheus) | [Alertmanager](https://github.com/cloudalchemy/ansible-alertmanager) | ...
* Elastic Stack : [ElasticSearch](https://github.com/elastic/ansible-elasticsearch) | LogStash | Kibana | [Beats](https://github.com/elastic/ansible-beats)

## Reference:
* [OS distro version](https://packagecloud.io/docs#os_distro_version)
* [`yum` variables](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/deployment_guide/sec-using_yum_variables)
