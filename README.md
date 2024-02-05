# IBM Security Verify Identity Manager Software Install on Red Hat 8

This installations is based on Response Files that are already prepared. 
if need to change directories or any package version please make sure to modify accordinly the right response file.

## Prerequisits

Download all required packages, in this example the packages was uploaded to /app/list/data/sources/

List of Packages:
* agent.installer.linux.gtk.x86_64_1.9.1003.20200730_2125.zip
* 8.0.8.5-ISS-JAVA-LinuxX64-FP005.tar  
* ibm-java-jre-8.0-5.30-linux-x86_64.tgz
* ibm-java-sdk-8.0-6.16-linux-x64-installmgr.zip
* 9.0.5-WS-APPCLT-FP005.zip    
* 9.0.5-WS-IHSPLG-FP005.zip                   
* 9.0.5-WS-WAS-FP005.zip                      
* was.repo.90501.applcient.zip
* was.repo.90501.ihs.zip
* was.repo.90501.nd.zip
* was.repo.90501.plugins.zip
* 9.0.0.0-ws-was-ifph27557.zip
* 9.0.5.0-ws-wasprod-ifph26220.zip
* 9.0.5.1-ws-wasprod-ifph29871.zip
* 9.0.5.2-ws-was-ifph27583.zip
* SIA_RMI_7140_SDI_7X_MP_ML.zip
* v11.5.9_linuxx64_universal_fixpack.tar.gz                  
* DB2_SEVPC_OA_11.5.4_MP_ML.zip  
* 6.4.0.28-ISS-ISDS-LinuxX64-IF0028.tar.gz    
* 7.2.0-ISS-SDI-FP0008.zip                       
* 8.0.55.31-ISS-GSKIT-LinuxX64-FP0031.tar.gz                                 
* sds-6.4.0.25-linux-x86-64.tar.gz                       
* IM_SEC_VFY_GVN_IGE_v10.0.2_LINUX.zip        
* SDI_7.2_XLIN86_64_ML.tar

Upload the installation Response Files, in this example the files was uploaded to /app/list/data/sources/responsefiles/

Security Patches should be uploaded to /app/list/data/sources/secu_fixes/

Security Patches Files:
* 9.0.0.0-ws-was-ifph27557.zip
* 9.0.5.0-ws-wasprod-ifph26220.zi
* 9.0.5.1-ws-wasprod-ifph29871.zip
* 9.0.5.2-ws-was-ifph27583.zip

## Installation Prerequisits

### Check the umask value should be 0022
```
umask
``` 

### Check SELinux status, if enabled "Current mode" should be permissive
verify the status and "Current mode" value
```
sestatus
```
change the "Current mode" value to permessive
``` 
setenforce 0 
```
verify the status and "Current mode" value
```
sestatus 
```
### Check ulimit value " Open files "
verify the value of open files 
```
ulimit -a
```
increase the valie is less than 8192
```
ulimit -n 8192 
```
### Install Linux packages:
```
yum -y install mksh
yum -y install gtk2-2.24.32-5.el8.x86_64
yum -y install gtk2-2.24.32-5.el8.i686
yum -y install libXtst-1.2.3-7.el8.x86_64 
yum -y install xorg-x11-fonts-Type1-7.5-19.el8.noarch
yum -y install psmisc-23.1-5.el8.x86_64
yum -y install ksh-20120801-259.el8.x86_64 
yum -y install libXp-1.0.3-3.el8.x86_64
yum -y install libXmu-1.1.3-1.el8.x86_64
yum -y install pam-1.3.1-16.el8.x86_64 
yum -y install elfutils-0.189-3.el8.x86_64 
yum -y install elfutils-libs-0.189-3.el8.x86_64 
yum -y install libstdc++-8.5.0-20.el8.x86_64 
yum -y install libstdc++-8.5.0-20.el8.i686 
yum -y install gcc-c++-8.5.0-20.el8.x86_64
```

