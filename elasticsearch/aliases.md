## Index Aliases

You can create an alias for one or more indexes in elasticsearch.

One use I thought of for this is background reindexing without
having to start a brand new cluster.

Here is the JSON syntax from the doc page: https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-aliases.html

I also think the elasticsearch-api gem supports it. You would need
to write your own outside searcher method that is not model-based if
you are using elasticsearch-model to index. Here's docs on that: http://www.rubydoc.info/gems/elasticsearch-api/Elasticsearch/API/Indices/Actions#put_alias-instance_method
