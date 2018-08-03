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

## Reference:
* [OS distro version](https://packagecloud.io/docs#os_distro_version)
* [`yum` variables](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/deployment_guide/sec-using_yum_variables)
