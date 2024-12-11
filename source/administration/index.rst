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

A key aspect of keeping the TMS server secure is to prevent unauthorized access to the host and the account on which *tms_server* runs.  Protect access to the TMS host using a firewall and other standard tools.  The server account, usually **tms**, should only be accessible by switching to it from root or other privileged accounts on the same host.

Another aspect of securing TMS is to make sure the TMS runtime directory, *${HOME}/.tms* by default, has the proper access permissions assigned to files and subdirectories.  TMS server will not start unless the following permissions hold:



Database Backup
===============

