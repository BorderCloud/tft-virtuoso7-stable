[![Test](https://github.com/BorderCloud/tft-virtuoso7-stable/actions/workflows/test.yml/badge.svg)](https://github.com/BorderCloud/tft-virtuoso7-stable/actions/workflows/test.yml) ([SPARQLScore](http://sparqlscore.com/)) [![Publish](https://github.com/BorderCloud/tft-virtuoso7-stable/actions/workflows/publish.yml/badge.svg)](https://github.com/BorderCloud/tft-virtuoso7-stable/actions/workflows/publish.yml)

# tft-virtuoso7-stable : Virtuoso 7 stable 

## Calculate SparqlScore with TFT in local

### Install
```
# Compile the docker's project 
docker build -t tft-virtuoso7-stable .
  
# Deploy network of SPARQL services

docker-compose up -d 
# docker-compose stop

git clone --recursive https://github.com/BorderCloud/TFT.git
cd TFT

# install SPARQL client
composer install 

# install JMeter for protocol tests
wget http://mirrors.standaloneinstaller.com/apache//jmeter/binaries/apache-jmeter-5.4.1.tgz
tar xvzf apache-jmeter-5.4.1.tgz 
mv  apache-jmeter-5.4.1 jmeter
rm apache-jmeter-5.4.1.tgz 
```

### Start tests
Add parameter debug if necessary '-d'
```
php ./tft-testsuite -a -t fuseki -q http://172.18.0.6:8080/test/query \
                    -u http://172.18.0.6:8080/test/update
          
php ./tft -t fuseki -q http://172.18.0.6:8080/test/query \
                    -u http://172.18.0.6:8080/test/update \
          -tt virtuoso -te http://172.18.0.2/sparql \
          -r http://example.org/buildid   \
          -o ./junit  \
          --softwareName="Virtuoso" \
          --softwareDescribeTag=X.X.X \
          --softwareDescribe="Name"
                    
php ./tft-score -t fuseki -q http://172.18.0.6:8080/test/query \
                          -u http://172.18.0.6:8080/test/update \
                -r  http://example.org/buildid
```

# Delete containers of TFT

```
docker stop instance.tft-database
docker rm instance.tft-database
docker stop instance.tft.example.org
docker rm instance.tft.example.org
docker stop instance.tft.example1.org
docker rm instance.tft.example1.org
docker stop instance.tft.example2.org
docker rm instance.tft.example2.org
docker stop instance.tft-virtuoso7-stable
docker rm instance.tft-virtuoso7-stable

```

# Delete all containers

```
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
```

# Check the network
```
docker network list
docker network inspect tft-virtuoso7-stable_tft
```
The result has to be :
* instance.tft-virtuoso7-stable" => 172.18.0.2
* instance.tft.example.org =>  172.18.0.3
* instance.tft.example1.org => 172.18.0.4
* instance.tft.example2.org => 172.18.0.5
* instance.tft-database =>     172.18.0.6

# Open bash in container
```
docker exec -it instance.tft-virtuoso7-stable  bash
docker exec -it instance.tft-database bash
```

#Realign SPARQL API with Varnish

## Install Varnish 6 and modules
```
yum install python-docutils automake autoconf libtool ncurses-devel libxslt groff pcre-devel pkgconfig

wget https://packagecloud.io/install/repositories/varnishcache/varnish60/script.rpm.sh
chmod +x ./script.rpm.sh
./script.rpm.sh
yum install varnish varnish-devel

git clone https://github.com/varnish/varnish-modules.git
cd varnish-modules
./bootstrap  
./configure
make
make install
```

## Check test
```
varnishtest rewriting.vtc
```

## Test on server
```
docker exec -it instance.tft-virtuoso7-stable bash
#varnishd -C -f /etc/varnish/default.vcl
vi /etc/varnish/default.vcl
systemctl start varnish
systemctl enable varnish

```

# Logs
```
journalctl -f -u virtuoso -n 1000
journalctl -f -u varnish
```
