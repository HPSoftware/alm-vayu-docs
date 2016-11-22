### Checklist
##### Site Admin
1. Check conectivity betwin ALM and iRiS service.
2. Enable global search on projects.
3. QC & SA logs.

##### Global Search Server
1. Fetcher, iRiS, Elasticsearch & tomcat logs.
2. Fetcher Health `curl http://gs-server:8080/iris-service`, `curl http://gs-server:8080/iris-service/api/health`
3. iRiS Health: `curl http://gs-server:8080/alm-fetcher-service`, `curl http://gs-server:8080/alm-fetcher-service/api/health`
4. Elasticsearch Health: `curl http://gs-server:9200`, `curl http://gs-server:9200/_cluster/health?pretty`
5. Verify *cluster name* configured in iRiS (`C:\Windows\System32\config\systemprofile\iris\iris-elasticsearch.properties`) & Elasticsearch (`[Global Search]\elasticsearch-1.7.4\config\elasticsearch.yml`).
6. Verify ALM fetcher started to index: `curl http://gs-server:8080/alm-fetcher-service/api/health` (Elasticsearch `curl http://gs-server:9200/_cat/indices?v`). Note: When _nextRuntime has a value other than "-1", this indicates that the indexing process has completed and the project data is ready to be searched with Global Search.
