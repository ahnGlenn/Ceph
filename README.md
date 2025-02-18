# 🚀 Ceph 학습 로드맵

🔹 1. Ceph 개념 이해
✔ Ceph의 핵심 개념
 - RADOS (Reliable Autonomic Distributed Object Store) → Ceph의 핵심 분산 스토리지 엔진
 - CRUSH 알고리즘 (Controlled Replication Under Scalable Hashing) → 데이터 배치 알고리즘
 - Ceph의 주요 컴포넌트
   - OSD (Object Storage Daemon): 실제 데이터를 저장하는 노드
   - Monitor (MON): 클러스터 상태 관리 및 노드 인증
   - Metadata Server (MDS): CephFS(파일 시스템)에서 메타데이터 관리
   - RGW (RADOS Gateway): S3 API를 지원하는 Object Storage 서비스

<br/>

✔ Ceph가 지원하는 스토리지 유형
- Object Storage (AWS S3와 유사)
- Block Storage (AWS EBS와 유사)
- File System (CephFS)

🔹 2. Ceph 설치 및 실습
