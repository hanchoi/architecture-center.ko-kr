---
title: "DevOps 검사 목록"
description: "DevOps와 관련된 지침을 제공하는 검사 목록입니다."
author: dragon119
ms.date: 01/10/2018
ms.custom: checklist
ms.openlocfilehash: 356fef2415347ae132915695a25fd9b50779bd8b
ms.sourcegitcommit: 3d9ee03e2dda23753661a80c7106d1789f5223bb
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/23/2018
---
# <a name="devops-checklist"></a>DevOps 검사 목록

DevOps는 개발, 품질 보증 및 IT 운영을 통합된 문화권으로 통합하고 소프트웨어를 제공하는 일련의 프로세스입니다. 이 검사 목록을 시작점으로 사용하여 DevOps 문화권 및 프로세스를 평가합니다.

## <a name="culture"></a>문화권

**조직 및 팀에서 비즈니스 연계를 확인합니다.** 조직 내에서 리소스, 용도, 목표 및 우선 순위의 충돌은 성공적인 작업에 위험 요소가 될 수 있습니다. 비즈니스, 개발 및 운영 팀이 모두 연계되어 있는지 확인합니다.

**전체 팀이 소프트웨어 수명 주기를 이해하는지 확인합니다.** 팀은 응용 프로그램의 전체 수명 주기 및 응용 프로그램이 현재 위치한 수명 주기 파트를 이해해야 합니다. 이렇게 하면 모든 팀 멤버가 지금 수행해야 하는 작업 및 나중에 계획하고 준비해야 하는 작업을 알 수 있습니다.

**주기 시간을 줄입니다.** 개발된 소프트웨어를 사용할 수 있도록 아이디어를 실현하는 데 걸리는 시간을 최소화하려고 합니다. 테스트 부담을 낮추기 위해 개별 릴리스의 크기 및 범위를 제한합니다. 가능하면 빌드, 테스트, 구성 및 배포 프로세스를 자동화합니다. 개발자 간 및 개발자와 운영자 간의 통신에 장애를 없앱니다. 

**프로세스를 검토하고 개선합니다.** 자동 및 수동 프로세스 및 프로시저는 최종본이 아닙니다. 지속적으로 향상하려는 목표를 가지고 현재 워크플로, 프로시저 및 설명서를 정기적으로 검토합니다.

**사전 계획을 수행합니다.** 오류에 대해 미리 계획합니다. 문제가 발생할 때 신속하게 식별하고 해결하기 위해 올바른 팀 멤버에게 에스컬레이션하고, 해결 방법을 확인하는 프로세스를 마련합니다.

**오류에서 알아봅니다.** 오류는 피할 수 없지만 오류가 반복되지 않도록 이를 통해 배워야 합니다. 작업에서 오류가 발생하는 경우 문제를 심사하고, 원인 및 솔루션을 문서화하고, 배운 교훈을 공유합니다. 가능하면 나중에 이러한 종류의 오류를 자동으로 감지하도록 빌드 프로세스를 업데이트합니다.

**속도에 최적화하고 데이터를 수집합니다.** 계획된 모든 개선 사항은 가설입니다. 가능한 가장 작은 증분으로 작동합니다. 새로운 아이디어를 실험으로 처리합니다. 효율성을 평가하기 위해 프로덕션 데이터를 수집할 수 있도록 실험을 계측합니다. 가정이 잘못된 경우 페일 패스트할 준비가 되어 있어야 합니다.

**학습할 시간을 허용합니다.** 실패 및 성공은 모두 학습에 좋은 기회를 제공합니다. 새 프로젝트로 이동하기 전에 중요한 교훈을 수집하고 팀에서 흡수할 수 있도록 충분한 시간을 허용합니다. 또한 팀에 기술을 연습하고, 실험하고, 새로운 도구와 기술에 대해 알아볼 수 있는 시간을 줍니다. 

