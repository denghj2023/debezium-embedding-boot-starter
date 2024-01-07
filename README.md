# debezium-embedding-boot-starter

* [Debezium Documentation](https://debezium.io/documentation/reference/1.5/index.html)

## 快速开始

* 引入依赖`debezium-embedding-boot-starter`
* 配置Debezium。
```yaml
debezium:
  connector:
    name: 'exampledb-connector'
    snapshot.mode: 'schema_only'
    connector.class: 'io.debezium.connector.mysql.MySqlConnector'

    offset.storage: 'org.apache.kafka.connect.storage.FileOffsetBackingStore'
    offset.storage.file.filename: 'debezium/${debezium.connector.name}-offsets.dat'
    offset.flush.interval.ms: 6000

    database.server.id: 987657
    database.server.name: 'exampledb'
    database.hostname: 'localhost'
    database.port: 3306
    database.user: 'root'
    database.password: 'root'
    database.history: 'io.debezium.relational.history.FileDatabaseHistory'
    database.history.file.filename: 'debezium/${debezium.connector.name}-dbhistory.dat'
    database.include.list: 'example'
    table.include.list: "example.t_example"
    decimal.handling.mode: 'string'
    converters: 'timeConverter'
    timeConverter.type: 'cn.superads.superx.debeziumembedding.TimeConverter'
  container:
    enabled: true
```

> [Debezium如何配置？](https://debezium.io/documentation/reference/1.5/connectors/mysql.html#how-the-mysql-connector-works)

* 实现`EventChangeListener`

_示例_
```java
@Component
@ChangeListener(destination = "exampledb.example.app")
public class AppChangeListener implements EventChangeListener<App> {
    @Override
    public void onChange(App entity, Operate operate) {
        System.err.println(operate + " -> " + JSON.toJSON(entity, JSONWriter.Feature.PrettyFormat));
    }
}
```