#### Check if packages are well installed:
```
echo gtk2 mksh libXtst xorg-x11-fonts-Type1 psmisc ksh libXp libXmu  pam elfutils elfutils-libs libstdc++ gcc-c++ | xargs -n1 rpm -q
```
### Give full permission to the user to the folder where packages are downloaded
```
chmod 777 /app/list/data/sources
```
## Installation of Websphere ND, IHS, PLG, Plugins, Java SDK, IBM Installation Manager
### Preparation of Componnent installation by unpacking them to the right forlders
Create folders and unzip packages for the following packages: Websphere ND, IHS, PLG, WAS Fix Pack 5, IHS Fix pack 5, Plugins, Java SDK, IBM Installation Manager
```
cd /app/list/data/sources/
```
```
mkdir -p /app/list/data/sources/WebSphere/ND
unzip was.repo.90501.nd.zip -d /app/list/data/sources/WebSphere/ND 
```
```
mkdir -p /app/list/data/sources/WebSphere/IHS
unzip was.repo.90501.ihs.zip -d /app/list/data/sources/WebSphere/IHS 
```
```    
mkdir -p /app/list/data/sources/WebSphere/PLG
unzip was.repo.90501.plugins.zip -d /app/list/data/sources/WebSphere/PLG 
```
```    
mkdir -p /app/list/data/sources/WAS-FP5
unzip 9.0.5-WS-WAS-FP005.zip -d /app/list/data/sources/WAS-FP5 
```
```    
mkdir -p /app/list/data/sources/IHS-FP5
unzip 9.0.5-WS-IHSPLG-FP005.zip -d /app/list/data/sources/IHS-FP5 
```
```  
mkdir -p /app/list/data/sources/WebSphere/Plugins
unzip was.repo.90501.plugins.zip -d /app/list/data/sources/WebSphere/Plugins 
```
```    
mkdir -p /app/list/data/sources/SDK
unzip ibm-java-sdk-8.0-6.16-linux-x64-installmgr.zip -d /app/list/data/sources/SDK 
```
```   
mkdir /app/list/data/sources/InstallationManager
unzip agent.installer.linux.gtk.x86_64_1.9.1003.20200730_2125.zip -d /app/list/data/sources/InstallationManager
cd InstallationManager
```

### Install IBM Installation Manager
```
cd /app/list/data/sources/InstallationManager 
./installc -acceptLicense -installationDirectory /tools/list/product/IBM/InstallationManager -dataLocation /app/list/data/IBM/InstallationManager -showProgress
```
#### Verifying the installation
```
/tools/list/product/IBM/InstallationManager/eclipse/tools/imutilsc -version
```
### Install IBM Websphere & IBM SDK
```
cd /tools/list/product/IBM/InstallationManager/eclipse 
./IBMIM --launcher.ini silent-install.ini -input /app/list/data/sources/responsefiles/websphere_install_response_file.xml -log /tmp/was_install.log -acceptLicense -showVerboseProgress
```
#### Verifying the installation
```
/tools/list/product/IBM/InstallationManager/eclipse/tools/imcl listInstalledPackages
```
### Install WebSphere Fix Pack
```
cd /tools/list/product/IBM/InstallationManager/eclipse 
./IBMIM --launcher.ini silent-install.ini -input /app/list/data/sources/responsefiles/websphere_fp5_install_response_file.xml -log /tmp/websphere_fp5_install.log -acceptLicense -showVerboseProgress
```    
#### Verifying the installation
```
/tools/list/product/IBM/InstallationManager/eclipse/tools/imcl listInstalledPackages
```
#### Positioning the WebSphere profiles directory in the data directory
```
mkdir -p /app/list/data/IBM/WebSphere/profiles
ln -s /app/list/data/IBM/WebSphere/profiles /tools/list/product/IBM/WebSphere/AppServer/profiles
```
### Install WebSphere ND
```
cd /tools/list/product/IBM/InstallationManager/eclipse 
./IBMIM --launcher.ini silent-install.ini -input /app/list/data/sources/responsefiles/websphere_install_response_file.xml -log /tmp/was_install.log -acceptLicense -showVerboseProgress
```
#### Verifying the installation
```
/tools/list/product/IBM/InstallationManager/eclipse/tools/imcl listInstalledPackages
```
### Install IBM HTTP Server
```
cd /tools/list/product/IBM/InstallationManager/eclipse 
./IBMIM --launcher.ini silent-install.ini -input /app/list/data/sources/responsefiles/ihs_install_response_file.xml -log /tmp/ihs_install.log -acceptLicense -showVerboseProgress
```
#### Verifying the installation
```
/tools/list/product/IBM/InstallationManager/eclipse/tools/imcl listInstalledPackages
```
### Install IBM HTTP Server Fix Pack
```
cd /tools/list/product/IBM/InstallationManager/eclipse 
./IBMIM --launcher.ini silent-install.ini -input /app/list/data/sources/responsefiles/ihs_fp5_install_response_file.xml -log /tmp/ihs_fp5_install.log -acceptLicense -showVerboseProgress
```
#### Verifying the installation
```
/tools/list/product/IBM/InstallationManager/eclipse/tools/imcl listInstalledPackages
```
### Install WebSphere Plugin (The plugin is directly installed with the Fix Pack)
```
cd /tools/list/product/IBM/InstallationManager/eclipse 
./IBMIM --launcher.ini silent-install.ini -input /app/list/data/sources/responsefiles/was_plugin_install_response_file.xml -log /tmp/was_plugin_fp5_install.log -acceptLicense -showVerboseProgress
```
#### Verifying the installation
```
/tools/list/product/IBM/InstallationManager/eclipse/tools/imcl listInstalledPackages
```
### Install the Security patches
```
cd /tools/list/product/IBM/InstallationManager/eclipse 
./IBMIM --launcher.ini silent-install.ini -input /app/list/data/sources/responsefiles/was_secfixes_install_response_file.xml -log /tmp/his_fp5_install.log -acceptLicense -showVerboseProgress
```
#### Verifying the installation
```
/tools/list/product/IBM/InstallationManager/eclipse/tools/imcl listInstalledPackages
```

