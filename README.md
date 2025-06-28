# Apache Flink, Paimon and SeaTunnel

![Static Badge](https://img.shields.io/badge/Just_testing-Not_production_ready-red)

Seeing if Seatunnel can read data stored in s3 created using Flink and Paimon

### Build a SeaTunnel image

At the time of writing, SeaTunnel doesn't have an up to date image on Dockerhub, so use the following to create a local image:

```shell
docker build -f DockerfileSeatunnel --build-arg SEATUNNEL_VERSION=2.3.10 -t seatunnel:2.3.10-flink-1.16.3 .
```

Once built, run `docker compose up -d`

### Submit Seatunnel Flink Engine Job:

Once the containers are running, submit the job to Flink using:

* Flink SQL Client
```shell
docker exec -it seatunnel /opt/flink/bin/sql-client.sh embedded -f /opt/flink/jobs/job.sql
```

* Seatunnel Config Template
```shell
docker exec -it seatunnel /opt/seatunnel/bin/start-seatunnel-flink-15-connector-v2.sh -c /opt/seatunnel/config/v2.streaming.conf.template

docker exec -it seatunnel /opt/seatunnel/bin/start-seatunnel-flink-15-connector-v2.sh -c /config/seatunnel.streaming.conf
```

* Seatunnel Config Job, Seatunnel From SqlServer To Paimon
```shell
docker exec -it seatunnel /opt/seatunnel/bin/start-seatunnel-flink-15-connector-v2.sh -c /config/sqlserver2paimon.stream.conf
```

# ${SEATUNNEL_HOME}/connectors default only console and fake, need install connectors manually

Starting from version 2.2.0-beta, the binary package no longer provides connector dependencies by default.

If you need a specific connector version, taking 2.3.10 as an example, you need to execute the following command:

```shell
cd /opt/seatunnel

sh bin/install-plugin.sh 2.3.10
```

Typically, you do not need all the connector plugins. You can specify the required plugins by configuring config/plugin_config. For example, if you want the sample application to work properly, you will need the connector-console and connector-fake plugins. You can modify the plugin_config configuration file as follows:

```
--seatunnel-connectors--
connector-fake
connector-console
--end--
```

You can find all supported connectors and the corresponding plugin_config configuration names under ${SEATUNNEL_HOME}/connectors/plugins-mapping.properties.

# PS: plugin_config invalid issue resolution

* E.g.1 because local the seatunnel binary package, defaultly, first download jars to /home/user/.m2/repository/, second mv to ${SEATUNNEL_HOME}/connectors

```
[INFO] Copying /home/Data.Eng/.m2/repository/org/apache/seatunnel/connector-cdc-mysql/2.3.10/connector-cdc-mysql-2.3.10.jar to /opt/paimon-poc/seatunnel/connectors
```

* E.g.2 because docker the seat}unnel binary package, defaultly, first download jars to /root/.m2/repository/, second mv to ${SEATUNNEL_HOME}/connectors

```
[INFO] Copying /root/.m2/repository/org/apache/seatunnel/connector-assert/2.3.10/connector-assert-2.3.10.jar to /opt/seatunnel/connectors
```

