> 1. RepositoryCustom 사용 제외 이유
> 2. RepositoryCustom 사용이 필요한 경우
>
> 

##  querydsl RepositoryCustom

1. RepositoryCustom 사용하지 않아도 되는 이유
RepositoryCustom을 사용하지 않고 QueryDSL을 사용할 수 있는 이유는, QueryDSL이 동적 쿼리 생성과 복잡한 조건 처리를 JPA 리포지토리에서 바로 처리할 수 있도록 해주기 때문입니다. 이를 통해 JPA 리포지토리에서 QueryDSL 쿼리를 직접 작성하고 실행할 수 있기 때문에 커스텀 인터페이스를 별도로 만들지 않아도 됩니다.

예시: QueryDSL을 사용한 리포지토리 내 쿼리
java
코드 복사
public List<ServiceDTO> findServicesByStatus(int status) {
    QStService stService = QStService.stService;
    return queryFactory
        .selectFrom(stService)
        .where(stService.serviceStatus.eq(status))
        .fetch();
}
위와 같이 리포지토리 내에서 **queryFactory**를 사용하여 QueryDSL 쿼리를 작성하고, 리포지토리 메서드에서 바로 호출할 수 있습니다. 이때 **RepositoryCustom**을 사용하지 않아도 QueryDSL을 실행할 수 있습니다.

2. RepositoryCustom을 사용해야 하는 이유
RepositoryCustom은 JPA 리포지토리의 기본 메서드 외에 추가적인 기능을 제공하는 데 사용됩니다. RepositoryCustom을 사용하는 주된 이유는 복잡한 쿼리 로직이나 동적 쿼리를 리포지토리 메서드에 직접 포함시키지 않기 위해입니다. 또한, 다양한 비즈니스 로직을 분리하고 테스트 및 유지 보수성을 높이기 위한 구조적 이유도 있습니다.

사용해야 하는 이유:
복잡한 쿼리 로직을 분리

QueryDSL을 사용할 때, 쿼리가 복잡하고 여러 조건을 처리해야 하는 경우, 이를 리포지토리 클래스에서 분리하여 커스텀 리포지토리 인터페이스(RepositoryCustom)로 분리하는 것이 코드 유지보수에 유리합니다.
가독성 및 테스트 용이성:

복잡한 쿼리 로직을 리포지토리의 기본 메서드에 직접 구현하면 가독성이 떨어지고, 쿼리 로직을 단위 테스트하기 어렵습니다. 이를 RepositoryCustom에 분리하여 테스트가 용이하도록 할 수 있습니다.
쿼리의 재사용:

RepositoryCustom을 사용하면 쿼리 로직을 재사용하기 쉬워집니다. 예를 들어, 여러 리포지토리에서 같은 종류의 동적 쿼리가 필요할 때, 커스텀 리포지토리에서 공통적으로 사용할 수 있는 쿼리 로직을 정의할 수 있습니다.
확장성:

RepositoryCustom을 사용하면 추가적인 기능을 확장하기 좋습니다. 예를 들어, 쿼리의 조건을 동적으로 변경해야 하거나, 여러 개의 쿼리를 하나로 합쳐야 하는 경우가 있을 때, RepositoryCustom을 사용하면 이를 더 쉽게 처리할 수 있습니다.
3. 예시: RepositoryCustom 사용
java
코드 복사
// Custom Repository Interface
public interface ServiceRepositoryCustom {
    List<ServiceDTO> findServicesWithComplexConditions(SimulSearchDTO param);
}

// Custom Repository Implementation
public class ServiceRepositoryCustomImpl implements ServiceRepositoryCustom {
    @PersistenceContext
    private EntityManager em;

    @Override
    public List<ServiceDTO> findServicesWithComplexConditions(SimulSearchDTO param) {
        QStService stService = QStService.stService;
        QStMsg stMsg = QStMsg.stMsg;

        BooleanBuilder builder = new BooleanBuilder();
        // 검색 조건 설정
        if (param.getStatus() != null) {
            builder.and(stService.serviceStatus.eq(param.getStatus()));
        }

        // QueryDSL을 사용하여 복잡한 쿼리 실행
        return new JPAQuery<>(em)
            .select(stService, stMsg.msgId.count().as("msgCount"))
            .from(stService)
            .leftJoin(stMsg).on(stMsg.serviceId.eq(stService.serviceName))
            .where(builder)
            .groupBy(stService.id)
            .fetch();
    }
}
위와 같이 RepositoryCustom을 사용하면 복잡한 쿼리 로직을 커스텀 구현체에 분리하여 유지보수성과 재사용성을 높일 수 있다.

##  JPA RepositoryCustom

RepositoryCustom을 JPA 전체에서 사용해야 하는 경우는 복잡한 비즈니스 로직이나 동적 쿼리, 유지보수성 등의 이유로 기존 JPA 리포지토리에 직접 쿼리 로직을 작성하는 것이 적합하지 않을 때입니다. 기본적인 CRUD 기능을 제공하는 JPA 리포지토리 외에도 복잡한 쿼리나 커스텀 로직을 처리할 때 RepositoryCustom을 활용합니다.

RepositoryCustom을 사용해야 하는 주요 경우
복잡한 쿼리 로직을 분리해야 할 때

복잡한 동적 쿼리, 조건부 필터링, 연관된 여러 엔티티들에 대한 복합적인 조인을 처리해야 할 때 RepositoryCustom을 사용하여 쿼리 로직을 리포지토리 외부로 분리합니다.
예를 들어, 여러 조건에 따라 다른 쿼리 로직을 동적으로 적용해야 하는 경우 RepositoryCustom을 사용하여 쿼리 로직을 깔끔하게 분리하고, 리포지토리는 단순한 CRUD 기능만 담당할 수 있습니다.
JPA 리포지토리의 제한을 넘는 기능을 구현할 때

