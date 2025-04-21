# Basic Auth
Username: solr
Password: **\***

# API Create Collection

## Step 1 

Contact with member in team get default_config.zip file

API Upload configSet file 

-   URL: `/solr/admin/configs?action=UPLOAD&name=default_config`
-   Method: `POST`
-   Body request: binary file

-   Body response: 
    ```json
    {
        "responseHeader": {
        "status": 0,
        "QTime": 246
        }
    }
    ```

## Step 2

API Create Collection 

-   URL: `/api/collections`
-   Method: `POST`
-   Body request: 
    ```json
    {
        "name": "techproducts_v2",
        "config": "default_config",
        "numShards": 1,
        "replicationFactor": 3
    }
    ```

-   Body response:
    ```json
    {
    "responseHeader": {
        "status": 0,
        "QTime": 1443
    },
    "success": {
        "172.19.0.7:8983_solr": {
            "responseHeader": {
                "status": 0,
                "QTime": 710
            },
            "core": "techproducts_v2_shard1_replica_n1"
        },
        "172.19.0.6:8983_solr": {
            "responseHeader": {
                "status": 0,
                "QTime": 1102
            },
            "core": "techproducts_v2_shard1_replica_n2"
        },
        "172.19.0.5:8983_solr": {
            "responseHeader": {
                "status": 0,
                "QTime": 1099
            },
            "core": "techproducts_v2_shard1_replica_n4"
        }
    }
    }
    ```

# API Delete Collection
-   URL: `/api/collections/collection_name`
-   Method: `DELETE`
-   Body response:
    ```json
    {
    "responseHeader": {
        "status": 0,
        "QTime": 165
    },
    "success": {
        "172.19.0.6:8983_solr": {
            "responseHeader": {
                "status": 0,
                "QTime": 22
            }
        },
        "172.19.0.7:8983_solr": {
            "responseHeader": {
                "status": 0,
                "QTime": 22
            }
        },
        "172.19.0.5:8983_solr": {
            "responseHeader": {
                "status": 0,
                "QTime": 26
            }
        }
    }
    }
    ```


# API List Collection
-   URL: `/solr/admin/collections?action=LIST`
-   Method: `GET`
-   Body response:
    ```json
    {
    "responseHeader": {
        "status": 0,
        "QTime": 0
    },
    "collections": [
        "solr_cloud",
        "techproducts_v2",
        "techproducts_v3"
    ]
    }
    ```


# API List configSets

-   URL: `/solr/admin/configs?action=LIST&omitHeader=true`
-   Method: `GET`,
-   Body response:
    ```json
    {
    "configSets": [
        "_default",
        "data_driven_schema_configs",
        "default_config"
    ]
    }
    ```

# API Delete configSet

-   URL: `/api/cluster/configs/config_name?omitHeader=true`
-   Method: `DELETE`
-   Body response:
    ```json
    {}
    ```

# Design Schema 

Change the schema in the managed-schema.xml file

 Default Fields 
```
  <field name="_nest_path_" type="_nest_path_"/>
  <field name="_root_" type="string" docValues="false" indexed="true" stored="false"/>
  <field name="_text_" type="text_general" multiValued="true" indexed="true" stored="false"/>
  <field name="_version_" type="plong" indexed="false" stored="false"/>
  <field name="id" type="string" multiValued="false" indexed="true" required="true" stored="true"/>
```

 Add address and name fields to the schema 
 ```
  <field name="_nest_path_" type="_nest_path_"/>
  <field name="_root_" type="string" docValues="false" indexed="true" stored="false"/>
  <field name="_text_" type="text_general" multiValued="true" indexed="true" stored="false"/>
  <field name="_version_" type="plong" indexed="false" stored="false"/>
  <field name="address" type="text_ja" uninvertible="true" indexed="true" stored="true"/>
  <field name="id" type="string" multiValued="false" indexed="true" required="true" stored="true"/>
  <field name="name" type="text_ja" uninvertible="true" indexed="true" stored="true"/>
 ```

 Attribute description:
- name: field name
- type: field type name (field type is used to define tokenizer and filter)
- multiValued: Multiple values (Array)
- indexed: create an index for this field
