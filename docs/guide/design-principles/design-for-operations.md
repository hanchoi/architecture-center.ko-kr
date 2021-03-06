---
title: "운영을 위한 디자인"
description: "운영 팀에 필요한 도구가 포함되도록 응용 프로그램을 디자인합니다."
author: MikeWasson
layout: LandingPage
ms.openlocfilehash: 76338cc27daf82ccb99df4e4c51c7a5ac6f26065
ms.sourcegitcommit: b0482d49aab0526be386837702e7724c61232c60
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/14/2017
---
# <a name="design-for-operations"></a>운영을 위한 디자인

## <a name="design-an-application-so-that-the-operations-team-has-the-tools-they-need"></a>운영 팀에 필요한 도구가 포함되도록 응용 프로그램을 디자인합니다.

클라우드의 운영 팀 역할이 크게 변경되었습니다. 더 이상 응용 프로그램을 호스트하는 인프라와 하드웨어 관리 업무를 담당하지 않습니다.  즉, 운영은 여전히 성공적인 클라우드 응용 프로그램 실행에 있어서 중요한 부분입니다. 운영 팀의 중요한 기능 중 일부는 다음과 같습니다.

- 배포
- 모니터링
- 에스컬레이션
- 사고 대응
- 보안 감사

강력한 로깅 및 추적은 클라우드 응용 프로그램에서 특히 중요합니다. 운영 팀이 디자인 및 계획에 참여하도록 하여, 성공적인 업무에 필요한 데이터 및 정보를 응용 프로그램을 통해 얻을 수 있도록 합니다.  <!-- to do: Link to DevOps checklist -->

## <a name="recommendations"></a>권장 사항

**모두 식별 가능으로 구성** 솔루션이 배포되고 실행되면, 기본적으로 로그 및 추적으로 시스템을 파악하게 됩니다. *추적*은 시스템으로의 경로를 기록하며, 병목 상태, 성능 문제 및 오류 발생 지점을 정확하게 파악하는 데 유용합니다. *로깅*은 응용 프로그램 상태 변경, 오류 및 예외와 같은 개별 이벤트를 캡처합니다. 프로덕션 환경에 로깅합니다. 그렇지 않으면 정말 필요할 때 확인할 수 없게 됩니다.

**모니터링을 위한 계측**. 모니터링을 통해 가용성, 성능 및 시스템 상태 측면에서 응용 프로그램의 작동 성능을 파악할 수 있습니다. 예를 들어, 모니터링은 SLA을 충족하는지 여부를 알려줍니다. 모니터링은 시스템의 정상적인 작동 중에 발생합니다. 모니터링은 가능한 한 실시간에 가까워야 합니다. 그래야 운영자가 문제에 신속하게 대응할 수 있습니다. 이상적으로 모니터링은 문제가 심각한 오류로 이어지기 전에 미리 방지하는 데 도움이 될 수 있습니다. 자세한 내용은 [모니터링 및 진단][monitoring]을 참조하세요.

**근본 원인 분석을 위한 계측**. 근본 원인 분석은 실패의 근본 원인을 찾는 프로세스입니다. 이러한 분석은 오류가 이미 발생한 후에 진행됩니다. 

**분산 추적 사용**. 동시성, 비동기성 및 클라우드 규모에 맞게 디자인된 분산 추적 시스템을 사용합니다. 추적에는 서비스 경계 너머로 흐르는 상관 관계 ID가 포함됩니다. 단일 작업이 여러 응용 프로그램 서비스 호출을 포함할 수 있습니다. 작업이 실패하면 상관 관계 ID가 실패의 원인을 파악하는 데 도움이 됩니다. 

**로그 및 메트릭 표준화**. 운영 팀은 솔루션의 다양한 서비스에서 로그를 집계해야 합니다. 모든 서비스가 자체 로깅 형식을 사용하는 경우 유용한 정보를 가져오기가 어렵거나 불가능할 수 있습니다. 상관 관계 ID, 이벤트 이름, 보낸 사람의 IP 주소 등의 필드를 포함하는 공통 스키마를 정의합니다. 개별 서비스는 기본 스키마를 상속하고, 추가 필드를 포함하는 사용자 지정 스키마를 파생할 수 있습니다.

**관리 작업 자동화**: 프로비저닝, 배포 및 모니터링 포함. 작업을 자동화하여 반복 가능하게 하고 인간 오류가 발생할 가능성을 줄입니다. 

**구성을 코드로 처리**. 버전 제어 시스템에서 구성 파일을 확인하여 변경 내용을 추적하고 버전을 관리하며, 필요한 경우 롤백할 수 있도록 합니다. 


<!-- links -->

[monitoring]: ../../best-practices/monitoring.md


