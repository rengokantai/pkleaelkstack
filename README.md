#### pklearningelk
#####1 intro (kibana host = 5601)
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
export PATH=$PATH:/opt/logstash/bin:/opt/kibana/bin
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

#####3 collect, parse
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



#####5 Why do we need elasticsearch
######Elasticsearch plugins
bigdesk - analyze nodes across the cluster  
hammer -  
head -  

#####8 putting it all together
######dataset
Ex, server.xml in conf folder
```
Value.. pattern="%h %l %u %t &quot;%r&quot; %s%b" />
```
eplianation:
```
%h host
%l logical username
%u remote user
%t rime
%r request
%s response http code
%b byte sent
```
######Config logstash
grokking patterns
```
input{
  file{
    path => "/var/lib/tomcat7/logs/logs.txt"
    start_position => "beginning"
  }
}
filter{
  grok{
    match => { "message" => "%{COMMONAPACHELOG}"}
  }
  date{
    match => ["timestamp","dd/MMM/yyyy:HH:mm:ss Z"]
  }
  mutate{
    convert => ["response","Integer"]
    convert => ["bytes","integer"]
  }
}
output{
  elasticsearch{
    host => "localhost"
  }
}
```
execute
```
logstash -f config.conf
```

######visualizing with kibana
discover search. compound search, ip with GET:
```
clientip:10.0.0.1 AND verb:GET
```
Or
```
clientip:10.0.0.1 AND verb:GET AND request: "/etc/.."
```
