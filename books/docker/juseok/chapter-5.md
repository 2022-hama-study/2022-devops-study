# 5. 여러개의 컨테이너 연동하기

- 포트 포워딩
- 도커 네트워크

### 1. 포트 포워딩(=맵핑)
일반적인 선에서 포트 포워딩(=포트 맵핑)으로 해결이 가능하다.  
`docker run` 으로 컨테이너를 생성할 때 `-p [호스트 포트]:[컨테이너 포트]` 로 이어주면 된다.  
하지만 보안을 강화하고자 한다면 아래 방법을 사용하면 된다.  

### 2. 도커 네트워크
도커의 네트워크는 AWS의 VPC(Virtual Private Cloud)와 기능이 같고 가상 내부 네트워크이다.  
네트워크를 생성할 때 옵션에 따라 외부통신/내부통신 용으로 생성할 수 있다.  

| Docker | AWS |
|:--:|:--:|
| 네트워크 | 서브넷 |
| none 네트워크 | private 서브넷 |
| host 네트워크 | public 서브넷 |
| 컨테이너 | 인스턴스 |
| veth | NACL |
| docker0 | 라우팅 테이블 |

<p>
    <img src="https://github.com/2022-hama-study/2022-devops-study/blob/master/books/docker/juseok/assets/chapter5-1.png", height="400x">
</p>  

Docker를 설치한 후 호스트의 네트워크 인터페이스를 살펴보면 `docker0`라는 가상 인터페이스가 생긴다.  
**docker0** 는 일반적인 가상 네트워크 인터페이스가 아니라 도커가 자체적으로 제공하는  
네트워크 드라이버 중 하나이며 **브릿지(bridge)** 에 해당한다.  
**docker0** 브릿지는 컨테이너간 통신을 위해 사용된다.  

네트워크를 별도로 생성하면 각 네트워크마다 각각 다른 대역의 서브넷이 할당된다.  
docker 커맨드 옵션 중 `--net` 또는 docker-compose 의 `networks` 옵션으로 네트워크를 지정하여 컨테이너를 생성하면  
해당 컨테이너는 지정한 네트워크의 **서브넷 대역 안에서** IP를 할당받게 된다.  
#
#### Example-1) 별도의 네트워크 `aquaNuri_net` 을 생성했을 때 생성한 네트워크를 컨테이너에 연결하는 예제
```yaml
version: "3"
services:
    analysis:
        image: aquaNuri_analysis
        container_name: analysis_server
        runtime: nvidia
        shm_size: "8gb"
        ulimits:
            memlock: -1
            stack: 67108864
        environment:
          - NVIDIA_VISIBLE_DEVICES=all
        privileged: true
        tty: true
        volumes:
          - /tmp/.X11-unix:/tmp/.X11-unix:ro
        environment:
          - DISPLAY=$DISPLAY
        networks:
          - default
          
    db:
        image: postgres
        environment:
          - POSTGRES_USER=postgres
          - POSTGRES_PASSWORD=postgres

networks:
    default:
        external:
            name: aquaNuri_net
```
#
#### Example-2) analysis_server 컨테이너는 aquaNuri_net 과 default 네트워크에만 연결, db는 default 네트워크에만 연결
여기서 정의된 `aquaNuri_net` 은 도커컴포즈파일 내에서 정의된 것이기 때문에 컨테이너를 내릴때 네트워크도 같이 소멸된다.  
```yaml
version: "3"
services:
    analysis:
        image: aquaNuri_analysis
        container_name: analysis_server
        runtime: nvidia
        shm_size: "8gb"
        ulimits:
            memlock: -1
            stack: 67108864
        environment:
          - NVIDIA_VISIBLE_DEVICES=all
        privileged: true
        tty: true
        volumes:
          - /tmp/.X11-unix:/tmp/.X11-unix:ro
        environment:
          - DISPLAY=$DISPLAY
        networks:
          - default
          - aquaNuri_net
          
    db:
        image: postgres
        environment:
          - POSTGRES_USER=postgres
          - POSTGRES_PASSWORD=postgres

networks:
    aquaNuri_net:
        driver: bridge
```