### Expected final result of all of previous installations:
```
com.ibm.cic.agent_1.9.1003.20200730_2125
com.ibm.java.jdk.v8_8.0.6016.20200909_2045
com.ibm.websphere.ND.v90_9.0.5005.20200807_2041
9.0.0.0-WS-WAS-IFPH27557_9.0.0.20201202_1733
9.0.5.0-WS-WASProd-IFPH26220_9.0.5000.20200908_1030 
9.0.5.1-WS-WASProd-IFPH29871_9.0.5001.20201118_1049
9.0.5.2-WS-WAS-IFPH27583_9.0.5002.20200923_1011
com.ibm.java.jdk.v8_8.0.6016.20200909_2045
com.ibm.websphere.IHS.v90_9.0.5005.20200807_2041
com.ibm.java.jdk.v8_8.0.6016.20200909_2045
com.ibm.websphere.PLG.v90_9.0.5005.20200807_2041
```
### Creating the WebSphere profile
the following parameters can be changed, but need to be modified in the response files that will came later.
profileName, profilePath, serverName, adminUserName, adminPassword, hostname

#### Initiating the profile creation
```
cd /tools/list/product/IBM/WebSphere/AppServer/bin 
./manageprofiles.sh -create -profileName isim6 -profilePath "/app/list/data/IBM/WebSphere/profiles/isim6" -templatePath "/tools/list/product/IBM/WebSphere/AppServer/profileTemplates/default" -serverName isimProd -enableService false -adminUserName wasadmin -adminPassword Ibm.2023 -enableAdminSecurity true -hostname isim.demo.com
```
#### start the Websphere server with serverName as parameter
```
/app/list/data/IBM/WebSphere/profiles/isim6/bin/startServer.sh isimProd
```
#### connect to websphere using Browser and login/password defined in the initiation of the creation
```
http://hostname:9060/ibm/console
```

## Install IBM Security Directory Integrator (include SDI, JRE and SDI Fix pack)
   
