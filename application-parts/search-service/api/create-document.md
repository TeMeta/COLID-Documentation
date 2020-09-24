# Create a document

Before a COLID entry becomes findable in Data Marketplace it has to be stored in the search index.
This section explains how to store a COLID entry in the search index by using the API.

## API Usage

```shell
POST "https://localhost:51800/api/document" 
{
	"id": "string",
	"content": {}
}
```

## Description

A COLID entry will be stored as a document in the search index. Each document is a collection of fields, which are the key-value pairs that contain the data. Each field can have his own data type like text, numeric or even nested documents. The document has to be in valid JSON format. 

As in the example above the request needs an unique `id` which will be used as document identifier. The field `content` contains the data of the COLID entry. 

Note: In the search service the dynamic mapping for Elasticsearch is disabled. Elasticsearch has the ability to be schema less. This means that documents can be indexed without explicitly specifying how to handle each of the different fields that might occur in a document. But because of a lot of special requirements on the search, aggregations and search result highlighting it is needed to know the underlying data and how it will be used in Elasticsearch. Therefore explicit mapping is defined to take full control of how fields are stored and indexed. This mapping is generated by the current metadata which is applied in the search service. For more details on the metadata of the search service have look at [Metadata Handling](concepts/). 

Each property which should be stored as a field in the document has the following structure:

```JSON
"https://URI-of-the property" : {
            "outbound" : [
              {
                "value" : "Here goes the value of the property",
                "uri" : "Some URI",
                "edge" : "URI of edge for linked properties"
              }
            ],
            "inbound" : [ ]
          },
```

Only properties which are included in the metadata will be indexed as fields of the document in the search index. Properties which are not included will also not be indexed because of the explicit mapping. The process of the generation of the explicit mapping generation based on the metadata is described in the concept [Index Mapping](concepts/index.md).

This API is used in an automatic pipeline which is designed to get notified by every new created or modified published COLID entry and to store it as new document in the search index. The update function is near real time. For more implementation details on this data pipeline checkout the [Indexing Crawler Service](docs/application-parts/indexing-crawler-service.md). 
