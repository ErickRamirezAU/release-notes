# Release notes for DataStax Luna Streaming Distribution
 
## About DataStax Luna Streaming Distribution
Luna Streaming Distribution 2.10 is compatible with Apache Pulsar&trade; 2.10.0 and subsequent releases.

### Components

   * [Datastax Luna Streaming](https://github.com/datastax/pulsar) (Apache Pulsar 2.10 fork)
   * [Datastax BookKeeper](https://github.com/datastax/bookkeeper) (Apache BookKeeper fork)
   * DataStax Burnell
   * DataStax Pulsar Admin Console
   * DataStax Pulsar Heartbeat
   * DataStax Apache Pulsar Cassandra Sink
   * DataStax Apache Pulsar Cassandra Source
   * DataStax Starlight For Kafka
   * DataStax Starlight For RabbitMQ
   * DataStax Starlight For JMS Broker filter
   * DataStax Snowflake connector
   
 
This release adds these features to the original Apache Pulsar 2.10 release:
  
   * Stability improvements
   * Third party libraries updates for security fixes
   * Dependency upgrades (for security, stability and performances)
   * An enhanced ElasticSearch Pulsar IO Sink
   * An enhanced version of Apache BookKeeper with security fixes
   * Apache ZooKeeper 3.8.0
 
*Note:* The DataStax Luna Streaming Distribution is designed for Java 11. However, because the product releases Docker images, you do not need to install Java (8 or 11) in advance. Java 11 is bundled in the Docker image.

### Security
Luna Streaming is developed with great attention to security.

Since it is based on Apache Pulsar&trade;, Luna Streaming Pulsar distribution has all the security processes and requirements required by the Apache Software Foundation&trade;, such as:
* Peer to peer code reviews.
* Clear process for handling possible security vulnerabilities.

On top of that, Luna Streaming development processes are enhanced with additional security best practices for all the components involved:
* Robust Continuous Integration and reproducible Release pipeline in a secure monitored environment. Such monitoring improves the overall security of the application. 
* Reactivity to third-party vulnerabilities. Every component is scanned and monitored daily, ensuring quick fixes in case of vulnerability disclosure.
* Regular docker images vulnerability scans on latest Luna Streaming releases, with a well-defined release procedure.

At the time of the creation of the release, all the components included in the _[all](#packaging)_ packages are scanned by [Sonatype IQ Server service](https://help.sonatype.com/iqserver) and they don't have any critical (severity 8 or higher) security issues.


### Upgrade considerations for Luna Streaming Distribution 2.10

As with Luna Streaming 2.7, this release uses non-root containers for enhanced security. When upgrading from a previous version (for example, 2.6.2_1.0.1), files created while running that version will have root permissions and will not be readable by containers running the new version.

To fix this, manually log into the ZooKeeper, BookKeeper, and Function Worker containers and make sure that all files in the `/pulsar/data/` and `pulsar/logs` directories are owned by UID 10000 (user pulsar). The group ID of the files should also be set to GID 10001 (group pulsar). Here is an example command:

```
chown -R 10000:10001 /pulsar/data
```

If you are using the Luna Streaming Helm chart, you can enable automatic repair of the permissions using the `fixRootlessPermissions` setting to solve this problem. For more details on this setting, go [here](https://github.com/datastax/pulsar-helm-chart).

If you are upgrading from Luna Streaming Distribution 2.8.0 or 2.8.3, you don't have to do any particular action.

### Upgrade considerations for custom Pulsar Functions and Pulsar IO Connectors

If you are upgrading from Apache Pulsar 2.7.0 or Luna Streaming 2.7.2 you may need to recompile your Pulsar Functions or Pulsar IO Connectors using Apache Pulsar 2.8.0 as a dependency.
This is because there is a breaking API change in Apache Pulsar 2.8.0 (and therefore in Luna Streaming 2.8.0) related to the `SchemaInfo` java class.
More context [here](https://github.com/apache/pulsar/issues/11338).

If you are upgrading from Luna Streaming Distribution 2.8.0 or 2.8.3, you don't have to do any particular action.

### Packaging

Luna Streaming Distribution comes with different packages tooled for different purposes. 
The distributions are available as Docker images and tarballs, and both methods follow the same packaging patterns.

#### Patterns

* **lunastreaming**: the basic Luna Streaming Distribution **including Pulsar SQL** feature.
* **lunastreaming-core**: the basic Luna Streaming Distribution **without Pulsar SQL** feature. Pick this distribution if you don't need Pulsar SQL features.
* **lunastreaming-all**: contains the Core Luna Streaming Distribution, including **Pulsar Offloaders** and the **Datastax Pulsar IO Connectors** listed above. Pick this distribution if you want the Datastax connectors or the offloading feature.

# Releases

## Luna Streaming Distribution 2.10 0.4
This is a maintenance release containing important security and stability updates.
### Most notable commits
* [1b2a97888e3](https://github.com/datastax/pulsar/commit/1b2a97888e3) [fix][common] Add back FutureUtil#waitForAll and FutureUtil#waitForAny methods with List parameter
* [1ba180cbc74](https://github.com/datastax/pulsar/commit/1ba180cbc74) PIP-105: Store Subscription properties
* [4a2f75e71eb](https://github.com/datastax/pulsar/commit/4a2f75e71eb) [Broker] Disable memory limit controller for broker client and replication clients (#15723)
* [abec5d8c3c2](https://github.com/datastax/pulsar/commit/abec5d8c3c2) [enh] Broker: make max size of Consumer metadata configurable (#15713)
* [ee43c96373f](https://github.com/datastax/pulsar/commit/ee43c96373f) [fix][owasp] Fix false positive google-http-client-gson-1.41.0.jar (#15651)
* [4f85fbcc2d2](https://github.com/datastax/pulsar/commit/4f85fbcc2d2) [enh] Issue 15455: Pulsar Admin: create subscripion with Properties (PIP-105) (#15503) (#15681)
* [73c313664f9](https://github.com/datastax/pulsar/commit/73c313664f9) [fix][security] Tiered storage: Upgrade Hadoop to 3.3.3 to get rid of CVE-2022-26612 (#15660)
* [95fddf691cd](https://github.com/datastax/pulsar/commit/95fddf691cd) [fix][broker] Fix NPE when set `AutoTopicCreationOverride` (#15653)
* [7b37a8f63cf](https://github.com/datastax/pulsar/commit/7b37a8f63cf) [fix][broker] fix MetadataStoreException$NotFoundException while doing topic lookup (#15633)
* [b31898f9aae](https://github.com/datastax/pulsar/commit/b31898f9aae) Fix potential to add duplicated consumer (#15051)
* [602e8ad73b1](https://github.com/datastax/pulsar/commit/602e8ad73b1) [cleanup][broker] Override close method to avoid caching exception (#15529)
* [c244892e7ee](https://github.com/datastax/pulsar/commit/c244892e7ee) [Java Client] Fix messages sent by producers without schema cannot be decoded (#15622)
* [c04bebcf0f3](https://github.com/datastax/pulsar/commit/c04bebcf0f3) [improve][common] Use `Collection` to instead of `List` parameter type (#15329)
* [206b7a58668](https://github.com/datastax/pulsar/commit/206b7a58668) [PIP-153][optimize][txn]  Optimize metadataPositions in MLPendingAckStore (#15137)
* [50fc82778e2](https://github.com/datastax/pulsar/commit/50fc82778e2) [fix][broker] Fix to avoid TopicStatsImpl NPE even if producerName is null (#15502)
* [2579fae36ca](https://github.com/datastax/pulsar/commit/2579fae36ca) Fix http produce msg redirect issue. (#15551)
* [b7b447bab6e](https://github.com/datastax/pulsar/commit/b7b447bab6e) [PIP-163][Txn]Add lowWaterMark check before appending entry to TB (#15424)
* [1055ae396a5](https://github.com/datastax/pulsar/commit/1055ae396a5) [fix][broker]Close publishLimiter when disable it (#15520)
* [aa7f2081786](https://github.com/datastax/pulsar/commit/aa7f2081786) [fix][txn] Topic transaction buffer recover don't close reader when throw RuntimeException (#15361)
* [37397912703](https://github.com/datastax/pulsar/commit/37397912703) Fix grant all permissions but can't list topic. (#15501)
* [4bcbf93b5d6](https://github.com/datastax/pulsar/commit/4bcbf93b5d6) [Authorization] Role with namespace produce authz can also get topics (#13773)
* [13f3c7922e2](https://github.com/datastax/pulsar/commit/13f3c7922e2) [fix][broker] Fix MultiRolesTokenAuthorizationProvider `authorize` issue. (#15454)
* [4d3ea536808](https://github.com/datastax/pulsar/commit/4d3ea536808) [security] Remove sensitive msg from consumer/producer stats log (#15483)
* [bb900fcb287](https://github.com/datastax/pulsar/commit/bb900fcb287) Support handling single role and non-jwt-token in MultiRolesTokenAuthorizationProvider (#14857)
* [e3a0ad6c7d3](https://github.com/datastax/pulsar/commit/e3a0ad6c7d3) [improve][client] improve logic when ACK grouping tracker checks duplicated message id (#15465)
* [550e14de451](https://github.com/datastax/pulsar/commit/550e14de451) [Improve][doc] Add config of IO and acceptor threads in proxy (#15340)
* [c4ac6d50229](https://github.com/datastax/pulsar/commit/c4ac6d50229) [fix][package-management] Fix the new path `/data` introduced regression (#15367)
* [f0700865253](https://github.com/datastax/pulsar/commit/f0700865253) [improve][java-client] Add pending messages information while print the producer stats (#15440)
* [717b3459a9a](https://github.com/datastax/pulsar/commit/717b3459a9a) fix bug in contructor of RocksdbMetadataStore (#15405)
* [37e09eaf14c](https://github.com/datastax/pulsar/commit/37e09eaf14c) [improve][broker-web&websocket&proxy&function-worker] Full-support set ssl provider, ciphers and protocols (#13740)
* [f7ce317244a](https://github.com/datastax/pulsar/commit/f7ce317244a) [Improve][admin|client] AsyncHttpConnector doesn't use the system properties configured (#15307)
* [4f7c7191c51](https://github.com/datastax/pulsar/commit/4f7c7191c51) [Broker] Fix typo in enum name and handle closing of the channel properly since writeAndFlush is asynchronous (#15384)
* [7d245e6d1e7](https://github.com/datastax/pulsar/commit/7d245e6d1e7) Add KeyStore support in WebSocket, Function Worker HTTPS Servers  (#15084)
* [626c0de3948](https://github.com/datastax/pulsar/commit/626c0de3948) [enh][monitor]: add metrics for pulsar web service thread pool (#14742)
* [3816b039331](https://github.com/datastax/pulsar/commit/3816b039331) [CI] Upgrade zlib version to 1.2.12 (#14964)
* [1fdbeee1e1a](https://github.com/datastax/pulsar/commit/1fdbeee1e1a) [improve][offloaders] Upgrade JClouds to 2.5.0 (#15649)
* [eb03e976f3a](https://github.com/datastax/pulsar/commit/eb03e976f3a) [Fix][Txn] Fix transaction PendingAck lowWaterMark (#15530)
* [e23a44f3289](https://github.com/datastax/pulsar/commit/e23a44f3289) [Fix][Txn]Fix transaction component recover fillQueue (#15418)
* [f4fc76f2aec](https://github.com/datastax/pulsar/commit/f4fc76f2aec) [Fix][txn] Make transaction stats consistent at end txn (#15472)
* [6b85f8a68e2](https://github.com/datastax/pulsar/commit/6b85f8a68e2) Upgrade Netty to 4.1.77.Final and netty-tcnative to 2.0.52.Final (#15646)
### Builtin connectors
* cassandra-enhanced-pulsar-sink-1.6.1-nar.nar
* pulsar-cassandra-source-2.0.0.nar
* pulsar-io-data-generator-2.10.0.4.nar
* pulsar-io-debezium-mongodb-2.10.0.4.nar
* pulsar-io-debezium-mssql-2.10.0.4.nar
* pulsar-io-debezium-mysql-2.10.0.4.nar
* pulsar-io-debezium-oracle-2.10.0.4.nar
* pulsar-io-debezium-postgres-2.10.0.4.nar
* pulsar-io-elastic-search-2.10.0.4.nar
* pulsar-io-jdbc-clickhouse-2.10.0.4.nar
* pulsar-io-jdbc-mariadb-2.10.0.4.nar
* pulsar-io-jdbc-postgres-2.10.0.4.nar
* pulsar-io-jdbc-sqlite-2.10.0.4.nar
* pulsar-io-kafka-2.10.0.4.nar
* pulsar-io-kinesis-2.10.0.4.nar
* pulsar-snowflake-connector-0.1.9.nar
* pulsar-protocol-handler-kafka-2.10.0.1.nar
* starlight-rabbitmq-2.10.0.0.nar
* pulsar-kafka-proxy-2.10.0.1.nar
* pulsar-jms-2.3.0-nar.nar

## Luna Streaming Distribution 2.10 0.3
This is a maintenance release containing important stability updates.
### Most notable commits
* [b0a2a5e4fd0](https://github.com/datastax/pulsar/commit/b0a2a5e4fd0) fix: org.apache.kafka.connect.errors.DataException: Invalid Java object for schema with type... (#15598)
* [8ef565e8171](https://github.com/datastax/pulsar/commit/8ef565e8171) [fix][broker] Fix deadlock in broker after race condition in topic creation failure (#15570)
* [2dba8e8084f](https://github.com/datastax/pulsar/commit/2dba8e8084f) Offloader metrics (#13833)
* [51e35187b50](https://github.com/datastax/pulsar/commit/51e35187b50) Pulsar Functions: allow a Function<GenericObject,?> to access the original Schema of the Message and use it (#14847)
* [b58211de4d4](https://github.com/datastax/pulsar/commit/b58211de4d4) Fix pulsar-client-all shade - include BookKeeper libraries
* [babea4bfb28](https://github.com/datastax/pulsar/commit/babea4bfb28) Upgrade to Datastax BookKeeper 4.14.5.1.0.0
* [19daad5a5bb](https://github.com/datastax/pulsar/commit/19daad5a5bb) [fix][test] Add receive timeout to avoid block thread. (#15088)
* [41417f9d8dd](https://github.com/datastax/pulsar/commit/41417f9d8dd) [WebSocket] Fix MultiTopicReader#getConsumer ClassCastException (#15534)

### Builtin connectors
* cassandra-enhanced-pulsar-sink-1.6.1-nar.nar
* pulsar-cassandra-source-2.0.0.nar
* pulsar-io-data-generator-2.10.0.3.nar
* pulsar-io-debezium-mongodb-2.10.0.3.nar
* pulsar-io-debezium-mssql-2.10.0.3.nar
* pulsar-io-debezium-mysql-2.10.0.3.nar
* pulsar-io-debezium-oracle-2.10.0.3.nar
* pulsar-io-debezium-postgres-2.10.0.3.nar
* pulsar-io-elastic-search-2.10.0.3.nar
* pulsar-io-jdbc-clickhouse-2.10.0.3.nar
* pulsar-io-jdbc-mariadb-2.10.0.3.nar
* pulsar-io-jdbc-postgres-2.10.0.3.nar
* pulsar-io-jdbc-sqlite-2.10.0.3.nar
* pulsar-io-kafka-2.10.0.3.nar
* pulsar-io-kinesis-2.10.0.3.nar
* pulsar-snowflake-connector-0.1.9.nar
* pulsar-protocol-handler-kafka-2.10.0.1.nar
* starlight-rabbitmq-2.10.0.0.nar
* pulsar-kafka-proxy-2.10.0.1.nar
* pulsar-jms-2.1.0-nar.nar

## Luna Streaming Distribution 2.10 0.2
This is a maintenance release containing important stability updates.

### Most notable commits
* [d0105dfe006](https://github.com/datastax/pulsar/commit/d0105dfe006) [Proxy] Remove unnecessary blocking DNS lookup in LookupProxyHandler (#15415)
* [de3fef3f49f](https://github.com/datastax/pulsar/commit/de3fef3f49f) [Proxy/Client] Fix DNS server denial-of-service issue when DNS entry expires (#15403)
* [91ab03c029f](https://github.com/datastax/pulsar/commit/91ab03c029f) [fix][kinesis-sink] Handle Avro collections native types (GenericData.Array and Utf8 map keys) (#15432)
* [d30126b01d1](https://github.com/datastax/pulsar/commit/d30126b01d1) [feat][elasticsearch-sink] Option to strip out non printable characters (#15431)
* [d65d6bd8e0e](https://github.com/datastax/pulsar/commit/d65d6bd8e0e) [fix][elasticsearch-sink] Handle Avro collections native types (GenericData.Array and Utf8 map keys) (#15430)
* [bfa5a02ee7a](https://github.com/datastax/pulsar/commit/bfa5a02ee7a) [feat][elasticsearch] Add hashed id support (#15428)
* [cfd467fd3f5](https://github.com/datastax/pulsar/commit/cfd467fd3f5) [feat][elasticsearch-sink] Option to output canonical JSON (#15426)
* [bda6fdd32bc](https://github.com/datastax/pulsar/commit/bda6fdd32bc) Sinks: allow to reset --timeout-ms to zero
* [f2f6e607d6a](https://github.com/datastax/pulsar/commit/f2f6e607d6a) Include circe-checksum in pulsar-client dependency (#60)
* [97a7b3b049e](https://github.com/datastax/pulsar/commit/97a7b3b049e) Include correct bookkeeper jars in pulsar-client jar (#16)


### Builtin connectors
* cassandra-enhanced-pulsar-sink-1.6.1-nar.nar
* pulsar-cassandra-source-2.0.0.nar
* pulsar-io-data-generator-2.10.0.2.nar
* pulsar-io-debezium-mongodb-2.10.0.2.nar
* pulsar-io-debezium-mssql-2.10.0.2.nar
* pulsar-io-debezium-mysql-2.10.0.2.nar
* pulsar-io-debezium-oracle-2.10.0.2.nar
* pulsar-io-debezium-postgres-2.10.0.2.nar
* pulsar-io-elastic-search-2.10.0.2.nar
* pulsar-io-jdbc-clickhouse-2.10.0.2.nar
* pulsar-io-jdbc-mariadb-2.10.0.2.nar
* pulsar-io-jdbc-postgres-2.10.0.2.nar
* pulsar-io-jdbc-sqlite-2.10.0.2.nar
* pulsar-io-kafka-2.10.0.2.nar
* pulsar-io-kinesis-2.10.0.2.nar
* pulsar-snowflake-connector-0.1.9.nar
* pulsar-protocol-handler-kafka-2.10.0.1.nar
* starlight-rabbitmq-2.10.0.0.nar
* pulsar-kafka-proxy-2.10.0.1.nar