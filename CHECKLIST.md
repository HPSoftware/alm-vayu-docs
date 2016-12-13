# Checklist
### Site Admin
1. Validate that [Global Search is enabled](http://alm-help.saas.hpe.com/en/12.53/online_help/Content/Admin/sa_enabling_GS.htm).
2. Check connectivity between ALM and iRiS service.
2. Check which projects are global search enabled.
3. Change log level to debug, run search query in global search and get QC & SA logs.
4. Verify that _ALLOW_WEBUI_ACCESS_ set to _Y_ at _Site Configuration_.

### Global Search Server
1. Fetcher, iRiS, Elasticsearch & tomcat logs. Note: In order to change Fetcher & iRiS services' log level to debug, modify `[Global Search]\apache-tomcat-8.0.30\webapps\alm-fetcher-service\WEB-INF\log4j2.properties`, `[Global Search]\apache-tomcat-8.0.30\webapps\iris-service\WEB-INF\log4j2.properties` and restart tomcat service.
2. iRiS Health `curl http://gs-server:8080/iris-service`, `curl http://gs-server:8080/iris-service/api/health`
3. Fetcher Health: `curl http://gs-server:8080/alm-fetcher-service`, `curl http://gs-server:8080/alm-fetcher-service/api/health`
4. Elasticsearch Health: `curl http://gs-server:9200`, `curl http://gs-server:9200/_cluster/health?pretty`, Elasticsearch indexes: `curl http://gs-server:9200/_cat/indices?v`
5. Verify *cluster name* configured in iRiS (`C:\Windows\System32\config\systemprofile\iris\iris-elasticsearch.properties`) & Elasticsearch (`[Global Search]\elasticsearch-1.7.4\config\elasticsearch.yml`).
6. Verify ALM fetcher started to index: `curl http://gs-server:8080/alm-fetcher-service/api/health` (Elasticsearch `curl http://gs-server:9200/_cat/indices?v`). Note: When _nextRuntime has a value other than "-1", this indicates that the indexing process has completed and the project data is ready to be searched with Global Search.

##### Reindex Elasticsearch
If you want to clean _Global Search_ data. For example, in case of running `curl http://gs-server:9200/_cluster/health` and elasticsearch status is red (means _Elasticsearch_ has data corruption, see [elasticsearch docs](https://www.elastic.co/guide/en/elasticsearch/guide/1.x/_cluster_health.html)) do:

1. Stop _HP Global Search_ windows service.
2. Delete all Elasticsearch indexes: `curl -X "DELETE" http://gs-server:9200/*`.
3. Start _HP Global Search_ windows service.
4. Wait till the _ALM Fetcher_ index data and run: `curl http://gs-server:9200/_cluster/health` and `curl http://gs-server:9200/_cat/indices?v` in order to verify _Elasticsearch_ state.
