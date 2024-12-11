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

##############
Welcome to TMS
##############

What Problem does TMS Address?
==============================

Scientific workflows often run for a long time and access resources across multiple sites. The challenge in automating these workflows is to (1) securely manage user credentials, and (2) execute without human intervention. The two main impediments to automation are restrictions on sharing credentials and Multi-Factor Authentication (MFA) challenges that require a human-in-the-loop.  

To address these challenges, **TMS** implements automated, no human-in-the-loop authentication that allows applications to connect to host systems on behalf of users and issue commands as those users. The goals for automated application
authentication under TMS are:

   - No manual key distribution
   - No human-in-the-loop, limited duration MFA
   - No user secrets shared with applications
   - Account access using federated identities

What is TMS?
============

In its fullest incarnation, TMS is a multi-tenant web application exposing a REST API to manage SSH keys, client applications, user delegations, user/MFA authentication, hosts, and user/host account mappings.  TMS also has a modules that run on hosts, such as High Performance Computing (HPC) login nodes, VMs or IoT devices.

In its initial incarnation, the **TMS MVP** (Minimal Viable Product) makes a number of simplifying assumptions and implements only a subset of the full API capabilities.  This is the TMS version currently available and it's comprised of two components.  The **tms_server** web application implements all APIs and maintains state in a Sqlite database.  The **KeyCmd** module is a small executable loaded by SSHD that runs on machines into which TMS client applications login on behalf of users.   


About This Documentation
========================

This documentation includes:

   - :doc:`getting-started/index` -- try TMS
   - :doc:`technical/index` -- design and API discussions
   - :doc:`deployment/index` -- install TMS components
   - :doc:`administration/index` -- maintaining TMS
   
Online Documentation
--------------------
   
From a running TMS server, one can extract TMS's OpenAPI v3 specification and interact with its live API web page.  The links below are examples of those available to developers at the Texas Advanced Computing Center (TACC).  Similar links will work in any TMS installation by replacing the hosts in the URLs with local hosts.

   - `JSON specification`_ -- Viewable OpenAPI specification
   - `YAML specification`_ -- Downloadable OpenAPI specification 
   - `API liveDocs`_ -- Interactive API web page 

.. _JSON specification: https://tms-server-dev.tacc.utexas.edu:3000/spec
.. _YAML specification: https://tms-server-dev.tacc.utexas.edu:3000/spec_yaml
.. _API livedocs: https://tms-server-dev.tacc.utexas.edu:3000
