Getting started
===============

.. contents::
   :local:

Example inventory
-----------------

To configure the Preseed server, you can add a host to ``[debops_preseed]``
group::

    [debops_preseed]
    hostname

Default configuration will prepare Preseed files for Debian Wheezy and Debian
Jessie, which system administrator account named after either
``ansible_ssh_user`` or the username present on Ansible Controller host.

Example playbook
----------------

Here's an example playbook which uses ``debops.preseed`` role::

    ---

    - name: Install Debian Preseed server
      hosts: debops_preseed

      roles:
        - role: debops.preseed
          tags: preseed

How to use Debian Preseed configuration
---------------------------------------

``debops.preseed`` will use ``debops.nginx`` role to set up a webserver for the
Preseed files. They will be served on a separate subdomain, by default
``seed``. Full address of a set of Preseed files will be for example, with
``example.org`` domain::

    debian.seed.example.org
    debian-vm.seed.example.org

You will need to define these subdomains in your DNS server, preferably as
a ``CNAME`` records pointed to the Preseed server.

If you have correctly configured DHCP server, which advertises your domain, you
will be able to access Preseed configuration using a shorter form::

    debian.seed
    debian-vm.seed

Debian Installer will automatically add your domain to the specified URL to get
Preseed files.

To enable Preseeded installation, after starting the Debian Installer (in this
particular case, Wheezy installer; Jessie might have different menu layout),
navigate the menu to "Advanced Options" -> "Automated Install".

Next, press the ``<Tab>`` key, this will let you enter additional boot options. Now
you can specify the URL of the Preseed file, default hostname and keymap
(without keymap specified D-I will still ask you for the layout even that it's
specified in the Preseed file).

An example boot command line in Debian Installer::

    url=debian.seed hostname=<host> keymap=us

After you press ``<Enter>``, Debian Installer should start the installation
process. If you specified ``debian.seed`` as the Preseed file, Debian Installer
should pause during the installation and let you configure the disk partitions
as you see fit. After configuring the partitions, automatic installation will
resume and when it's finished host will be automatically rebooted. After that
you should be able to SSH to it using the configured admin account.

The alternative Pressed configuration, ``debian-vm.seed`` is configured to
automatically partition and format first hard drive for its full capacity,
without asking the user. This Preseed configuration is designed primarly for
virtual machines, which usually have 1 partition stored in an image file or
a block device.

