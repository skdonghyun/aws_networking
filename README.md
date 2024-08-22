# aws_networking

실습 주소(이번주 금요일 까지)
> https://tinyurl.com/y8d7edcf

- 첫시간
5단계 간략 설명, 실습은 알아서.

## AWS 기반 네트워킹
- 리전 : 물리적인 데이터 센터
- 가용영역 :  논리적인 그룹, aws 3개 이상의 가용영역 있음, 100~150km
- subnet은 가용영역 단위에 배포
- VPC와 subnet ip 비트 단위 찾아보기
![alt text](AWS_기반_네트워킹1.png)

- 클래스 없는 도메인 간 라우팅(CIDR) <- CIDR 찾아보기
    - CIDR 표기법은 IP 주소와 해당 네트워크 마스크를 표시하는 방법
        - IPv4주소는 8비트로 구성된 4개의 그룹 (10.22.0.0/16)
            - /16은 네트워크 식별을 위해 처음 16비트를 예약함, 나머지 비트는 호스트 범위 (10.22.0.0~10.22.255.255)
            - 예 : 192.168.10.10
        - IPv6주소는 128비트이며 16비트 또는 4개의 16진수로 구성된 8개 그룹으로 나눔
            - a:b:c:d:e:f:g:h 각각 16비트
            - 보통은 64비트를 network 주소, 나머지 64비트를 호스트로
            - 예 : 2001:0db8:1234:1a00:00fc:0000:0000:0001
            - 퍼블릭 주소 : 글로벌 유니캐스트(2000::/3)
- 같은 VPC면 통신 된다.(인바운드 및 아웃바운드 트래픽 필터링 필요 : 방화벽 서비스)
- 0(network 주소), 1(디폴트), 2(라우터(도메인 네임 관리)), 3, 255(브로드캐스트 주소)
![alt text](AWS_기반_네트워킹2.png)

## 리전->vpc->subnet->ec2
- ec2 만들떄 public 여부 선택 가능
## nat gateway
- public private ip가 있음. 
- private ec2 -> nat -> internet -> nat -> private ec2 (인바운드가 직접 못들어옴, 아웃 바운드해서 들어오는건 가능)

## 서브넷 특성
- 서브넷은 VPC CIDR 블록의 하위 집합
- 각 서브넷은 하나의 가용 영역내에만 존재합니다.
- 하나의 가용 영역에 여러 서브넷을 포함할 수 있습니다.
- 작은 서브넷보다 큰 서브넷을 사용하는 것이 좋습니다.

## 퍼블릭 서브넷과 프라이빗 서브넷 비교
- VPC에는 여러 라우팅 테이블 만들수 있음
- 서브넷마다 필요한 라우팅 테이블 associate
- 프라이빗 서브넷은 nat 게이트웨이를 사용하여 제한된 아웃 바운드 퍼블릭 인터넷 액세스 지원

## 듀억스택
- v4, v6 같이쓰는 모듈

## EC2 네트워크 카드 여러개 가능

## 라우팅 테이블
- VPC 레벨에서 정의 하는거
- 실제 패킷 라우팅은 subnet
- 경로
- 실제 퍼블릭 IP가 있어야 외부와 통신 가능
- VPC에는 기본 라우팅 테이블이 있다.
- 모든 서브넷에는 연결된 라우팅 테이블이 있어야함. 설정 안하면 기본으로 설정됨

## 인터넷 게이트웨이 (v4)
- AWS가 알아서 가용성 관리해줌
- private ip가 public ip로 바꿈

## 송신 전용 인터넷 게이트웨이 (v6) egree 

## NAT 게이트웨이
- Ipv4 전용
- public subnet에 NAT 할당

 ## 네트워크 액세스 제어 목록 (NACL)
- subnet 수준의 검사(inbound, outbound)
- 기본적으로 모든 인바운드 및 아웃바운드 트래픽 허용
- 규칙에 따라 트래픽 허용 또는 거부 가능
- 거부 셋팅 순서 중요 제일 처음 순서가 먼저 먹음
- 나가는 규칙 들어오는 규칙 다 있어야함
 ## SG (security group)
- IF 레벨에서 검사
- ex) db<-> app
- 기본 규칙은 모든 아웃바운드 허용, 모든 인바운드 트래픽 차단