### SDI Installation
#### Create a directory and untar the SDI File
```
cd /app/list/data/f89/sources 
mkdir /app/list/data/f89/sources/SDI
tar -xvf SDI_7.2_XLIN86_64_ML.tar -C /app/list/data/f89/sources/SDI
```
#### Create the SDI installation directory, this directory need to be the same than the one in the response file
```
mkdir /tools/list/product/IBM/SDI
```
#### Install SDI
```
cd /app/list/data/f89/sources/SDI/linux_x86_64 
./install_sdiv72_linux_x86_64.bin -i silent -f /app/list/data/f89/sources/responsefiles/tdi_respfile72.txt
```
#### Verify SDI Installation
```
/tools/list/product/IBM/SDI/bin/tdiVerifyInstall.sh
```

### Updating IBM SDI JRE
#### Create a directory and untar the JRE File
```
mkdir /app/list/data/f89/sources/IBM-JRE-FP
tar -zxvf /app/list/data/f89/sources/ibm-java-jre-8.0-5.30-linux-x86_64.tgz -C /app/list/data/f89/sources/IBM-JRE-FP
```
#### Apply patch
```
cd /app/list/data/f89/sources/IBM-JRE-FP/ 
mv /tools/list/product/IBM/SDI/jvm /tools/list/product/IBM/SDI/jvm.oob 
cp -rp ibm-java-x86_64-80 /tools/list/product/IBM/SDI/jvm
```
#### Verify the IBM SDI JRE Update (expected result Java version "1.8.0_201")
```
/tools/list/product/IBM/SDI/jvm/jre/bin/java -version
```
#### Modify the java.policy file path: /tools/list/product/IBM/SDI/jvm/jre/lib/security/java.policy
Replace:
```
grant codeBase "file: ${java.home} /lib/ext/*" {
        permission java.security. AllPermission;
};
```          
By: (The SDI installation path)
```
grant codeBase "file:/tools/list/product/IBM/SDI/ -" {
        permission java.security. AllPermission;
};
```
### Install the SDI Fix Pack (for REHL 8, install FP0008 or later)
#### Create a directory and untar the SDI FP0008 File
```
cd /app/list/data/f89/sources 
mkdir /app/list/data/f89/sources/SDI-FP0008
unzip 7.2.0-ISS-SDI-FP0008.zip -d /app/list/data/f89/sources/SDI-FP0008
```
### Install SDI FP0008 
```
cp /tools/list/product/IBM/SDI/maintenance/UpdateInstaller.jar /tools/list/product/IBM/SDI/maintenance/UpdateInstaller.oob 
cd /app/list/data/f89/sources/SDI-FP0008/7.2.0-ISS-SDI-FP0008/ 
cp UpdateInstaller.jar /tools/list/product/IBM/SDI/maintenance/ 
/tools/list/product/IBM/SDI/bin/applyUpdates.sh -update SDI-7.2-FP0008.zip
```
#### Correct Log4j & AMC 
```
cd /tools/list/product/IBM/SDI/bin
chmod 744 ./bin/updateLog4j.sh ./bin/removeAMC.sh
./updateLog4j.sh
./removeAMC.sh
```
#### Verify SDI FP0008 Installation 
```
/tools/list/product/IBM/SDI/bin/applyUpdates.sh -queryreg
```

## Install IBM RMI Dispatcher
### Create a directory and untar the RMI DIspatcher File
```
cd /app/list/data/f89/sources 
       mkdir /app/list/data/f89/sources/IBM-RMI
       unzip SIA_RMI_7140_SDI_7X_MP_ML.zip -d /app/list/data/f89/sources/IBM-RMI
```
### Install RMI Dispatcher
```
cd /app/list/data/f89/sources/IBM-RMI/ 
/tools/list/product/IBM/SDI/jvm/jre/bin/java -jar DispatcherInstall.jar -i silent -DUSER_INSTALL_DIR="/tools/list/product/IBM/SDI" -DUSER_SELECTED_SOLDIR="/tools/list/product/IBM/SDI/timsol" -DUSER_INPUT_RMI_PORTNUMBER=1099 -DUSER_INPUT_WS_PORTNUMBER=1098
```    
### Verify RMI Dispatcher Installation
Check the installation status in the Dispatcher_Installer.log file in the current installation directory

