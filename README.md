Sphere Tools - automating installation of VMware Tools on select hosts in your environment! (Linux* only)
--------------------------------------------------------------------------------------------------------
Requirements:
------------------------
A vSphere host
Ansible installed on the localhost
At least one virtual machine hosted on vSphere and in the /etc/ansible/hosts
Python 2.7 (ie not 3/may need changes for Python 3)

Optional:
-----------------------
cowsay installed on your ansible host (makes it more amoosing)

How it works and files you will need to modify:
------------------------------------------------
The Ansible script will create a file of hosts to send to the python script with interacts with vSphere's SOAP API. This is derived from the /etc/ansible/hosts file. It will ommit any 
entries that you have hashed (#hostname) out. It will authenticate to the VMware server using credentials stored in the vimcreds.yml file. After you have changed these contents to
match your username and password, you should then encrypt the file with "ansible-vault encrypt vimcreds.yml" and store the password for the vimcreds in passfile. 
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!!ENSURE THAT THE PASSFILE IS DELETED AFTER USE!!
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
If this remains on your system, anyone could learn your vSphere authentication details. It is the intention to add functionality to automatically delete the passfile and create an empty 
one to this script soon. It has remained absent for debugging.

Run with "ansible-playbook -K --vault-password-file passfile install_tools.yml"

An empty passfile has been included with this package for editing. Note the lack of extension.

The user_path variable in the install_tools.yml file will be needed to be changed to the directory in which this script resides.

You will additionally need to change the exsi_host and exsi_port variables in the sphere_tools.py script to match the hostname or ip of your vSphere host and the port number of the host
respectively.


Example of files:
-----------------

vimcreds.yml
***************
username: fred
pword: secureadminpassword1234

passfile
***************
thisisthekeyiusedtoencryptvimcreds

sphere_tools.py
***************
#~~~~~~~~~~~Environmental Variables~~~~~~~~~~~#
#CHANGE THESE TO MATCH  YOUR ENVIRONMENT!!!
esxi_host="10.31.33.7"
esxi_port="1337"


Notes:
---------------------
Due to the manner in which the python script is constructed (a lot of arrays), it will take some time to complete. The time will increase with the more virtual machines that the ESXI host
has and also how many hosts are specified as targets for this script. This is a once off at the start of the script.

The installation of vSphere tools on individual hosts can likewise be time consuming. Leave it to run in the background and get a coffee.


*Tested on RHEL but the commands should be RHELevant across most distros.
