---
title: "데이터 웨어하우징 및 데이터 마트"
description: 
author: zoinerTejada
ms:date: 02/12/2018
ms.openlocfilehash: eec883c68cf94637c3061814d0841c73b58d7e52
ms.sourcegitcommit: 90cf2de795e50571d597cfcb9b302e48933e7f18
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/14/2018
---
# <a name="data-warehousing-and-data-marts"></a>데이터 웨어하우징 및 데이터 마트

데이터 웨어하우스는 많은 또는 모든 주제 영역에 걸쳐, 하나 이상의 개별 원본에서 가져온 통합된 데이터의 조직적인 중앙 관계형 리포지토리입니다. 데이터 웨어하우스는 현재 및 기록 데이터를 저장하고, 다양한 방식으로 데이터의 보고 및 분석에 사용됩니다.

![Azure의 데이터 웨어하우징](./images/data-warehousing.png)

데이터 웨어하우스로 데이터를 이동하기 위해 중요한 비즈니스 정보를 포함하는 다양한 원본에서 데이터가 주기적으로 추출됩니다. 데이터는 이동될 때, 서식이 지정되고, 정리되고, 유효성이 검사되고, 요약되고, 다시 구성됩니다. 또는 데이터를 가장 낮은 세부 수준으로 저장하고, 보고를 위해 웨어하우스에 집계된 보기를 제공할 수 있습니다. 두 경우 모두에서 데이터 웨어하우스는 BI(비즈니스 인텔리전스) 도구를 사용하여 보고, 분석 및 중요한 비즈니스 의사 결정을 수행하는 데 사용되는 데이터의 영구 저장 공간이 됩니다.

## <a name="data-marts-and-operational-data-stores"></a>데이터 마트 및 운영 데이터 저장소

최대 규모의 데이터를 관리하는 것은 복잡하며, 전체 엔터프라이즈의 모든 데이터를 나타내는 단일 데이터 웨어하우스를 유지하는 것은 별로 일반적이지 않습니다. 대신, 조직에서는 분석을 위해 원하는 데이터를 노출하는 *데이터 마트*라는 좀 더 작고 좀 더 특정 영역을 중심으로 하는 데이터 웨어하우스를 만듭니다. 오케스트레이션 프로세스는 운영 데이터 저장소에 유지 관리되는 데이터로 데이터 마트를 채웁니다. 운영 데이터 저장소는 원본 트랜잭션 시스템 및 데이터 마트 간의 중간 매개 역할을 합니다. 운영 데이터 저장소에서 관리되는 데이터는 원본 트랜잭션 시스템에 있는 데이터의 정리된 버전으로, 일반적으로 데이터 웨어하우스 또는 데이터 마트에서 유지 관리되는 기록 데이터의 하위 집합입니다. 

## <a name="when-to-use-this-solution"></a>이 솔루션을 사용해야 하는 경우

운영 체제의 대량의 데이터를 이해하기 쉽고 정확한 최신 형식으로 변환해야 할 경우 데이터 웨어하우스를 선택합니다. 데이터 웨어하우스는 운영/OLTP 데이터베이스에서 사용될 수 있는 동일한 간결한 데이터 구조를 따를 필요가 없습니다. 비즈니스 사용자 및 분석가에게 적절해 보이는 열 이름을 사용하고, 스키므를 다시 구성하여 데이터 관계를 간소화하고, 여러 테이블을 하나로 통합할 수 있습니다. 이러한 단계는 DBA(데이터베이스 관리자) 또는 데이터 개발자의 도움 없이 임시 보고서를 만들거나, 보고서를 만들고 BI 시스템에서 데이터를 분석해야 하는 사용자를 안내하는 데 도움이 됩니다.

성능상의 이유로 기록 데이터를 원본 트랜잭션 시스템과는 따로 보관하려는 경우, 데이터 웨어하우스를 사용하는 것이 좋습니다. 데이터 웨어하우스는 중앙 위치를 제공하여 일반 형식, 공통 키, 공용 데이터 모델 및 일반적인 액세스 방법으로 여러 위치의 기록 데이터에 쉽게 액세스할 수 있도록 합니다.

데이터 웨어하우스는 읽기 액세스에 최적화되어 있으므로, 원본 트랜잭션 시스템에 대해 보고서를 실행하는 방식에 비해 보고서를 더 빠르게 생성할 수 있도록 합니다. 또한 데이터 웨어하우스는 다음과 같은 이점을 제공합니다.