**작업을 문서화합니다.** 제품 코드와 품질 수준이 같은 모든 도구, 프로세스 및 자동화된 작업을 문서화합니다. 복구 프로세스 및 기타 유지 관리 프로시저와 함께 지원하는 모든 시스템의 현재 디자인 및 아키텍처를 문서화합니다. 이론적으로 최적인 프로세스가 아닌 실제로 수행하는 단계에 집중합니다. 정기적으로 설명서를 검토하고 업데이트합니다. 코드의 경우 특히 공용 API에서 의미 있는 주석을 포함하고 도구를 사용하여 가능하면 코드 문서를 자동으로 생성하도록 합니다. 

**지식을 공유합니다.** 사용자가 설명서를 찾을 수 있어야 유용합니다. 설명서를 쉽게 검색할 수 있도록 구성합니다. 창의적으로 브라운 백(비공식 프레젠테이션) 비디오 또는 뉴스레터를 사용하여 지식을 공유합니다.

## <a name="development"></a>개발

**프로덕션과 유사한 환경을 개발자에게 제공합니다.** 개발 및 테스트 환경이 프로덕션 환경과 일치하지 않으면 문제를 테스트하고 진단하기가 어렵습니다. 따라서 가능하면 프로덕션 환경과 유사한 개발 및 테스트 환경을 유지합니다. 프로덕션에 사용되는 데이터가 (개인 정보 또는 규정 준수로 인해) 샘플 데이터이고 실제 프로덕션 데이터가 아닌 경우 테스트 데이터는 해당 데이터와 일치해야 합니다. 샘플 테스트 데이터를 생성하고 익명화하도록 계획합니다.

**모든 권한이 있는 팀 멤버가 인프라를 프로비전하고 응용 프로그램을 배포할 수 있는지 확인합니다.** 프로덕션과 유사한 리소스를 설정하고 응용 프로그램을 배포하는 작업에는 복잡한 수동 작업 또는 시스템의 기술 세부 정보가 포함되지 않아야 합니다. 적절한 사용 권한이 있는 모든 사용자는 운영 팀으로 이동하지 않고도 프로덕션과 유사한 리소스를 만들거나 배포할 수 있어야 합니다. 

> 이 권장 사항이 프로덕션 배포에 라이브 업데이트를 푸시할 수 있는 모든 사용자를 의미하지는 않습니다. 프로덕션과 유사한 환경을 만드는 개발 및 QA 팀에 대한 갈등이 감소하게 됩니다.

**정보를 위해 응용 프로그램을 계측합니다.** 응용 프로그램의 상태를 이해하려면 수행되는 방법 및 오류 또는 문제가 발생하는지를 알고 있어야 합니다. 항상 디자인 요구 사항인 계측을 포함하고 처음부터 응용 프로그램에 계측을 빌드합니다. 계측은 근본 원인 분석뿐만 아니라 원격 분석 및 메트릭을 위한 이벤트 로깅에 포함되어 응용 프로그램의 전체 상태 및 사용량을 모니터링해야 합니다.

**기술적인 문제를 추적합니다.** 대부분의 프로젝트에서 릴리스 예약은 한 단계 이상의 수준에 대한 코드 품질보다 우선될 수 있습니다. 이 경우에 항상 추적합니다. 모든 바로 가기 또는 기타 비 최적화 구현을 문서화하고 나중에 이러한 문제를 다시 확인할 시간을 예약합니다.

**프로덕션에 직접 업데이트를 푸시하는 것이 좋습니다.** 전체 릴리스 주기 시간을 줄이려면 프로덕션에 직접 테스트된 코드 커밋 제대로 푸시하는 것이 좋습니다. [기능 토글][feature-toggles]을 사용하여 사용할 수 있는 기능을 제어합니다. 그러면 기능을 활성화하거나 비활성화하는 토글을 사용하여 개발에서 신속하게 릴리스로 이동할 수 있습니다. 토글은 [카나리아 릴리스][canary-release]와 같은 테스트를 수행하는 경우에도 유용합니다. 여기서 특정 기능은 프로덕션 환경의 하위 집합에 배포됩니다.

