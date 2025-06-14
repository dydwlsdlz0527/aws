## IAM
> 💡AWS Identity and Access Management(IAM)를 사용하면 AWS 서비스와 리소스에 대한 액세스를 안전하게 관리할 수 있다. 또한 AWS 사용자 및 그룹을 만들고 관리하며 AWS 리소스에 대한 액세스를 허용 및 거부할 수 있다.

- AWS 리소스/사용자/서비스에 대한 안전한 접근 제어를 지원하는 서비스
- 리소스에 대한 인증 및 권한 부여 기능 보유
- 그 외 다양한 기능
  - 암호나 엑세스키를 공유하지 않고, AWS 계정 접근
  - 세분화 된 권한을 통해 API 별 권한 허용/거부
  - MFA(Multi-Factor Authentication)
  - SAML/OIDC 등을 통한 다양한 Identity Federation 기능
  - 사용자의 패스워드 정책 관리
- AWS 상에서 제로 트러스트를 구현하게 해주는 중요한 서비스

## IAM: User & Group
- 루트 계정: AWS 계정 생성 시 자동으로 만들어지며, 공유하거나 일상적으로 사용하지 않아야 합니다.
- 사용자 (User): 조직 내 사람을 의미하며, IAM을 통해 개별적으로 생성됩니다.
- 그룹 (Group)
  - 사용자들을 논리적으로 묶는 단위입니다.
  - 그룹은 사용자만 포함할 수 있고, 다른 그룹을 포함할 수 없습니다.
- 사용자와 그룹의 관계
  - 사용자는 그룹에 속하지 않아도 되고, 여러 그룹에 동시에 속할 수 있습니다.

## IAM: Permissions
- 사용자(User) 또는 그룹(Group) 에게 정책(Policy) 을 할당할 수 있습니다.
- 정책(Policy) 은 JSON 형식의 문서로, 사용자가 어떤 작업을 할 수 있는지 정의합니다.
- AWS에서는 최소 권한 원칙(Least Privilege Principle) 을 따릅니다. -> 사용자에게 **꼭 필요한 권한만 부여**해야 하며, 그 이상은 주지 않습니다.

## IAM: Policies Structure
![iam](images/iam.png)
- Version
  - 정책 언어의 버전을 지정합니다.
  - 보통 "2012-10-17"을 사용합니다.
- Id
  - 정책의 고유 식별자입니다.
  - 정책을 추적하거나 관리할 때 유용합니다.
- Statement
  - 정책의 핵심 부분으로, 하나 이상의 Statement 객체를 포함합니다.
  - 각 Statement는 특정 권한을 정의합니다.
    - Sid : Statement의 식별자로 정책 내에서 구분하기 쉽게 이름을 붙일 수 있습니다.
    - Effect : 정책의 효과를 지정합니다. **Allow** 또는 **Deny**
    - Principal : 이 정책이 적용되는 사용자, 역할, 계정을 지정합니다. 주로 신뢰 정책(trust policy)에서 사용됩니다.
    - Action : 허용하거나 거부할 **작업(액션)**을 지정합니다.
    - Resource : 작업이 적용되는 리소스를 지정합니다.
    - Condition : 정책이 적용되는 **조건**을 지정합니다.

## IAM: Password Policy
- 비밀번호 최소 길이 설정
  - 사용자가 설정할 수 있는 최소 비밀번호 길이를 지정합니다.
- 특정 문자 유형 요구
  - 다음과 같은 문자 유형을 포함하도록 요구할 수 있습니다.
    - 숫자, 소문자, 대문자, 특수문자
- IAM 사용자 비밀번호 변경 허용
  - 모든 IAM 사용자가 자신의 비밀번호를 직접 변경할 수 있도록 허용할 수 있습니다.
- 비밀번호 만료 설정
  - 일정 기간 후 비밀번호를 반드시 변경하도록 요구할 수 있습니다.
- 비밀번호 재사용 방지
  - 이전에 사용한 비밀번호를 다시 사용할 수 없도록 제한할 수 있습니다.

## IAM Guidelines & Best Practices