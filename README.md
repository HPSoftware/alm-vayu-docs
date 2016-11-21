# Global Search - Unofficial Workarounds

Global Search is a powerful engine that enables you to search across all or a specific ALM module.

Global Search deployment contains 3 services: ALM-Fetcher (a service which uses ALM REST API to fetch data from ALM and index it into Elasticsearch using iRiS), iRiS (Elasticsearch client acts as a mediator), and [Elasticsearch](https://www.elastic.co/).

See Global Search Overview [Movie](https://www.youtube.com/watch?v=CyRUYm1iNv0&feature=youtu.be).

### Cluster Installation
Note: High-availability clusters are currently not supported in Global Search. 

1. Install global-search using installer, but do not configure a valid ALM server (so the ALM fetcher will not start indexing).
2. Open `services.msc` and stop `HP Global Search` and `HP Global Search ES` services.
3. Set Cluster Name to a uniqe name like `hpe-alm-production-global-search-27Oct2016` in:
  - Elasticsearch: `C:\Program Files\HP\Global Search\elasticsearch-1.7.4\config\elasticsearch.yml` (look for `cluster.name`)
  - iRiS: `C:\Windows\System32\config\systemprofile\iris\iris-elasticsearch.properties` (look for `cluster.name`)
4. Configure ALM fetcher. Open: `C:\Program Files\HP\Global Search\apache-tomcat-8.0.30\webapps\alm-fetcher-service\WEB-INF\classes\alm-fetcher-service.properties`
  - Configure ALM fetcher to start manually - set `auto.start=false`
  - Set `alm.server.url`, `alm.user`, `alm.password`
5. Open `services.msc` and start `HP Global Search` and `HP Global Search ES` services.
6. Verify services up and running & cluster name set as expected:
  - iRiS: `curl http://vm-gs:8080/iris-service`, `curl http://vm-gs1:8080/iris-service/api/health`
  - Fetcher: `curl http://vm-gs:8080/alm-fetcher-service`
  - Elasticsearch: `curl http://vm-gs:9200`
7. Do steps 1-6 on 2 or more global-search machines.
8. Congfigure LB for iRiS services.
9. Start only 1 ALM fetcher using: `curl http://vm-gs1:8080/alm-fetcher-service/api/start`
10. Other ALM fetcher should be started on active ALM fetcher failure.
9. Verify ALM fetcher started to index: `curl http://vm-gs2:8080/alm-fetcher-service/api/health`.Note: When _nextRuntime has a value other than "-1", this indicates that the indexing process has completed and the project data is ready to be searched with Global Search.

> To prevent network access to Global Search data, make sure that the Global Search services are
protected behind a firewall, and that only the ALM server and other trusted services (like monitoring tool) can access
it.