### Modify the solution.properties file 
by adding the following lines in the end of the file (path: /tools/list/product/IBM/SDI/timsol/solution.properties)
```
com.ibm.di.SSLProtocols=TLSv1.2
com.ibm.di.SSLServerProtocols=TLSv1.2
com.ibm.jsse2.overrideDefaultProtocol=TLSv12
```

## Install IBM DB2
### Create a directory and untar the DB2 File
```
cd /app/list/data/f89/sources 
mkdir -p /app/list/data/f89/sources/DB2_v11.5.9
tar -xf v11.5.9_linuxx64_universal_fixpack.tar.gz -C /app/list/data/f89/sources/DB2_v11.5.9
```
### Install IBM DB2
```
cd /app/list/data/f89/sources/DB2_v11.5.9/universal 
./db2_install -b /tools/list/product/IBM/db2/V11.5.9 -p SERVER -n -y
```
### Verify IBM DB2 Installation
```
/tools/list/product/IBM/db2/V11.5.9/adm/db2licm -l
```
### Adding license key
```
cd /app/list/data/f89/sources 
unzip DB2_SEVPC_OA_11.5.4_MP_ML.zip
/tools/list/product/IBM/db2/V11.5.9/adm/db2licm -a /app/list/data/f89/sources/std_vpc/db2/license/db2std_vpc.lic
```
### Verify license
```
/tools/list/product/IBM/db2/V11.5.9/adm/db2licm -l
```

## Install IBM Security Directory Server (SDS, JAVA, GSKit, Fix Pack)

### Install GSKit
#### Untar the GSKit file and install rpms
```
cd /app/list/data/f89/sources 
tar -zxf 8.0.55.31-ISS-GSKIT-LinuxX64-FP0031.tar.gz
cd 8.0.55.31-ISS-GSKIT-LinuxX64-FP0031/
rpm -Uhv 64/*  
```
#### Verify GSKit Installation
```
rpm -qa | grep -i gsk
```
### Install IBM Security Directory Server (if you use rpm untar if you use iso mount the iso)
#### Untar the file (i'm not using iso file)
```
cd /app/list/data/f89/sources 
tar -zxf sds-6.4.0.25-linux-x86-64.tar.gz
cd sdsV6.4
```        
#### Create SDS folder
```
mkdir -p /opt/ibm/ldap/V6.4/install
touch /opt/ibm/ldap/V6.4/install/IBMLDAP_INSTALL_SKIPDB2REQ
```
#### Agree and accept SDS 6.4 license
```
cd license
./idsLicense -q
```       
#### Install IBM SDS 
```
cd ../images
rpm -ihv --force idsldap-license64* idsldap-clt* idsldap-msg64-en* idsldap-srv* idsldap-web*
```
#### Verify IBM SDS Installation
```
rpm -qa | grep -i idsldap
```
#### Remove the IBMLDAP_INSTALL_SKIPDB2REQ file.
```
/bin/rm /opt/ibm/ldap/V6.4/install/IBMLDAP_INSTALL_SKIPDB2REQ
```        
    
### Install IBM Security Directory Server Fix Pack 

#### Unpack and install latest recommended SDS 6.4 Fix Level:
```
cd /app/list/data/f89/sources
tar -zxf 6.4.0.28-ISS-ISDS-LinuxX64-IF0028.tar.gz
cd 6.4.0.28-ISS-ISDS-LinuxX64-IF0028/
./idsinstall -u -f
```
<Press 1 and then enter key if it asks for license agreement>

#### Verify by using:
```
rpm -qa | grep -i idsldap
```
### Install Java for SDS
####  Install Java:
```
cd /opt/ibm/ldap/V6.4/
tar -xf /app/list/data/f89/sources/8.0.8.5-ISS-JAVA-LinuxX64-FP005.tar
```
#### Verify Java Installation
```
ls -la (Java folder should be present in /opt/ibm/ldap/V6.4/)
```
 
