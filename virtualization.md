# 가상화
- 하나의 물리적인 머신(하드웨어)에서 여러 OS 실행
- [가상 머신](https://www.redhat.com/ko/topics/virtualization/what-is-a-virtual-machine) : 물리적 하드웨어 시스템(오프프레미스/클라우드 또는 온프레미스에 위치)에 구축되어 자체 CPU, 메모리, 네트워크, 스토리지를 갖추고 가상 컴퓨터 시스템으로 작동하는 가상 환경
- [하이퍼바이저](https://www.redhat.com/ko/topics/virtualization/what-is-a-hypervisor) : 가상 머신을 생성하고 구동하는 소프트웨어. 하이퍼바이저로 사용되는 물리 하드웨어를 호스트라고 한다. (인텔 기반 제품 : VMWare, Virtualbox, Parellels)
- Provisioning : IT 인프라를 생성, 설정하는 프로세스. 사용자의 요구에 맞게 시스템 자원을 할당, 배치, 배포


## [Vagrant](https://developer.hashicorp.com/vagrant)
- 가상 머신 기반 개발 환경을 관리하는 도구, 가상화 인스턴스를 관리해주는 SW (Provisioning 역할을 해준다)
