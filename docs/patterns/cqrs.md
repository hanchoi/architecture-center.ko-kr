---
title: CQRS
description: "별도의 인터페이스를 사용하여 데이터를 업데이트하는 작업과 데이터를 읽는 작업을 분리합니다."
keywords: "디자인 패턴"
author: dragon119
ms.date: 06/23/2017
pnp.series.title: Cloud Design Patterns
pnp.pattern.categories:
- data-management
- design-implementation
- performance-scalability
ms.openlocfilehash: 80f4a8880cf2212acf82dadb67b0181e1cbae099
ms.sourcegitcommit: a7aae13569e165d4e768ce0aaaac154ba612934f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2018
---
# <a name="command-and-query-responsibility-segregation-cqrs-pattern"></a>CQRS(명령 및 쿼리 책임 분리) 패턴

[!INCLUDE [header](../_includes/header.md)]

별도의 인터페이스를 사용하여 데이터를 업데이트하는 작업과 데이터를 읽는 작업을 분리합니다. 이 패턴은 시스템 성능, 확장성 및 보안을 극대화할 수 있습니다. 유연성을 높여 시간이 지나면서 시스템이 진화되는 것을 지원하여 업데이트 명령으로 인해 도메인 수준에서 병합 충돌이 발생하는 것을 방지합니다.

## <a name="context-and-problem"></a>컨텍스트 및 문제점

전통적인 데이터 관리 시스템에서 명령(데이터 업데이트)과 쿼리(데이터 요청)는 단일 데이터 리포지토리의 동일한 엔터티 집합에 대해 실행됩니다. 이 엔터티는 SQL Server와 같은 관계형 데이터베이스에서 하나 이상의 테이블 행의 하위 집합일 수 있습니다.

일반적으로 이러한 시스템에서 이루어지는 모든 CRUD(만들기, 읽기, 업데이트 및 삭제) 작업은 동일한 엔터티 표현에 적용됩니다. 예를 들어 고객을 나타내는 DTO(데이터 전송 개체)는 DAL(데이터 액세스 계층)을 통해 데이터 저장소에서 검색되어 화면에 표시됩니다. 사용자는 데이터 바인딩을 통해 DTO의 일부 필드를 업데이트하고 DTO는 DAL을 통해 데이터 저장소에 다시 저장됩니다. 읽기와 쓰기 작업에는 동일한 DTO가 사용됩니다. 아래 그림은 기존의 CRUD 아키텍처를 보여 줍니다.

![기존 CRUD 아키텍처](./_images/command-and-query-responsibility-segregation-cqrs-tradition-crud.png)

기존의 CRUD는 데이터 작업에 제한된 비즈니스 논리가 적용된 경우에만 작업을 제대로 디자인합니다. 개발 도구가 제공하는 스캐폴드 메커니즘은 데이터 액세스 코드를 매우 빠르게 만들 수 있는데, 이 코드는 나중에 필요에 따라 사용자 지정할 수 있습니다.

그러나 기존 CRUD 방식에는 다음과 같은 몇 가지 단점이 있습니다.

- 꼭 필요한 작업의 일부가 아니더라도 정확하게 업데이트되어야 하는 추가 열이나 속성과 같이 데이터의 읽기와 쓰기 표현 사이에 불일치가 나타나는 경우가 많습니다.

- 여러 작업자가 동일한 데이터 집합을 동시에 사용하는 공동 작업 도메인의 데이터 저장소에 레코드가 잠겨 있는 경우 데이터 경합을 초래할 위험이 있습니다. 또는 낙관적 잠금을 사용하는 경우 동시 업데이트로 인해 업데이트 충돌이 발생할 수 있습니다. 이러한 위험은 시스템의 복잡성과 처리량이 늘어날수록 증가합니다. 또한 기존의 접근 방식은 데이터 저장소와 데이터 액세스 계층에 가해지는 부하뿐 아니라 정보를 검색하는 데 필요한 쿼리의 복잡성으로 인해 성능에 좋지 않은 영향을 미칠 수 있습니다.

- 엔터티 각각이 읽기와 쓰기 작업의 대상으로 잘못된 컨텍스트에 데이터를 노출시킬 수 있기 때문에 보안 및 권한 관리가 더 복잡해질 수 있습니다.

