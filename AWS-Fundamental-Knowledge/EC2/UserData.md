## EC2 User Data
- EC2 인스턴스의 최초 실행 시 지정한 스크립트 실행 가능
  - 별도 설정을 통해서 재부팅마다 실행하도록 설정 가능
- 두 가지 모드
  - Shell Script
  - cloud-unit : 리눅스 이미지의 부트 스트래핑을 위한 오픈소스 애플리케이션
- 주요 사용 사례
  - EC2 인스턴스 설정(보안 설정, 인스턴스 설정 등)
  - 외부 패키지 다운로드
  - 설치 애플리케이션 실행
  - 기타 EC2 실행 시 필요한 동작

## EC2 Instance Metadata
>💡인스턴스 메타데이터는 실행 중인 인스턴스를 구성 또는 관리하는데 사용될 수 있는 인스턴스 관련 데이터입니다. 인스턴스 메타데이터는 호스트 이름, 이벤트 및 보안 그룹과 같은 범주로 분류됩니다.

- EC2 인스턴스의 속성 및 정보 데이터
  - AMID, IPv4/IPv6주소, EBS 맵핑, 보안 그룹 연동 상황, IAM 역할 연동 등
- 실행 중인 EC2 인스턴스의 메타데이터 IMDS(Instance Metadata Service)로 조회 가능
  - HTTP Endpoint 지원
  - IP주소 : 169.254.169.254(IPv4), fd00:ec2::254(IPv6)
  - 두 가지 모드
    - IMDS v1 : Request/Response 기반
    - IMDS v2 : 세션 기반(default)
- 주요 사용 사례
  - 인스턴스 별 설정, IAM 임시 자격증명 조회 등(AWS CLI, SDK 등이 내부적으로 활용)
- EC2 실행 시 메타데이터 액세스 가능 여부 설정 가능
  - 기본 활성화
  - 버전 선택 가능
- 가격 : 무료
- 참고로 Tag 조회의 경우 별도로 활성화 해야 Metadata로 Tag 조회 가능
  - 활성화 하지 않을 경우 목록에서 보이지 않음

### Instance Metadata Service V1
- 별도의 보안 인증이 필요 없는 Request/Response 기반
  - Link Local IP(169.254.169.254)를 사용하기 때문에 해당 EC2 인스턴스에서만 요청 가능
- CloudWatch Metric을 활용해서 조회 횟수 기록 가능
- 예시
  - 인스턴스 이름 가져오기 : curl http://169.254.169.254/latest/meta-data/tags/instance/Name

### Instance Metadata Service V2
- 보안 토큰을 발급받아 요청할 때 마다 토큰을 사용해 인증하는 세션 방식
  - 토큰의 유효 기간은 1초에서 최대 6시간
- IMDSv1보다 더 높은 보안 수준 제공
  - IAM 정책 등을 활용하여 EC2 인스턴스가 IMDS v2를 사용하도록 강제 가능
- 예시
  - TOKEN = `curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds:21600"`
  - curl -H "X-aws-ec2-metadata-token: $TOKEN" -v http://169.254.169.254/latest/meta-data/tags/instance/Name