# 🛠️ 4RSENAL Terraform 기반 IaC 및 GitOps 인프라 구축 프로젝트

> **인프라의 선언적 관리(IaC)를 위해 인프라 전체 아키텍처를 테라폼 코드로 자산화하고, GitHub 및 Terraform Cloud와 AWS를 연동하여 코드 푸시부터 실제 프로비저닝까지 완벽한 GitOps 가상 파이프라인을 구축 및 검증한 프로젝트입니다.**

---

## 👥 Team Information (4rsenal)
- **조장:** 신재섭
- **조원:** 전병욱, 이은석, 정태환

<br>

## 🛠️ Tech Stack & Environment
- **Infrastructure as Code:** `Terraform (HashiCorp)`, `IaC`
- **Cloud Infrastructure:** `AWS (VPC, Route 53, ALB, Auto Scaling Group, Launch Template)`
- **DevOps & CI/CD:** `GitHub Repository`, `Terraform Cloud`
- **IDE & Tools:** `VSCode`, `AWS CLI`

---

## 📌 주요 수행 내용 (Key Features)

### 1️⃣ 테라폼 환경 구축 및 형상 관리 체계 수립
* **프로비저닝 개발 환경 최적화:** VSCode 인프라 개발 환경에 AWS CLI 및 테라폼 모듈을 구성하여 로컬 워크스페이스 세팅 완료
* **IaC 작동 원리 검증:** 테라폼 초기화(`init`), 문법 검사(`validate`), 실행 계획 수립(`plan`), 실제 인프라 적용(`apply`) 및 자원 회수(`destroy`)로 이어지는 선언적 인프라 수명 주기(Lifecycle) 확립
* **상태 파일 관리:** 인프라 현재 형상을 기록하는 `terraform.tfstate` 관리 메커니즘을 파악하고 아키텍처 구조 분석

### 2️⃣ 테라폼 기반 가상 네트워크(VPC) 인프라 코드화
* **네트워크 레이어 설계:** `provider "aws"` 선언을 시작으로 리전 설정을 고정하고, 기본 가상 네트워크 아키텍처를 완전한 코드로 자산화
* **고가용성 서브넷 파티셔닝:** 고가용성 보장을 위해 다중 가용 영역(AZ 2A, 2C) 기반의 퍼블릭 서브넷 2개 및 프라이빗 서브넷 2개를 리소스로 명시하여 자동 분할 프로비저닝 구축
* **외부 통신 라우팅 연동:** 인터넷 게이트웨이 및 전용 라우팅 테이블 설정을 테라폼 변수(`variable`)와 출력문(`output`) 구조로 템플릿화하여 모듈 확장성 확보

### 3️⃣ Auto Scaling Group 및 Launch Template 자동화
* **인스턴스 동적 확장 아키텍처:** 부하 유동성에 대응하기 위해 오토 스케일링 그룹 리소스(`aws_autoscaling_group`) 선언
  * **리소스명:** `terraform-pri-asg`
  * **스케일링 가용 범위:** Minimum `2`, Maximum `4`, Desired `3` 구조로 임계치 설정
* **기동 템플릿 정의:** 가상 머신의 스펙 및 초기 배포 환경을 표준화하기 위해 시작 템플릿(`aws_launch_template`) 연동 및 `$Latest` 버전 라이프사이클 정책 적용
* **안정적인 교체 주명 주기:** 인프라 업데이트 시 가용성 공백을 방지하기 위해 `create_before_destroy = true` 선언으로 선-생성 후-삭제 라이프사이클 정책 바인딩

### 4️⃣ ALB 로드 밸런싱 및 Route 53 도메인 통합 프로비저닝
* **트래픽 분산 레이어 자동화:** `aws_lb` 및 `aws_lb_target_group` 리소스를 코드로 결합하여 오토 스케일링 그룹(`target_group_arns`)과의 완벽한 헬스 체크 유예 기간(120초) 및 ELB 유형 연동 체계 구축
* **Route 53 DNS 코드화:** 테라폼을 통해 네임서버(NS) 레코드를 Route 53 호스팅 영역에 자동으로 등록하고, 아웃풋 결과(`output`) 조회를 통해 도메인 매핑 상태 최종 검증

### 5️⃣ GitHub x Terraform Cloud x AWS 연동 기반 GitOps 파이프라인
* **원격 상태 관리 체계 이관:** 로컬 수동 프로비저닝의 위험성을 배제하기 위해 원격 백엔드 저장소인 `Terraform Cloud` 초기 구성
* **VCS 기반 파이프라인 결합:** 전용 GitHub Repository를 생성하여 소스 코드를 푸시하면 Terraform Cloud가 Webhook을 트리거하여 `Plan` 및 `Apply`를 자동 수행하는 CI/CD 워크플로우 활성화
* **클라우드 프로바이더 보안 자격 증명:** Terraform Cloud와 AWS IAM 자격 증명을 연동하여, 코드가 안전하고 투명하게 가상 인프라 자원을 생성 및 유지 관리할 수 있는 무인(Unattended) 배포 환경 최종 검증 성공

---

## 📈 배운 점 및 성과 (Retrospective)
- **코드로 전개하는 인프라(IaC) 숙련:** 콘솔 수동 클릭 방식에서 벗어나 100% 코드를 기반으로 대규모 인프라 자원을 일관성 있게 생성, 수정, 삭제하는 모던 DevOps 엔지니어링 역량을 확보했습니다.
- **현대적 GitOps 파이프라인 체득:** GitHub-Terraform Cloud-AWS로 이어지는 3자 연동 아키텍처를 직접 설계하며 형상 관리와 자동 배포 워크플로우를 완벽히 내재화했습니다.
- **인프라 자산의 모듈화 및 재사용성 확보:** 변수(`variable`) 및 의존성 관계(`depends_on`) 제어 설정을 통해 복잡한 아키텍처 간의 프로비저닝 순서를 안정적으로 통제하고 재사용 가능한 인프라 템플릿 설계 능력을 배양했습니다.
