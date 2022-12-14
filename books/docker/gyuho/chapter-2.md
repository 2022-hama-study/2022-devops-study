# 2 개요

도커는 애플리케이션을 환경과 함께 이미지로 포장해서 공유한다. 이미지는 여러개를 겹칠 수 있으며, 보통 도커허브에서 공유하는 베이스 이미지들을 겹쳐 독자적인 이미지를 만든다. 제품 환경에서는 모든 도커 컨테이너들을 한 대의 호스트 머신에서 작동시키는 경우는 드물며, 트래픽, 가용성, 신뢰도를 따져 분산 환경을 구축한다. 이런 분산 환경을 관리하는 것이 오케스트레이션이다.

- 도커의 구성
    - Docker Engine
        
        도커의 핵심으로 이미지 생성, 컨테이너 기동을 한다.
        
    - Docker Registry
        
        이미지를 공유하는 기능
        
    - Docker Compose
        
        여러 컨테이너 구성 정보를 코드로 정의하고 명령을 실행함으로 애플리케이션 실행 환경을 구성하는 컨테이너들을 일원 관리하기 위한 툴
        
    - Docker Machine
        
        클라우드환경의 도커 실행 환경을 명령으로 자동생성
        
    - Docker Swarm
        
        여러 도커  호스트를 클러스터화.
        
        Manager : 클러스터 관리, API 제공
        
        Node : 컨테이너 실행
        
        Kubernetes
        
- 도커의 동작
    - 컨테이너 구획화 : namespace
        
        PID namespace : namespace가 다른 프로세스끼리 서로 격리
        
        Network namespace : 네트워크 디바이스, IP주소, 포트번호, 라우팅 테이블, 필터링 테이블과 같은 네트워크 리소스를 namespace 별로 격리.
        
        UID namespace : 유저가 namespace 안밖으로 서로 다른 이름(권한)을 가짐으로 보안강화
        
        MOUNT namespace : 리눅스의 파일시스템을 사용하기 위해 마운트가 필요함. namespace 별로 격리된 가상의 파일 시스템을 생성.
        
        UTS namespace : namespace 별로 호스트 명이나 도메인 명을 독자적으로 가짐
        
        IPC namespace : 프로세스 간 통신(IPC) 오브젝트를 namespace별로 독립적으로 가짐.
        
    - 릴리스 관리 장치 : cgroup
        
        도커의 물리 머신 상의 자원을 여러 컨테이너가 공유하는데, 리눅스 커널 기능인 cgroup으로 자원의 할당을 관리. 특정 프로세스가 자원을 다 써서 다른 프로세스가 동작 못하는 경우를 막음.
        
    - 네트워크
        
        NIC(Network Interface Card)란?
        
        컴퓨터에 랜선 꼽는 곳이다. 
        
        NAT(Network Address Translation)란?
        
        프라이빗 IP 주소가 할당된 클라이언트가 인터넷 상에 있는 서버에 접근할 때, NAT 라우터는 프라이빗 IP 주소를 글로벌 IP주소로 바꿔준다. 1:1로 변환하기 때문에 동시에 여러 클라이언트가 접근할 수 없다.
        
        NAPT(Network Address Port Translation, = IP 마스커레이드)란?
        
        프라이빗 IP와 함께 포트 번호도 변환하여 여러 클라이언트가 접근할 수 있다. 도커가 사용한다.
        
        도커를 설치하면 물리NIC가 docker0이라는 가상 브리지 네트워크로 연결됩니다. 
        
        도커 컨테이너가 실행되면 각각 컨테이너는 가상 NIC로 가상 브리지에 연결되고 가상 브리지는 NAPT역할을 합니다.
        
    - 데이터
        
        도커는 파일을 복사할 때 수정이 가해지지 않으면 참조만 시킨다. 이런 방식으로 이미지 관리를 한다. 이미지 관리하는 스토리지 디바이스는 여러 종류가 있다.
        
        AUFS
        
        Btrfs
        
        Device Mapper
        
        OverlayFS
        
        ZFS : 오라클이 개발한 새로운 파일시스템. 익숙하지 않으면 안쓰는게 좋다함.
