https://www.sonarqube.org/

https://docs.sonarqube.org/latest/architecture/architecture-integration/

https://hub.docker.com/_/sonarqube

SonarQube is an open source platform for continuous inspection of code quality.

docker run -d --name sonarqube \
    -p 9000:9000 \
    -e sonar.jdbc.username=sonar \
    -e sonar.jdbc.password=sonar \
    -e sonar.jdbc.url=jdbc:postgresql://localhost/sonar \
    sonarqube
    
Option 4: Customized image
In some environments, it may make more sense to prepare a custom image containing your configuration. A Dockerfile to achieve this may be as simple as:

FROM sonarqube:7.4-community
COPY sonar.properties /opt/sonarqube/conf/
You could then build and try the image with something like:

$ docker build --tag=sonarqube-custom .
$ docker run -ti sonarqube-custom


https://www.vogella.com/tutorials/SonarQube/article.html

$ docker run -d --name sonarqube -p 9000:9000 -p 9092:9092 sonarqube

mvn sonar:sonar   -Dsonar.host.url=http://localhost:9000   -Dsonar.login=yourkey

mvn sonar:sonar -Dsonar.analysis.mode=preview -Dsonar.issuesReport.html.enable=true

https://docs.sonarqube.org/latest/

https://github.com/SonarSource/docker-sonarqube/blob/master/recipes.md

https://github.com/SonarSource/docker-sonarqube/blob/master/recipes/docker-compose-postgres-example.yml



https://docs.sonarqube.org/latest/user-guide/fixing-the-water-leak/


Integration piece..


Developers code in their IDEs and use SonarLint to run local analysis.
Developers push their code into their favourite SCM : git, SVN, TFVC, ...
The Continuous Integration Server triggers an automatic build, and the execution of the SonarScanner required to run the SonarQube analysis.
The analysis report is sent to the SonarQube Server for processing.
SonarQube Server processes and stores the analysis report results in the SonarQube Database, and displays the results in the UI.
Developers review, comment, challenge their Issues to manage and reduce their Technical Debt through the SonarQube UI.
Managers receive Reports from the analysis. Ops use APIs to automate configuration and extract data from SonarQube. Ops use JMX to monitor SonarQube Server.


The SonarQube Server and the SonarQube Database must be located in the same network
SonarScanners don't need to be on the same network as the SonarQube Server.
There is no communication between SonarScanners and the SonarQube Database.

https://docs.sonarqube.org/latest/requirements/requirements/

Platform notes
Linux
If you're running on Linux, you must ensure that:

vm.max_map_count is greater or equals to 262144
fs.file-max is greater or equals to 65536
the user running SonarQube can open at least 65536 file descriptors
the user running SonarQube can open at least 2048 threads
You can see the values with the following commands:

sysctl vm.max_map_count
sysctl fs.file-max
ulimit -n
ulimit -u
You can set them dynamically for the current session by running the following commands as root:

sysctl -w vm.max_map_count=262144
sysctl -w fs.file-max=65536
ulimit -n 65536
ulimit -u 2048
To set these values more permanently, you must update either /etc/sysctl.d/99-sonarqube.conf (or /etc/sysctl.conf as you wish) to reflect these values.

If the user running SonarQube (sonarqube in this example) does not have the permission to have at least 65536 open descriptors, you must insert this line in /etc/security/limits.d/99-sonarqube.conf (or /etc/security/limits.conf as you wish):

sonarqube   -   nofile   65536
sonarqube   -   nproc    2048
You can get more detail in the Elasticsearch documentation.

If you are using systemd to start SonarQube, you must specify those limits inside your unit file in the section [service] :

[Service]
...
LimitNOFILE=65536
LimitNPROC=2048
...



seccomp filter
By default, Elasticsearch uses seccomp filter. On most distribution this feature is activated in the kernel, however on distributions like Red Hat Linux 6 this feature is deactivated. If you are using a distribution without this feature and you cannot upgrade to a newer version with seccomp activated, you have to explicitly deactivate this security layer by updating sonar.search.javaAdditionalOpts in $SONARQUBEHOME/conf/sonar.properties_:

sonar.search.javaAdditionalOpts=-Dbootstrap.system_call_filter=false
You can check if seccomp is available on your kernel with:

$ grep SECCOMP /boot/config-$(uname -r)
If your kernel has seccomp, you will see:

CONFIG_HAVE_ARCH_SECCOMP_FILTER=y
CONFIG_SECCOMP_FILTER=y
CONFIG_SECCOMP=y
For more detail, see the Elasticsearch documentation.



https://docs.sonarqube.org/latest/requirements/benchmark/

https://docs.sonarqube.org/latest/requirements/hardware-recommendations/

https://docs.sonarqube.org/latest/setup/overview/

https://docs.sonarqube.org/latest/setup/get-started-2-minutes/


https://docs.sonarqube.org/latest/setup/install-server/

