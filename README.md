emarins.ocp
=========

This role can be used to install all necessary services to the instrastructure server.  It installs named (DNS), HAPROXY (load balancer) and VSFTP.
Supported Platforms = IBM Z (s390x)

Requirements
------------

Download the required files from the bastion Provider page on the Red Hat OpenShift Cluster Manager site (https://cloud.redhat.com/openshift/).

You need to download following files:

- OpenShift installer
- Pull secret
- Red Hat Enterprise Linux CoreOS (RHCOS) Images
- Command-line interface (Openshift client)

Role Variables
--------------

The central configuration file for the automation is located at defaults/main.yml. It is recommended that you created your own variable file and update the values to fit your environment.


globalvars.yml file is a sample that you can use to update values to fit your environment. You need to copy this file and put in the same directory of your playbook file. This file contains detailed description for each tag. We strongly recommend to open this file and read the content.


playbook_ocp.yml file is a playbook sample file that loads the emarins.ocp rule and globalvars.yml file


The description of the settable variables for this role can be found below. You need to ensure you include these variables into your playbook to ensure a successfull installation.


Dependencies
------------

There is not any dependency to use other Galaxy roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: emarins.ocp }

License
-------

BSD

Author Information
------------------

Eric Marins is a Senior IT Architect in Brazil, focused on hybrid cloud solutions, Infrastructure and Platform solutions and competencies, including High Availability, Disaster Recovery, Networking, Linux and Cloud. Eric works with CIO Private Cloud designing and implementing complex hybrid cloud solutions involving Infrastructure-as-a-Service (IaaS) and Platform-as-a-Service (PaaS) capabilities
