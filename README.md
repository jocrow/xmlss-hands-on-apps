XML Summer School - Hands On - Applications
-------------------------------------------
These are the applications used for teaching the "Hands On" course
at the XML Summer School 2012. http://xmlsummerschool.com/curriculum-2012/hands-on-introduction-2012/

The source repository now lives here on GitHub.

The seewhatithink.com application runs on eXist-db 2.0 and is provided as a database backup. To install
eXist-db visit http://www.exist-db.org.

The Printer application is written in Scala and requires you to have Maven installed to compile
and run the application. Maven can be installed by visiting http://maven.apache.org/

About
-----
These are the instructions for installing the seewhatithink.com
training Web Application, Printer Application and class room tools
into an Oracle VirtialBox Virtual Machine with Ubuntu Linux
for the XML Summer School www.xmlsummerschool.com

If you have a Ubuntu Linux machine then you may ignore the
Virtual Box pre-requisite install steps.

Author: Adam Retter <adam.retter@googlemail.com>
Licence: Apache License Version 2.0



Requirements
------------
Oracle/Sun Virtual Box >= 4.1.0
available from - http://www.virtualbox.org/wiki/Downloads

Ubuntu Linux 10.04 LTS Desktop Install CD (32 bit or 64 bit)
available from - http://www.ubuntu.com/download/ubuntu/download



Virtual Box Pre-requisites
--------------------------
1) Install the VirtualBox software for your Operating System.

2) Install a Ubuntu Virtual Machine into Virtual Box

3) Install the VBox Guest Additions into Virtual Box
When prompted choose the username 'xmlss' for the default user
and the password 'xmlss'.

NOTE - Exact instructions for the above have not been detailed,
as they can easily be found via Google.



How to Install XMLSS Training Application Pre-Requisites
--------------------------------------------------------
The training applications for the XMLSS require
Subversion client, Sun JDK 6, Maven 3, Scala 2.9
and MySQL Server 5.1 to be installed first.

1) Enable the lucid patner package repository, by uncommenting the line from /etc/apt/sources.list e.g.

sudo vi /etc/apt/sources.list

deb http://archive.canonical.com/ubuntu lucid partner


2) Install Subversion, MySQL Server 5.1 and Sun JDK 6 packages

sudo apt-get install subversion mysql-server sun-java-6-jdk sun-java-6-source sun-java-6-fonts sun-java-6-plugin

When prompted set the mysql root password as 'root'


3) Set the JAVA_HOME environment variable.
Append the following line to the file /home/xmlss/.profile

export JAVA_HOME="/usr/lib/jvm/java-6-sun"


4) Download and install Maven 3.0.3 from maven.apache.com
wget http://mirrors.ukfast.co.uk/sites/ftp.apache.org//maven/binaries/apache-maven-3.0.3-bin.tar.gz
tar zxvf apache-maven-3.0.3-bin.tar.gz
sudo mv apache-maven-3.0.3 /usr/local
sudo ln -s /usr/local/apache-maven-3.0.3 /usr/local/maven


5) Download and install Scala 2.9.1 from www.scala-lang.org
wget http://www.scala-lang.org/downloads/distrib/files/scala-2.9.1.final-installer.jar
sudo java -jar ./scala-2.9.1.final-installer.jar

When prompted, choose /usr/local/scala as the installation path.


6) Add Maven and Scala to your PATH environment variable.
Append the following line to /home/xmlss/.profile

PATH="$PATH:/usr/local/maven/bin:/usr/local/scala/bin"



How to Install XMLSS whatithink.com eXist-db Training Web Application
---------------------------------------------------------------------
1) Checkout eXist-db trunk from SourceForge Subversion e.g.

sudo svn co http://svn.code.sf.net/p/exist/code/trunk/eXist /usr/local/eXist
sudo chown -R xmlss:xmlss /usr/local/eXist


2) Set an Environment Variable EXIST_HOME that points to the location where you just checked out eXist-db.
Append the following line to /home/xmlss/.profile, and also execute the same line at the command prompt.

export EXIST_HOME="/usr/local/eXist"


3) Enable the xslfo module in $EXIST_HOME/extensions/build.properties e.g.

# XSL FO transformations (Uses Apache FOP)
include.module.xslfo = true


4) Build the eXist-db source code 
cd $EXIST_HOME
./build.sh


5) Uncomment the xslfo XQuery extension module in $EXIST_HOME/conf.xml e.g.

<module uri="http://exist-db.org/xquery/xslfo" class="org.exist.xquery.modules.xslfo.XSLFOModule">
    <parameter name="processorAdapter" value="org.exist.xquery.modules.xslfo.ApacheFopProcessorAdapter"/>
</module>


