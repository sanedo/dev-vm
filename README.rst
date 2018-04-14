Development Virtual Machine
===========================

Introduction and Scope
----------------------

This automates the creation of a development VM.


Configuring and Starting a VM
-----------------------------

These instructions assume that `Vagrant 2.0+ is installed`_, `VirtualBox 5.1+ is installed`_,
`Ansible 2.4+ is installed`_.

  1. Install the vagrant plugins required.

  ::
    [@host]$ vagrant plugin install vagrant-hostmanager
    [@host]$ vagrant plugin install vagrant-bindfs
    [@host]$ vagrant plugin install vagrant-vbguest

  3. Run ``vagrant up`` on the host.

  4. From this point, provisioning will take approximately 30 minutes.

.. _`Vagrant 2.0+ is installed`: https://www.vagrantup.com/downloads.html
.. _`VirtualBox 5.1+ is installed`: https://www.virtualbox.org/wiki/Downloads
.. _`Ansible 2.4+ is installed`: http://docs.ansible.com/ansible/latest/intro_installation.html

Sharing Files Between Host and Guest
------------------------------------

While projects are executed from the guest, it is common to want to edit them
from the host, often with the power of an IDE.  To do so, both guest and host
must have access to the set of files.


Via NFS
~~~~~~~

Sharing with NFS is not available on Windows hosts.

In this setup, files on the host are synced to the guest allowing developers to
work on projects on the host machine, but use the resources in the guest
machine to run the project.  Developers may be interested in this workflow to
take advantage of rich tools available natively on their host without the
performance impact and file permissions issues of a Samba share.

The shared directory is ``../code`` w.r.t. to the dev-vm checkout.


Troubleshooting
---------------


VM Clock Is Out of Sync
~~~~~~~~~~~~~~~~~~~~~~~

It is common for the VM to lose time after repeatedly being suspended.

::

  [vagrant@dev ~]$ sudo ntpdate -u pool.ntp.org
