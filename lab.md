#User Data para la instancia de EC2

#!/bin/sh
yum update
yum install -y ruby
cd /home/ec2-user
aws s3 cp s3://aws-codedeploy-us-west-1/latest/install .
chmod +x ./install
./install auto

#Validar que el agente esta ejecutandose 

sudo service codedeploy-agent status

#appspec.yaml

version: 0.0
os: linux
files:
  - source: /index.html
    destination: /var/www/html/
hooks:
  BeforeInstall:
    - location: deploy/before_install
      timeout: 300
      runas: root
  AfterInstall:
    - location: deploy/restart_server
      timeout: 300
      runas: root

#Before install hook

#!/bin/sh
yum update
yum install -y httpd
service httpd start

#After install hook

#!/bin/sh
service httpd restart