#### Create soft links for SDS tools:
```
/opt/ibm/ldap/V6.4/bin/idslink -i -s fullsrv -f
```
#### Create or update /opt/ibm/ldap/V6.4/etc/ldapdb.properties file
```
ls -l /opt/ibm/ldap/V6.4/etc/ldapdb.properties
```
If the file is not existing, then:
```
echo "currentDB2InstallPath=/tools/list/product/IBM/db2/V11.5.9" > /opt/ibm/ldap/V6.4/etc/ldapdb.properties
echo "currentDB2Version=11.5.9" >> /opt/ibm/ldap/V6.4/etc/ldapdb.properties
```
If the file exists, then use vi editor to make sure there are two lines in there as follows:
```
currentDB2InstallPath=/tools/list/product/IBM/db2/V11.5.9
currentDB2Version=11.5.9
```

## Configure IBM DB2

### Create OS Users for DB2 administration server
```
useradd db2das1 
passwd db2das1
```  
### Create OS Users for DB2 administrator user
```
useradd db2inst1 
passwd db2inst1
```
### Create OS Users for DB2 that will be used by ISIM
```
useradd isimdbuser 
passwd isimdbuser
```
### Create Db2 administration server (DAS) User in the OS
```
cd /tools/list/product/IBM/db2/V11.5.9/instance 
./dascrt -u db2das1
```
### Create the DB2 Instance 
```
./db2icrt -a server -u db2das1 db2inst1
```
### Create the DB2 database (itimdb the name of the database)
```
cd /tools/list/product/IBM/db2/V11.5.9/instance/
mkdir -p /app/list/data/f89/IBM/instances/db2
chown db2inst1:db2inst1 /app/list/data/f89/IBM/instances/db2
su - db2inst1 
db2start 
db2 create database itimdb on /app/list/data/f89/IBM/instances/db2 using codeset UTF-8 territory us
```
### Connect to the database (os user: db2inst1) and execute the following SQL commands
```
db2 connect to itimdb
db2 create bufferpool ENROLEBP size automatic pagesize 32k 
db2 update db cfg for itimdb using logsecond 12 
db2 update db cfg for itimdb using logfilsiz 10000 
db2 update db cfg for itimdb using auto_runstats off 
db2 grant createtab, create_external_routine, connect, implicit_schema on database to user isimdbuser 
db2 disconnect current 
db2stop 
db2start 
db2 update database manager configuration using svcename 50000 
db2set DB2COMM=tcpip db2set DB2AUTH=OSAUTHDB db2set -all 
db2 commit 
db2 force application all 
exit
```
### Stop and start the DB2 (as root user)
```
su - db2inst1 -c "db2stop && db2start" 
```
### Optionally, add firewall entry on firewalld to allow external connections to port 50000 
```
firewall-cmd --add-port=50000/tcp --permanent 
firewall-cmd --add-port=50000/tcp
```


## Configure IBM SDS

### Create OS user to own the LDAP instance:
```
idsadduser -u idsldap -w Ibm.2023 -l /home/idsldap -g idsldap -n  
```
### Change permissions
```
usermod -aG idsldap root 
mkdir /app/list/data/f89/IBM/instances/ldapdb2 
chmod -R 755 /tools/list/product/IBM/db2/V11.5.9/instance
chmod 777 /app/list/data/f89/IBM/instances/ldapdb2/idsslapd-idsldap/etc/ibmslapd.conf
```
### Create LDAP instance    
```
idsicrt -I idsldap -e encrypt_seed -g encrypt_salt -l /app/list/data/f89/IBM/instances/ldapdb2 -r "Default LdapDb2 Instance" -n
```
### Verify the LDAP instance creation   
```
/opt/ibm/ldap/V6.4/sbin/idsilist -a
```
### Modify right on the IBM & instance
```
chmod -R 755 /app/list/data/f89/IBM
```
### Configure database for LDAP
```
idscfgdb -I idsldap -a idsldap -w Ibm.2023 -t idsldap -l /app/list/data/f89/IBM/instances/ldapdb2 -n
```  
### Configure administrator DN and password
```
idsdnpw -I idsldap -u cn=root -p Ibm.2023 -n
```   
### Add LDAP suffix
```
idscfgsuf -I idsldap -s dc=swisslife,dc=com -n
```
### Create the following LDIF file
```
cat > /tmp/suffix.ldif <<EOF
dn:dc=swisslife,dc=com
objectclass:domain
EOF
```
### Add suffix context
```
idsldif2db -I idsldap -i /tmp/suffix.ldif
```
### Execute Runstats on the instance
```
idsrunstats -I idsldap
```
### Start the instance
```
ibmslapd -I idsldap -n 
ibmdiradm -I idsldap -k 
ibmdiradm -I idsldap
 ```   
