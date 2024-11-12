##  RepositoryImpl 에서 RepositoryCustom를 상속 받는 이유




          JpaRepository와  RepositoryCustom을 둘다 상속받고 있는  Repository interface를 implements 해야하지 않을까?

**Sol)**  
Spring Data JPA의 Custom Repository 구조 규칙 때문 <br>
Spring Data JPA는 Custom Repository 기능을 지원할 때,
특정 규칙에 따라 자동으로 Custom Repository 구현체를 연결<br>
RepositoryImpl 클래스 이름에서 Impl 부분을 찾아 RepositoryCustom 인터페이스의 구현체로 자동 인식

**Result)**  
StSystemRepositoryImpl에서는 StSystemRepositoryCustom만 구현하면 <br>
Spring Data JPA가 StSystemRepository의 Custom 기능으로 자동 연결

**정리)**  
RepositoryCustom (인터페이스): Custom 쿼리 메서드를 정의<br>
RepositoryImpl (클래스): RepositoryCustom을 구현, Custom 쿼리 메서드를 작성
<br><br>
####  Source 예시)
 
```java
package kr.co.artl.simulator.repository;
import org.springframework.data.jpa.repository.JpaRepository;
import kr.co.artl.simulator.domain.StSystem;

public interface StSystemRepository extends JpaRepository<StSystem, Long>, StSystemRepositoryCustom{
}
```
```java
package kr.co.artl.simulator.repository;

import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;

import kr.co.artl.common.dto.SimulSearchDTO;
import kr.co.artl.simulator.domain.StSystem;

public interface StSystemRepositoryCustom {

    //List<StSystem> findAll();
    Page<StSystem> findSystemsBySearchParam(SimulSearchDTO param, Pageable pageable);
}

```

```java
package kr.co.artl.simulator.repository;

import java.util.ArrayList;
import java.util.List;

import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageImpl;
import org.springframework.data.domain.Pageable;
import org.springframework.data.domain.Sort;
import org.springframework.stereotype.Repository;

import com.querydsl.core.types.Order;
import com.querydsl.core.types.OrderSpecifier;
import com.querydsl.core.types.dsl.ComparableExpressionBase;
import com.querydsl.core.types.dsl.Expressions;
import com.querydsl.core.types.dsl.PathBuilder;
import com.querydsl.jpa.impl.JPAQueryFactory;

import jakarta.persistence.EntityManager;
import kr.co.artl.common.dto.SimulSearchDTO;
import kr.co.artl.simulator.domain.QStSystem;
import kr.co.artl.simulator.domain.StSystem;

@Repository
public class StSystemRepositoryImpl implements StSystemRepositoryCustom{	
    private final JPAQueryFactory queryFactory;
    private final EntityManager entityManager; 

    public StSystemRepositoryImpl(EntityManager entityManager) {
        this.queryFactory = new JPAQueryFactory(entityManager);
        this.entityManager = entityManager; 
    } 
	
//	@Override
//    public List<StSystem> findAll() {
//        QStSystem stSystem = QStSystem.stSystem;
//        return queryFactory
//                .selectFrom(stSystem)
//                .fetch();
//    }
    
    @Override
    public Page<StSystem> findSystemsBySearchParam(SimulSearchDTO param,Pageable pageable) {
        QStSystem stSystem = QStSystem.stSystem;
	
        // 정렬 조건이 있는 경우 OrderSpecifier 리스트 생성
        List<OrderSpecifier<?>> orderSpecifiers = getOrderSpecifiers(pageable.getSort(), stSystem);
	
        List<StSystem> systems = queryFactory
	    .selectFrom(stSystem)
	    .orderBy(orderSpecifiers.toArray(new OrderSpecifier[0])) // OrderSpecifier 적용
            .offset(pageable.getOffset())
            .limit(pageable.getPageSize())
            .fetch();
	
	    long total = queryFactory.selectFrom(stSystem).fetch().size();
	    return new PageImpl<>(systems, pageable, total);
	}
   /*
    * Spring Data JPA Sort를 QueryDSL OrderSpecifier로 변환하는 함수
    * 
    *  반복문 사용이유 :
    *  로직 상 sort기준을 하나만 사용하게되겠지만
    *  Sort 클래스의 필드 order는 List<Order> orders로 선언되어있다. 
    */
    private List<OrderSpecifier<?>> getOrderSpecifiers(Sort sort, QStSystem qStSystem) { 
        List<OrderSpecifier<?>> orders = new ArrayList<>();
        for (Sort.Order order : sort) {
            Order direction = order.isAscending() ? Order.ASC : Order.DESC;

            // 필드 이름으로 PathBuilder를 통해 동적으로 필드를 가져오기
            PathBuilder<Comparable> pathBuilder = new PathBuilder<>(Comparable.class, qStSystem.getMetadata().getName());
            
            // 필드 이름을 사용하여 동적으로 ComparableExpressionBase 생성
            ComparableExpressionBase<?> fieldPath = Expressions.comparablePath(Comparable.class, pathBuilder, order.getProperty());

            // OrderSpecifier 생성
            orders.add(new OrderSpecifier<>(direction, fieldPath));
        }
        return orders;
    }	
}
```