## <a name="testing"></a>테스트

**테스트를 자동화합니다.** 수동으로 소프트웨어를 테스트하는 작업은 복잡하고 오류에 취약합니다. 일반 테스트 작업을 자동화하고 테스트를 빌드 프로세스로 통합합니다. 자동화된 테스트를 사용하면 일관된 테스트 검사 및 재현성이 가능합니다. 통합 UI 테스트는 자동화된 도구에서 수행해야 합니다. Azure는 테스트를 구성하고 실행할 수 있는 개발 및 테스트 리소스를 제공합니다. 자세한 내용은 [개발 및 테스트][dev-test]를 참조하세요.

**오류에 대한 테스트입니다.** 시스템이 서비스에 연결할 수 없는 경우 어떻게 응답하나요? 서비스를 다시 사용할 수 있으면 복구할 수 있나요? 오류 삽입으로 테스트 및 스테이징 환경에서 검토의 표준 파트를 테스트합니다. 테스트 프로세스 및 사례의 완성도가 높은 경우 프로덕션에서 이러한 테스트를 실행하는 것이 좋습니다. 

**프로덕션에서 테스트합니다.** 릴리스 프로세스는 프로덕션에 대한 배포로 끝나지 않습니다. 테스트를 준비하여 배포된 코드가 예상 대로 작동하는지 확인합니다. 자주 업데이트되는 배포의 경우 유지 관리의 일환으로 프로덕션 테스트를 예약합니다.

**성능 테스트를 자동화하여 성능 문제를 조기에 식별합니다.** 심각한 성능 문제의 영향은 코드의 버그만큼 심각할 수 있습니다. 자동화된 기능 테스트가 응용 프로그램 버그를 방지할 수 있는 반면 성능 문제를 감지하지 못할 수 있습니다. 대기 시간, 로드 시간 및 리소스 사용량과 같은 메트릭에 허용 가능한 성능 목표를 정의합니다. 릴리스 파이프라인에서 자동화된 성능 테스트를 포함하여 응용 프로그램이 해당 목표를 충족하도록 합니다.

**용량 테스트를 수행합니다.** 응용 프로그램은 테스트 조건 하에서 잘 작동한 다음 규모 또는 리소스 제한으로 인해 프로덕션에 문제가 발생할 수 있습니다. 항상 최대 예상 용량 및 사용량 제한을 정의합니다. 응용 프로그램이 해당 제한을 처리할 수 있는지 확인할 뿐만 아니라 해당 제한을 초과하는 경우 어떤 상황이 발생하는지도 테스트합니다. 용량 테스트를 정기적으로 수행해야 합니다.

초기 릴리스 이후 프로덕션 코드를 업데이트할 때마다 성능 및 용량 테스트를 실행해야 합니다. 기록 데이터를 사용하여 테스트를 세부 조정하고 수행해야 하는 테스트의 형식을 결정합니다.

**자동화된 보안 침투 테스트를 수행합니다.** 응용 프로그램이 보안 상태를 유지하는 것은 다른 기능을 테스트하는 것만큼 중요합니다. 자동화된 침투 테스트를 빌드 및 배포 프로세스의 표준 파트로 만듭니다. 배포된 응용 프로그램에서 열린 포트, 끝점 및 공격을 모니터링하여 기본 보안 테스트 및 취약성 검사를 예약합니다. 자동화된 테스트를 실행하더라도 정기적으로 심층 보안 검토가 필요합니다.

**자동화된 비즈니스 연속성 테스트를 수행합니다.** 백업 복구 및 장애 조치를 포함하여 대규모 비즈니스 연속성에 대한 테스트를 개발합니다. 이 테스트를 정기적으로 수행하도록 자동화된 프로세스를 설정합니다.

## <a name="release"></a>해제

