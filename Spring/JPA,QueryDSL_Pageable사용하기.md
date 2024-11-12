#### 설명은 주석으로 담

####  Source 예시)
```java
package kr.co.artl.simulator.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.data.domain.Sort;
import org.springframework.data.web.PageableDefault;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import kr.co.artl.common.dto.SimulSearchDTO;
import kr.co.artl.simulator.domain.StSystem;
import kr.co.artl.simulator.service.SimulService;

@RestController
@RequestMapping("/api/simulator")
public class SimulController {

  private final SimulService simulService;

  @Autowired // Spring Boot 4.3 이후, 생성자가 하나뿐인 경우에 @Autowired 생략 가능
  public SimulController(SimulService simulService) { // 의존성 주입은 생성자로 하는 것이 안전
    this.simulService = simulService;
  }

  @GetMapping
  public Page<StSystem> getSystemList(
      SimulSearchDTO param,
      @PageableDefault(size = 10, sort = "reg_date", direction = Sort.Direction.ASC) Pageable pageable) {

    // Page 객체 반환
    return simulService.getSystems(param, pageable);
  }
}
```

```java
package kr.co.artl.simulator.service;

import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import kr.co.artl.common.dto.SimulSearchDTO;
import kr.co.artl.simulator.domain.StSystem;
import kr.co.artl.simulator.repository.StSystemRepository;

@Service
@Transactional(readOnly = true)
public class SimulService {

  private final StSystemRepository stSystemRepository;

  public SimulService(StSystemRepository stSystemRepository) {
    this.stSystemRepository = stSystemRepository;
  }

  // pagination 적용을 위한 jpa data pageable 사용 ::: 반환객체 page
  public Page<StSystem> getSystems(SimulSearchDTO param, Pageable pageable) {
    return stSystemRepository.findSystemsBySearchParam(param, pageable);
  }
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
public class StSystemRepositoryImpl implements StSystemRepositoryCustom {

  /*
   * StSystemRepositoryImpl이 StSystemRepositoryCustom을 구현하는 이유
   *
   * 문제:
   * JpaRepository와 StSystemRepositoryCustom을 동시에 상속받고 있는
   * StSystemRepository 인터페이스에 바로 구현을 해야 하는 것 아닌가?
   *
   * 설명:
   * Spring Data JPA의 Custom Repository 규칙에 따라 특정 네이밍 규칙에 맞는
   * 커스텀 구현체를 자동으로 인식하여 연결할 수 있다.
   * 즉, StSystemRepositoryImpl 클래스 이름에서 Impl 부분을 찾아
   * StSystemRepositoryCustom 인터페이스의 구현체로 자동 인식하게 된다.
   *
   * 결론:
   * StSystemRepositoryImpl에서는 StSystemRepositoryCustom만 구현하면 Spring Data JPA가
   * StSystemRepository의 커스텀 기능으로 자동 연결해 준다.
   */

  private final JPAQueryFactory queryFactory;
  private final EntityManager entityManager;

  public StSystemRepositoryImpl(EntityManager entityManager) {
    this.queryFactory = new JPAQueryFactory(entityManager);
    this.entityManager = entityManager;
  }

  @Override
  public Page<StSystem> findSystemsBySearchParam(SimulSearchDTO param, Pageable pageable) {
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
   * 반복문 사용이유:
   * 로직 상 sort 기준을 하나만 사용하게 되겠지만
   * Sort 클래스의 필드 order는 List<Order> orders로 선언되어 있다.
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
