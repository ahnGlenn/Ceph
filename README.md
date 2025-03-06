# 🔥 Ceph?
Ceph는 확장성이 뛰어나고 내결함성을 갖춘 오픈소스 분산 스토리지 시스템<br/>
  ```
    "대규모 데이터를 안정적으로 저장하고 관리할 수 있는 클라우드 네이티브 스토리지 솔루션"
    ✔ 오브젝트 스토리지 (S3 API 지원)
    ✔ 블록 스토리지 (VM 및 컨테이너용 디스크)
    ✔ 파일 스토리지 (분산 파일 시스템)
    등을 통합 제공하며, 확장성과 자동 복구 기능을 갖춰 대기업 및 클라우드 환경에서 많이 사용
    (3가지를 제공하지만, ceph자체는 object 스토리지로 설계!)

    !!! 따라서 (file, object, block)이 들어와도 RADOS를 통해 RADOS ojbect포맷으로 변환되어 ceph에 저장됨.
  ```

<br/><br/> 

# 🔥 About Ceph <br/>
> ref : https://computing-jhson.tistory.com/112<br/>
![Screenshot 2025-02-21 at 12 48 40 AM](https://github.com/user-attachments/assets/3c1456f3-c247-4a88-abbe-8399e514d2af)
![Screenshot 2025-02-21 at 12 48 26 AM](https://github.com/user-attachments/assets/a10834e0-7a0e-4cb5-b713-b1b442827fc7)

1. Monitors(모니터, ceph-mon)<br/>
 ```
   ceph 클러스터의 전체 상태를 관리 > map(클러스터의 구성정보 저장)으로 관리
   ✔ 모니터 맵 : 모니터 노드들의 정보 저장
   ✔ 매니저 맵 : 매니저 노드들의 정보 저장
   ✔ OSD 맵 : 저장소 노드(osd, 객체)의 상태를 저장
   ✔ MDS 맵 : 메타데이터 서버 정보를 저장
   ✔ CRUSH 맵 : 데이터를 어떻게 배치할건지 결정하는 맵
 ```

2. Managers (매니저, ceph-mgr) <br/>
 ```
   클러스터의 "실시간 상태" 및 "성능 모니터링" (! 모니터는 구성정보 및 맵을 관리)
   ✔ 현재 스토리지 사용량, 성능, 시스템 부하 등을 실시간으로 수집하고 관리
   ✔ Ceph의 정보를 웹 대시보드 또는 REST API 형태로 제공하는 다양한 모듈을 실행
 ```

3. OSDs(오브젝트 스토리지 데몬, ceph-osd)
 ```
  데이터를 실제로 저장, 복제, 복구를 수행하는 역할
  ✔ 데이터를 저장하고 복제, 복구, 재배치(rebalancing)등을 수행
  ✔ 다른 OSD들과 통신하며, 정상적인 상태인지 하트비트(Heartbeat)를 체크
 ``` 

4. Ceph MDS (Metadata Server)
 ```
  CephFS(파일 시스템)에서 파일과 디렉터리의 메타데이터를 관리하는 역할
  ✔ ls, find 같은 명령어를 실행할 때, 데이터를 직접 검색하는 대신 MDS에서 빠르게 정보를 제공
  ✔ CephFS를 사용할 때만 필요하며, 블록 스토리지(RBD)나 오브젝트 스토리지(RGW)에서는 필요하지 않음
 ```

5. CRUSH : Ceph의 데이터 배치(Placement) 및 복제(Replication) 알고리즘 <br/>
 ``` 
  1) Ceph 데이터저장 > Object단위로 저장 > 배치그룹에 속하게됨(PG) > CRUSH알고리즘 수행하여 PG가 어떤 OSD에 속할지 계산하는 알고리즘

  2) CRUSH 맵의 주요 구성 요소
    - 디바이스(Devices): 개별 OSD를 나타냄.
    - 버킷(Buckets): OSD를 그룹화하는 논리적 구조 (예: 랙, 호스트, 데이터센터 등).
    - 규칙(Rules): 데이터 복제 및 배치 정책 (예: "SSD에 2개, HDD에 1개 복제").
    - 웨이트(Weight): 각 OSD의 저장 용량 비율을 결정.

    !! CRUSH 맵을 직접 수정하여 커스텀 데이터 배치 전략을 구현할 수도 있다.
 ```

6. RADOS
```
  Ceph의 핵심 분산 스토리지 엔진.
  모든 interface들이 최종적으로 RADOS를 호출. -> libRADOS, RADOSGW(RADOS Gateway), RBD(RADOS Block Device), CephFS(Ceph File System)

  1) Object storage -> libRADOS, RADOSGW
  2) Block storage  -> RBD
  3) File storage   -> CephFS
```

7. LIBRADOS
```
  ✔ RADOS 시스템에 개발자가 직접 접근할 수 있는 library.
  ✔ RADOS의 가장 기본적인 interface로 RBD, RADOSGW의 기반.
  즉, Ceph의 object storage interface.
  이를통해 사용자는 Ceph에 RADOS object를 저장/읽기를 한다.
```

8. RADOSGW(gateway)
```
 ✔ RADOS에 접근할 수 있는 RESTfull HTTP API를 제공하는 서비스(HTTP 서버).
 ✔ 이것 또한 libRADOS와 마찬가지로 object storage interface로서, 사용자가 RADOS object를 저장할 혹은 읽어올 수 있다
   (aws의 S3, OpenStack의 Swift와 호환되는 API도 제공)
```

9. RBD
```
  ✔ RADOS 상에 block device image를 만들 수 있도록 제공하는 서비스.
  ✔ 클라우드(openstack 등) 상 가상머신의 image로 활용된다.
  
```

10. CephFS
```
  ✔ CephFS는 Object Storage인 RADOS 상에 File system을 사용할 수 있도록 제공하는 서비스.
  ✔ POSIX와 호환되는 API를 제공.
  ✔ 일반 사용자는 CephFS를 통해 파일을 폴더 단위로 계층적으로 관리.
  다만 File system을 사용하기 위해서는 폴더 계층 정보를 저장해야 하기에 추가적으로 MDS(Metadata server)가 Ceph 클러스터에 포함되어야 한다.
```

<br/><br/>

# 🚀 OPENSTACK이란? <br/>
```
  클라우드 인프라를 구축하고 운영할 수 있는 오픈소스 플랫폼입니다.
  쉽게 말해, AWS 같은 퍼블릭 클라우드를 직접 구축할 수 있도록 도와주는 솔루션입니다.

  ✔ 자체 클라우드 구축 솔루션
```

<br/><br/>

# 🚀 Ceph 학습 로드맵<br/>

🔹 1. Ceph 개념 이해<br/>

✔ Ceph의 핵심 개념
  - RADOS (Reliable Autonomic Distributed Object Store) → Ceph의 핵심 분산 스토리지 엔진
  - CRUSH 알고리즘 (Controlled Replication Under Scalable Hashing) → 데이터 배치 알고리즘
  - Ceph의 주요 컴포넌트
    - OSD (Object Storage Daemon): 실제 데이터를 저장하는 노드
    - Monitor (MON): 클러스터 상태 관리 및 노드 인증
    - Metadata Server (MDS): CephFS(파일 시스템)에서 메타데이터 관리
    - RGW (RADOS Gateway): S3 API를 지원하는 Object Storage 서비스
      
✔ Ceph가 지원하는 스토리지 유형
  - Object Storage (AWS S3와 유사)
  - Block Storage (AWS EBS와 유사)
  - File System (CephFS)

<br/>

🔹 2. Ceph 설치 및 실습<br/>

✔ 로컬 환경에서 Ceph 클러스터 구축 (싱글 노드)
  - Vagrant + VirtualBox를 활용한 가상 환경 구축
  - Docker + Ceph 컨테이너 환경 활용
  - Ceph-Ansible 또는 Ceph-Deploy를 이용한 설치
    
✔ Ceph를 클러스터 환경에서 구축 (멀티 노드)
  - Ubuntu 서버 3대 이상에 Ceph 클러스터 구성
  - Ceph OSD, Monitor, MDS 배포 및 테스트
    
✔ 실습 예제
  - rados bench 를 사용한 Ceph 성능 테스트
  - CephFS를 마운트하여 파일 시스템 활용
  - RGW를 설정하여 S3 API로 객체 저장 및 검색

<br/>

🔹 3. Ceph API & 백엔드 개발<br/>
Ceph를 활용한 개발을 위해 API 및 백엔드 연동 방법을 학습필요<br/>

✔ Ceph API 활용
  - Librados API (C++, Python, Go) → RADOS 기반 데이터 조작
  - Ceph RESTful API → 관리 및 모니터링
  - Ceph RGW S3 API → AWS S3 호환 API를 활용한 객체 저장
    
✔ 실습 프로젝트 예시
  - Python으로 Ceph RADOS에 직접 데이터 저장/읽기
  - Go로 S3 API를 사용하여 객체 업로드/다운로드 구현
  - CephFS를 활용한 대용량 파일 저장 백엔드 서비스 개발

<br/>

🔹 4. Ceph 운영 및 성능 최적화<br/>
실무에서는 Ceph의 성능을 최적화하고 운영하는 방법이 중요.<br/>

✔ Ceph 운영 기초
  - Ceph 모니터링 (ceph status, ceph df, ceph osd tree)
  - 장애 복구 (ceph osd repair, ceph pg repair)
  - Ceph 튜닝 (osd_memory_target, filestore_xfs_extent 조정)
    
✔ 고급 Ceph 성능 최적화
  - 블록 크기 및 PG 설정 최적화 (osd_pool_default_pg_num)
  - SSD vs HDD 환경에서의 성능 차이 분석
  - BlueStore (Ceph의 최신 저장소 엔진) 설정

<br/>

🔹 5. 실제 서비스와 Ceph 연동<br/>
실무용 Ceph를 백엔드 개발과 연결하는 방법을 숙지필요<br/>

✔ 실제 서비스에서 Ceph 사용 사례
  - OpenStack과 Ceph 연동 (Cinder, Glance, Nova)
  - Kubernetes에서 Ceph RBD 및 CephFS 활용
  - 대규모 Object Storage 시스템 구축 사례 분석
    
✔ 백엔드 애플리케이션 개발과 Ceph 연동
  - Django/Flask 백엔드에서 Ceph S3 API 활용
  - Java/Spring Boot에서 Ceph RBD를 블록 스토리지로 사용
  - Kubernetes에서 Ceph CSI를 활용하여 데이터 저장
