#### pklearningelk
#####1 intro
elasticsearch
```
add-apt-repository ppa:webupd8team/java
apt-get update
apt-get install oracle-java8-installer
java -version
wget https://download.elastic.co/elasticsearch/release/org/elasticsearch/distribution/deb/elasticsearch/2.3.2/elasticsearch-2.3.2.deb
dpkg -i elasticsearch-2.3.2.deb
service elasticsearch start
```
install kopf
```
cd /usr/share/elasticsearch/bin
./plugin install lmenezes/elasticsearch-kopf
```

logstash
```
wget https://download.elastic.co/logstash/logstash/packages/debian/logstash_2.3.2-1_all.deb
dpkg -i logstash_2.3.2-1_all.deb
```
(persontal test, must use [this method] (https://www.elastic.co/guide/en/logstash/current/installing-logstash.html)
```
/opt/logstash/bin/logstash -e 'input { stdin { } } output { stdout {} }'
```
export path
```
export PATH=$PATH:/opt/logstash/bin
```
create a conf file: logstash.conf
```
input{}
filter{}
output{}
```
use:
```
logstash -f logstash.conf
```
test syntax:
```
logstash --configtest logstash.conf
```
===

install kibana
```
wget -qO - https://packages.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
echo "deb http://packages.elastic.co/kibana/4.5/debian stable main" | sudo tee -a /etc/apt/sources.list
sudo apt-get update && sudo apt-get install kibana
```
run
```
/opt/kibana/bin/kinaba
```
config
```
vim /opt/kibana/config/kibana.yml
```
uncomment:
```
server.port: 5601
server.host:
elasticsearch.url: "http://localhost:9200"
```


install filebeat(optional
```
curl -L -O https://download.elastic.co/beats/filebeat/filebeat_1.2.2_amd64.deb
sudo dpkg -i filebeat_1.2.2_amd64.deb
```

#####3
######logstash plugins
list all logstash plugins:
```
/opt/logstash/bin/logstash-plugin list
```
sort by group
```
/opt/logstash/bin/logstash-plugin list --group output
```

######plugin
data types
codec
```
codec => "json"
```
