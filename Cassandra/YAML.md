`commit_failure_policy` 

​	commit disk failure 시 정책

- die: gossip, thrift를 끊고 JVM 종료 --> node 교체
- stop(default): gossip, thrift를 끊고 node를 죽은 채로 두고, JMX로 inspection은 가능하게
- stop_commit: commit log를 끄고 writes는 collect, reads는 계속 서비스
- ignore: fatal errors 무시하고 batch들을 fail로

`disk_optimization_strategy`

​	disk read 시 optimizing 전략

- ssd(default)
- spinning

`disk_failure_policy`

​	disk failure 시 정책

- die: file system error나 single SSTable error에 대해 gossip, thrift를 끊고 JVM 종료 --> node 교체
- stop_paranoid: single SSTable error 시에도 gossip, thrift를 끊음
- stop(default, recommend setting): gossip, thrift를 끊고 node를 죽은 채로 두고, JMX로 inspection은 가능하게
- best_effort(recommend setting): 실패한 disk 사용 중지하고 가능한 남은 SSTables 기반으로 request에 응답, 옛날 데이터에 대해 consistency level 1 가능하게 함
- ignore: fatal errors 무시하고 requests를 fail로, file system error들은 log에 기록되지만 나머지는 그냥 무시됨

`endpoint_snitch`

​	implements `IEndpointSnitch` interface, uses to locate nodes and route requests

- SimpleSnitch(default): public clouds의 single-datacenter deployment나 single-zone deployment에서 사용, datacenter나 rack info 인식하지 않음, 전략 순서는 접근성 --> read repair 비활성화 시 cache locality 향상
- GossipingPropertyFileSnitch: recommended for production, local node를 위해 cassandra-rackdc.properties 파일에서 rack, datacenter 읽어와서 gossip을 통해 다른 nodes에 전달, cassandra-topology.properties 파일이 있으면 ProopertyFileSnitch에서 변경 시 해당 파일 사용
- PropertyFileSnitch: cassandra-topology.properties 파일에 명시돼있는 rack, datacenter 이용해서 proximity 측정
- Ec2Snitch: single region의 EC2 deployment에서 사용, EC2 API에서 region, availability zone info 가져옴, region=datacenter, availability zone=rack --> multiple regions에서는 사용 불가
- Ec2MultiRegionSnitch: cross-region 연결을 위해 broadcast_address로 설정된 public ip 사용 --> seed addresses 명시해야함, storage_port, ssl_storage_port 열어놔야함, intra-region traffic의 경우에는 연결 이후 private ip로 변경
- RackInferringSnitch: 근접성을 각 노드 ip 주소의 2, 3번째 octet에 의해 결정, snitch class를 직접 custom할 때 사용
- GoogleCloudSnitch: Google Cloud Platform deployment에서 사용, region=datacenter, availability zone=rack, 모든 통신은 private ip 주소 사용
- CloudstackSnitch: Apache Cloudstack environment에서 사용

`seed_provider`

​	host의 주소들은 cluster의 contact points

​	joining node는 seeds의 한 node에 contact해서 topology of ring 파악

- class_name(default: org.apache.cassandra.locator.SimpleSeedProvider): seed logic에 대한 class, customize 가능하지만 대체로 필요하지는 않음
- seeds(default: 127.0.0.1): 새로운 node가 cluster에 join할 때 bootstrapping 해주기 위한 gossip에 필요한 ip 주소들, cluster가 여러 node들로 이루어져 있으면 변경
  - multiple datacenter cluster는 각 datacenter에서 적어도 하나의 node는 seeds에 포함해야 하고, 한 datacenter당 여러 node 등록해두면 fault tolerance에 좋음, 모든 node를 등록하는건 gossip performance 안좋게 함

`enable_user_defined_functions`

​	default: false

​	Java UDF

​	UDF는 server side에서 실행되기 때문에 보안상 위험이 있음

​	cassandra 3.x에서는 sandbox에서 실행돼서 위험한 코드 실행 방지

`enable_scripted_user_defined_functions`

​	default: false

​	javascript같은 언어로 만들어진 UDF 허용	

`compaction_throughput_mb_per_sec`

​	default: 16

​	instance 사이에서의 압축을 mb/sec로 조절

​	cassandra에 빠르게 넣을수록 SSTable count를 낮추기 위해 압축도 빨라져야함

​	write throughput mb/sec * 16~32: 권장

`compaction_large_partition_warning_threshold_mb`

​	dafault: 100

​	지정된 값보다 큰 partition을 압축하는 경우 warning log

`memtable_heap_space_in_mb`

​	default: 1/4 of heap size

​	memtable에 할당되는 on-heap memory

​	이 값과 memtable_offheap_space_in_mb 값으로 자동 memtable flush 기준 설정

`memtable_offheap_space_in_mb`

​	default: 1/4 of heap size

​	memtable에 할당되는 off-heap memory

`concurrent_reads`

​	default: 32 -> LVM(Logical Volume Managed), RAID drive에 알맞음

​	read 시 disk에서 data를 fetch할 때 너무 많은 data를 가지고 오면 병목현상 발생

​	16 * number of drives: 권장

`concurrent_writes`

​	default: 32

​	write는 보통 I/O bound가 아님 -> write시에는 CPU 숫자가 중요

​	8 * number of cpu cores: 권장

`concurrent_counter_writes`

​	default: 32

​	counter write 시에는 현재 값 read 후 write

​	16 * number of drives: 권장

`concurrent_batchlog_writes`

​	default: 32

​	concurrent batchlog write 제한, concurrent_writes와 비슷함

`concurrent_materialized_view_writes`

​	default: materialized view write 제한

​	materialized view write 시 read 필요 -> concurrent read나 concurrent write보다 작게 설정