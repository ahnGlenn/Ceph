# 🔥 Ceph?
Ceph는 확장성이 뛰어나고 내결함성을 갖춘 오픈소스 분산 스토리지 시스템<br/>
  ```
    "대규모 데이터를 안정적으로 저장하고 관리할 수 있는 클라우드 네이티브 스토리지 솔루션"<br/>
    ✔ 오브젝트 스토리지 (S3 API 지원),<br/>
    ✔ 블록 스토리지 (VM 및 컨테이너용 디스크),<br/>
    ✔ 파일 스토리지 (분산 파일 시스템)<br/>
    등을 통합 제공하며, 확장성과 자동 복구 기능을 갖춰 대기업 및 클라우드 환경에서 많이 사용
    (3가지를 제공하지만, ceph자체는 object 스토리지로 설계!)
  ```

# 🔥 About Ceph <br/>
1. Monitors(모니터, ceph-mon)<br/>
 ```
   ceph 클러스터의 전체 상태를 관리 > map(클러스터의 구성정보 저장)으로 관리
   ✔ 모니터 맵 : 모니터 노드들의 정보 저장
   ✔ 매니저 맵 : 매니저 노드들의 정보 저장
   ✔ OSD 맵 : 저장소 노드(osd, 객체)의 상태를 저장
   ✔ MDS 맵 : 메타데이터 서버 정보를 저장
   ✔ CRUSH 맵 : 데이터를 어떻게 배치할건지 결정하는 맵
 ```
2.  <br/>
3. CRUSH : <br/>
4. How to save data? <br/>
 ```
 오브젝트단위로 저장하며, CRUSH알고리즘을 이용해, 데이터를 어디에 저장할지 결정.
 1) 데이터를 특정 Placement Group(PG, 배치그룹)에 저장
 2) PG가 속한 OSD를 찾아 데이터 저장
 3) 장  
 ```

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
