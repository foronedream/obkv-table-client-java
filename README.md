# OBKV Table Client
OBKV Table Client is Java Library that can be used to access table data from [OceanBase](https://github.com/oceanbase/oceanbase) storage layer. Its access method is different from JDBC, it skips the SQL parsing layer, so it has significant performance advantage.

## Quick Start

Create table in the OceanBase database:

``` sql
CREATE TABLE IF NOT EXISTS `test_varchar_table` (
    `c1` varchar(20) NOT NULL,
    `c2` varchar(20) DEFAULT NULL,
    PRIMARY KEY (`c1`)
) PARTITION BY KEY(`c1`) PARTITIONS 10;
```

Import the dependency for your maven project:
``` xml
<dependency>
    <groupId>com.oceanbase</groupId>
    <artifactId>obkv-table-client</artifactId>
    <version>1.2.6</version>
</dependency>
```

The code demo:
``` java
    // 1. initail ObTableClient
    ObTableClient obTableClient = new ObTableClient();
    obTableClient.setFullUserName("full_user_name");
    obTableClient.setParamURL("param_url");
    obTableClient.setPassword("password");
    obTableClient.setSysUserName("sys_user_name");
    obTableClient.setSysPassword("sys_user_passwd");
    obTableClient.init();
    
    // set primary key for partition table
    obTableClient.addRowKeyElement("test_varchar_table", new String[]{"c1"});
    
    // 2. single execute
    // return affectedRows
    obTableClient.insert("test_varchar_table", "foo", new String[] { "c2" }, new String[] { "bar" });
    // return Map<String, Object>
    obTableClient.get("test_varchar_table", "foo", new String[] { "c2" });
    // return affectedRows
    obTableClient.delete("test_varchar_table", "foo");

    // 3. batch execute
    TableBatchOps batchOps = obTableClient.batch("test_varchar_table");
    batchOps.insert("foo", new String[] { "c2" }, new String[] { "bar" });
    batchOps.get("foo", new String[] { "c2" });
    batchOps.delete("foo");
    
    List<Object> results = batchOps.execute();
    // the results include 3 item: 1. affectedRows; 2. Map; 3. affectedRows.
```
**NOTE:**
1. param_url is generated by [ConfigServer](https://ask.oceanbase.com/t/topic/35601923).
2. More example [Demo](https://github.com/oceanbase/obkv-table-client-java/tree/master/example)
4. full_user_name: the user for accessing obkv, which format is `user_name@tenant_name#cluster_name`
5. sys_user_name: `root@sys` or `proxy@sys`, which have privileges to access routing system view

## Release Notes
Latest release notes could be found in [Release notes](https://github.com/oceanbase/obkv-table-client-java/wiki#release-notes)

## Documentation

- English [Coming soon]
- [Simplified Chinese (简体中文)](https://github.com/oceanbase/obkv-table-client-java/wiki/OBKV-Java%E5%AE%A2%E6%88%B7%E7%AB%AF-%E8%AF%B4%E6%98%8E%E6%96%87%E6%A1%A3)

## Licencing

OBKV Table Client is under [MulanPSL - 2.0](http://license.coscl.org.cn/MulanPSL2) licence. You can freely copy and use the source code. When you modify or distribute the source code, please obey the MulanPSL - 2.0 licence.

## Contributing

Contributions are warmly welcomed and greatly appreciated. Here are a few ways you can contribute:

- Raise us an [Issue](https://github.com/oceanbase/obkv-table-client-java/issues)
- Submit Pull Requests. For details, see [How to contribute](CONTRIBUTING.md).

## Support

In case you have any problems when using OceanBase Database, welcome reach out for help:

- GitHub Issue [GitHub Issue](https://github.com/oceanbase/obkv-table-client-java/issues)
- Official forum [Official website](https://open.oceanbase.com)
- Knowledge base [Coming soon]

