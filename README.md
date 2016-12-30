{
  "name": "kiran-sftpd",
  "version": "0.1.2",
  "author": "Kiran Martha",
  "summary": "manage sFTP servers",
  "license": "Apache 2.0",
  "source": "git://github.com/marthakiran/sftpd.git",
  "project_page": "https://github.com/marthakiran/sftpd",
  "operatingsystem_support": [
    {
      "operatingsystemrelease": [
        "5.0",
        "6.0",
        "7.0"
      ],
      "operatingsystem": "RedHat|CentOS"
    },
    {
      "operatingsystemrelease": [
        "12.04",
        "10.04",
        "16.05"
      ],
      "operatingsystem": "Ubuntu"
    }
  ],
  "issues_url": "https://github.com/marthakiran/sftpd/issues",
  "dependencies": [
    {"version_requirement":">= 1.0.0","name":"puppetlabs/stdlib"}
  ]
}
[root@puppet1 kiran-sftpd-0.1.2]# 
[root@puppet1 kiran-sftpd-0.1.2]# cat README.md 
# kiran-sftpd

Module is going to install openssh and openssh-client application
and configure itself for sftpd server

# Installing SSH server and start the service for you.

usage:

      class { 'sftpd': }
---
Options for copy the configuration file of "SSH" service. 
1. By default its using template for ssh configuration "${path}/templates/sshd_config.erb" .
2. "${path}/files/sshd_config" file copy to remote host "${path}/files/sshd_config" .

If you want to use "${path}/files/sshd_config" file instead of template , change as follows
# change "copy_config" into "yes" and  "template_config" into "no".
Use:

        sftpd::config { 'sshd_config':
	       	copy_config             => yes,
	       	template_config         => no,
        }

-----

# If the selinux disabled on agent .
# If it "yes", It may take little bit time to set the boolean paramater for selinux.

	sftpd::config { 'sshd_config':
                selenable	=>  no,
        }

---

### Usage in manifest file to create Groups and Users .
#+ To create Groups .

     sftpd::groups { 'sftpgroup1':
      groupname	   => sftpgroup,
      ensure          => present,
     }
	
---
# To create Users .
    
     sftpd::users { 'sftpuser1':
          user            => test,
          ensure          => present,
          usergroups      => ['sftpgroup'],
          password        => '$1$ju6myyQV$P.UOTsdfadskflfdsafasdkdsfja/', 	#pass the hash
          managehome      => true,
          upload_dir      => market,				                #share directory EX: upload
     }

NOTE: To generate the password hash you can use the following command on your linux platforms
    #openssl passwd -1   ; prompt for password 

---

# It's support hiera also , so you can pass values thorugh hiera.
# using hiera for user,share directory and groups creation. 
  
# To create a group.

     sftpd::groups:
       'sftpgroup1':
          groupname: sftpgroup
          ensure: present
---

# To create user and upload directory . 

     sftpd::users:
      'sftpuser1':
         ensure: present
         user: test1
         usergroups:
           - sftpgroup
         password: '$1$ju6myyQV$P.UOTwLX0adfadsfasUK5Jan/'
         managehome: true		
         upload_dir: market
---
# Deleting the Users and Group from hiera .
# Delete The Group .

      sftpd::groups:
        'sftpgroup1':
           ensure: absent
           groupname: sftpgroup
----
# Delete The User .

      sftpd::users:
        'sftpuser1':
        ensure: absent
----