JPA 리포지토리 기본 기능은 CRUD 및 간단한 쿼리에 적합합니다. 그러나 JPA의 기본 메서드만으로는 복잡한 조건 쿼리나 집계 쿼리 등을 다루기 어렵습니다. 이때 RepositoryCustom을 활용하여 쿼리 작성 로직을 분리하고, 비즈니스 로직을 재사용 가능하게 만듭니다.
예를 들어, group by, having, join 등이 포함된 복잡한 쿼리나, 집계 함수(예: count(), sum(), avg())가 필요한 경우에 RepositoryCustom을 사용하여 커스텀 쿼리 메서드를 정의할 수 있습니다.
동적 쿼리를 효율적으로 처리해야 할 때

동적 쿼리를 작성할 때는 조건에 따라 쿼리가 달라지므로 JPA의 Specification이나 QueryDSL을 사용하거나, RepositoryCustom을 사용하여 조건을 동적으로 변경할 수 있습니다. 이를 통해 다양한 조건에 맞는 쿼리를 생성할 수 있습니다.
예시:

java
코드 복사
public interface ProductRepositoryCustom {
    List<Product> findByCustomCriteria(CustomCriteria criteria);
}

public class ProductRepositoryCustomImpl implements ProductRepositoryCustom {
    @PersistenceContext
    private EntityManager em;

    @Override
    public List<Product> findByCustomCriteria(CustomCriteria criteria) {
        StringBuilder jpql = new StringBuilder("SELECT p FROM Product p WHERE 1=1");
        if (criteria.getCategory() != null) {
            jpql.append(" AND p.category = :category");
        }
        if (criteria.getPriceMin() != null) {
            jpql.append(" AND p.price >= :priceMin");
        }
        Query query = em.createQuery(jpql.toString());
        if (criteria.getCategory() != null) {
            query.setParameter("category", criteria.getCategory());
        }
        if (criteria.getPriceMin() != null) {
            query.setParameter("priceMin", criteria.getPriceMin());
        }
        return query.getResultList();
    }
}
이 경우, 동적 쿼리를 RepositoryCustom에 분리하여 비즈니스 로직을 효율적으로 관리할 수 있습니다.

JPA의 기본 쿼리 메서드 외에 특수한 기능을 추가해야 할 때

JPA는 기본적인 CRUD 메서드와 단순한 JPQL 쿼리만 지원합니다. 예를 들어, 복잡한 집계나 native SQL 쿼리를 사용해야 하는 경우, RepositoryCustom을 사용하여 커스텀 구현을 추가합니다.
예를 들어, Native Query를 통해 데이터베이스에 최적화된 쿼리를 작성하고, 이를 RepositoryCustom을 통해 분리하여 관리할 수 있습니다.
java
코드 복사
public interface ProductRepositoryCustom {
    List<Object[]> findTotalRevenueByCategory();
}

public class ProductRepositoryCustomImpl implements ProductRepositoryCustom {
    @PersistenceContext
    private EntityManager em;

    @Override
    public List<Object[]> findTotalRevenueByCategory() {
        String nativeQuery = "SELECT category, SUM(price) FROM product GROUP BY category";
        return em.createNativeQuery(nativeQuery).getResultList();
    }
}
비즈니스 로직의 복잡성을 분리하고자 할 때

RepositoryCustom을 사용하면 비즈니스 로직을 Service 레이어에서 분리하고, 쿼리 로직을 별도의 커스텀 리포지토리에서 관리할 수 있습니다. 이렇게 함으로써 서비스 클래스가 더 간결해지고, 쿼리 로직에 대한 변경이 있을 때 RepositoryCustom만 수정하면 되므로 유지보수성이 높아집니다.
복잡한 트랜잭션 처리나 멀티 단계 작업이 필요한 경우

JPA의 기본 repository.save() 등의 메서드는 단순한 저장 작업만 처리할 수 있습니다. 그러나 복잡한 트랜잭션 작업이나 여러 단계로 나누어진 작업을 처리하려면, RepositoryCustom을 사용하여 커스텀 메서드를 정의하고, 여러 단계를 처리할 수 있습니다.
5. RepositoryCustom을 사용하지 않아도 되는 경우
단순한 CRUD 작업: 기본적인 save(), findById(), delete() 등의 CRUD 작업은 RepositoryCustom 없이 JpaRepository에서 기본으로 제공하는 메서드를 사용해 처리할 수 있습니다.

기본적인 JPQL 쿼리: JPQL이나 HQL을 사용하는 단순한 쿼리는 RepositoryCustom 없이도 JpaRepository에서 @Query 어노테이션을 통해 쉽게 처리할 수 있습니다.

간단한 Specification 사용: **Specification**을 사용하여 동적 쿼리를 작성할 때는 RepositoryCustom을 사용하지 않고도 조건에 맞는 쿼리를 처리할 수 있습니다.

결론
RepositoryCustom은 복잡한 비즈니스 로직, 동적 쿼리 처리, native SQL 쿼리, 조건에 따라 변하는 쿼리 등을 다룰 때 유용합니다. 리포지토리에서 직접 처리하기 어려운 복잡한 쿼리 로직이나 비즈니스 로직을 분리하여 관리하고, 코드의 유지보수성과 확장성을 높이기 위해 사용됩니다.

따라서, 단순한 CRUD와 기본적인 쿼리에는 RepositoryCustom을 사용할 필요가 없고, 복잡한 쿼리 로직이나 특수한 요구 사항이 있을 때 RepositoryCustom을 사용하는 것이 바람직합니다.