6) Uncomment datetime XQuery extension module in $EXIST_HOME/conf.xml e.g.

<module uri="http://exist-db.org/xquery/datetime" class="org.exist.xquery.modules.datetime.DateTimeModule"/>


7) Checkout the eXist-db database backup of the whatithink.com web application
from SourceForge Subversion e.g.

sudo svn co https://seewhatithink.svn.sourceforge.net/svnroot/seewhatithink/trunk /usr/local/seewhatithink
sudo chown -R xmlss:xmlss /usr/local/seewhatithink


8) Startup the eXist-db database e.g.

cd $EXIST_HOME
bin/startup.sh


9) Restore the database backup from Step (8) e.g.

cd $EXIST_HOME
bin/backup.sh -u admin -r ../seewhatithink/applications/whatithink.com/db/__contents__.xml 
 

10) Shutdown the eXist-db database 
cd $EXIST_HOME
bin/shutdown.sh


11) Startup the eXist-db database 
cd $EXIST_HOME
bin/startup.sh


*** You can now visit the web application at http://localhost:8080/exist/apps/xmlss/whatithink.com/



How to Install XMLSS whatithink.com Printers Training Web Application
---------------------------------------------------------------------
1) Create the MySQL database for the Printer web application -
cd /usr/local/seewhatithink/applications/Printer-database
mysql -u root -p < create-database.sql


2) Load the MySQL database schema for the Printer web application -
cd /usr/local/seewhatithink/applications/Printer-database
mysql -u root -p -f xmlss_printer < printer-mysql.sql


3) Build the Printer web application
cd /usr/local/seewhatithink/applications/Printer
mvn install


4) Run the Printer web application
cd /usr/local/seewhatithink/applications/Printer
./printer.sh

*** You can now visit the Printer web application at http://localhost:9090/



How to Install additional Class Room software for the XMLSS training course
---------------------------------------------------------------------------
1) Download and Install NetBeans 7.0 Java EE edition from www.netbeans.com

sudo ./netbeans-7.0-ml-javaee-linux.sh
De-select Glassfish, Select Tomcat
Install to /usr/local

2) Download the ubuntu package for MySQL Workbench from the MySQL web site
sudo apt-get install libctemplate0 libzip1 python-pysqlite2 mysql-client python-crypto python-paramiko liblua5.1-0
sudo dpkg -i mysql-workbench-gpl-5.2.27-1ubu1004-i386.deb

3) Download and Install Aqua Datastudio from www.aquafold.com
Studio - LINUX (x86 - 32bit) (build: 9.0.15)

tar zxvf ads-linux-x86-9.0.15.tar.gz
sudo mv datastudio /usr/local
Add Item to Applications->Programming Menu


4) Download and Install oXygen 12 from www.oxygenxml.com
http://www.oxygenxml.com/download_oxygenxml_editor.html

chmod +x oxygen-Xbit.sh
sudo ./oxygen-Xbit.sh
Install to /usr/local/Oxygen XML Editor 12
Add Item to Applications->Programming Menu


5) Download and Install Protege 4.1 from http://protege.stanford.edu/

chmod +x install_protege_4.1rc4.bin
sudo ./install_protege_4.1rc4.bin
Install to /usr/local/Protege_4.1_rc4
Add Item to Applications->Programming Menu


6) Download and Install Google Chrome from http://www.google.com/chrome
Choose the .deb package for either the 32bit or 64bit version of Ubuntu
that you previously installed.

sudo dpkg -i google-chrome-stable_current_i386.deb


7) Cosmetics - 
    * Add Shortcuts to the desktop for eXist-db Server, eXist-db Client and Printer Webapp
    * Add Shortcuts to the desktop for the two webapps

TODO -
Install oXygen licence and Aqua DataStudio licence
Add eXist-db to Server Browser in oXygen
Add MySQL to Server Browser in Aqua DataStudio


Howto Shrinking the VM disk in VirtualBox
-----------------------------------------
If you find that your Virtual Machine hard disk file
has become rather large on your Host machine, then you
can attempt to compact the .vdi file


In the guest Ubuntu Virtual Machine - 

sudo apt-get install zerofree
sudo apt-get clean
restart

At startup choose Recovery Mode, and then at the Menu first choose "Clean".

Restart the VM with the Boot CD in the virtual cd, choose 'Try Ubuntu without Installing'

sudo mkdir /mnt/sda1
sudo mount -n -o ro -t ext4 /dev/sda1 /mnt/sda1 
sudo /mnt/sda1/usr/sbin/zerofree /dev/sda1

Shutdown the Guest when done and remove the media from the virtual cd drive.

On the host you now need to compact the VDI file - e.g. -
$ VBoxManage modifyhd XMLSS11-Test-VM-64bit.vdi --compact
