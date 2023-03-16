ossec 
=====

Role to install OSSEC-HIDS agent and server

Requirements
------------

No requirements outside ansible itself.

Role Variables
--------------

* `ossec_profile`: Can be 'agent' or 'server', defaults to `agent`
* `ossec_root_dir`: Defaults to `/var/ossec`

Dependencies
------------

This role does not depend on other roles.

Example Playbook
----------------

To apply the role:

    - hosts: servers
      roles:
         - { role: ossec }

License
-------

BSD

Author Information
------------------

Brad Lhotsky \<brad@divisionbyzero.net>