https://docs.sonarqube.org/latest/setup/install-server/


reate an empty schema and a sonarqube user. Grant this sonarqube user permissions to create, update, and delete objects for this schema.


If you want to use a custom schema and not the default "public" one, the PostgreSQL search_path property must be set:

ALTER USER mySonarUser SET search_path to mySonarQubeSchema

Installing the Web Server
First, check the requirements. Then download and unzip the distribution (do not unzip into a directory starting with a digit).

SonarQube cannot be run as root on Unix-based systems, so create a dedicated user account to use for SonarQube if necessary.

$SONARQUBE-HOME (below) refers to the path to the directory where the SonarQube distribution has been unzipped.

Setting the Access to the Database
Edit $SONARQUBE-HOME/conf/sonar.properties to configure the database settings. Templates are available for every supported database. Just uncomment and configure the template you need and comment out the lines dedicated to H2:

Example for PostgreSQL
sonar.jdbc.username=sonarqube
sonar.jdbc.password=mypassword
sonar.jdbc.url=jdbc:postgresql://localhost/sonarqube


Adding the JDBC Driver
Drivers for the supported databases (except Oracle) are already provided. Do not replace the provided drivers; they are the only ones supported.

For Oracle, copy the JDBC driver into $SONARQUBE-HOME/extensions/jdbc-driver/oracle.

Configuring the Elasticsearch storage path
By default, Elasticsearch data is stored in $SONARQUBE-HOME/data, but this is not recommended for production instances. Instead, you should store this data elsewhere, ideally in a dedicated volume with fast I/O. Beyond maintaining acceptable performance, doing so will also ease the upgrade of SonarQube.

Edit $SONARQUBE-HOME/conf/sonar.properties to configure the following settings:

sonar.path.data=/var/sonarqube/data
sonar.path.temp=/var/sonarqube/temp
The user used to launch SonarQube must have read and write access to those directories.

Starting the Web Server
The default port is "9000" and the context path is "/". These values can be changed in $SONARQUBE-HOME/conf/sonar.properties:

sonar.web.host=192.0.0.1
sonar.web.port=80
sonar.web.context=/sonarqube
Execute the following script to start the server:

On Linux/Mac OS: bin//sonar.sh start
On Windows: bin/windows-x86-XX/StartSonar.bat
You can now browse SonarQube at http://localhost:9000 (the default System administrator credentials are admin/admin).

Tuning the Web Server
By default, SonarQube is configured to run on any computer with a simple Java JRE.

For better performance, the first thing to do when installing a production instance is to use a Java JDK and activate the server mode by uncommenting/setting the following line in $SONARQUBE-HOME/conf/sonar.properties:

sonar.web.javaOpts=-server
To change the Java JVM used by SonarQube, simply edit $SONARQUBE-HOME/conf/wrapper.conf and update the following line:

wrapper.java.command=/path/to/my/jdk/bin/java
Advanced Installation Features
Running SonarQube as a Service on Windows or Linux
Running SonarQube behind a Proxy
Running SonarQube Community Edition with Docker
Next Steps
Once your server is installed and running, you may also want to Install Plugins. Then you're ready to begin Analyzing Source Code.



https://docs.sonarqube.org/latest/setup/operate-server/


Configure & Operate the Server
Running SonarQube as a Service on Windows
Install/uninstall NT service (may have to run these files via Run As Administrator):
%SONARQUBE_HOME%/bin/windows-x86-64/InstallNTService.bat
%SONARQUBE_HOME%/bin/windows-x86-64/UninstallNTService.bat
Start/stop the service:
%SONARQUBE_HOME%/bin/windows-x86-64/StartNTService.bat
%SONARQUBE_HOME%/bin/windows-x86-64/StopNTService.bat
Running SonarQube as a Service on Linux with SystemD
On Unix system using SystemD, you can install SonarQube as a service. You cannot run SonarQube as root in 'nix systems. Ideally, you will created a new account dedicated to the purpose of running SonarQube. Let's suppose:

The user used to start the service is sonarqube
The group used to start the service is sonarqube
The Java Virtual Machine is installed in /opt/java/
SonarQube has been unzipped into /opt/sonarqube/
Then create the file /etc/systemd/system/sonarqube.service based on the following

[Unit]
Description=SonarQube service
After=syslog.target network.target

[Service]
Type=simple
User=sonarqube
Group=sonarqube
PermissionsStartOnly=true
ExecStart=/bin/nohup /opt/java/bin/java -Xms32m -Xmx32m -Djava.net.preferIPv4Stack=true -jar /opt/sonarqube/lib/sonar-application-7.4.jar
StandardOutput=syslog
LimitNOFILE=65536
LimitNPROC=8192
TimeoutStartSec=5
Restart=always

[Install]
WantedBy=multi-user.target
Note

