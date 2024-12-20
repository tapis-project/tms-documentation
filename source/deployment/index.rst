.. _deployment:

..
    Comment: Heirarchy of headers will now be!
    1: ### over and under
    2: === under
    3: --- under
    4: ^^^ under
    5: ~~~ under

.. raw:: html

    <style> .red {color:#FF4136; font-weight:bold; font-size:20px} </style>

.. role:: red

#############################################
Deployment
#############################################

.. _native_server_label:

Native Server 
=============

The result of a native installation is that the *tms_server* executable runs under a dedicated user ID, such as **tms**, on a Linux system.  Typically, the program is launched as a system service using *systemctl*.  To guarantee binary compatibility with the operating system, *tms_server* is built on the system on which it runs.  The installation procedure along with utility scripts are detailed in the `README-NATIVE`_ file.

.. _README-NATIVE: https://github.com/tapis-project/tms_server/blob/main/deployment/native/README-NATIVE.md  

See `Server Configuration`_ for more information on server configuration.

.. _docker_server_label:

Docker Server
=============

To run *tms_server* in a docker container, follow the procedure in `README-DOCKER`_.

.. _README-DOCKER: https://github.com/tapis-project/tms_server/blob/main/deployment/docker/README-DOCKER.md

See `Server Configuration`_ for more information on server configuration.

.. _keycmd_label:

KeyCmd 
======
To build and install *tms_keycmd*, please follow the procedure in `README-KEYCMD`_.

.. _README-KEYCMD: https://github.com/tapis-project/tms_keycmd/blob/main/README.md


#############################################
Configuration
#############################################

.. _server_config_label:

Server Configuration
====================

By default, *tms_server* program uses the **${HOME}/.tms** directory for all its runtime I/O.  The directory contains these subdirectories:

   - **certs** - Contains the full certificate chain and private key file used by TLS (HTTPS).
   - **config** - Contains *tms.toml*, which specify the server's runtime parameters, and *log4rs.yml*, which configures the server's logging output. 
   - **database** - Contain the Sqlite database files.
   - **logs** - Contains roller log files written by the server.
   - **migrations** - Contains database schema migration files.

See the default `tms.toml`_ for a detailed description of all server parameters and `log4rs.yml`_ for the standard logging configuration.  All default data files shipped with TMS reside under the `resources`_ directory.

.. _tms.toml: https://github.com/tapis-project/tms_server/blob/main/resources/config/tms.toml
.. _log4rs.yml: https://github.com/tapis-project/tms_server/blob/main/resources/config/log4rs.yml
.. _resources: https://github.com/tapis-project/tms_server/tree/main/resources

Server Tenancy and Customization
--------------------------------

In addition to TMS's runtime directory, server installation creates the **${HOME}/tms_customizations** directory.  The installation process writes the **tms-install.out** file to the directory.  This file contains the installation output log and administrator credentials generated by TMS for the two predefined tenants.  These tenants have the following characteristics.

   - **default** - The default tenant available for production use.
   - **test** - A tenant prepopulated with client and user information for testing purposes.

The tenant credentials in *tms-install.out* file will look something like this::

***************************************************************************
***************************************************************************
**** Below are the administrator user IDs and passwords for the        ****
**** standard tenants created at installation time.  The passwords are ****
**** NOT saved by TMS, only hashes of them are saved.  Please store    ****
**** the passwords permanently in a safe place accessible to TMS       ****
**** administrators.                                                   ****
****                                                                   ****
****        THIS IS THE ONLY TIME THESE PASSWORDS ARE SHOWN.           ****
****                                                                   ****
****      THE PASSWORDS ARE NOT RECOVERABLE IF THEY ARE LOST!          ****
****                                                                   ****
**** Tenant: default                                                   ****
**** Administrator ID: ~~admin                                         ****
**** Password: db588e822c8c836200381501aa05e8dd75250cfd98643518        ****
****                                                                   ****
**** Tenant: test                                                      ****
**** Administrator ID: ~~admin                                         ****
**** Password: 91b42891784beb7d26010e273ecae58e51119b2523cde163        ****
****                                                                   ****
***************************************************************************
***************************************************************************

This is the only place these credentials appear in the clear. They should be backed up to a secrets store for safe keeping. The *tms-install.out* file is never ready by TMS, so it can be removed from the TMS server.

.. _keycmd_config_label:

KeyCmd Configuration
====================
To configure *tms_keycmd*, please follow the procedure in `README-KEYCMD-CFG`_.

.. _README-KEYCMD-CFG: https://github.com/tapis-project/tms_keycmd/blob/main/README.md#configuration-of-sshd