* 여러 원본의 모든 기록 데이터가 저장되며, 신뢰할 수 있는 단일 원본으로서 데이터 웨어하우스에서 액세스될 수 있습니다.
* 데이터 웨어하우스로 데이터를 가져올 때 정리하고, 일관된 코드 및 설명을 제공할 뿐만 아니라 보다 정확한 데이터를 제공하여 데이터 품질을 개선할 수 있습니다.
* 보고 도구는 쿼리 처리 주기 동안 트랜잭션 원본 시스템과 경쟁하지 않습니다. 데이터 웨어하우스는 대부분의 읽기 요청을 충족하면서, 트랜잭션 시스템이 쓰기 작업을 처리하는 데 주로 초점을 맞출 수 있도록 합니다.
* 데이터 웨어하우스는 서로 다른 소프트웨어의 데이터를 통합하는 데 도움이 될 수 있습니다.
* 데이터 마이닝 도구는 웨어하우스에 저장된 데이터에 대해 자동 방법을 사용하여 숨겨진 패턴을 찾는 데 도움이 될 수 있습니다.
* 데이터 웨어하우스를 사용하면 허가되지 않은 사용자의 액세스는 제한하면서 허가된 사용자에게 쉽게 보안 액세스를 제공할 수 있습니다. 비즈니스 사용자에게 원본 데이터에 대한 액세스 권한을 부여할 필요가 없으므로, 하나 이상의 프로덕션 트랜잭션 시스템에 대한 잠재적인 공격 벡터가 제거됩니다.
* 데이터 웨어하우스를 데이터 위에 [OLAP 큐브](online-analytical-processing.md)와 같은 비즈니스 인텔리전스 솔루션을 보다 쉽게 만들 수 있습니다.

## <a name="challenges"></a>과제

비즈니스의 요구에 맞게 데이터 웨어하우스를 적절히 구성하려는 경우 다음과 같은 해결 과제에 직면할 수 있습니다.

* 비즈니스 개념을 적절히 모델링하는 데 필요한 시간 확정. 데이터 웨어하우스는 정보를 중심으로 하므로, 개념 매핑이 나머지 프로젝트를 구동하게 됩니다. 따라서 이 단계는 중요합니다. 이 단계에서는 비즈니스 관련 용어 및 일반 형식(예: 통화 및 날짜)이 표준화되고, 비즈니스 사용자에게 보다 적절한 방식으로 데이터가 다시 구성되지만, 데이터 집계 및 관계의 정확도도 계속 보장됩니다.
* 데이터 오케스트레이션 계획 및 설정. 고려 사항으로는 원본 트랜잭션 시스템의 데이터를 데이터 웨어하우스로 복사하는 방법과, 운영 데이터 저장소의 기록 데이터를 웨어하우스로 이동하는 시기가 포함됩니다.
* 데이터를 웨어하우스로 가져올 때 데이터를 정리하여 품질 유지 또는 개선.

## <a name="data-warehousing-in-azure"></a>Azure의 데이터 웨어하우징

Azure에서는 고객의 트랜잭션이든, 다양한 부서에서 사용되는 다양한 비즈니스 응용 프로그램이든, 하나 이상의 데이터 원본이 있을 수 있습니다. 이 데이터는 일반적으로 하나 이상의 [OLTP](online-transaction-processing.md) 데이터베이스에 저장됩니다. 데이터는 네트워크 공유, Azure Storage Blob 또는 Data Lake 같은 기타 저장소 미디어에 유지될 수 있습니다. 데이터가 데이터 웨어하우스 자체 또는 Azure SQL Database 같은 관계형 데이터베이스에 저장할 수도 있습니다. 분석 데이터 저장소 계층의 목적은 데이터 웨어하우스 또는 데이터 마트에 대해 분석 및 보고 도구에서 실행한 쿼리를 충족하는 것입니다. Azure에서 이 분석 저장소 기능은 Azure SQL Data Warehouse를 사용하거나 Hive 또는 대화형 쿼리를 통해 Azure HDInsight를 사용하여 충족될 수 있습니다. 또한 데이터 저장소의 데이터를 데이터 웨어하우스에 주기적으로 이동하거나 복사하기 위해 일정 수준의 오케스트레이션이 필요합니다. 이 작업은 Azure Data Factory 또는 Azure HDInsight의 Oozie를 사용하여 수행할 수 있습니다.

관련 서비스

* [Azure SQL Database](/azure/sql-database/)
* [VM의 SQL Server](/sql/sql-server/sql-server-technical-documentation)
* [Azure Data Warehouse](/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is)
* [HDInsight의 Apache Hive](/azure/hdinsight/hadoop/hdinsight-use-hive)
* [HDInsight의 대화형 쿼리(Hive LLAP)](/azure/hdinsight/interactive-query/apache-interactive-query-get-started)


## <a name="technology-choices"></a>기술 선택

- [데이터 웨어하우스](../technology-choices/data-warehouses.md)
- [파이프라인 오케스트레이션](../technology-choices/pipeline-orchestration-data-movement.md)