> CRUD 접근 방식의 제한에 대한 자세한 내용은 [CRUD, Only When You Can Afford It](https://blogs.msdn.microsoft.com/maarten_mullender/2004/07/23/crud-only-when-you-can-afford-it-revisited/)(CRUD, 사용 가능한 경우에만)을 참조하세요.

## <a name="solution"></a>해결 방법

CQRS(명령 및 쿼리 책임 분리)는 별도의 인터페이스를 사용하여 데이터를 업데이트하는 작업(명령)에서 데이터를 읽는 작업(쿼리)을 분리하는 패턴입니다. 이는 쿼리와 업데이트에 사용되는 데이터 모델이 다르다는 것입니다. 그런 다음 이 모델을 아래 그림과 같이 분리할 수 있는데, 반드시 분리해야 하는 것은 아닙니다.

![기본 CQRS 아키텍처](./_images/command-and-query-responsibility-segregation-cqrs-basic.png)

CQRS 기반 시스템에서 데이터의 쿼리와 업데이트가 분리된 모델을 사용하면 CRUD 기반 시스템에 사용하는 단일 데이터 모델에 비해 디자인과 구현이 간단해집니다. 그러나 여기에는 CRUD 디자인에는 없는 단점이 한 가지 있는데, 그것은 바로 스캐폴드 메커니즘을 사용해 CQRS 코드를 자동으로 생성할 수 없다는 점입니다.

데이터를 읽기 위한 쿼리 모델과 데이터를 쓰기 위한 업데이트 모델은 SQL 뷰를 사용하거나 프로젝션을 즉시 생성해 동일한 물리적 저장소에 액세스할 수 있습니다. 그러나 일반적으로는 다음 그림에서와 같이 성능, 확장성, 보안을 최대화하기 위해 데이터를 여러 물리적 저장소로 분리합니다.

![읽기 및 쓰기 저장소가 분리된 CQRS 아키텍처](./_images/command-and-query-responsibility-segregation-cqrs-separate-stores.png)

읽기 저장소는 쓰기 저장소의 읽기 전용 복제본이거나 읽기 및 쓰기 저장소가 전혀 다른 구조일 수 있습니다. 읽기 저장소의 읽기 전용 복제본을 여러 개 사용하면 특히 읽기 전용 복제본이 응용 프로그램 인스턴스에 가깝게 위치하는 분산 시나리오에서 쿼리 성능과 응용 프로그램 UI 응답성을 크게 높일 수 있습니다. 일부 데이터베이스 시스템(SQL Server)은 가용성을 최대화하기 위한 장애 조치(failover)용 복제본과 같은 추가 기능을 제공합니다.

별도의 읽기 및 쓰기 저장소를 사용하면 부하를 감안해 각각을 적절하게 확장할 수도 있습니다. 예를 들어 보통 읽기 저장소는 쓰기 저장소보다 부하가 훨씬 더 높습니다.

쿼리/읽기 모델에 비정규화된 데이터([구체화된 뷰 패턴](materialized-view.md) 참조)가 포함되어 있으면 응용 프로그램에서 각각의 보기를 위해 데이터를 읽거나 시스템 내 데이터를 쿼리할 때 성능이 최대화됩니다.

## <a name="issues-and-considerations"></a>문제 및 고려 사항

이 패턴을 구현할 방법을 결정할 때 다음 사항을 고려하세요.

- 데이터 저장소를 읽기 및 쓰기 작업을 위한 별도의 물리적 저장소로 분리하면 시스템의 성능과 보안이 좋아지지만 복원력과 결과적 일관성 면에서 더 복잡해질 수 있습니다. 읽기 모델 저장소는 쓰기 모델 저장소의 변경 사항을 반영하도록 업데이트되어야 합니다. 사용자가 오래된 읽기 데이터를 기반으로 요청을 발급하면 변경된 사항을 검색하기 어려울 수 있는데, 그런 경우 작업을 완료할 수 없습니다.

    > 결과적 일관성에 대한 자세한 내용은 [데이터 일관성 입문서](https://msdn.microsoft.com/library/dn589800.aspx)를 참조하세요.

- 가장 가치 있는 시스템의 제한된 구역에 CQRS 적용을 고려해야 합니다.

- 결과적 일관성을 배포하는 일반적인 접근 방식은 이벤트 소싱을 CQRS와 함께 사용하는 것입니다. 그러면 쓰기 모델이 명령의 실행으로 구동되는 이벤트의 추가 전용 스트림이 됩니다. 이러한 이벤트는 읽기 모델로 작용하는 구체화된 뷰를 업데이트하는 데 사용합니다. 자세한 내용은 [이벤트 소싱 및 CQRS](https://msdn.microsoft.com/library/dn568103.aspx#EventSourcingandCQRS)를 참조하세요.

## <a name="when-to-use-this-pattern"></a>이 패턴을 사용해야 하는 경우

다음 경우에 이 패턴을 사용합니다.

- 여러 작업이 동일한 데이터에서 동시에 수행되는 공동 작업 도메인. CQRS를 사용하면 동일한 유형으로 보이는 데이터를 업데이트하더라도 도메인 수준에서 병합 충돌(충돌이 발생할 때 명령으로 병합할 수 있는 충돌)을 최소화할 수 있을 정도로 자세하게 명령을 정의할 수 있습니다.

- 여러 단계를 거치거나 복잡한 도메인 모델을 사용하는 복잡한 프로세스를 통해 사용자를 안내하는 작업 기반 사용자 인터페이스. DDD(도메인 기반 디자인) 기법에 이미 익숙한 팀에도 유용합니다. 쓰기 모델은 비즈니스 논리, 입력 유효성 검사, 비즈니스 유효성 검사와 함께 완전한 명령 처리 스택을 보유하기 때문에 쓰기 모델의 각 집계(데이터 변경을 위한 단위로 처리되는 관련 개체의 각 클러스터)는 항상 일관성을 유지합니다. 읽기 모델은 비즈니스 논리 또는 유효성 검사 스택을 보유하지 않으며 보기 모델에 사용할 DTO를 반환하기만 합니다. 결과적으로 읽기 모델과 쓰기 모델의 일관성이 유지됩니다.

- 데이터 읽기 성능을 데이터 쓰기 성능과 별도로 세밀하게 조정해야 하는 시나리오(특히 읽기/쓰기 속도가 매우 빠르고 수평 확장이 필요한 경우). 예를 들어 많은 시스템에서 읽기 작업 수가 쓰기 작업 수보다 몇 배 더 많습니다. 이런 사실을 감안한다면 읽기 모델을 확장하고 쓰기 모델은 하나 또는 소수의 인스턴스에서만 실행하는 것이 좋습니다. 소수의 쓰기 모델 인스턴스는 병합 충돌 발생을 최소화하는 데도 기여합니다.

- 개발자 중 한 팀은 쓰기 모델에 포함되는 복잡한 도메인 모델에 집중하고 또 한 팀은 읽기 모델과 사용자 인터페이스에 집중할 수 있는 시나리오.

- 시스템이 시간이 지나면서 진화할 것으로 예상되어 여러 버전의 모델을 포함할 수 있거나 비즈니스 규칙이 정기적으로 변하는 시나리오

- 특히 이벤트 소싱과 조합해 다른 시스템과 통합하는 경우. 이때 하위 시스템 하나의 일시적인 장애가 다른 시스템의 가용성에 영향을 주지 않아야 합니다.

다음 경우에는 이 패턴을 권장하지 않습니다.

- 도메인 또는 비즈니스 규칙이 간단한 경우

- 간단한 CRUD 스타일 사용자 인터페이스 및 관련된 데이터 액세스 작업이 충분한 경우

- 전체 시스템에 구현하는 경우. CQRS가 유용할 수 있지만 필요하지 않을 때 상당하고 불필요한 복잡성을 추가할 수 있는 전체 데이터 관리 시나리오의 특정 구성 요소가 존재합니다.

## <a name="event-sourcing-and-cqrs"></a>이벤트 소싱 및 CQRS

CQRS 패턴은 이벤트 소싱 패턴과 함께 사용되는 경우가 많습니다. CQRS 기반 시스템은 별도의 읽기 및 쓰기 데이터 모델을 사용하며 각각 관련 작업에 맞춤화되고 종종 물리적으로 분리된 저장소에 배치됩니다. [이벤트 소싱](event-sourcing.md) 패턴과 함께 사용할 때 이벤트 저장소는 쓰기 모델이며 정보의 공식적인 출처입니다. 보통 CQRS 기반 시스템의 읽기 모델은 고도로 비정규화된 뷰의 형태로 데이터의 구체화된 뷰를 제공합니다. 이러한 뷰는 응용 프로그램의 인터페이스 및 디스플레이 요구 사항에 맞춤화되어 디스플레이 및 쿼리 성능을 모두 최대화하는 데 기여합니다.

특정 시점의 실제 데이터 대신 이벤트의 스트림을 쓰기 저장소로 사용하면 단일 집계에서 업데이트 충돌을 방지하고 성능과 확장성을 최대화할 수 있습니다. 읽기 저장소를 채우는 데 사용하는 데이터의 구체화된 뷰를 비동기적으로 생성하는 데 이벤트를 사용할 수 있습니다.

이벤트 저장소는 정보의 공식적인 출처이기 때문에, 시스템이 진화하거나 읽기 모델을 변경해야 할 때 구체화된 뷰를 삭제하고 모든 지난 이벤트를 재생해 현재 상태의 새로운 표현을 생성할 수 있습니다. 사실상 구체화된 뷰는 데이터의 지속형 읽기 전용 캐시입니다.

CQRS를 이벤트 소싱 패턴과 함께 사용할 때는 다음을 고려해야 합니다.

- 쓰기 및 읽기 저장소가 분리되는 시스템처럼 이 패턴을 기반으로 하는 시스템만이 결과적으로 일관성을 유지할 수 있습니다. 생성되는 이벤트와 업데이트되는 데이터 저장소 사이에는 약간의 지연이 나타나게 됩니다.

- 이벤트를 시작하고 처리하며 쿼리나 읽기 모델에 필요한 적절한 뷰 또는 개체를 어셈블하거나 업데이트하는 코드를 생성해야 하기 때문에, 이벤트 소싱 패턴은 더 복잡합니다. CQRS 패턴을 이벤트 소싱 패턴과 함께 사용하는 경우 패턴이 복잡하기 때문에 성공적으로 구현하는 것이 더 어려워질 수 있으므로 시스템 디자인에 대한 다른 접근 방식이 필요합니다. 그러나 이벤트 소싱은 도메인을 더 쉽게 모델링할 수 있고 데이터를 변경한 의도가 보존되기 때문에 뷰를 다시 작성하거나 새로 만들기가 더 쉽습니다.

- 특정 엔터티 또는 엔터티 모음을 위한 이벤트를 재생하고 처리하여 데이터의 읽기 모델 또는 프로젝션에 사용할 구체화된 뷰를 생성하려면 상당한 처리 시간과 리소스 사용이 필요할 수 있습니다. 장기간 값의 합계 또는 분석이 필요한 경우에 특히 그런데 그 이유는 관련된 모든 이벤트를 검사해야 하기 때문입니다. 이 문제는 발생한 특정 동작의 총 개수 또는 엔터티의 현재 상태와 같이 예약된 간격으로 데이터의 스냅숏을 구현해 해결할 수 있습니다.

## <a name="example"></a>예

다음 코드는 읽기 모델과 쓰기 모델에 다른 정의를 사용하는 CQRS 구현의 예제 중 일부를 보여 줍니다. 모델 인터페이스는 기본 데이터 저장소의 기능을 지정하지 않고 진화할 수 있으며 분리되기 있기 때문에 독립적으로 세밀하게 조정할 수 있습니다.

다음 코드는 읽기 모델 정의를 보여줍니다.

```csharp
// Query interface
namespace ReadModel
{
  public interface ProductsDao
  {
    ProductDisplay FindById(int productId);
    ICollection<ProductDisplay> FindByName(string name);
    ICollection<ProductInventory> FindOutOfStockProducts();
    ICollection<ProductDisplay> FindRelatedProducts(int productId);
  }

  public class ProductDisplay
  {
    public int Id { get; set; }
    public string Name { get; set; }
    public string Description { get; set; }
    public decimal UnitPrice { get; set; }
    public bool IsOutOfStock { get; set; }
    public double UserRating { get; set; }
  }

  public class ProductInventory
  {
    public int Id { get; set; }
    public string Name { get; set; }
    public int CurrentStock { get; set; }
  }
}
```

시스템에서 사용자가 제품을 평가하는 것을 허용합니다. 응용 프로그램 코드는 다음 코드에 제시된 `RateProduct` 명령을 사용하여 제품을 평가합니다.

```csharp
public interface ICommand
{
  Guid Id { get; }
}

public class RateProduct : ICommand
{
  public RateProduct()
  {
    this.Id = Guid.NewGuid();
  }
  public Guid Id { get; set; }
  public int ProductId { get; set; }
  public int Rating { get; set; }
  public int UserId {get; set; }
}
```

시스템이 `ProductsCommandHandler` 클래스를 사용하여 응용 프로그램이 전송한 명령을 처리합니다. 일반적으로 클라이언트는 큐와 같은 메시징 시스템을 통해 명령을 도메인에 보냅니다. 명령 처리기는 이러한 명령을 수락하고 도메인 인터페이스의 메서드를 호출합니다. 요청이 충돌할 가능성이 줄어들도록 세분화된 각 명령이 디자인됩니다. 다음 코드는 `ProductsCommandHandler` 클래스의 개요를 보여 줍니다.

```csharp
public class ProductsCommandHandler :
    ICommandHandler<AddNewProduct>,
    ICommandHandler<RateProduct>,
    ICommandHandler<AddToInventory>,
    ICommandHandler<ConfirmItemShipped>,
    ICommandHandler<UpdateStockFromInventoryRecount>
{
  private readonly IRepository<Product> repository;

  public ProductsCommandHandler (IRepository<Product> repository)
  {
    this.repository = repository;
  }

  void Handle (AddNewProduct command)
  {
    ...
  }

  void Handle (RateProduct command)
  {
    var product = repository.Find(command.ProductId);
    if (product != null)
    {
      product.RateProduct(command.UserId, command.Rating);
      repository.Save(product);
    }
  }

  void Handle (AddToInventory command)
  {
    ...
  }

  void Handle (ConfirmItemsShipped command)
  {
    ...
  }

  void Handle (UpdateStockFromInventoryRecount command)
  {
    ...
  }
}
```

다음 코드는 쓰기 모델의 `IProductsDomain` 인터페이스를 보여 줍니다.

```csharp
public interface IProductsDomain
{
  void AddNewProduct(int id, string name, string description, decimal price);
  void RateProduct(int userId, int rating);
  void AddToInventory(int productId, int quantity);
  void ConfirmItemsShipped(int productId, int quantity);
  void UpdateStockFromInventoryRecount(int productId, int updatedQuantity);
}
```

또한 `IProductsDomain` 인터페이스가 도메인에서 의미가 있는 메서드를 포함하는 방법도 알려 줍니다. 일반적으로 CRUD 환경에서 이러한 메서드는 `Save` 또는 `Update`와 같은 제네릭 이름을 포함하고 DTO만 인수로 사용할 수 있습니다. CQRS 접근 방식은 이 조직의 비즈니스 및 인벤토리 관리 시스템에 대한 요구를 충족하도록 디자인할 수 있습니다.

## <a name="related-patterns-and-guidance"></a>관련 패턴 및 지침

이 패턴을 구현할 때 유용한 패턴 및 지침은 다음과 같습니다.

- 다른 아키텍처 스타일과 함께 CQRS를 비교하려면 [아키텍처 스타일](/azure/architecture/guide/architecture-styles/) 및 [CQRS 아키텍처 스타일](/azure/architecture/guide/architecture-styles/cqrs)을 참조하세요.

- [데이터 일관성 입문서](https://msdn.microsoft.com/library/dn589800.aspx). CQRS 패턴을 사용할 때 읽기 및 쓰기 데이터 저장소 간의 결과적 일관성때문에 일반적으로 발생하는 문제와 이러한 문제의 해결 방법을 설명합니다.

- [데이터 분할 지침](https://msdn.microsoft.com/library/dn589795.aspx). CQRS 패턴에 사용하는 읽기 및 쓰기 데이터 저장소를 별도로 관리하고 액세스할 수 있는 파티션으로 분할해 확장성을 향상시키고, 경합을 줄이며, 성능을 최적화할 수 있는 방법을 설명합니다.

- [이벤트 소싱 패턴](event-sourcing.md). 이벤트 소싱을 CQRS 패턴과 함께 사용하여 복잡한 도메인의 작업을 간소화하면서 성능, 확장성 및 응답성을 향상시킬 수 있는 방법을 자세히 설명합니다. 또한 트랜잭션 데이터의 일관성을 제공하면서 보상 동작을 가능하게 하는 전체 감사 내역과 기록을 유지하는 방법도 설명합니다.

- [구체화된 뷰 패턴](materialized-view.md). CQRS를 구현한 읽기 모델은 쓰기 모델 데이터의 구체화된 뷰를 포함할 수 있습니다. 또는 구체화된 뷰를 생성하는 데 읽기 모델을 사용할 수 있습니다.

- 패턴 및 사례 가이드 [CQRS 과정](http://aka.ms/cqrs). 특히 [Introducing the Command Query Responsibility Segregation Pattern](https://msdn.microsoft.com/library/jj591573.aspx)(명령 쿼리 책임 분리 패턴 소개)은 패턴 및 유용한 경우를 탐색하며, [Epilogue: Lessons Learned](https://msdn.microsoft.com/library/jj591568.aspx)(에필로그: 교훈)는 이 패턴을 사용할 때 발생하는 문제 중 일부를 이해하는 데 도움이 됩니다.

- [Martin Fowler의 CQRS](http://martinfowler.com/bliki/CQRS.html) 게시물. 패턴의 기본 내용을 설명하고 다른 유용한 리소스에 연결되는 링크를 제공합니다.

- [Greg Young의 게시물](http://codebetter.com/gregyoung/). CQRS 패턴의 여러 측면을 탐색합니다.
