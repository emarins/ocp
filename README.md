Role Name
=========

This role helps you to install all necessary services (DNS, HAPROXY (load balancer) and VSFTP) in the bastion server.
Additionally, it creates all necessary files and reduce the complexity to install Openshift on IBM Z. 

Supported Platforms = IBM Z (s390x)

version: 0.9

Requirements
------------

All you need is to install the Red Hat 8 server and download Red Hat Openshift 4.2 and RHCOS

  ## If you need details in how to install a Red Hat server on IBM Z, please check web site URI
    - http://www.redbooks.ibm.com/redbooks/pdfs/sg248303.pdf

  ## Downloading Red Hat OpenShift 4.2 and RHCOS
     Ensure you download the latest file versions

     Access the OpenShift Infrastructure Providers page at https://cloud.redhat.com/openshift/install

     *) Click on "Run on IBM Z" link
     *) On Download section,
          Download the OpenShift installer. (openshift-install-linux.tar.gz)
     *) Download the Pull Secret. Save content to a txt file because Openshift installer will prompt you for your pull secret during installation ( Use pull-secret as the file name)
     *) Download the Red Hat Enterprise Linux CoreOS (RHEL CoreOS) image for z
        For DASD installations, download rhcos-4.2.18-s390x-metal-dasd.raw.gz file
        For SAN installations, download rhcos-4.2.18-s390x-metal-zfcp.raw.gz file
     *) Download the command-line tools if you want to run the commands from a desktop or outside the bastion server. (openshift-client-linux.tar.gz)


Role Variables
--------------

Default role variables are liste bellow. Those variables define the Binary files needed for Openshift and the prerequisit Open Source software. This role installs and all necessary Linux services in one Red Hat 8 server.

  # Openshift domain name. Update with your Openshift domain name.
    domain_name: ibm.local

  # Openshift Cluster name. Update with your Openshift cluster name.  This name will compose the FQDN.
    cluster_name: ocp4

  # The result of cluster name and domain name above will be ocp4.ibm.local.

  # DNS network information where bootstrap, master and worker nodes will reside. Ask Network team if you do not know how to get it.
    dns_network: 192.168.0.0/24 

  # Default gateway for the cluster network. Ask Network team if you do not know how to get it. Used for the PARM file.
    defaultgw: 192.168.0.1

  # Reverse DNS zone for the DNS network. Used for DNS reverse zone. Ask Network team if you do not know how to get it.
    domain_reverse: 0.168.192.in-addr.arpa

  # Network subnet mask. Used for the PARM file. Ask Network team if you do not know how to get it.
    netmask: 255.255.255.0

  # IPL disk address for the RHCOS instances. Update with the disk that will be used to install RHCOS. Used for the PARM file.
    ipldisk: "0100"

  # RHCOS image name. Used for the PARM file and other things. You downloaded from Openshift Infrastructure Providers Page.
    rhcos_image: rhcos-4.2.18-s390x-metal-dasd.raw.gz

  # Primary DNS Server. Used in the DNS forwarder /etc/named.conf 
    dns1: 8.8.8.8

  # Secondary DNS Server. Used in the DNS forwarder /etc/named.conf  
    dns2: 8.8.4.4  

  # Virtual Network Interface (VNIC). Check NICDEF parameter in server VM directory to fill out this field.Used for PARM file. Ask your zVM admin for support if you do not know how to find it. 
   vnic_addresses: "0.0.1000,0.0.1001,0.0.1002"

  # Layer 2 information. Ask network team if you do not know how to get it. Used in PARM file. 1 = on
    vnic_layer2: 1 

  # RHCOS kernel file. Used to build  bootstrap, master and worker nodes. You downloaded from Openshift Infrastructure Providers Page.
    rhcos_kernel: rhcos-4.2.18-s390x-installer-kernel

  # RHCOS initrd file. Used to build bootstrap, master and worker nodes. You downloaded from Openshift Infrastructure Providers Page.
    rhcos_initrd: rhcos-4.2.18-s390x-installer-initramfs.img

  # Openshift installer file name. You downloaded from Openshift Infrastructure Providers Page.
    openshift_installer: openshift-install-linux.tar.gz

  # Openshift Client file name. You downloaded from Openshift Infrastructure Providers Page.
    openshift_client: openshift-client-linux.tar.gz

  # Temporary folder that will be used to install Openshift
    ocp_stage_dir: /stage

  # Number of worker nodes. Recommended is two worker nodes. It will automatically update HAPROXY accordingly.
    worker_replicas: 2

  # Where you need to put the instalation files that were downloaded from Openshift Infrastructure Providers Page. 
    ocp_download_dir: /opt/downloads

  # RHCOS file names. Update with the latest file names. You downloaded these files from Openshift Infrastructure Providers Page.
    ocp_files:
      - rhcos-4.2.18-s390x-metal-dasd.raw.gz
      - rhcos-4.2.18-s390x-metal-zfcp.raw.gz

  # Master, Worker, bootstrap and other nodes information.
    all_nodes:
	  # Master Nodes
	  - id: 1
	    name: master1        # You need to update it
	    ip: 192.168.0.24     # You need to update it
	    vmid: RHOCPM01       # You need to update it
	    role: control_node

	  - id: 2
	    name: master2        # You need to update it
	    ip: 192.168.0.25     # You need to update it
	    vmid: RHOCPM02       # You need to update it
	    role: control_node

	  - id: 3
	    name: master3        # You need to update it
	    ip: 192.168.0.26     # You need to update it
	    vmid: RHOCPM03       # You need to update it
	    role: control_node

	  # Workers Nodes
	  - id: 1
	    name: worker1        # You need to update it
	    ip: 192.168.0.27     # You need to update it
	    vmid: RHOCPW01       # You need to update it
	    role: worker_node

	  - id: 2
	    name: worker2        # You need to update it
	    ip: 192.168.0.28     # You need to update it
	    vmid: RHOCPW02       # You need to update it
	    role: worker_node

	  # Bootstrap
	  - id: 0
	    name: bootstrap      # You need to update it
	    ip: 192.168.0.23     # You need to update it
	    vmid: RHOCPB00       # You need to update it
	    role: bootstrap_node

	  # Bastion
	  - id: 0
	    name: rh8serv1       # You need to update it
	    ip: 192.168.0.33     # You need to update it
	    vmid: RH8SERV1       # You need to update it
	    role: bastion_node

	  # etcd
	  - id: 0
	    name: etcd-0       # Do not change etcd-0 name
	    ip: 192.168.0.24   # Update with the IP of Master node 1
	    role: etcd_node

	  - id: 1
	    name: etcd-1       # Do not change etcd-1 name
	    ip: 192.168.0.25   # Update with the IP of Master node 2
	    role: etcd_node

	  - id: 2
	    name: etcd-2       # Do not change etcd-2 name
	    ip: 192.168.0.26   # Same as Master node 3
	    role: etcd_node    # Update with the IP of Master node 3

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

MIT, Apache-2.0

Author Information
------------------

Eric Marins is a Senior IT Architect in Brazil, focused on hybrid cloud solutions, Infrastructure and Platform solutions and competencies, including High Availability, Disaster Recovery, Networking, Linux and Cloud. Eric works with CIO Private Cloud designing and implementing complex hybrid cloud solutions involving Infrastructure-as-a-Service (IaaS) and Platform-as-a-Service (PaaS) capabilities

I love the Simplicity of Ansible, the IBM Z platform, Python language, Linux and Open Sources software.