### Optionally, whitelist port 389 on firwalld to allow external connections 
```
firewall-cmd --add-port=389/tcp --permanent 
firewall-cmd --add-port=389/tcp
```

Install IBM Verify Identity Manager

### Configure installvariables.properties file
Fill the following parameters
```
            KEYSTORE_PASSWORD= (choose password for the IBM Security Identity Manager keystore)
            SERVER_NAME= isimProd (created during Websphere profile creation)
            WAS_PASSWORD=wasadmin
            WAS_PASSWORD=Ibm.2023
            EJB_USER=isimsystem (Specify the WAS administrator's user id )
            EJB_USER_PWD=Ibm.2023 (Specify the WAS administrator's user password )
            SHORT_LOCAL_HOST_NAME= isim.demo.com 
            WAS_HOME= tools/list/product/IBM/WebSphere/AppServer (Websphere installation home if not default home)
            WAS_PROFILE_NAME=isim6 (Websphere profile name created in my env isim6)
            ORG_NAME= (your organisation name)
            TENANT_ID= (your tenant ID)
            ROOT_DN=cn=root
```
### Configure configResponse.properties file
Fill the following parameters
```
            ldapConfigResponse.enrole.ldapserver.ip=[Nom du serveur LDAP] 
            ldapConfigResponse.java.naming.security.principal=cn=root
            ldapConfigResponse.java.naming.security.credentials=[Mot de passe cn=root] 
            ldapConfigResponse.enrole.defaulttenant.id=[Your tenantID] 
            ldapConfigResponse.enrole.organization.name=[Your Organisation Name]
            ldapConfigResponse.enrole.ldapserver.root=[Your root DN ex: dc=company,dc=local]
            dbConfigResponse.database.db.ip=[Nom dur serveur de base de données] 
            dbConfigResponse.database.db.user=isimdbuser
            dbConfigResponse.database.db.password=[password for db user that will be used by isim]
            dbConfigResponse.database.db.adminPwd=DB2ADMIN_USER_PASSWORD 
            sysConfigResponse.enRoleMail.mail.baseurl=[Login page URL] 
            sysConfigResponse.enRole.enrole.appServer.systemUser.credentials=WAS_ADMIN_PASSWORD 
            sysConfigResponse.enRole.enrole.appServer.ejbuser.credentials=EJB_USER_PASSWORD
```
### Extract installation file
```
mkdir -p /tmp/isim/
unzip /app/list/data/f89/sources/IM_SEC_VFY_GVN_IGE_v10.0.2_LINUX.zip -d /tmp/isim/ 
cd /tmp/isim/ 
chmod +x instlinux.bin
```
### Use the right Java SDK
```
rm -rf /usr/bin/java 
ln -s /tools/list/product/IBM/WebSphere/AppServer/java/8.0/jre/bin/java /usr/bin/java
```
### Start the Installation
```
./instlinux.bin LAX_VM /tools/list/product/IBM/WebSphere/AppServer/java/8.0/jre/bin/java -i silent -f /app/list/data/f89/sources/responsefiles/installvariables.properties -DITIM_CFG_RESP_FILE_DIR=/app/list/data/f89/sources/responsefiles
```   
### Monitor Installation (open a new terminal)
```
tail -f /tools/list/product/IBM/isim/install_logs/itim_install_activity.log
```


## Authors
@Amine HAMIDA ====> amine.hamida@ibm.com
@Ayoub BAHAR  ====> ayoub.bahar@ibm.com
