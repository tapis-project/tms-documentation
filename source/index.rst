=========================================
Welcome to the Trust Manager System (TMS)
=========================================

What Problem is TMS Addressing?
_______________________________

Scientific workflows often run for a long time and access resources across multiple sites. The challenge in automating these workflows is to (1) securely manage user credentials, and (2) execute without human intervention. The two main impediments to automation are restrictions on sharing credentials and Multi-Factor Authentication (MFA) challenges that require a human-in-the-loop.  

To address these challenges, **TMS** implements automated, no human-in-the-loop authentication that allows applications to connect to host systems on behalf of users and issue commands as those users. The goals for automated application
authentication under TMS are:

   - No manual key distribution
   - No human-in-the-loop, limited duration MFA
   - No user secrets shared with applications
   - Account access using federated identities

What is TMS?
____________

In its fullest incarnation, TMS is a multi-tenant web application exposing a REST API to manage SSH keys, client applications, user delegations, user/MFA authentication, hosts, and user/host account mappings.  TMS also has a modules that run on hosts, such as High Performance Computing (HPC) login nodes, VMs or IoT devices.

In its initial incarnation, the **TMS MVP** (Minimal Viable Product) makes a number of simplifying assumptions and implements only a subset of the full API's capabilities.  This is the TMS version currently available.  


About This Documentation
________________________

This documentation includes:

   - :doc:`getting-started/index` -- a basic introduction to TMS
   - :doc:`technical/index` -- design and API discussions
   
From a running TMS server, one can extract TMS's OpenAPI v3 specification and interact with its live API web page.  The links below are examples of those available to developers at the Texas Advanced Computing Center (TACC).  Similar links will work in any TMS installation by appropriately replacing the host component of the URLs.

   - specification_ -- OpenAPI JSON specification
   - LiveDocs_ -- Live API web page 

.. _specification: https://tms-server-dev.tacc.utexas.edu:3000/spec
.. _LiveDocs: https://tms-server-dev.tacc.utexas.edu:3000
