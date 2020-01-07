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

아래 공식 문서를 참조하자.

[공식 문서](https://docs.gitlab.com/omnibus/docker/README.html)

```sh
#gitlab 이미지 설치
docker pull gitlab/gitlab-ce:latest

#gitlab 실행
sudo docker run --detach \
    --hostname 192.168.0.72 \
    --publish 7443:443 --publish 7080:80 --publish 7022:22 \
    --name gitlab \
    --restart always \
    --volume /Users/icwking/devroot/45.dev_docker/service/gitlab/config:/etc/gitlab \
    --volume /Users/icwking/devroot/45.dev_docker/service/gitlab/logs:/var/log/gitlab \
    --volume /Users/icwking/devroot/45.dev_docker/service/gitlab/data:/var/opt/gitlab \
    gitlab/gitlab-ce:latest

```
--publish host port : docker port

--restart always : 도커가 자동으로 실행시켜주는 옵션

--hostname : 깃의 호스트 이름이다. 추후에 깃 프로젝트를 생성후 clone을 하게 되면 호스트이름을 베이스로 주소를 생성한다.


## 4. GitLab docker-compose 파일 작성 및 실행
아래 공식 문서를 참조하자.

[공식 문서](https://docs.gitlab.com/omnibus/docker/README.html#install-gitlab-using-docker-compose)

### docker-compose.yml 파일 작성

```yml
web:
  image: 'gitlab/gitlab-ce:latest'
  restart: always
  hostname: 'gitlab.example.com'
  container_name: gitlab
  environment:
    GITLAB_OMNIBUS_CONFIG: |
      external_url 'https://gitlab.example.com:6080'
      gitlab_rails['gitlab_shell_ssh_port'] = 6022
      # Add any other gitlab.rb configuration here, each on its own line
  ports:
    - '6080:6080'
    - '6443:443'
    - '6022:22'
  volumes:
    - '/srv/gitlab/config:/etc/gitlab'
    - '/srv/gitlab/logs:/var/log/gitlab'
    - '/srv/gitlab/data:/var/opt/gitlab'
```

### docker-compose로 gitlab 실행 
```sh
docker-compose up -d
```

### gitlab 버전 업그레이드

```sh
# docker-compose.yml의 위치로 이동
docker-compose pull   # gitlab update
docker-compose up -d  # gitlab 재실행
```

### gitlab 백업

```sh
#docker exec -t <컨테이너 이름> gitlab-backup create
docker exec -it gitlab gitlab-backup create
```
/srv/gitlab/data/backups 디렉토리에 1578373571_2020_01_07_12.6.2_gitlab_backup.tar 형식으로 백업파일이 생성된다.

### gitlab 복구
```sh
docker exec -it gitlab gitlab-backup restore
```