**배포를 자동화합니다.** 테스트, 스테이징 및 프로덕션 환경에 대한 응용 프로그램 배포를 자동화합니다. 자동화를 사용하면 보다 빠르고 안정적인 배포 및 지원되는 모든 환경에 대해 일관된 배포가 가능합니다. 수동 배포로 인해 발생하는 사용자 실수의 위험이 제거됩니다. 또한 잠재적인 가동 중지의 영향을 최소화하기 위해 편리한 시간에 릴리스를 쉽게 예약할 수 있습니다.

**연속 통합을 사용합니다.** CI(연속 통합)은 정기적으로 중앙 코드 베이스에 모든 개발자 코드를 병합한 다음 표준 빌드 및 테스트 프로세스를 자동으로 수행하는 방법입니다. CI를 사용하면 전체 팀이 동시에 충돌 없이 코드 베이스에서 작업할 수 있게 됩니다. 그러면 코드 결함을 가능한 빨리 발견할 수 있습니다. CI 프로세스는 코드를 커밋하거나 체크 인할 때마다 실행하는 것이 좋습니다. 적어도 매일 한 번씩 실행해야 합니다.

> [트렁크 기반 배포 모델][trunk-based]을 적용하는 것이 좋습니다. 이 모델에서 개발자는 단일 분기(트렁크)에 커밋합니다. 커밋이 빌드를 중지하지 않아야 합니다. 이 모델은 모든 기능 작업을 트렁크에서 수행하기 때문에 CI를 용이하게 하며 커밋이 발생할 때 병합 충돌을 해결합니다.

**지속적인 업데이트를 사용하는 것이 좋습니다.** CD(지속적인 업데이트)는 자동으로 코드를 빌드하고, 테스트하고, 프로덕션과 유사한 환경에 배포하여 항상 코드를 배포할 준비가 되어 있는 사례입니다. 지속적인 업데이트를 추가하여 전체 CI/CD 파이프라인을 만들면 코드 결함을 가능한 빠르게 감지할 수 있고 적절히 테스트된 업데이트를 빠른 시간 내에 릴리스할 수 있습니다.

> 지속적인 *배포*는 CI/CD 파이프라인을 통해 전달된 업데이트를 자동으로 사용하고 프로덕션에 배포하는 추가 프로세스입니다. 지속적인 배포에는 강력한 자동 테스트 및 고급 프로세스 계획이 필요하며 모든 팀에 적합하지 않을 수 있습니다.

**작게 증분으로 변경합니다.** 코드가 많이 변경되면 잠재적으로 버그가 발생할 수 있습니다. 가능하면 조금씩 변경합니다. 그러면 각 변경 내용의 잠재적인 영향을 제한하여 쉽게 문제를 이해하고 디버그할 수 있습니다.

**변경 내용에 대한 노출을 제어합니다.** 사용자에게 업데이트를 표시할 시기를 제어해야 합니다. 기능 토글을 사용하여 최종 사용자가 기능을 사용할 시기를 제어하는 것이 좋습니다.

**배포 위험을 줄이는 릴리스 관리 전략을 구현합니다.** 프로덕션에 응용 프로그램 업데이트를 배포하면 항상 일정한 위험이 따릅니다. 이 위험을 최소화하려면 [카나리아 릴리스][canary-release] 또는 [청록색 배포][blue-green]와 같은 전략을 사용하여 하위 집합의 사용자에게 업데이트를 배포합니다. 업데이트가 예상 대로 작동하는지 확인한 다음 업데이트를 나머지 시스템에 롤아웃합니다.

**모든 변경 내용을 문서화합니다.** 사소한 업데이트 및 구성 변경 내용은 혼란과 버전 관리 충돌을 일으킬 수 있습니다. 항상 작은 것이라도 변경 내용을 명확하게 기록해둡니다. 적용된 패치, 정책 변경 내용 및 구성 변경 내용을 비롯한 변경된 모든 내용을 기록합니다. (이러한 로그에 중요한 데이터를 포함하지 마십시오. 예를 들어 기밀이 업데이트된 사실 및 변경 내용을 작성한 사용자를 기록할 수 있지만 업데이트된 기밀을 기록하지 마십시오.) 변경 내용의 기록은 전체 팀에게 표시되어야 합니다. 

