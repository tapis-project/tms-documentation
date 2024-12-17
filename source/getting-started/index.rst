.. _getting-started:

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

###############
Getting Started
###############

Quickstart
==========

The no-installation way to try out TMS server is to run its Docker container in MVP mode and install the KeyCmd module on a host machine on which you have an account.  The other option is to build and run *tms_server* natively on a host.  Both the :ref:`docker_testing_label` and :ref:`native_testing_label` methods are discussed in subsections below.  

For further information, see the :ref:`docker_server_label` and :ref:`native_server_label` discussions. Also see :ref:`server_config_label` for information on how to configure TMS.  Finally, see :ref:`keycmd_label` to learn how to install the KeyCmd module on a host and to :ref:`keycmd_config_label` on how to configure it.

.. _docker_testing_label:

Docker Testing on Localhost
===========================

Installation
------------

To run TMS server in a docker container, clone the `tms_server repository`_ to your machine in an account that can issue docker commands.  We will refer to the directory into which the repository is cloned as **${TMS_HOME}**.

Perform the first step, *Installing TMS Server (tms_server)*, described in the `README-DOCKER`_ file.  Assuming current TMS version is 0.1.0, issue these commands::

   cd ${TMS_HOME}/deployment/docker
   ./docker_install.sh 0.1.0 


This step will create the **${HOME}/tms-docker** directory.  In addition, the *${HOME}/tms-docker/tms_customizations/tms-install.out* file contains the administrative credentials generated for the two standard tenants, *default* and *test*.

Automatically Generated Test Data
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Test data is also generated as part of installation.  The following values are used to create test resources in the *test* tenant:

   - Application: *testapp1*
   - Application version: *1.0*
   - Client: *testclient1*
   - Client password: *secret1*
   - User: *testuser1*
   - Host: *testhost1*
   - Host account: *testhostaccount1*
   
 
The automatically generated resources that use the above values are: 

   - *testclient1* client for *testapp1:1.0* with *secret1* password 
   - *testuser1* MFA record
   - *testuser1* to *testhostaccount1* identity mapping on *testhost1*
   - *testuser1* to *testclient1* delegation

In the :ref:`creating_keys_label` we implicitly depend on these resource definitions to create new key pairs.


Customization and Execution
---------------------------

For illustration purposes, we'll run TMS in Minimal Viable Product (MVP) mode, which will require setting some configuration parameters.  This corresponds to the `Customizing the TMS Runtime`_ section in `README-DOCKER`_.  

First, copy the default configuration file to use as a starting point::

   cd ${HOME}/tms-docker/tms_customizations
   cp ${TMS_HOME}/resources/config/tms.toml .

Use a test editor to edit *${HOME}/tms-docker/tms_customizations/tms.toml*.  Assign the following values to runtime parameters::

   enable_mvp = true
   enable_test_tenant = true

Save the changes and then start the server::

   cd ${TMS_HOME}/deployment/docker
   ./docker_run.sh 0.1.0

At this point, you should see that a container named *tms_server_container* is running.  With the server started, we can now copy the customized tms.toml file into the container's named volume::

   cd ${HOME}/tms-docker/tms_customizations
   docker cp tms.toml tms_server_container:/tms-root/.tms/config/tms.toml

Depending on the version of Docker that you are running, you might see a message like this::

   Successfully copied 4.61kB to tms_server_container:/tms-root/.tms/config/tms.toml

If you see an error response like *"Error response from daemon: No such container: tms_server_container"*, the copy failed.  Assuming success, the server needs to be restarted to activate the configuration changes::

   docker restart tms_server_container

Testing TMS
-----------

Let's begin by querying the TMS version::

   curl -k https://localhost:3000/v1/tms/version

The *-k* parameter on **curl** allows self-signed certificate shipped with TMS to be accepted, which is appropriate for initial testing but not for production use.  The results (with formatting) will look something like this::

   {
    "git_branch": "main",
    "git_commit": "55e1ca0",
    "git_dirty": "true",
    "result_code": "0",
    "result_msg": "success",
    "rustc_version": "rustc 1.82.0 (f6e511eec 2024-10-15)",
    "source_ts": "2024-11-27T17:36:09Z",
    "tms_version": "0.1.0"
   }

.. _creating_keys_label:

Creating Keys
-------------

In MVP mode, we can easily create key pairs in the *test* tenant using the test resources generated during installation.  To create a new key pair, issue this command::

   curl -k -X 'POST' 'https://localhost:3000/v1/tms/pubkeys/creds' -H 'accept: application/json; charset=utf-8'   -H 'Content-Type: application/json; charset=utf-8' -d '{"client_user_id": "testuser1", "host": "testhost1", "host_account": "testhostaccount1", "num_uses": -1, "ttl_minutes": -1}' -H 'X-TMS-CLIENT-ID: testclient1' -H 'X-TMS-CLIENT-SECRET: secret1' -H 'X-TMS-TENANT: test'

The key pair is returned in the result along with other information::

   {
    "expires_at": "9999-12-31T23:59:59Z",
    "key_bits": "256",
    "key_type": "ed25519",
    "max_uses": "2147483647",
    "private_key": "-----BEGIN OPENSSH PRIVATE KEY-----\nb3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW\nQyNTUxOQAAACBpjaF1iAYrBDJhFfdPhrTq8b+QukHc+6z6nIR2Qg5UyQAAAIjj2l7K49pe\nygAAAAtzc2gtZWQyNTUxOQAAACBpjaF1iAYrBDJhFfdPhrTq8b+QukHc+6z6nIR2Qg5UyQ\nAAAECQETTznM5DxU0dwHllam57VSNx/f7lottWHCYtacnOM2mNoXWIBisEMmEV90+GtOrx\nv5C6Qdz7rPqchHZCDlTJAAAAAAECAwQF\n-----END OPENSSH PRIVATE KEY-----\n",
    "public_key": "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIGmNoXWIBisEMmEV90+GtOrxv5C6Qdz7rPqchHZCDlTJ",
    "public_key_fingerprint": "SHA256:YqVuver8X2/EcTTiOAlsLnXhRRvnDs+cplptOEPGHDE",
    "remaining_uses": "2147483647",
    "result_code": "0",
    "result_msg": "success"
   }
  
The command can be repeated to create any number of keys.

.. _tms_server repository: https://github.com/tapis-project/tms_server
.. _Customizing the TMS Runtime: https://github.com/tapis-project/tms_server/blob/main/deployment/docker/README-DOCKER.md#customizing-the-tms-runtime
.. _README-DOCKER: https://github.com/tapis-project/tms_server/blob/main/deployment/docker/README-DOCKER.md

.. _native_testing_label:

Native Testing on Localhost
===========================


