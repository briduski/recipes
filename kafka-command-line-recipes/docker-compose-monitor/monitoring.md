# Cluster example
`docker-compose.yml` - contains example of kafka cluster 
- 3 Zookeepers 
- 4 Kafka Brokers - brokers run with agent to pull JMX metrics
- 1 Confluent Schema registry 
--------
- 1 Prometheus - collecting JMX metrics 
- 1 Grafana - to build dashboards
## Docker compose for metrics
- [monitoring kafka and components - confluent distribution](https://docs.confluent.io/current/installation/docker/operations/monitoring.html)
- [jmx exporter](https://github.com/prometheus/jmx_exporter)
- [github:exklamationmark/devenv](https://github.com/exklamationmark/devenv)

## Important metrics
- Amount of active Controller -> should be always 1
- Number of Under replicated partitions (URP) -> should be 0, otherwise brokers are lagging
- Number of offline partitions -> should be 0
