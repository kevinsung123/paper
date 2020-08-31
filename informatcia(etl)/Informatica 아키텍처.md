
![Informatica PowerCenter 아키텍처-Informatica 자습서](https://www.tutorialkart.com/wp-content/uploads/2018/01/Informatica-architecture.png)

## Informatica 아키텍처
- Informatica ETL 도구 서비스 및 구성요소
	1. 저장소 서비스 - 메타데이터 유지 관리 및 다른 서비스에대한 엑세스 제공
	2. 통합 서비스 - 소스에서 대상으로 데이터 이동 엔진
	3. 보고 서비스 - 보고서 생성 활성화
	4. 노드 - 위 서비스가 실행되는 컴퓨팅 플랫폼
	5. 디자이너 - 소스와 대상간의 매핑생성
	6. 워크플로우 매니저 - 워크플로우 및 기타 작업 및 해당 실행 만드는데 사용
	7. 워크플로우 모니터 - 워크플로우 실행을 모니터링
	8. 저장소매니저 - 저장소의 개체를 관리
- **Informatica Administrator :** 도메인 및 PowerCenter를 관리하는 데 사용되는 웹 응용 프로그램

- **Informatica 도메인 :** 도메인은 Powercenter의 서비스 관리 및 관리를위한 기본 단위입니다. 도메인의 구성 요소는 하나 이상의 노드, 서비스 관리자, 응용 프로그램 서비스입니다.
	- 전체 아키텍처는 SOA(Service Orientied Architecture)
	- Informatica의 도메인은 도구의 기본관리 단위
	- 노드 및 서비스의 모음, 또한 노드와 서비스는 관리 요구 사항에 따라 폴더와 하위폴더로 분류가능

- **노드 :** 노드는 도메인에서 기계의 논리적 표현입니다. 도메인에는 여러 개의 노드가있을 수 있습니다. 마스터 게이트웨이 노드는 도메인을 호스팅하는 노드입니다. 통합 서비스 또는 저장소 서비스와 같은 애플리케이션 서비스를 실행하도록 노드를 구성 할 수 있습니다. 다른 노드의 모든 요청은 마스터 게이트웨이 노드를 통과합니다.

**서비스 관리자 :** 서비스 관리자는 도메인 및 응용 프로그램 서비스를 지원합니다. 서비스 관리자는 도메인의 각 노드에서 실행됩니다. 서비스 관리자는 컴퓨터에서 응용 프로그램 서비스를 시작하고 실행합니다.

**응용 프로그램 서비스 :** Informatica 서버 기반 기능을 나타내는 서비스 그룹입니다. 애플리케이션 서비스에는 Powercenter 리포지토리 서비스, 통합 서비스, 데이터 통합 ​​서비스, 메타 데이터 관리 서비스 등이 있습니다.

**Powercenter 리포지토리 :** 메타 데이터는 관계형 데이터베이스에 저장됩니다. 테이블에는 데이터 추출, 변환 및로드에 대한 지시 사항이 포함되어 있습니다.

**[Powercenter 리포지토리 서비스](https://www.tutorialkart.com/informatica/creating-informatica-powercenter-repository-service/) :** 클라이언트의 요청을 수락하여 리포지토리에서 메타 데이터를 만들고 수정합니다. 또한 통합 서비스의 메타 데이터에 대한 워크 플로 실행 요청을 수락합니다.

**Powercenter 통합 서비스 :** [Informatica 통합 서비스](https://www.tutorialkart.com/informatica/creating-powercenter-integration-service-in-informatica/) 는 소스에서 데이터를 추출하고 워크 플로에서 코딩 된 지침에 따라 데이터를 변환하고 대상에 데이터를로드합니다.

**Metadata Manager 서비스 :** 메타 데이터 관리자 웹 응용 프로그램을 실행합니다. 다양한 메타 데이터 저장소에서 메타 데이터를 분석 할 수 있습니다.
- 도메인 2가지 유형 서비스
1. Service Manager
	- 서비스 관리자는 인증, 권한부여 및 로깅과 같은 도메인 작업을 관리 
	- node에서 응용프로그램 서비스를 실행하고 사용자 및 그룹을 관리
2. 응용프로그램 서비스
	- 통합서비스, 저장소 서비스 및 보고 서비스 같은 서버 특정 서비스 의미
	- node, 구성에 따라 다른 서비스가 실행
