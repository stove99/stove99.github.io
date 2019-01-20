---
layout: post
title:  "Docker 로 Gitlab 설치하기"
date:   2019-01-20
keywords: "docker,gitlab"
categories: [Docker]
tags: [docker,gitlab]
icon: icon-docker
---

# 이미지 가져오기 및 실행하기
http 는 8181 포트로 https 는 8182 포트로 ssh 접속은 8183 포트로 설정
> `--publish 8181:80 --publish 8182:443 --publish 8183:22`

생성되는 데이터는 /srv/gitlab 으로 지정해서 데이터 유지되게 함
> `--volume /srv/gitlab/config:/etc/gitlab`
>
> `--volume /srv/gitlab/logs:/var/log/gitlab`
>
> `--volume /srv/gitlab/data:/var/opt/gitlab`

``` bash
sudo docker run  \
    --detach \
    --hostname stove99.github.io \
    --publish 8181:80 --publish 8182:443 --publish 8183:22 \
    --name gitlab \
    --restart always \
    --volume /srv/gitlab/config:/etc/gitlab \
    --volume /srv/gitlab/logs:/var/log/gitlab \
    --volume /srv/gitlab/data:/var/opt/gitlab \
    gitlab/gitlab-ce:latest
```

요렇게 실행한 다음 브라우져에 http://127.0.0.1:8181 로 접속되면 된다.

※ 처음 실행할때 실행 시간이 쫌 걸리는데 sudo docker ps gitlab 명령으로 서버가 다 올라갔는지 알 수 있다.




# Gitlab 설정하기 방법1
docker exec 로 컨테이너 쉘로 접속해 직접 이것저것 작업하고 gitlab-ctl 로 재시작하기

``` bash
# 컨테이너로 접속
sudo docker exec -it gitlab /bin/bash

# 설정파일 편집
vi /etc/gitlab/gitlab.rb

# Gitlab 재시작
gitlab-ctl reconfigure
gitlab-ctl restart
```




# Gitlab 설정하기 방법2
docker exec 로 컨테이너 vi 로 직접 편집하기

``` bash
# 설정파일 편집
sudo docker exec -it gitlab vi /etc/gitlab/gitlab.rb

# Gitlab 재시작
sudo docker restart gitlab
```




# gitlab.rb 에서 변경할만한 부분
별로 수정할건 딱히 없지만 외부 URL 설정과 SSH 접속포트 설정정보는 바꿀 경우가 많을것 같다.

이곳에서 설정하는 정보에 따라 프로젝트 clone url 이 바뀜

|-----------------|------------|
| 설정            |             |
|-----------------|:-----------|
| 외부 URL        | external_url : "http://gitlab.example.com:8929" |
| SSH 접속 포트   | gitlab_rails['gitlab_shell_ssh_port'] = XXX     |






# Gitlab 업데이트 하기
컨테이너와 이미지를 싹 지우고 최신 이미지를 받아서 다시 컨테이너를 실행하는 방식으로 업데이트 하면 된다.

``` bash
# 정지후 이미지 삭제 > 최신버전 받기
sudo docker stop gitlab
sudo docker rm gitlab
sudo docker pull gitlab/gitlab-ce:latest

# 컨테이너 다시 생성
sudo docker run  \
    --detach \
    --hostname stove99.github.io \
    --publish 8181:80 --publish 8182:443 --publish 8183:22 \
    --name gitlab \
    --restart always \
    --volume /srv/gitlab/config:/etc/gitlab \
    --volume /srv/gitlab/logs:/var/log/gitlab \
    --volume /srv/gitlab/data:/var/opt/gitlab \
    gitlab/gitlab-ce:latest
```