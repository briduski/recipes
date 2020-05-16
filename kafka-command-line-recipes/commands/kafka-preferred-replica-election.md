# Balancing leadership
Broker id=4 stops, leadership to 'its' partitions is transferred to other replicas.

Broker id=4 restarts, no leadership at all for id=4  => broker almost not used, only for durability

Eureka!! Preferred replicas.

If the list of replicas for a partition is 4,2,1,3 then node 4 is preferred as the leader to either node 2,1 or 3 because it is earlier in the replica list.

-  Restore leadership to the restored(restarted) replicas by running the command

    **kafka-preferred-replica-election.sh --zookeeper zk1:2181**

-  Or configure Kafka to do this automatically by setting

    **auto.leader.rebalance.enable=true**