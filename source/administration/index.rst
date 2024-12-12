.. _administration:

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
Administration
#############################################

Maintaining Security
====================

A key aspect of keeping the TMS server secure is to prevent unauthorized access to the host and the account on which *tms_server* runs.  The TMS host should be protected using a firewall and other standard security measures.  The server account, usually **tms**, should only be accessible by switching to it from root or other privileged accounts on the same host, and should be used only to run the TMS server.

Another aspect of securing TMS is to make sure the TMS runtime directory (*${HOME}/.tms* by default) and *${HOME}/tms_customizations* have the proper access permissions assigned to their files and subdirectories.  TMS server will not start unless the Linux owner/group/other permissions listed in octal in the following subsections hold.

${HOME}/.tms Permissions
------------------------

   1. *${HOME}/.tms* - 700
   2. *${HOME}/.tms* subdirectories - 700
   3. *${HOME}/.tms*/certs/* - 600
   4. *${HOME}/.tms*/config/* - 600
   5. *${HOME}/.tms*/database/* - 644
   6. *${HOME}/.tms*/logs/* - 664
   7. *${HOME}/.tms*/migrations/* - 600


${HOME}/tms_customizations Permissions
-------------------------------------- 

   1. *${HOME}/tms_customizations* - 700
   2. *${HOME}/tms_customizations/tms-install.out* - 600


Database Backup
===============

The Sqlite database files reside in *${HOME}/.tms*/database* by default.  It's configured to use write-ahead logging, so there can be up to three files in the *database* directory.  TMS does not prescribe which `Sqlite backup`_ method is used, but stronly recommends backing up regularly. 

.. _Sqlite backup: https://www.sqlite.org/backup.html
