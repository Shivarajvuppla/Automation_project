s3bucket="upgrad-shivaraj1"
name="shivaraj"

apt update -y

if [[ apache2 != $(dpkg --get-selections apache2 | awk '{print $1}')]];
then
    apt install apache2 -y
fi

running=$(systemctl status apache2 | grep active | awk '{print $3}' | tr -d'()')
if [[running != ${running}]]; 
then
    systemctl start apache2
fi
enabled=$(systemctl is -enabled apache2 | grep "enabled")
if [[ enabled != ${enabled}]];
then
    systemctl enable apache2
fi
timestamp = $(date '+%d%m%Y-%H%M%S')
cd /var/log/apache2
tar -cf /temp/${name}-httpd-logs-${timestamp}.tar *.log
if [[ -f /tmp/${name}-httpd-logs-${timestamp}.tar ]];
then 
    aws cp /tmp/${name}-httpd-logs-${timestamp}.tar s3://${s3bucket}/${name}-httpd-logs-${timestamp}.tar
fi
	
 4  
README.md
@@ -1 +1,3 @@
# Automation_Project
# Automation_Project

That is consist of Installing Apachi server