**배포를 자동화합니다.** 모든 배포를 자동화하고 시스템에서 롤아웃 중에 발생하는 문제를 감지하도록 준비합니다. 업데이트가 모든 프로덕션 인스턴스에서 바꾸기 전까지 완화 프로세스가 프로덕션에서 기존 코드 및 데이터를 보존하도록 합니다. 자동화된 방법은 수정을 롤포워드하거나 변경 내용을 롤백합니다.

**인프라를 변경할 수 없도록 하는 것이 좋습니다.** 변경할 수 없는 인프라는 인프라를 프로덕션에 배포한 후에 수정하지 않아야 한다는 원칙입니다. 그렇지 않으면 임시 변경 작업이 수행되어 무엇이 변경되었는지 정확하게 알기 어려운 상태에 빠질 수 있습니다. 변경할 수 없는 인프라는 새 배포의 일부로 전체 서버를 대체하여 작동합니다. 그러면 코드 및 호스팅 환경을 테스트하고 블록으로 배포할 수 있습니다. 배포되면 인프라 구성 요소는 다음 빌드 및 배포 주기까지 수정되지 않습니다. 

## <a name="monitoring"></a>모니터링

**시스템을 관찰할 수 있게 합니다.** 운영 팀은 항상 시스템 또는 서비스의 상태에 대해 명확하게 확인해야 합니다. 상태를 모니터링하도록 외부 상태 끝점을 설정하고 운영 메트릭을 계측하도록 응용 프로그램을 코딩했는지 확인합니다. 시스템에서 이벤트를 상호 연결할 수 있도록 공통적이며 일관된 스키마를 사용합니다. [Azure 진단][azure-diagnostics] 및 [Application Insights][app-insights]는 Azure 리소스의 상태를 추적하는 표준 방법입니다. Microsoft [Operation Management Suite][oms]에서는 클라우드 또는 하이브리드 솔루션에 중앙 집중식 모니터링 및 관리 기능을 제공합니다.

**로그 및 메트릭을 집계하고 상관 관계를 지정합니다**. 제대로 계측된 원격 분석 시스템은 다량의 원시 성능 데이터 및 이벤트 로그를 제공합니다. 운영 담당자가 항상 최신 시스템 상태를 알 수 있도록 원격 분석 및 로그 데이터를 짧은 시간 동안 처리하고 상관 관계를 지정해야 합니다. 가능하면 이벤트가 서로 관련되어 있도록 모든 문제의 일관된 보기를 제공하는 방식으로 데이터를 구성하고 표시합니다.

> 데이터를 처리하는 방법 및 저장해야 하는 기간에 대한 요구 사항은 회사 보존 정책은 참조하세요. 

**자동화된 경고 및 알림을 구현합니다.** 잠재적인 문제나 현재 문제를 나타내는 패턴 또는 조건을 감지하고 문제를 해결할 수 있는 팀 멤버에게 경고를 보내도록 [Azure Monitor][azure-monitor]와 같은 모니터링 도구를 설정합니다. 거짓 긍정을 방지하도록 경고를 조정합니다.

**만료할 자산 및 리소스를 모니터링합니다.** 인증서와 같은 일부 리소스 및 자산은 지정된 시간 후에 만료됩니다. 만료될 자산, 만료 시기 및 해당 기능을 사용하는 서비스 또는 기능을 추적해야 합니다. 자동화된 프로세스를 사용하여 이러한 자산을 모니터링합니다. 자산이 만료되기 전에 작업 팀에게 알리고 만료가 응용 프로그램을 중단하는 경우 에스컬레이션합니다.

## <a name="management"></a>관리

**운영 작업을 자동화합니다.** 반복적인 작업 프로세스를 수동으로 처리하는 작업은 오류가 발생하기 쉽습니다. 이러한 작업을 자동화하면 항상 일관된 실행 및 품질을 보장할 수 있습니다. 자동화를 구현하는 코드는 소스 제어에서 버전 관리되어야 합니다. 다른 코드와 마찬가지로 자동화 도구를 테스트해야 합니다.

