#!/bin/bash
dnf -y install httpd libnsl httpd-devel mod_ssl gcc redhat-rpm-config chkconfig procps wget nano tar
wget https://web.archive.org/web/20120629104002/http://download.macromedia.com/pub/coldfusion/cf9_installer/ColdFusion_9_WWEJ_linux64.bin -O ColdFusion_9_WWEJ_linux64.bin
chmod 755 ColdFusion_9_WWEJ_linux64.bin
wget https://rpmfind.net/linux/centos/7.9.2009/os/x86_64/Packages/compat-libstdc++-33-3.2.3-72.el7.i686.rpm
dnf -y install compat-libstdc++-33-3.2.3-72.el7.i686.rpm
systemctl enable httpd
cp /usr/bin/apxs /usr/sbin/apxs
rm -f compat-libstdc++-33-3.2.3-72.el7.i686.rpm

echo "" > installer.properties
echo "INSTALLER_UI=SILENT
SILENT_LICENSE_MODE=developer
# SILENT_SERIAL_NUMBER=serial_number
# Serial number of the previous version of ColdFusion.
# This is required only if the serial number you specified as the SILENT_SERIAL_NUMBER is a serial number for an upgrade.
# SILENT_PREV_SERIAL_NUMBER=serial_number
SILENT_WEBROOT_FOLDER=/var/www/html
# Applies only for Windows.
# Whether to install ColdFusion ODBC Services.
SILENT_ENABLE_RDS=false
SILENT_INSTALL_ODBC=false
SILENT_INSTALL_VERITY=false
SILENT_INSTALL_SOLR=false
# Whether to install the Getting Started Experience, Tutorials, and Documentation.
# Values are true and false.
# Set the value to false if you are installing in a production environment.
SILENT_INSTALL_SAMPLES=false
# Applies only for Windows systems with .Net Framework installed.
# Whether to install .Net Integration Services.
SILENT_INSTALL_JNBRIDGE=false
# Applies only to Server configuration on UNIX systems.
# Whether to start ColdFusion 9 automatically when the system boots. SILENT_CONFIGURE_SYSTEM_INIT
#Installation directory for the EAR or WAR file.
# Provided is a sample path.
SILENT_INSTALL_FOLDER=/opt/coldfusion9
SILENT_ADMIN_USERNAME=admin
SILENT_ADMIN_PASSWORD=Adm1n$" > installer.properties

./ColdFusion_9_WWEJ_linux64.bin -f installer.properties

cd /opt/coldfusion9/runtime/lib/
mv wsconfig.jar wsconfig_backup.jar
mkdir wsc
unzip wsconfig_backup.jar -d wsc
sed -i 's/remote_addr/client_addr/g' wsc/connectors/src/mod_jrun22.c
(cd wsc && zip -r ../wsconfig.jar .)
rm -fR wsc
chown --reference=wsconfig_backup.jar wsconfig.jar
chmod --reference=wsconfig_backup.jar wsconfig.jar


/opt/coldfusion9/bin/coldfusion start
/opt/coldfusion9/runtime/bin/wsconfig -server coldfusion -coldfusion -ws Apache -dir /etc/httpd/conf -bin /usr/sbin/httpd -script /usr/sbin/apachectl -v
/opt/coldfusion9/bin/cf-init.sh install

firewall-cmd --add-service=http --permanent && firewall-cmd --reload
chcon -R --reference=/var/www /var/www/html
chown -R nobody:nobody /var/www/html
setsebool -P httpd_can_network_connect 1
chown -R nobody /opt/coldfusion9/logs
chmod -R 760 /opt/coldfusion9/logs

/opt/coldfusion9/bin/coldfusion stop 
systemctl enable coldfusion_9
systemctl start coldfusion_9

dnf -y install policycoreutils-python-utils
ausearch -c 'coldfusion_9' --raw | audit2allow -M my-coldfusion9
semodule -X 300 -i my-coldfusion9.pp
systemctl start coldfusion_9
dnf clean all
