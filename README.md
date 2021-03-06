# ElastAlert Server Deploy as Docker or Kubernetes pod :whale: :sailboat:

> A server that runs [ElastAlert](https://github.com/Yelp/elastalert) and exposes REST API's for manipulating rules and alerts. It works great in combination with our [ElastAlert Kibana plugin](https://github.com/bitsensor/elastalert-kibana-plugin).



---

## Installation
The most convenient way to run the ElastAlert server is by using our Docker container image. The default configuration uses `localhost:9200` as ElasticSearch host, if this is not the case in your setup please edit `es_host` and `es_port` in both the `elastalert.yaml` and `config.json` configuration files.

To run the Docker image you will want to mount the volumes for configuration and rule files to keep them after container updates. In order to do that conveniently, please do: `https://github.com/sahirabbas-k/elastalert-docker.git && cd elastalert-docker`

```bash
docker run -d -p 3030:3030 \
    -v `pwd`/config/elastalert.yaml:/opt/elastalert/config.yaml \
    -v `pwd`/config/elastalert-test.yaml:/opt/elastalert/config-test.yaml \
    -v `pwd`/config/config.json:/opt/elastalert-server/config/config.json \
    -v `pwd`/rules:/opt/elastalert/rules \
    -v `pwd`/rule_templates:/opt/elastalert/rule_templates \
    --net="host" \
    --name sahirabbask/elastalert:latest
```

OR 
```bash
docker-compose up -d
```

## Building Docker image

Clone the repository
```bash
git clone https://github.com/sahirabbas-k/elastalert-docker.git && cd elastalert-docker
```

Build the image
```
make build
```
which is equivalent of
```
docker pull alpine:latest && docker pull node:latest
docker build -t elastalert .
```

## Deployment on Kubernetes

Before deploying **Elastalert** on **Kubernetes**, it is necessary to modify some files:

* **03a-elastalert-elastconfig-yml-configmap.yaml** (set your **Elasticsearch** server)
* **03b-elastalert-logstash-configmap.yaml** (set your index, by default is **Logstash** index)
* **03c-elastalert-elastconfig-json-configmap.yaml** (set your **Elasticsearch** server)**
* **03d-elastalert-logstash-sample-configmap.yaml** (set your index, by default is **Logstash** index)

Download **repository**:

```
git clone git clone https://github.com/sahirabbas-k/elastalert-docker.git
```

Deploy **Elastalert**:

```
cd elastalert-docker
kubectl create -f kubernetes/
```