**프로비전에 대해 infrastructure-as-code 방법을 사용합니다.** 리소스를 프로비전하는 데 필요한 수동 구성의 양을 최소화합니다. 대신 스크립트 및 [Azure Resource Manager][resource-manager] 템플릿을 사용합니다. 유지한 다른 코드와 마찬가지로 소스 제어에 스크립트 및 템플릿을 유지합니다. 

**컨테이너를 사용하는 것이 좋습니다.** 컨테이너는 응용 프로그램을 배포하는 데 표준 패키지 기반 인터페이스를 제공합니다. 컨테이너를 사용하는 응용 프로그램은 배포 프로세스를 크게 간소화하는 응용 프로그램을 실행하는 데 필요한 모든 소프트웨어, 종속성 및 파일을 포함하는 자체 포함 패키지를 사용하여 배포됩니다. 

또한 컨테이너는 응용 프로그램과 기본 운영 체제 간에 추상화 계층을 만들 수도 있습니다. 그러면 환경 모두에 일관성을 제공합니다. 이 추상화는 컨테이너를 호스트에서 실행되는 다른 프로세스 또는 응용 프로그램에서 격리할 수도 있습니다. 

**복원력 및 자동 복구를 구현합니다.** 복원력은 오류로부터 복구하는 응용 프로그램의 기능입니다. 복원력에 대한 전략에는 일시적 오류를 재시도하고 보조 인스턴스 또는 다른 지역으로 장애 조치하는 것이 포함됩니다. 자세한 내용은 [Azure용 복원 응용 프로그램 디자인][resiliency]을 참조하세요. 해당 문제를 즉시 보고하고 가동 중단 또는 기타 시스템 오류를 관리하도록 응용 프로그램을 계측할 수 있습니다.

**작업 설명서가 있습니다.** 작업 설명서 또는 *runbook*은 시스템을 유지 관리하는 운영 스태프에게 필요한 프로시저 및 관리 정보를 문서화합니다. 또한 서비스에 대한 오류 또는 기타 중단 시 수행할 수 있는 모든 작업 시나리오 및 완화 계획을 문서화합니다. 개발 프로세스 중에 이 문서를 만들고 나중에 최신 상태로 유지합니다. 이 문서는 유동적으로 정기적으로 검토되고 테스트되고 개선되어야 합니다. 

공유 설명서가 중요합니다. 팀 멤버가 지식을 만들고 공유하도록 도와줍니다. 전체 팀이 문서에 액세스해야 합니다. 팀의 누구나 문서를 쉽게 업데이트할 수 있어야 합니다.

**대기 중 프로시저를 문서화합니다.** 대기 중 의무, 일정 및 프로시저가 문서화되고 모든 팀 멤버에게 공유되도록 합니다. 이 정보를 항상 최신으로 유지합니다.

**타사 종속성에 대한 에스컬레이션 프로시저를 문서화합니다.** 응용 프로그램에서 사용자가 직접 제어할 수 없는 외부의 타사 서비스를 사용하는 경우 가동 중단 시 대처할 계획이 있어야 합니다. 계획된 완화 프로세스에 대한 설명서를 만듭니다. 지원 연락처 및 에스컬레이션 경로를 포함합니다.

**구성 관리를 사용합니다.** 운영에 명시적으로 기록된 구성 변경 내용을 계획해야 합니다. 그러려면 구성 관리 데이터베이스 형식 또는 configuration-as-code 방법을 사용할 수 있습니다. 구성은 예상되는 작업이 실제로 준비되도록 정기적으로 감사해야 합니다.

