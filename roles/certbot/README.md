Role Name
=========

This role is used to install certbot and create certificates for domain on ubuntu server


Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml,.
The role needs variable `certs` which `list of dict with keys email and domain`
Eg: [{'email':'test@email.com','domain':'test.com'},{'email':'test2@email.com','domain':'test2.com'}]



Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:
- hosts: servers
  roles:
     - certbot

- hosts: servers
  roles:
    - { role: certbot, certs: [{'email':'test@email.com','domain':'test.com'},{'email':'test2@email.com','domain':'test2.com'}] }

Inventory:
--------------
[all]
1.2.3.4 ansible_user=test
[all:vars]
certs=[{'email':'test@email.com','domain':'test.com'},{'email':'test2@email.com','domain':'test2.com'}]

Author Information
------------------

Lalam Revathi, revathisanthosh.lalam@gmail.com
