## 검색엔진 정의

#### 무엇을 제공하는가?

유저는 키워드 혹은 문장으로 질의 -> 유저 분석(지리적 요인, 접속 기기 종류, 개인 정보 등)을 통한 유저의 요구사항 도출 -> 그에 맞는 컨텐츠 수집 및 정렬 -> 검색 내용 및 CTR 등을 또 다시 데이터화하여 유저 분석, 컨텐츠 품질 척도?에 활용

요즘은 유저의 질의에 따른 단순 컨텐츠 나열보다는 유저의 의도를 파악하고 그에 맞는 시각화 자료 등이 존재하면 바로 띄워주는 추세 -> 의도 파악의 중요성 매우 커짐



#### 사용자가 얻는 것은 무엇인가?

- 유저: 니즈에 맞는 컨텐츠를 빠르게, 정확하게
- 검색 엔진: 트래픽=수입 원천
- 웹사이트: 제품, 서비스 등 홍보



## 검색엔진 사용 추세 (오픈소스, 응용프로그램)

#### 오픈소스

다양한 use case에 따라 유연하게 대처

기술적인 지식 필요

ex. ElasticSearch, Solr



#### 응용프로그램(Saas: Search as a Service)

기술적인 지식 없이도 다양한 검색 기능, 호스팅, 운영, 유지보수 등 서비스에서 지원

커스터마이즈 상대적으로 자유롭지 않음

ex. Algolia, Elastic Cloud



응용프로그램은 특정 사용자에 특화된 검색 및 분석이 필요한 경우에 좋음

검색 시 유저의 의도 파악 중요성이 커짐 -> 각 유저를 분석하고, 특성에 맞는 결과를 제공해야 함 -> 특정 사용자에 특화된 서비스에는 맞지 않음



## ES, Solr, Lucene 소개 간단히, 대표적으로 쓰는 회사

#### ElasticSearch

open source distributed search engine based on lucene

real-time indexing, high scalability -> 검색, 색인 속도가 빨라야 할 때

회사: uber, slack, udemy, 삼성SDS



#### Apache Solr

open source search engine based on lucene

문서 등 사이즈가 큰 고정 데이터 검색에 좋음

회사: Netflix (https://cwiki.apache.org/confluence/display/solr/PublicServers)



#### Apache Lucene

open source IR library

라이브러리이기 때문에 추가 개발 필요

회사: apple (https://twitter.com/cutting/status/1137030687003774976) (?)



## ES, Solr, Lucene 비교

#### ElasticSearch

대용량 데이터를 Near Real-Time으로 저장, 검색, 분석

ElasticSearch 단독이나 ELK(ElasticSearch + Logstash + Kibana) 스택으로 사용

- Logstash: 다양한 소스의 로그, 트랜잭션 데이터 수집, 집계, 파싱 -> ElasticSearch로 전달
- ElasticSearch: 받은 데이터 검색, 집계하여 관심 있는 정보 획득
- Kibana: 데이터 시각화, 모니터링

Scale out: 샤드 -> 규모 수평적 증가

고가용성: Replica -> 안전성 보장

Schema Free: JSON 통한 데이터 검색

Restful: 데이터 CRUD를 http Restful API 통해 수행

- SELECT == GET
- INSERT == PUT
- UPDATE == POST
- DELETE == DELETE



#### Solr

NoSQL: 여러 클러스터로 분산된 파일 분산 검색 처리

Highly Scalable: 전체 클러스터에 relica 추가 -> 확장

Full Text Search: 단어, 문장 등 검색, 오타 교정, 검색어 자동 완성, 와일드카드(부분 검색) 질의 기능

Restful: http 호출을 사용한 검색 지원 -> 결과는 JSON, XML, CSV



#### Lucene

SW에 색인, 검색 기능 간단하게 추가할 수 있도록 지원



| ElasticSearch                                 | Solr                                              | Lucene     |
| --------------------------------------------- | ------------------------------------------------- | ---------- |
| search engine                                 | search engine                                     | IR library |
| JSON                                          | XML, CSV, Word, PDF 등 다양한 data source handler |            |
| scaling, data analytics -> insights, patterns | enterprise-directed text search                   |            |



## 세 가지 오픈소스 적합한 상황 제시 후, 적절한 모델 제시

#### ElasticSearch

상품 검색, 이상징후 감지 및 모니터링, 대용량 데이터



#### Solr

문서 원문 검색, 다양한 형태의 데이터가 입력으로 들어오는 경우



#### Lucene

간단한 검색 기능이 포함된 서비스