Because the sonar-application jar name ends with the version of SonarQube, you will need to adjust the ExecStart command accordingly on install and at each upgrade.
The SonarQube data directory, /opt/sonarqube/data, and the extensions directory, /opt/sonarqube/extensions should be owned by the sonarqube user. As a good practice, the rest should be owned by root
Once your sonarqube.service file is created and properly configured, run

sudo systemctl enable sonarqube.service
sudo systemctl start sonarqube.service
Running SonarQube as a Service on Linux with initd
The following has been tested on Ubuntu 8.10 and CentOS 6.2.

Create the file /etc/init.d/sonar with this content:

#!/bin/sh
#
# rc file for SonarQube
#
# chkconfig: 345 96 10
# description: SonarQube system (www.sonarsource.org)
#
### BEGIN INIT INFO
# Provides: sonar
# Required-Start: $network
# Required-Stop: $network
# Default-Start: 3 4 5
# Default-Stop: 0 1 2 6
# Short-Description: SonarQube system (www.sonarsource.org)
# Description: SonarQube system (www.sonarsource.org)
### END INIT INFO
 
/usr/bin/sonar $*
Register SonarQube at boot time (RedHat, CentOS, 64 bit):

sudo ln -s $SONAR_HOME/bin/linux-x86-64/sonar.sh /usr/bin/sonar
sudo chmod 755 /etc/init.d/sonar
sudo chkconfig --add sonar
Securing the Server Behind a Proxy
This section helps you configure the SonarQube Server if you want to run it behind a proxy. This can be done for security concerns or to consolidate multiple disparate applications.

Server Configuration
To run the SonarQube server over HTTPS, you must build a standard reverse proxy infrastructure.

The reverse proxy must be configured to set the value X_FORWARDED_PROTO: https in each HTTP request header. Without this property, redirection initiated by the SonarQube server will fall back on HTTP.

Using an Apache Proxy
We assume that you've already installed Apache 2 with module modproxy, that SonarQube is running and available on `http://privatesonarhost:sonarport/and that you want to configure a Virtual Host forwww.public_sonar.com`.

At this point, edit the HTTPd configuration file for the www.public_sonar.com virtual host. Include the following to expose SonarQube via mod_proxy at http://www.public_sonar.com/:

ProxyRequests Off
ProxyPreserveHost On
<VirtualHost *:80>
  ServerName www.public_sonar.com
  ServerAdmin admin@somecompany.com
  ProxyPass / http://private_sonar_host:sonar_port/
  ProxyPassReverse / http://www.public_sonar.com/
  ErrorLog logs/somecompany/sonar/error.log
  CustomLog logs/somecompany/sonar/access.log common
</VirtualHost>
Apache configuration is going to vary based on your own application's requirements and the way you intend to expose SonarQube to the outside world. If you need more details about Apache HTTPd and mod_proxy, please see http://httpd.apache.org.



Troubleshooting/FAQ
Grant more memory to the web server / compute engine / elastic search
To grant more memory to a server-side process, uncomment and edit the relevant javaOpts property in $SONARQUBE_HOME/conf/sonar.properties, specifically:

sonar.web.javaOpts (minimum values: -server -Xmx768m)
sonar.ce.javaOpts
sonar.search.javaOpts

Failed to connect to the Marketplace via proxy
Double check that settings for proxy are correctly set in $SONARQUBE_HOME/conf/sonar.properties. Note that if your proxy username contains "" (backslash), then it should be escaped - for example username "domain\user" in file should look like:

http.proxyUser=domain\\user
For some proxies, the exception "java.net.ProtocolException: Server redirected too many times" might mean an incorrect username or password has been configured.

Exception java.lang.RuntimeException: can not run elasticsearch as root
SonarQube starts an Elasticsearch process, and the same account that is running SonarQube itself will be used for the Elasticsearch process. Since Elasticsearch cannot be run as root, that means SonarQube can't be either. You must choose some other, non-root account with which to run SonarQube, preferably an account dedicated to the purpose.



https://docs.sonarqube.org/latest/setup/install-plugin/

Install a Plugin
There are two options to install a plugin into SonarQube:

Marketplace - Installs plugins automatically, from the SonarQube UI.
Manual Installation - You'll use this method if your SonarQube instance doesn't have access to the Internet.
Marketplace
If you have access to the Internet and you are connected with a SonarQube user having the Global Permission "Administer System", you can go to Administration > Marketplace.

Find the plugin you want to install
Click on Install and wait for the download to be processed
Once download is complete, a "Restart" button will be available to restart your instance.

See Marketplace for more details on how to configure your SonarQube Server to connect to the Internet.

Manual Installation
In the page dedicated to the plugin you want to install (ex: for Python: SonarPython), click on the "Download" link of the version compatible with your SonarQube version.

Put the downloaded jar in $SONARQUBE_HOME/extensions/plugins, removing any previous versions of the same plugins.

Once done, you will need to restart your SonarQube Server.

License
If you installed a Commercial Edition, you will need to set the License Key in Administration > Configuration > License Manager before being able to use it.


