Ansible Role: Java
==================

[![Build Status](https://travis-ci.org/gantsign/ansible-role-java.svg?branch=master)](https://travis-ci.org/gantsign/ansible-role-java)

Role to install the [Oracle Java JDK](http://www.oracle.com/technetwork/java/index.html).

Requirements
------------

* Ubuntu
* Ansible >= 2.0

Role Variables
--------------

The following variables will change the behavior of this role (default values
are shown below):

```yaml
# Java version number
java_version: '8u102'

# Base installation directory for any Java distribution
java_install_dir: /opt/java

# The root folder of this Java installation
java_home: "{{ java_install_dir }}/jdk{{ java_version }}"

# Directory to store files downloaded for Java installation
java_download_dir: "{{ x_ansible_download_dir | default('~/.ansible/tmp/downloads') }}"
```

### Supported Java versions

The following versions of Java are supported without any additional
configuration (for other versions follow the Advanced Configuration
instructions):

* 8u102

Advanced Configuration
----------------------

The following role variables are dependent on the Java version; to use a
Java version **not pre-configured by this role** you must configure the
variables below.

### Minor Version Configuration

The below has to be configured for every minor release of Java.

```yaml
# SHA256 sum for the redistributable JDK package (i.e. jdk-{{ java_version }}-linux-x64.tar.gz)
java_redis_sha256sum: 7cfbe0bc0391a4abe60b3e9eb2a541d2315b99b9cb3a24980e618a89229e04b7

# The build number for this JDK version
java_version_build: 14
```

### Major Version Configuration 

The below only has to be configured for a new major release of Java.

```yaml
# SHA256 sum for the redistributable JCE Policy Files (i.e. jce_policy-8.zip)
java_jce_redis_sha256sum: f3020a3922efd6626c2fff45695d527f34a8020e938a49292561f18ad1320b59

# Directory on remote mirror where JCE redistributable can be found
java_jce_redis_mirror: "{{ java_mirror_base }}/jce/8"

# The JCE redistributable file name
java_jce_redis_filename: jce_policy-8.zip

# The root folder name inside the JCE redistributable 
java_jce_redis_folder: UnlimitedJCEPolicyJDK8
```

Example Playbook
----------------

```yaml
- hosts: servers
  roles:
     - { role: gantsign.java }
```

Role Facts
----------

This role exports the following Ansible facts for use by other roles:

* `ansible_local.java.general.java_home`

    * e.g. `/opt/java/jdk1.8.0_102`
    
* `ansible_local.java.general.version`

    * e.g. `1.8.0_102`

Development & Testing
---------------------

This project uses [Molecule](http://molecule.readthedocs.io/) to aid in the
development and testing; the role is unit tested using
[Testinfra](http://testinfra.readthedocs.io/) and
[pytest](http://docs.pytest.org/).

To develop or test you'll need to have installed the following:
* Linux (e.g. [Ubuntu](http://www.ubuntu.com/))
* [Docker](https://www.docker.com/)
* [Python](https://www.python.org/) (including python-pip)
* [Ansible](https://www.ansible.com/)
* [Molecule](http://molecule.readthedocs.io/)

To run the role (i.e. the `tests/test.yml` playbook), and test the results
(`tests/test_role.py`), execute the following command from the project root
(i.e. the directory with `molecule.yml` in it):

```bash
molecule test
```

License
-------

MIT

Author Information
------------------

John Freeman

GantSign Ltd.
Company No. 06109112 (registered in England)