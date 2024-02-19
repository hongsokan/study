# 도커


## [명령어](https://docs.docker.com/engine/reference/commandline/docker/) 

```bash
# 이미지 목록
docker images

# 도커 hub 에서 이미지 다운
docker pull [이미지명]

# 도커 hub 에서 이미지 검색
docker search [이미지명]

# 이미지 삭제
docker rmi [이미지명]

# 컨테이너 생성 및 컨테이너 접속
# -i : 사용자가 입출력을 할 수 있는 상태
# -t : 가상 터미널 환경을 에뮬레이션
# -d : 데몬 프로세스 형태로 실행
# -p : 포트 (호스트포트 : 컨테이너포트)
docker run [옵션] [이미지명] [실행파일]

# 실행 중인 컨테이너에 명령어 실행 (리눅스 명령어)
docker exec [컨테이너아이디/컨테이너명] [명령어]

# 실행 중인 컨테이너 목록
# -a : 종료된 컨테이너도 포함
docker ps [옵션]

# 실행 중인 컨테이너 종료
docker stop [컨테이너아이디/컨테이너명]

# 종료된 컨테이너 시작
docker start [컨테이너아이디/컨테이너명]

# 컨테이너 접속
docker attach [컨테이너아이디/컨테이너명]

# 컨테이너 삭제
docker rm [컨테이너아이디/컨테이너명]
```


## [호스트와 컨테이너의 파일시스템 연결](https://opentutorials.org/course/4781/30615)
```bash
# -p 로컬호스트포트:컨테이너포트
# -v 로컬호스트파일경로:컨테이너파일경로
docker run -p 8080:80 -v ~/Desktop/htdocs:/usr/local/apache2/htdocs/ httpd
```



