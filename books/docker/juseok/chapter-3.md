# 3. 도커를 사용해보자

## 3-1. 도커를 실행하기 위한 조건
- 64비트 운영체제
- 리눅스 운영체제(Ubuntu 18.04 LTS 이상)

## 3-2. 도커 설치 (on Ubuntu(linux))
### 사전작업
```bash
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

```bash
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### apt 서버 update & docker 엔진 install
```bash
sudo apt-get update && sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

### 설치 확인
```bash
sudo docker run hello-world
```

----

## 도커 컴포즈 설치
```bash
python3 -m pip install docker-compose
```

----

## 엔비디아 도커 설치 (호스트 서버에 nvidia driver 설치되어 있어야 함)
호스트 서버의 GPU를 공유하여 사용하려면 `nvidia-docker2` 가 설치되어 있어야 한다.  
이게 설치되어 있지 않으면 `docker run --gpus all` 옵션을 사용할 수 없고,  
마찬가지로 docker-compose 스크립트에서 `runtime: nvidia` 로 gpu자원을 공유할 수 없다.
  
### 사전작업
```bash
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
      && curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
      && curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list | \
            sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
            sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
```
  
### apt 서버 update & nvidia-docker 엔진 install
```bash
sudo apt-get update && sudo apt-get install -y nvidia-docker2
sudo systemctl restart docker
```
  
### 설치 확인
```bash
sudo docker run --rm --gpus all nvidia/cuda:11.0.3-base-ubuntu20.04 nvidia-smi
```
