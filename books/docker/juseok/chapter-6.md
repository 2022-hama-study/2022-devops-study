# 6. 실전에 활용 가능한 컨테이너 사용법을 익히자

## 목차

### "내게 필요한 지식이 무엇인지 정리하자"
1. [컨테이너와 호스트 사이에 파일 복사](#1-컨테이너와-호스트-사이에-파일-복사-) (⭐⭐⭐) → 개발자
2. [볼륨 마운트](#2-볼륨-마운트) → 개발자
3. [컨테이너로 이미지 만들기](#3-컨테이너로-이미지-만들기) → 개발자
4. [도커파일로 이미지 만들기](#4-도커파일로-이미지-만들기) (⭐⭐⭐⭐) → 개발자
5. [컨테이너 개조](#5-컨테이너-개조) → 개발자
6. [도커 허브 로그인](#6-도커-허브-로그인) → 개발자 및 서버 엔지니어
7. [도커 컴포즈](#7-도커-컴포즈) (⭐⭐⭐) → 개발자 및 서버 엔지니어
8. [쿠버네티스](#8-쿠버네티스) (⭐⭐⭐) → 서버 엔지니어 및 보안 엔지니어

----

## 1. 컨테이너와 호스트 사이에 파일 복사 👍

### 볼륨 마운트 없이 명령어로 복사가 가능하다!
(개인적으로 완전 꿀기능인것 같다. 볼륨 마운트를 하지않았을 때.. 
도커를 내리면 안되는 상황이 종종 있다.. 
이럴때마다 파일내용을 손수 복사해서 메모장에 옮겨놓고 
마운트 옵션을 추가한 후 컨테이너를 재시작했는데
이젠 그럴 필요가 없다!!
진짜진짜 꿀기능이다!!)
- 호스트(로컬) → 컨테이너
```bash
$ docker cp [호스트 경로] [컨테이너 이름]:[컨테이너 경로]
```

- 컨테이너 → 호스트(로컬)
```bash
$ docker cp [컨테이너 이름]:[컨테이너 경로] [호스트 경로]
```

---

## 2. 볼륨 마운트

### 자주 쓰지는 않지만 지우면 안 되는 파일 : 도커 볼륨
도커 엔진이 관리하는 영역 내에 생성되는 일종의 파티션  
도커 엔진의 관리하에 있으므로 사용자가 파일 위치를 신경 쓸 필요가 없다.  
'바탕화면에 내버려 뒀다가 실수로 지워버리는' 일도 일어나지 않는다. ⭐  
운영체제마다 경로를 기재하는 방식이 다른데 볼륨을 사용하면 환경에 따라 경로가 바뀌는 일이 없다.  
하지만 관리 방법이 아주 복잡하다.  
컨테이너를 경유하지 않고 직접 볼륨에 접근할 방법이 없는것이 단점이다.  

-  볼륨 생성
```bash
$ docker volume create [볼륨 이름]
```

- 볼륨 삭제
```bash
$ docker volume rm [볼륨 이름]
```

### 자주 사용하는 파일 : 바인드 마운트
디렉토리나 파일을 직접 `[호스트 경로]:[컨테이너 경로]` 로 연결하는 방법  
파일을 **호스트에서도 직접 수정하거나 접근**할 수 있어야 한다면 **바인드 마운트**를 사용해야한다.  

- 바인드 마운트
```bash
$ docker run -v [호스트 디렉토리 또는 파일]:[컨테이너 디렉토리 또는 파일]
```

----

## 3. 컨테이너로 이미지 만들기
지금까지는 아마 도커파일로 이미지를 생성했을것이다.  
하지만 이미 존재하는 컨테이너를 이용하면 누구나 쉽게 이미지를 재생산 할 수 있다.  
(개인적으로 권장하지는 않는다.. 추후 관리가 개판이 되기에..)  
기존 컨테이너를 복제하거나 이동해야 할 때, 또는 개발중에 변경된 사항이 많아 기억하기 힘들 때 유용하다.  
이를 제외한 일반적인 상황에서는 도커파일을 작성하는 습관을 들이자..  

- 컨테이너 → 이미지 변환
```bash
$ docker commit [컨테이너 이름] [저장할 이미지 이름]
```

----

## 4. 도커파일로 이미지 만들기
이 책에서는 도커파일(Dockerfile)은 이미지 만드는 것밖에 할 수 없다고 쓰여있지만,  
도커파일에는 많은 정보들이 포함되기 때문에 한눈에 프로젝트 구조를 *한 눈에* 파악할 수 있기도 하고,  
디펜던시도 명시적으로 관리 할 수 있다.

- Dockerfile 로 이미지 생성하기
```bash
$ docker build -t [생성할 이미지 이름] [도커파일이 위치하고 있는 경로]
```

- 주요 Dockerfile 인스트럭션  

| 인스트럭션 | 내용 |  
|:--------|:----|  
|FROM|토대가 되는 이미지 지정|  
|ENV|환경변수 지정|  
|ARG|docker build 커맨드 사용 시 입력받을 수 있는 인자 선언|  
|LABEL|이름이나 버전, 저작자 정보를 설정|  
|SHELL|이미지 빌드 시 사용할 쉘 설정|  
|ADD|원격지에 있는(Url로 불러와야하는)파일이나 폴더를 추가|  
|COPY|호스트에 있는 파일이나 폴더를 추가|  
|RUN|빌드 시 실행할 명령어 지정|  
|WORKDIR|RUN, CMD, ENTRYPOINT, ADD, COPY 에 정의된 명령어를 실행하는 작업 디렉토리를 지정|  
|USER|RUN, CMD, ENTRYPOINT 에 정의된 명령어를 실행하는 사용자 또는 그룹 지정|  
|CMD|컨테이너를 실행할 때 최초 실행 할 명령어 지정(오버라이드 허용)|  
|ENTRYPOINT|컨테이너를 실행할 때 최초 실행 할 명령어 지정(오버라이드 비허용)|  
|EXPOSE|강제로 통신에 사용할 포트를 지정|  

- ADD vs COPY  
ADD 는 원격지의 파일을 이미지 안에 포함시키고자 할 때 사용하고,  
COPY 는 호스트(로컬) 파일을 이미지 안에 포함시켜야 할 때 사용한다.

- CMD vs ENTRYPOINT  
CMD 와 ENTRYPOINT 의 역할은 같지만, 차이가 조금 있다.  
예를 들어 `CMD ["echo", "hello"]` 라고 지정했다면,  
컨테이너를 실행 할 때 `docker run [이미지 이름] echo hi` 라고 하면 hello가 출력되지 않고 hi가 출력된다.  
하지만 `ENTRYPOINT ["echo", "hi"]` 로 지정했을 때는 hello hi 둘 다 출력된다.  
명령어가 오버라이드 되지 않고 강제실행, 고정되는 것이다.  
`ENTRYPOINT`는 다른 명령어가 들어와도 무조건 실행해야하는 명령어가 있을 때 사용한다.

----

## 5. 컨테이너 개조
개...조..?  
이책에서는 컨테이너 안에 들어가서 작업하고 변동사항이 발생한것을 개조라고 표현했다..  
이미 실행중인 컨테이너에 접속하기위해서는 아래 명령어를 이용한다.  

- 컨테이너 접속
```bash
$ docker exec -it [컨테이너 이름] bash
```

----

## 6. 도커 허브 로그인

### 이미지는 어디서 내려받는 걸까? => 도커허브!  
도커파일을 처음 작성할때마다 FROM 인스트럭션 뒤에 필요한 이미지를 적어 넣었다.  
그럼 이 이미지들은 어디서 오는 걸까?  
바로 도커허브이다.  

- 도커허브? 도커 레지스트리?  
이미지를 배포하는 장소를 통칭 `도커 레지스트리` 라고 하고  
`도커허브`는 '도커 제작사에서 운영하는 공식 도커 레지스트리' 를 말한다.  

- 레지스트리 vs 레파지토리  
`레지스트리`는 이미지를 배포하는 **장소** 이고,  
`레파지토리`는 레지스트리를 구성하는 **단위** 이다.  

깃허브로 비유하자면, 레지스트리는 깃허브..  
레파지토리는 레파지토리.. OK?  

도커 허브에 이미지를 업로드하려면 이미지마다 태그를 부여해야한다.  
아래는 태그 붙이는 예시이다.  
`hama.com/hama_devops_study:0.0.1`  
[레지스트리 주소]/[레포지토리 이름]:[버전] 형식을 띈다.  
왠만하면 버전을 붙여주자.. 나중에 골치 아프다..  

이미 이미지에 태그를 달지않고 올려버렸다..  
하지만 걱정마시라.. 나중에도 붙일 수 있다.  
```bash
$ docker tag [원본 이미지 이름] [레지스트리 주소]/[레파지토리]:버전
```
위 커맨드로 태그를 붙여주면 태그가 붙은 새로운 이미지가 생성(복제)된다.  

- 로컬에서 도커허브에 이미지 업로드  
레파지토리는 처음 업로드할 때는 존재하지 않는다.  
push 커맨드를 실행하면서 자동 생성된다.  
```bash
$ docker push [레지스트리 주소]/[레파지토리 이름]:[버전]
```

- 비공개 레지스트리 만드는 방법  
외부 공개 목적이 아닌 사내 개발이 목적이라면 대게 사내용 도커 레지스트리를 만들고  
여기에 개발환경 이미지를 배포하는 체계를 갖는다고 한다.  
도커 레지스트리는 간단하게 만들 수 있다.  

레지스트리용 컨테이너(`registry`)가 따로 있으므로 이를 사용하면 된다.  
즉, 레지스트리도 도커를 통해 운영이 가능하다.  

- docker-compose.yaml 파일로 만들어 보면, 아래와 같이 구성할 수 있다.  

이걸 제대로 구축하려면 SSL 인증부터 도메인까지 준비해야 할 것이 너무 많다.  
그냥 AWS ECR 을 쓰도록 하자..  
AWS ECR 과 도커 비공개 레지스트리는 같은 개념이다.  
호스팅 주체가 AWS 냐 Docker 냐의 차이일 뿐..  
```yaml
version: "3"
services:
  registry:
    restart: always
    image: registry:2
    ports:
      - 8888:5000
    environment:
      REGISTRY_HTTP_TLS_CERTIFICATE: /certs/domain.crt
      REGISTRY_HTTP_TLS_KEY: /certs/domain.key
    volumes:
      - ./path/:/var/lib/registry
      - $HOME/docker_registry/certs:/certs
```

----

## 7. 도커 컴포즈
개발자라면..! 정말 정말 알아놓으면 업무효율이 증대되는 꿀 기술이라고 생각한다.  
도커를 사용하다 보면 `docker run ...` 이런식으로 도커 명령어로 컨테이너를 관리한다.  
그런데 옵션이 한두개일때는 별 상관이 없지만 3개, 4개 그 이상이 되기 시작하면.. 헷갈리고 매번 그 긴 명령어를 기억하기란 정말 쉽지 않은 일이다.  

도커파일은 이미지를 생성하기 위해 사용한다면, 도커 컴포즈 파일은 컨테이너를 띄우고 관리하기 위해 사용한다고 할 수 있을 것 같다.  
  
하지만 도커 컴포즈를 사용하면 위 문제를 해결 할 수 있다!  
예를 들어보면,  
`docker run -dit --gpus all -e NVIDIA_VISIBLE_DEVICES=all -v ./Projects:/home/hama/Projects --workdir /home/hama/Projects --restart always --network host --name examples_1 examples:0.0.3 /bin/bash`  
보기만해도.. 정신사납다..  

도커 컴포즈 파일을 작성해보자.  
```yaml
version: "3"
services:
  examples:
    image: examples:0.0.3
    container_name: examples_1
    runtime: nvidia
    environment:
      - NVIDIA_VISIBLE_DEVICES=all
    volumes:
      - ./Projects:/home/hama/Projects
    working_dir: /home/hama/Projects
    network_mode: "host"
    tty: true
    restart: always
```
이처럼 도커 컴포즈 파일을 작성하면 길게 늘어져 있는 도커 옵션을 한눈에 파악할 수 있다. = 가독성 향상!  
그리고 외우지 않아도 파일로 관리 할 수 있기 때문에 프로젝트가 많아져도 관리의 부담이 줄어든다는 장점이 있다.  

사실 이게 도커 컴포즈를 사용해야하는 첫번째 이유이고..  
두번째는 도커 네트워크, 도커 볼륨을 이 파일안에서 정의 할 수 있다는 점이다.    

위 예제는 로컬 서버에 이미 있는 이미지로 `컨테이너를 띄우기` 위한 컴포즈파일 예시이고  
아래는 원하는 이미지가 없을 때 미리 작성해둔 도커파일로 `이미지를 빌드`하는 예시이다.  
```yaml
version: "3"
services:
  examples:
    image: examples:0.0.3
    container_name: examples_1
    build:
      context: .
      dockerfile: Dockerfile
    runtime: nvidia
    environment:
      - NVIDIA_VISIBLE_DEVICES=all
    volumes:
      - ./Projects:/home/hama/Projects
    working_dir: /home/hama/Projects
    network_mode: "host"
    tty: true
```
이처럼 빌드 시스템?으로서도 활용이 가능하다.  

컨테이너를 여러개 정의해서 한번에 띄우는 것도 가능하다.  
```yaml
version: "3"
services:
    vision:
        container_name: vision
        image: vision:0.0.1
        build:
          context: ./
          dockerfile: vision/Dockerfile
        runtime: nvidia
        volumes:
         - ./:/Projects
        ports:
         - 8000:8000
        working_dir: /Projects
        tty: true
        restart: always
        command: >
          python3 main.py
          --host=0.0.0.0
          --port=8000
          --config=/Projects/vision/configs/config.json

    analyzer:
        container_name: analyzer
        image: analyzer:0.0.1
        build:
            context: ./
            dockerfile: notifier/Dockerfile
        volumes:
         - ./:/Projects
        working_dir: /Projects
        tty: true
        restart: always
        command: python3 main.py -c /Projects/analyzer/configs/config.json

    recorder:
        container_name: recorder
        image: recorder:0.0.1
        build:
            context: ./
            dockerfile: recorder/Dockerfile
        volumes:
         - ./:/Projects
        working_dir: /Projects
        tty: true
        restart: always
        command: python3 main.py -c /Projects/recorder/configs/config.json

    storage:
        container_name: storage
        image: python:3.7-alpine
        volumes:
         - /hdd_ext/outputs:/outputs/
        ports:
         - 9805:9805
        restart: always
        working_dir: /outputs/
        command: python3 -m http.server 9805

    monitor:
        container_name: monitor
        image: monitor:0.0.1
        build:
            context: ./
            dockerfile: monitor/Dockerfile
        privileged: true
        volumes:
         - ./:/Projects
        working_dir: /Projects
        tty: true
        restart: always
        command: python3 main.py -c /Projects/monitor/configs/config.json
```
마구잡이로 작성하긴 했지만..  
대충 이런 느낌으로 포트를 열어 데이터를 넘길 수도 있고  
한꺼번에(동시에) 관리가 가능하다.  

----

## 8. 쿠버네티스
(작성중...)
