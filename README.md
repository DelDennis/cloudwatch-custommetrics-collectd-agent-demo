## Sample code to GitHub for Custom metrics support on CloudWatch Agent

A demo to use custom metrics support on CloudWatch Agent. The demo basically focuses on getting custom metrics for a DB engine - PostGres in addition to the Restful Service Monitoring in the original Repository. This demo will also show how to use other collectd Plugins to monitor the postgres DB processes.

## License Summary

This sample code is made available under a modified MIT license. See the LICENSE file.

## Pre-Requisites before running the installation script

In order to get metrics from the PostGresDB, this demo assumes the PostGresDB V 9.2+ is installed on the same machine as CollectD and CloudWatch agent.

## Post Installation Steps

1. The OS user name of CollectD Process must match with the PostGres User name. You may instead choose
to map the OS name to PostGres User name by adding entry in pg_indent.conf
2. In this demo, the OS Name is "root" owning the collectD process and thus the same user needs to be created in PostGres USer table. You may again choose to map it to your own PostGres user in pg_indent.conf
  The below steps to add user "root" and give required permissions to get the metrics from PG -
  a. Create User "root" in PG 
    CREATE USER root WITH PASSWORD '';
  b. Give permissions to this user to query the Stats table of PG
    GRANT ALL PRIVILEGES ON <db> TO root;
  c. Allow the connections created from Local machine to be trusted for testing purpose only. In Production, you will need to     change the auth to MD5 from TRUST.  Go to pg_hba.conf and change the lines to TRUST for local connections for testing         purpose only.
    # "local" is for Unix domain socket connections only
    local   all             all                                     trust
    # IPv4 local connections:
    host    all             all             127.0.0.1/32            trust
    # IPv6 local connections:
    host    all             all             ::1/128                 trust
3. Test the connection to PG by "sudo -u root psql template1"  by Connecting with Root user and see if you are able to fire queries against the DB.
4. Restart PostGresDB service - "sudo systemctl restart postgresql.service"
5. Restart CollectD Service - "sudo /etc/init.d/collectd restart"

## Troubleshooting 

# Restarting CloudWatch Agent
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a restart

# Log for CloudWatch Agent
/opt/aws/amazon-cloudwatch-agent/logs/amazon-cloudwatch-agent.log

# Config for CloudWatch Agent
/opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json

# Restarting PostGres Service
sudo systemctl restart postgresql.service

# Install unzip, wget, other packages in RHEL 2.6 
Sudo yum -y install unzip
 
Sudo yum -y install wget
 
Sudo yum -y install https://archive.fedoraproject.org/pub/epel/7/x86_64/Packages/e/epel-release-7-11.noarch.rpm
 
sudo setenforce 0

# Logs of CollectD for troubleshooting
/var/log/collectd.log





