# 배경지식

- 시스템기반
    
    기능 요구사항 : 시스템이나 소프트웨어에서 무엇을 할 수 있는지
    
    비기능 요구사항 : 시스템의 성능, 신뢰성, 확장성, 운용성, 보안
    
    구성
    
    미들웨어
    
    DB(Data Base)
    
    RPC(Remote Procedure Control)
    
    MOM(Message Oriented Middleware)
    
    TP(Transaction Processing)-Monitor
    
    ORB(Object Request Broker)
    
    WAS(Web Application Server)
    
    OS
    
    클라이언트OS
    
    서버OS
    
    하드웨어/네트워크
    
- 시스템 이용 형태
    
    온프레미스 : 높은 가용성, 기밀성, 특수한 요구사항
    
    클라우드 : 트래픽변동, 백업, 빠른 서비스
    
    퍼블릭클라우드
    
    프라이빗클라우드
    
- 폭포형 개발 방법 vs 애자일 개발 방법
    
    폭포형
    
    타당성 검토 → 계획 → 요구 분석 → 설계 → 구현 → 테스트 → 유지보수
    
    애자일(Agile Software Development)
    
    개발과 함께 즉시 피드백을 받아서 유동적으로 개발하는 방법이다.
    
    켄트 벡이 주창한 [익스트림 프로그래밍](https://namu.wiki/w/%EC%9D%B5%EC%8A%A4%ED%8A%B8%EB%A6%BC%20%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D) (XP, Extreme Programming)과 [테스트 주도 개발](https://namu.wiki/w/%ED%85%8C%EC%8A%A4%ED%8A%B8%20%EC%A3%BC%EB%8F%84%20%EA%B0%9C%EB%B0%9C)
    (TDD)이 대표적이다.
    
- 시스템 기반의 구축/운용 흐름
    - 시스템 구축 계획, 요구사항 정의
        
        시스템 구축 범위 선정
        
        인프라 요구사항 정의
        
        예산 책정
        
        프로젝트 체계화
        
        기존 시스템과의 연계
        
        시스템 마이그레이션 계획
        
    - 인프라 설치
        
        인프라 아키텍처 설계
        
        네트워크 토폴로지 설계
        
        장비선택, 조달(클라우드 : 서비스 선택)
        
        OS, 미들웨어 선택, 조달(클라우드 : 서비스 선택)
        
        시스템 운용 설계
        
        시스템 마이그레이션 설계
        
    - 인프라 구축
        
        네트워크 부설
        
        서버 설치
        
        OS 셋업
        
        미들웨어 셋업
        
        애플리케이션, 라이브러리 설치
        
        테스트(네트워크 확인, 부하 테스트, 운용 테스트)
        
        시스템 릴리스 및 마이그레이션
        
    - 운용
        
        서버 프로세스, 네트워크, 리소스, 배치 Job 모니터링
        
        데이터 백업 및 정기 유지보수
        
        OS, 미들웨어 버전 업그레이드
        
        애플리케이션 버전 업그레이드
        
        시스템 장애 시 대응
        
        사용자 서포트(헬프데스크)
        
- 하드웨어 네트워크 기초지식
    
    서버장비 : CPU 메모리 스토리지
    
    MAC주소 vs IP주소
    
    OSI참조모델과 통신프로토콜
    
    방화벽 : 패킷필터형, 애플리케이션게이트웨이형
    
    라우터, 레이어3스위치
    
- OS 기초지식(리눅스)
    
    리눅스는 두 가지 뜻을 가지고 있다.
    
    리눅스 커널 : OS의 코어
    
    리눅스 배포판 : 리눅스 커널 + 각종 커맨드, 라이브러리, 애플리케이션
    
    종류 : Debian 계열, Red Hat 계열, Slackware 계열 등
    
    - 리눅스 커널
        
        주요 기능 : 디바이스 관리, 프로세스 관리, 메모리 관리
        
        쉘스크립트로 제어(bash, csh, tcsh, zsh)
        
        리눅스 파일 시스템
        
        디바이스가 달라도 가상 파일 시스템에서 쉽게 접근하도록 함
        
        ext2, ext3, ext4, tmpfs, UnionFS, ISO-9660, NFS
        
        - 리눅스 디렉토리 구성(FHS 규격에 의해 표준화되어있다)
            
            /root
            
            /bin : 기본 커맨드
            
            /boot : OS 시작에 필요한 파일
            
            /dev : 디바이스 파일
            
            /etc : 설정 파일
            
            /home : 사용자 홈 디렉토리
            
            /lib : 공유 라이브러리
            
            /mnt : 파일 시스템의 마운트 포인트용 디렉토리
            
            /media : CD/DVD-ROM의 마운트 포인트
            
            /opt : 애플리케이션 소프트웨어 패키지
            
            /proc : 커널이나 프로세스에 대한 정보
            
            /root : 특권 사용자용 홈 디렉토리
            
            /sbin : 시스템 관리용 마운트
            
            /srv : 시스템 고유의 데이터
            
            /tmp : 임시 디렉토리
            
            /usr : 각종 프로그램이나 커널 소스를 놓아두는 디렉토리
            
            /var : 로그나 메일 등 가변적인 파일을 놓아두는 디렉토리
            
    - 리눅스 보안
        
        권한 설정
        
        네트워크 필터링
        
        iptables로 리눅스 내장 패킷필터링, 네트워크주소변환(NAT) 기능 설정
        
        SELinux
        
- 미들웨어 기초지식
    - 웹서버/웹 어플리케이션 서버
        
        클라이언트의 브라우자거 보낸 HTTP 요청을 받아 웹콘텐츠(HTML, CSS) 응답으로 반환하거나 다른 서버사이드 프로그램 호출
        
        Apache HTTP Server, Internet Information Services, Nginx
        
    - 데이터베이스 서버
        
        RDBMS : MySQL, PostgreSQL, OracleDB
        
        NoSQL : Redis, MongoDB, Apache Cassandra
        
    - 시스템 감시 툴
        
        Zabbix, Datadog, Mackerel
        
- 인프라 구성 관리 기초 지식
