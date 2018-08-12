[![Build Status](https://travis-ci.org/BorderCloud/tft-virtuoso7-stable.svg)](https://travis-ci.org/BorderCloud/tft-virtuoso7-stable)

# tft-virtuoso7-stable : Virtuoso 7 stable 

## Calculate SparqlScore with TFT in local

### Install
```
# Download docker's images 
docker pull bordercloud/tft-jena-fuseki

# Compile the docker's project 
docker build -t tft-virtuoso7-stable .
  
# Deploy network of SPARQL services

# 172.17.0.2
docker run --privileged --name instance.tft-virtuoso7-stable -h tft-virtuoso7-stable -d tft-virtuoso7-stable
# 172.17.0.3
docker run --privileged --name instance.tft.example.org -h example.org -d bordercloud/tft-virtuoso7-stable
# 172.17.0.4
docker run --privileged --name instance.tft.example1.org -h example1.org -d bordercloud/tft-virtuoso7-stable
# 172.17.0.5
docker run --privileged --name instance.tft.example2.org -h example2.org -d bordercloud/tft-virtuoso7-stable
# 172.17.0.6 for local
docker run --privileged --name instance.tft_database -d bordercloud/tft-jena-fuseki

# docker network inspect bridge

git clone --recursive https://github.com/BorderCloud/TFT.git
cd TFT

# install SPARQL client
composer install 

# install JMeter for protocol tests
wget http://mirrors.standaloneinstaller.com/apache//jmeter/binaries/apache-jmeter-4.0.tgz
tar xvzf apache-jmeter-4.0.tgz 
mv  apache-jmeter-4.0 jmeter
rm apache-jmeter-4.0.tgz 
```

### Start tests
Add parameter debug if necessary '-d'
```
php ./tft-testsuite -a -t fuseki -q http://172.17.0.6/test/query \
                    -u http://172.17.0.6/test/update
                    
php ./tft -t fuseki -q http://172.17.0.6/test/query \
                    -u http://172.17.0.6/test/update \
          -tt virtuoso -tq http://172.17.0.2:8890/sparql/ \
                       -tu http://172.17.0.2:8890/sparql/ \
          -r http://example.org/buildid   \
          -o ./junit  \
          --softwareName="Virtuoso" \
          --softwareDescribeTag=X.X.X \
          --softwareDescribe="Name"
                    
php ./tft-score -t fuseki -q http://172.17.0.6/test/query \
                          -u http://172.17.0.6/test/update \
                -r  http://example.org/buildid
```


# Delete containers of TFT

```
docker stop instance.tft_database
docker rm instance.tft_database
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
