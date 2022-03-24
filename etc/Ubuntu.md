# Ubuntu 20.04 LTS 네트워크 연결하기(고정IP 할당까지)
## 네트워크 연결 (GUI환경)
1. 네트워크탭 - Wired Settings - Wired의 설정창 - IPv4 Manual로 변경 - 원하는 IP 입력
2. Wired Off 하고 다시 On

## 고정 IP 할당 (CLI)
``` shell
vi /etc/netplan/50-cloud-init.yaml

network:
  ethernets:
    ens160:
      addresses:
      - 192.168.1.60/24
      gateway4: 192.168.1.1
      nameservers:
        addresses:
        - 8.8.8.8
        search:
        - 8.8.4.4
  version: 2
```

``` shell
netplan apply
```


# Ubuntu 서버 설치
## 참고사이트 (https://jay-ji.tistory.com/52)
1. USB 포맷
2. ISO 파일 쓰기 - etcher(https://www.balena.io/etcher/)

> ubuntu (https://releases.ubuntu.com/20.04/)