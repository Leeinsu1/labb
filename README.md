# 프로젝트명
> 본 프로젝트는 플래시 메모리 상에서의 파일 시스템에 따른 DBMS 성능을 실험한다. 또한, discard 명령이 파일 시스템의 성능에 끼치는 영향을 연구한다.

플래시 메모리에서 FFS(Ext4, XFS)와 LFS(F2FS)의 사용에 따른 DBMS(MySQL/RocksDB) OLTP(벤치마크 툴: TPC-C, YCSB) 성능을 비교한다.

![](../header.png)

## 배경 지식

### 플래시 메모리

윈도우:

### MySQL
MySQL은 가장 많이 쓰이는 오픈 소스 RDBMS이다. MySQL서버는 크게 MySQL 엔진과 스토리지 엔진으로 나누어지는데, MySQL 엔진은 SQL 문장을 분석, 최적화등 클라이언트로부터 오는 요청을 처리하고, 스토리지 엔진은 실제 데이터를 디스크 스토리지에 저장하거나 조회한다. MySQL에 다양한 스토리지 엔진을 사용 가능하지만 그 중에서 B+ Tree기반의 InnoDB 스토리지 엔진이 가장 널리 쓰인다. InnoDB는 메모리 영역과 디스크 영역으로 나누어 진다. 사용자가 DB를 변경한다면 in-place-update방식으로 디스크 영역에 데이터가 쓰인다.

Reference: https://jeong-pro.tistory.com/239

### TPC-C
TPC란 Transaction Processing Performance Council 에서 발표한 벤치마크 모델들이다. TCP는 OLTP 시스템의 처리 성능을 평가하는 기준이 된다. 그 중 TPC-C는 TPC-A 모델이나 TPC-B 모델보다 복잡한 유통업의 수주·발주 OLTP 성능 평가를 위한 벤치마크 모델이다.

### RocksDB
RocksDB는 SSD에 최적화된 Key-Value 형태의 로그 구조 데 이터베이스 엔진이다. RocksDB의 데이터 저장 구조는 Log-Structured Merge Tree(LSM-tree)를 기반으로 한다. RocksDB는 쓰기 요청 시 메모리의 Active Memtable이란 이름의 임시 버퍼에 데이터를 쓴다. 해당 Memtable이 일정 크기가 되면 읽기 전용 Memtable이 되고, 일정 개수 이상의 Memtable이 모이면 저장 장치에 내려가(flush) SST 파일 형태로 저장된다. 따라서 RocksDB는 SST 파일 단위로(기본 64MB) I/O를 진행하고, 데이터를 append-only 로그에 저장함으로써 순차 쓰기를 보장한다.

### YCSB
YCSB(Yahoo! Cloud Serving Benchmark)는 클라우드 서비스와 NoSQL의 성능을 측정하기위한 벤치마크 툴이다. YCSB는 기본적으로 제공하는 다른 작동방식을 가진 workload들이 있다. 또 사용자가 이를 수정하거나 직접 만들어 사용할 수도 있다.
### 파일 시스템

### 성능 측정 지표


## 실험 환경

스크린 샷과 코드 예제를 통해 사용 방법을 자세히 설명합니다.

_더 많은 예제와 사용법은 [Wiki][wiki]를 참고하세요._

## 쉘 사용법

모든 개발 의존성 설치 방법과 자동 테스트 슈트 실행 방법을 운영체제 별로 작성합니다.

```sh
make install
npm test
```

## 결과 분석 방법


## 프로젝트 정보

학번 - 이름 - github주소 - 이메일

## SSD 초기화 및 File System Mount
1. 기존 SSD Unmount
```sh
sudo umount /dev/[PARTITION]
```

2. SSD Blksidcard
```sh
sudo blkdiscard /dev/[DEVICE]
```

3. 파디션 생성
```sh
sudo fdisk /dev/[DEVICE]
//command: n (new partition), w (write, save)
//Whole SSD in 1 partition
```

4. File system 생성
```sh
//F2FS
sudo mkfs.f2fs /dev/[PARTITION] -f

//Ext4
sudo mkfs.ext4 /dev/[PARTITION] -E discard,lazy_itable_init=0,lazy_journal_init=0 -F

//XFS
sudo mkfs.xfs /dev/[PARTITION] -f
```

5. Data directory에 mount
```sh
//F2FS default
sudo mount /dev/[PARTITION] /path/to/datadir 

//F2FS lfs mode
sudo mount -o mode=lfs /dev/[PARTITION] /path/to/datadir 

///Ext4, XFS
sudo mount -o discard /dev/[PARTITION] /path/to/datadir 
```

6. 권한 설정
```sh
sudo chown -R USER[:GROUP] /path/to/datadir 
sudo chmod -R 777 /path/to/datadir
```

## MySQL , TPC-C 실험

## RocksDB, YCSB 실험

<!-- Markdown link & img dfn's -->
[npm-image]: https://img.shields.io/npm/v/datadog-metrics.svg?style=flat-square
[npm-url]: https://npmjs.org/package/datadog-metrics
[npm-downloads]: https://img.shields.io/npm/dm/datadog-metrics.svg?style=flat-square
[travis-image]: https://img.shields.io/travis/dbader/node-datadog-metrics/master.svg?style=flat-square
[travis-url]: https://travis-ci.org/dbader/node-datadog-metrics
[wiki]: https://github.com/yourname/yourproject/wiki