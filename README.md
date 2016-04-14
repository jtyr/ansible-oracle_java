oracle_java
===========

Ansible role which installs a particulat version of Oracle Java. It only works
for RedHat at the moment and it requires you to configure YUM repo from which the
Oracle Java RPM is intalled (see
[`yumrepo`](https://github.com/picotrading/ansible-yumrepo) and
[`pulp`](https://github.com/picotrading/ansible-yumrepo)). You can use this role
to install multiple version of Oracle Java and choose which one is the default
one for which the `/usr/bin/` links will be created. All Java packages not
managed by this role will be automatically uninstalled.


Examples
--------

```
---

# Example of how to install default version (jdk-1.8.0_77)
- hosts: myhost1
  roles:
    - role: oracle_java
      oracle_java_finish: yes

# Example of how to install a particular version (jre-1.8.0_25)
- hosts: myhost2
  roles:
    - role: oracle_java
      oracle_java_distribution: jre
      oracle_java_version_minor: 8
      oracle_java_version_update: 25
      oracle_java_finish: yes

# Example of installation of multiple versions (version jdk-1.8.0_25 will be
# set as default):
- hosts: myhost3
  roles:
    - role: oracle_java
      oracle_java_distribution: jre
      oracle_java_version_minor: 8
      oracle_java_version_update: 25
      oracle_java_default: yes
    - role: oracle_java
      oracle_java_distribution: jdk
      oracle_java_version_minor: 8
      oracle_java_version_update: 71
      oracle_java_finish: yes

# Example of how to uninstall all Java packages
- hosts: myhost4
  roles:
    - role: common/oracle_java
      oracle_java_distribution: none
      oracle_java_finish: yes
```


Role variables
--------------

List of variables used by the role:

```
# URL to the Java YUM repo (if empty, no repo file is created)
oracle_java_yumrepo_url: ""

# Additional YUM repo params
oracle_java_yumrepo_params: {}

# Default Java distribution [jdk|jre|none]
oracle_java_distribution: jdk

# Default major version
oracle_java_version_major: 1

# Default minor version
oracle_java_version_minor: 8

# Default patch version
oracle_java_version_patch: 0

# Default update version
oracle_java_version_update: 77

# Indicates whether this Java versin should be set as default alternative
oracle_java_default: no

# Flag which triggers the package cleaning
oracle_java_finish: no

# Version as a string
oracle_java_version_string: '{{ oracle_java_version_major }}.{{ oracle_java_version_minor }}.{{ oracle_java_version_patch }}_{{ oracle_java_version_update }}'

# Directory name
oracle_java_version_dir: '{{ oracle_java_distribution }}{{ oracle_java_version_string }}'

# Java8 has the version as part of the package name
oracle_java_distribution_string: '{{ oracle_java_distribution + oracle_java_version_string if oracle_java_version_minor == 8 else oracle_java_distribution }}'

# Alternatives priority
oracle_java_version_priority: '{{ oracle_java_version_major}}{{ "%02d" % oracle_java_version_minor }}{{ "%02d" % oracle_java_version_patch }}{{ "%03d" % oracle_java_version_update }}'
```


License
-------

MIT


Author
------

Jiri Tyr
