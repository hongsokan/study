[참고링크](https://sleepyeyes.tistory.com/36)

```shell
vi /etc/profile

export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64
export JEUS_HOME=/home/dev/jeus8
PATH=$JEUS_HOME/bin:$PATH

source /etc/profile
```

## jeus 설치
```shell
chmon 755 jeus8001_unix_generic_ko.bin
./jeus8001_unix_generic_ko.bin

source /etc/profile
````

## jeus 실행 ([명령어](https://technet.tmaxsoft.com/upload/download/online/jeus/pver-20140827-000001/reference-book/reference_jeus_Script.html))
```shell
startDomainAdminServer -domain jeus_domain -u administrator -p password
```

## jeus 끄기
```shell
stopServer -host localhost:9736 -u administrator -p password
```
