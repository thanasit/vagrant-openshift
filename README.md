# vagrant-openshift
**Install docker**
````text
sudo yum install docker docker-registry -y
sudo vi /etc/sysconfig/docker
	INSECURE_REGISTRY='--insecure-registry 172.30.0.0/16'
sudo systemctl enable docker
sudo systemctl start docker
````

**Install openshift**
````text
sudo mkdir -p /apps/openshift/
sudo mkdir -p /opt/jboss/openshift/
cd /apps/openshift/
sudo wget https://github.com/openshift/origin/releases/download/v1.5.0/openshift-origin-server-v1.5.0-031cbe4-linux-64bit.tar.gz
sudo tar xzvf openshift-origin-server-v1.5.0-031cbe4-linux-64bit.tar.gz
sudo ln -s /apps/openshift/openshift-origin-server-v1.5.0-031cbe4-linux-64bit /apps/openshift/openshift-origin-server
sudo ln -s /apps/openshift/openshift-origin-server /opt/jboss/openshift/current

sudo useradd openshift
sudo passwd openshift
sudo usermod -aG wheel openshift
sudo chown -R openshift:openshift /opt/jboss/
sudo chown -R openshift:openshift /apps/openshift/
````

Prerequisites
````text
export PATH="$(pwd)":$PATH
export KUBECONFIG="$(pwd)"/openshift.local.config/master/admin.kubeconfig
export CURL_CA_BUNDLE="$(pwd)"/openshift.local.config/master/ca.crt
chmod +r "$(pwd)"/openshift.local.config/master/admin.kubeconfig
````

**Demo openshift**

Start cluster
````text
sudo ./openshift cli cluster up --public-hostname=192.168.50.10 --routing-suffix=192.168.50.10.xip.io --host-config-dir=/opt/jboss/openshift/current/openshift.local.config --host-data-dir=/opt/jboss/openshift/current/openshift.local.data
````

Restart cluster
````text
sudo ./openshift cli cluster up --public-hostname=192.168.50.10 --routing-suffix=192.168.50.10.xip.io --host-config-dir=/opt/jboss/openshift/current/openshift.local.config --host-data-dir=/opt/jboss/openshift/current/openshift.local.data --use-existing-config
````

Down cluster
````text
sudo ./openshift cli cluster down
````

````text
sudo ./oc new-project hello-ews-openshift --description="This is an example project to demonstrate OpenShift v3" --display-name="Hello ews openshift"
sudo ./oc status
sudo ./oc project hello-ews-openshift
sudo ./oc new-app openshift/wildfly-101-centos7~https://thanasit:Passw0rd@github.com/thanasit/hello-ews-openshift.git#master
sudo ./oc logs -f bc/hello-ews-openshift
````