**Azure 지원 계획을 가져오고 프로세스를 이해합니다.** Azure에서는 다양한 [지원 계획][azure-support-plans]을 제공합니다. 요구 사항에 대한 올바른 계획을 결정하고 전체 팀이 사용하는 방법을 알고 있어야 합니다. 팀 멤버는 계획의 세부 정보, 지원 프로세스의 작동 방식 및 Azure에서 지원 티켓을 여는 방법을 이해해야 합니다. 확장성이 높은 이벤트를 사용하는 경우 Azure 지원에서는 서비스 제한을 증가시켜 지원할 수 있습니다. 자세한 내용은 [Azure 지원 FAQ](https://azure.microsoft.com/support/faq/)를 참조하세요.

**리소스에 대한 액세스 권한을 부여할 때 최소 권한 원칙을 따릅니다.** 리소스에 대한 액세스를 주의 깊게 관리합니다. 사용자에게 리소스에 대한 액세스 권한이 명시적으로 지정되지 않으면 기본적으로 액세스는 거부되어야 합니다. 해당 작업을 완료하기 위해 필요한 액세스 권한만을 사용자에게 부여합니다. 사용자 사용 권한을 추적하고 기본 보안 감사를 수행합니다.

**역할 기반 액세스 제어를 사용합니다.** 사용자 계정 및 리소스에 대한 액세스 권한을 할당하는 작업은 수동 프로세스가 아니어야 합니다. RBAC([역할 기반 액세스 제어][rbac])를 사용하여 [Azure Active Directory][azure-ad] ID 및 그룹에 따라 액세스 권한을 부여합니다. 

**버그 추적 시스템을 사용하여 문제를 추적합니다.** 문제를 추적할 좋은 방법이 없으면 항목을 누락하거나, 작업이 중복되거나, 추가적인 문제가 발생할 수 있습니다. 버그의 상태를 추적하는 데 개인 간 비공식 통신을 사용하지 마십시오. 버그 추적 도구를 사용하여 문제에 대한 세부 정보를 기록하고, 리소스를 할당하여 문제를 해결하고, 프로세스 및 상태의 감사 내역을 제공합니다. 

**변경 관리 시스템에서 모든 리소스를 관리합니다.** DevOps 프로세스의 모든 측면은 관리 및 버전 관리 시스템에 포함되어야 합니다. 따라서 해당 변경 내용을 쉽게 추적하고 감사할 수 있습니다. 여기에는 코드, 인프라, 구성, 설명서 및 스크립트가 포함됩니다. 이러한 모든 형식의 리소스를 테스트/빌드/검토 프로세스 전체의 코드로 다룹니다. 

**검사 목록을 사용합니다.** 작업 검사 목록을 만들어서 프로세스가 계속되도록 합니다. 큰 설명서에서 무언가 쉽게 놓질 수 있습니다. 검사 목록을 따르면 간과할 수 있는 세부 정보에 주의를 기울일 수 있습니다. 검사 목록을 유지 관리하고 작업을 자동화하고 프로세스를 간소화하는 방법을 지속적으로 찾습니다.

DevOps에 대한 자세한 내용은 Visual Studio 사이트에서 [DevOps란?][what-is-devops]을 참조하세요.

<!-- links -->

[app-insights]: /azure/application-insights/
[azure-ad]: https://azure.microsoft.com/services/active-directory/
[azure-diagnostics]: /azure/monitoring-and-diagnostics/azure-diagnostics
[azure-monitor]: /azure/monitoring-and-diagnostics/monitoring-overview
[azure-support-plans]: https://azure.microsoft.com/support/plans/
[blue-green]: https://martinfowler.com/bliki/BlueGreenDeployment.html
[canary-release]:https://martinfowler.com/bliki/CanaryRelease.html
[dev-test]: https://azure.microsoft.com/solutions/dev-test/
[feature-toggles]: https://www.martinfowler.com/articles/feature-toggles.html
[oms]: https://www.microsoft.com/cloud-platform/operations-management-suite
[rbac]: /azure/active-directory/role-based-access-control-what-is
[resiliency]: ../resiliency/index.md
[resource-manager]: /azure/azure-resource-manager/
[trunk-based]: https://trunkbaseddevelopment.com/
[what-is-devops]: https://www.visualstudio.com/learn/what-is-devops/
