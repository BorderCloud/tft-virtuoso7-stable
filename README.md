[![Build Status](https://travis-ci.org/BorderCloud/tft-virtuoso7-stable.svg)](https://travis-ci.org/BorderCloud/tft-virtuoso7-stable)

# tft-virtuoso7-stable

```
docker pull bordercloud/tft-jena-fuseki
docker pull bordercloud/tft_virtuoso7_stable
docker run --privileged --name instance.tft_database -d bordercloud/tft-jena-fuseki
docker run --privileged --name instance.tft.example.org -h example.org -d bordercloud/tft-virtuoso7-stable
docker run --privileged --name instance.tft.example1.org -h example1.org -d bordercloud/tft-virtuoso7-stable
docker run --privileged --name instance.tft.example2.org -h example2.org -d bordercloud/tft-virtuoso7-stable
docker run --privileged --name instance.tft_virtuoso7_stable -h tft_virtuoso7_stable -d bordercloud/tft-virtuoso7-stable
git clone --recursive https://github.com/BorderCloud/TFT.git
cd TFT
composer install --dev
php ./tft-testsuite -a -t fuseki -q http://172.17.0.2/test/query -u http://172.17.0.2/test/update
php ./tft -t fuseki -q http://172.17.0.2/test/query -u http://172.17.0.2/test/update -tt virtuoso -tq http://172.17.0.6:8890/sparql/ -tu http://172.17.0.6:8890/sparql/ -r http://test.com/test -o ./junit --softwareName="virtuoso" --softwareDescribeTag="7/stable"
php ./tft-score -t fuseki -q http://172.17.0.2/test/query -u http://172.17.0.2/test/update -r http://test.com/test
```
