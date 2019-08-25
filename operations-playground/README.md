# Flink Operations Playground

The Flink operations playground let's you explore and play with [Apache Flink](https://flink.apache.org)'s features to manage and operate stream processing jobs, including

* Observing automatic failure recovery of an application
* Upgrading and rescaling an application
* Querying the runtime metrics of an application

It is based on a [docker-compose](https://docs.docker.com/compose/) environment and super easy to setup.

## Setup

The operations playground requires a custom Docker image in addition to public images for Flink, Kafka, and ZooKeeper. 

The `docker-compose.yaml` file of the operations playground is located in the `operations-playground` directory. Assuming you are at the root directory of the [`flink-playgrounds`](https://github.com/apache/flink-playgrounds) repository, change to the `operations-playground` folder by running

```bash
cd operations-playground
```

### Building the custom Docker image

Build the Docker image by running

```bash
docker-compose build
```

### Starting the Playground

Once you built the Docker image, run the following command to start the playground

```bash
docker-compose up -d
```

You can check if the playground was successfully started, if you can access the WebUI of the Flink cluster at [http://localhost:8081](http://localhost:8081).

### Stopping the Playground

To stop the playground, run the following command

```bash
docker-compose down -v
```

## Logs

### JobManager

The JobManager logs can be tailed via docker-compose.

```bash
docker-compose logs -f jobmanager
```

After the initial startup you should mainly see log messages for every checkpoint completion.

### TaskManager

The TaskManager log can be tailed in the same way.

```bash
docker-compose logs -f taskmanager
```

After the initial startup you should mainly see log messages for every checkpoint completion.

## Flink CLI

The Flink CLI can be used from within the client container. For example, to print the help message of the Flink CLI you can run

```bash
docker-compose run --no-deps client flink --help
```

## Flink REST API

The Flink REST API is exposed via localhost:8081 on the host or via jobmanager:8081 from the client container, e.g. to list all currently running jobs, you can run:

```bash
curl localhost:8081/jobs
```

Note: If the curl command is not available on your machine, you can run it from the client container (similar to the Flink CLI):

```bash
docker-compose run --no-deps client curl jobmanager:8081/jobs 
```

## Kafka Topics

You can look at the records that are written to the Kafka Topics by running

//input topic (1000 records/s)

```basj
docker-compose exec kafka kafka-console-consumer.sh \
  --bootstrap-server localhost:9092 --topic input
```

//output topic (24 records/min)
```bash
docker-compose exec kafka kafka-console-consumer.sh \
  --bootstrap-server localhost:9092 --topic output
```  

## Further instructions

The playground setup and more detailed instructions are presented in the
["Getting Started" guide](https://ci.apache.org/projects/flink/flink-docs-release-1.8/getting-started/docker-playgrounds/flink-operations-playground.html) of Flink's documentation.
