emarins.ocp
=========

This role can be used to install all necessary services to the instrastructure server.  It installs named (DNS), HAPROXY (load balancer) and VSFTP.
Supported Platforms = IBM Z (s390x)

Requirements
------------

Download the required files from the Infrastructure Provider page on the Red Hat OpenShift Cluster Manager site (https://cloud.redhat.com/openshift/).

You need to download following files:

- OpenShift installer
- Pull secret
- Red Hat Enterprise Linux CoreOS (RHCOS) Images
- Command-line interface (Openshift client)

Role Variables
--------------

The description of the settable variables for this role can be found below. You need to ensure you include these variables into your playbook to ensure a successfull installation.

Parameter			Value (sample)  			Description
domain_name			Ibm.local				Domain Name
cluster_name			ocp4	         		 	Cluster Name
dns_network			192.168.0.0/24			        DNS Network for the Openshift Cluster. Used to set up DNS
domain_reverse			0.168.192.in-addr.arpa			DNS Network for the Openshift Cluster. Used to set up DNS
infrastructure_server_name	infraserver				Short name for your infrastructure server (without domain name)
infrastructure_server_ip	192.168.0.33				IP address of your infrastructure server.
bootstrap_server_name		bootstrap				Short name for your bootstrap server (without domain name)
bootstrap_server_ip		192.168.0.23				IP address of your bootstrap server
bootstrap_defaultgw		192.168.0.1				IP address for the default gw of your bootstrap server
load_balancer			192.168.0.33				IP address where HAPROXY service is installed. In our case it is installed in the infrastructure server.
bootstrap_netmask		255.255.255.0				Bootstrap subnet mask.  Used to build REDHAT.PARM file
bootstrap_ipldisk		0100					Bootstrap IPL disk.  Used to build REDHAT.PARM file
bootstrap_image			rhcos-4.2.10-s390x-metal-dasd.raw.gz	Name of the bootstrap Image
dns_forwarders			192.168.0.50; 192.168.0.51		DNS forwarders addresses. Put the official DNS server addresses of your network.
vnic_addresses			0.0.1000,0.0.1001,0.0.1002		Virtual NIC addresses used for the virtual Linux guests. You need to specify 3 virtual address.
vnic_layer2i			1					If the VNIC is using layer2 or not. Used to build REDHAT.PARM file
rhcos_kernel			rhcos-4.2.18-s390x-installer-kernel 	Name of the RHCOS kernel file that you downloaded from Redhat site.
rhcos_initrd			rhcos-4.2.18-s390x-installer-initramfs.img	Name of the RHCOS initrd file that you downloaded from Redhat site.
openshift_installer		openshift-install-linux-4.2.23-s390x.tar.gz	Name of the Openshift installer file that you downloaded from Redhat site.
openshift_client		openshift-client-linux-4.2.23-s390x.tar.gz	Name of the Openshift Client file that you downloaded from Redhat site.
ocp_stage_dir			/stage					Name of the folder where the ignition files will be created.
worker_replicas			2					Number of compute nodes (workers) for your cluster
control_plane_servers		- id: 1					Information of your master nodes (control nodes). In our case, 3 master nodes.
    				  name: master
    				  ip: 192.168.0.24
  				- id: 2
    				  name: master
    				  ip: 192.168.0.25
  				- id: 3
    				  name: master
    				  ip: 192.168.0.26
compute_servers	  		- id: 1					Information of your compute nodes (worker nodes).  They need to be specified in this format.
    				  name: worker
    				  ip: 192.168.0.27
  				- id: 2
    				  name: worker
    				  ip: 192.168.0.28
etcd_servers	  		- id: 0  				Information of your etcd nodes.  They need to be specified in this format.
    				  name: etcd-0 
    				  ip: 192.168.0.24
  				- id: 1
    				  name: etcd-1
    				  ip: 192.168.0.25
  				- id: 2
    				  name: etcd-2
    				  ip: 192.168.0.26			Information of your etcd nodes.  They need to be specified in this format.
dns_packages	 		- bind-utils				DNS package names.  Used to install DNS in the infrastructure server.
 				- bind
ocp_install_dir			/var/www/html/ocp			Folder where you put the downloaded files from Redhat site
ocp_files		  	- rhcos-4.2.18-s390x-metal-dasd.raw.gz	RHCOS image names that were downloaded from Redhat site
                  		- rhcos-4.2.18-s390x-metal-zfcp.raw.gz


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

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
