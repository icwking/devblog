# CentOS7 GitLab Install

## 1. CentOS7 Docker 설치

```sh
# yum 패키지 업데이트
yum -y update

# docker 설치
yum -y install docker

# 서비스에 등록
systemctl enable docker.service

# docker 실행
systemctl start docker.service

# docker 상태확인
systemctl status docker.service

# docker 버전 확인
docker --version
```

## 2. docker-compose 설치

[docker-compose 설치 공식 가이드](https://docs.docker.com/compose/install/)

```sh
# docker-compose 설치
curl -L "https://github.com/docker/compose/releases/download/1.25.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# docker-compose 실행옵션 권한 
chmod +x /usr/local/bin/docker-compose

# docker-compose 버전확인
docker-compose --version
```

## 3. GitLab 설치

```sh

```

## 4. GitLab docker-compose 파일 작성 및 실행

```sh

```
