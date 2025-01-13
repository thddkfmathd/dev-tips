
> querydsl 반환타입 tuple을 dto로 변환...?이부분 수정필요함 
>
> mapper적용예시및 생성코드
>
> 

```
package kr.co.artl.simulator.dto;

import org.mapstruct.Mapper;
import org.mapstruct.Mapping;
import org.mapstruct.Mappings;

import kr.co.artl.simulator.domain.StService;
import kr.co.artl.simulator.domain.StSystem;

@Mapper(componentModel = "spring")
public interface SimulMapper {
	
	@Mapping(target = "resMsgCnt", ignore = true) // 매핑하지 않음
    ServiceDTO toServiceDTO(StService entity);
    
    StService toStService(ServiceDTO dto);

    SystemDTO toSystemDTO(StSystem entity);
    
    StSystem toStSystem(SystemDTO dto);
}

```
package kr.co.artl.simulator.dto;

import javax.annotation.processing.Generated;
import kr.co.artl.simulator.domain.StService;
import kr.co.artl.simulator.domain.StSystem;
import org.springframework.stereotype.Component;

@Generated(
    value = "org.mapstruct.ap.MappingProcessor",
    date = "2025-01-13T11:28:49+0900",
    comments = "version: 1.5.3.Final, compiler: IncrementalProcessingEnvironment from gradle-language-java-8.10.2.jar, environment: Java 21.0.5 (Oracle Corporation)"
)
@Component
public class SimulMapperImpl implements SimulMapper {

    @Override
    public ServiceDTO toServiceDTO(StService entity) {
        if ( entity == null ) {
            return null;
        }

        ServiceDTO.ServiceDTOBuilder serviceDTO = ServiceDTO.builder();

        serviceDTO.id( entity.getId() );
        serviceDTO.serviceId( entity.getServiceId() );
        serviceDTO.serviceName( entity.getServiceName() );
        serviceDTO.systemId( entity.getSystemId() );
        serviceDTO.system( entity.getSystem() );
        serviceDTO.msgType( entity.getMsgType() );
        serviceDTO.outMsgType( entity.getOutMsgType() );
        serviceDTO.responseType( entity.getResponseType() );
        serviceDTO.userId( entity.getUserId() );
        serviceDTO.regDate( entity.getRegDate() );
        serviceDTO.timeout( entity.getTimeout() );
        serviceDTO.inputSync( entity.getInputSync() );
        serviceDTO.outputSync( entity.getOutputSync() );
        serviceDTO.outputProtocol( entity.getOutputProtocol() );
        serviceDTO.outputMethod( entity.getOutputMethod() );
        serviceDTO.responseMsg( entity.getResponseMsg() );
        serviceDTO.fmDefaultCode( entity.getFmDefaultCode() );
        serviceDTO.serviceStatus( entity.getServiceStatus() );
        serviceDTO.userName( entity.getUserName() );
        serviceDTO.headerMapping( entity.getHeaderMapping() );
        serviceDTO.individualMapping( entity.getIndividualMapping() );
        serviceDTO.globalPos( entity.getGlobalPos() );
        serviceDTO.globalLength( entity.getGlobalLength() );

        return serviceDTO.build();
    }

    @Override
    public StService toStService(ServiceDTO dto) {
        if ( dto == null ) {
            return null;
        }

        StService stService = new StService();

        stService.setId( dto.getId() );
        stService.setServiceId( dto.getServiceId() );
        stService.setServiceName( dto.getServiceName() );
        stService.setSystemId( dto.getSystemId() );
        stService.setMsgType( dto.getMsgType() );
        stService.setUserId( dto.getUserId() );
        stService.setRegDate( dto.getRegDate() );
        stService.setTimeout( dto.getTimeout() );
        stService.setInputSync( dto.getInputSync() );
        stService.setOutputSync( dto.getOutputSync() );
        stService.setOutMsgType( dto.getOutMsgType() );
        stService.setOutputProtocol( dto.getOutputProtocol() );
        stService.setOutputMethod( dto.getOutputMethod() );
        stService.setResponseType( dto.getResponseType() );
        stService.setResponseMsg( dto.getResponseMsg() );
        stService.setFmDefaultCode( dto.getFmDefaultCode() );
        stService.setServiceStatus( dto.getServiceStatus() );
        stService.setSystem( dto.getSystem() );
        stService.setUserName( dto.getUserName() );
        stService.setHeaderMapping( dto.getHeaderMapping() );
        stService.setIndividualMapping( dto.getIndividualMapping() );
        stService.setGlobalPos( dto.getGlobalPos() );
        stService.setGlobalLength( dto.getGlobalLength() );

        return stService;
    }

    @Override
    public SystemDTO toSystemDTO(StSystem entity) {
        if ( entity == null ) {
            return null;
        }

        SystemDTO.SystemDTOBuilder systemDTO = SystemDTO.builder();

        systemDTO.systemId( entity.getSystemId() );
        systemDTO.system( entity.getSystem() );
        systemDTO.handler( entity.getHandler() );
        systemDTO.protocol( entity.getProtocol() );
        systemDTO.syncType( entity.getSyncType() );
        systemDTO.port( entity.getPort() );
        systemDTO.status( entity.getStatus() );
        systemDTO.ip( entity.getIp() );
        systemDTO.encoding( entity.getEncoding() );
        systemDTO.regDate( entity.getRegDate() );
        systemDTO.timeout( entity.getTimeout() );
        systemDTO.msgType( entity.getMsgType() );
        systemDTO.refService( entity.getRefService() );
        systemDTO.fmDefaultCode( entity.getFmDefaultCode() );
        systemDTO.fmJobCode( entity.getFmJobCode() );
        systemDTO.fmMethod( entity.getFmMethod() );
        systemDTO.stressTest( entity.getStressTest() );

        return systemDTO.build();
    }

    @Override
    public StSystem toStSystem(SystemDTO dto) {
        if ( dto == null ) {
            return null;
        }

        StSystem stSystem = new StSystem();

        stSystem.setSystemId( dto.getSystemId() );
        stSystem.setSystem( dto.getSystem() );
        stSystem.setProtocol( dto.getProtocol() );
        stSystem.setIp( dto.getIp() );
        stSystem.setPort( dto.getPort() );
        stSystem.setStatus( dto.getStatus() );
        stSystem.setEncoding( dto.getEncoding() );
        stSystem.setTimeout( dto.getTimeout() );
        stSystem.setRegDate( dto.getRegDate() );
        stSystem.setMsgType( dto.getMsgType() );
        stSystem.setRefService( dto.getRefService() );
        stSystem.setFmDefaultCode( dto.getFmDefaultCode() );
        stSystem.setFmJobCode( dto.getFmJobCode() );
        stSystem.setFmMethod( dto.getFmMethod() );
        stSystem.setSyncType( dto.getSyncType() );
        stSystem.setHandler( dto.getHandler() );
        stSystem.setStressTest( dto.getStressTest() );

        return stSystem;
    }
}

```

package kr.co.artl.simulator.repository;

import java.util.ArrayList;
import java.util.List;
import java.util.Optional;

import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageImpl;
import org.springframework.data.domain.Pageable;
import org.springframework.data.domain.Sort;
import org.springframework.stereotype.Repository;

import com.querydsl.core.BooleanBuilder;
import com.querydsl.core.types.Order;
import com.querydsl.core.types.OrderSpecifier;
import com.querydsl.core.types.Predicate;
import com.querydsl.core.types.dsl.ComparableExpressionBase;
import com.querydsl.core.types.dsl.Expressions;
import com.querydsl.core.types.dsl.PathBuilder;
import com.querydsl.core.types.dsl.StringExpression;
import com.querydsl.core.types.dsl.StringPath;
import com.querydsl.jpa.impl.JPAQueryFactory;

import kr.co.artl.simulator.domain.QStSystem;
import kr.co.artl.simulator.domain.StSystem;
import kr.co.artl.simulator.dto.SimulMapper;
import kr.co.artl.simulator.dto.SimulSearchDTO;
import kr.co.artl.simulator.dto.SystemDTO;
import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;

/*
 * QueryDSL: fetch() 결과없는경우 빈 리스트 반환 -> optional 처리 없어도됨
 *           fetchOne()은 없을 경우 null 반환 -> optional 처리
 */

@Slf4j
@Repository
@RequiredArgsConstructor //final,notnull 필드 대상 생성자 자동생성 ( @Autowired 생략가능 )
public class SystemRepository {
	
	private final JPAQueryFactory queryFactory;
	private final SimulMapper simulMapper;
	
    public List<SystemDTO> findAllSystems() {
    	try {
    	QStSystem stSystem = QStSystem.stSystem;
    	
    	// QueryDSL로 엔티티 조회
        List<StSystem> systems = queryFactory
                .selectFrom(stSystem)
                .fetch();

        // Entity -> DTO 변환
        return systems.stream()
                .map(simulMapper::toSystemDTO) // MapStruct 매핑
                .toList();
    	}catch (Exception e) {
    		log.error("Error - findAllSystems: {}", e.getMessage(), e);
            return new ArrayList<>();
    	}
    }
   
	public Page<SystemDTO> findSystemsBySearchParam(SimulSearchDTO param,Pageable pageable) {
		try {
	    QStSystem stSystem = QStSystem.stSystem;
	    
	    Predicate condition  = getFieldSearchCondition(stSystem,param);
	    // 정렬 조건이 있는 경우 OrderSpecifier 리스트 생성
	    List<OrderSpecifier<?>> orderSpecifiers = getOrderSpecifiers(pageable.getSort(), stSystem);
	    
	    //쿼리실행
	    List<StSystem> systems = queryFactory
	            .selectFrom(stSystem)
	            .where(condition)
	            .orderBy(orderSpecifiers.toArray(new OrderSpecifier[0])) 
	            .offset(pageable.getOffset())
	            .limit(pageable.getPageSize())
	            .fetch();
	    
	    // Entity -> DTO 변환
        List<SystemDTO> systemDTOs = systems.stream()
                .map(simulMapper::toSystemDTO) // MapStruct 사용
                .toList();
	    
	   
	    //총 데이터수 :: 총 검색 건수, 이를 위해 한번더 쿼리를 날려야해 성능이저하된다면 수정필요
	    long total = queryFactory
	            .select(stSystem.count())
	            .from(stSystem)
	            .where(condition)
	            .fetchOne();
	    
	    return new PageImpl<>(systemDTOs, pageable, total);
	    
		}catch (IllegalArgumentException e) {
            log.error("Error - getOrderSpecifiers / getFieldSearchCondition : {}", e.getMessage(), e);
            return Page.empty();
        } catch (Exception e) {
            log.error("Error - findSystemsBySearchParam: {}", e.getMessage(), e);
            return Page.empty();
        }
	}   
    /*
     * Spring Data JPA Sort를 QueryDSL OrderSpecifier로 변환하는 함수
     * 
     *  반복문 사용이유 : sort에 할당될 기준이 하나더라도 Sort 클래스의 필드 order는 List<Order> orders로 선언되어있다. 
     */
     private List<OrderSpecifier<?>> getOrderSpecifiers(Sort sort, QStSystem qStSystem) throws IllegalArgumentException { 
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
    private Predicate getFieldSearchCondition (QStSystem stSystem,SimulSearchDTO param) throws IllegalArgumentException  {   	
    	/* PathBuilder로 검색 필드를 지정 전,그 값이 실제 존재 컬럼인지 검증은? db의 metadata를 읽어 검증 or 리스트로 하드코딩 */
    	/*  queryDsl에서 type casting (.stringValue()) 적용 시 성능 저하 예상됨 ::: EXPLAIN 쿼리로 성능 확인    */
    	// entity get함수에서 원하는 타입으로 반환하게끔 리팩토링하는 방법도 있음
    	
    	BooleanBuilder builder = new BooleanBuilder();
 
    	//1. status
    	if(param.getStatus() != null) builder.and(stSystem.status.eq(param.getStatus()));
    	
    	//2.searchService.searchStr
    	if (!param.getSearchService().isEmpty() && !param.getSearchStr().isEmpty()) {
    		
    		// 숫자 자료형( port )은 like 사용위해 string으로 cast 필요
    	    if (param.getSearchService().equals("port")) {	          	    	
    	    	StringExpression castedPort = stSystem.port.stringValue(); 
    	    	builder.and(castedPort.like("%" + param.getSearchStr() + "%"));
    	    } else {
    	        PathBuilder<String> entityPath = new PathBuilder<>(String.class, stSystem.getMetadata().getName());
    	        StringPath fieldPath = entityPath.getString(param.getSearchService());
    	        builder.and(fieldPath.containsIgnoreCase(param.getSearchStr())); //대소문자 구분없음
    	    }
    	}
    	
    	//3.date range
    	 if (param.getStartDate() != null && param.getEndDate() != null) {
    		 builder.and(stSystem.regDate .between(param.getStartDate(), param.getEndDate()));
    	 }
    	 System.out.println("Final Query Condition: " + builder);

    	 return builder;
    }
 
    
    public boolean isPortDupl(List<Long> systemIdValList ,List<Integer> portValList) {
    	try {
    	QStSystem stSystem = QStSystem.stSystem;
    	
    	// systemIdValList에 포함되지 않은 시스템들 중 portValList랑 겹치는게 있는지
    	long duplicateCount = queryFactory
    	        .select(stSystem.count())
    	        .from(stSystem)
    	        .where(stSystem.port.in(portValList) 
    	            .and(stSystem.systemId.notIn(systemIdValList))) 
    	        .fetchOne();
    	
    	return duplicateCount > 0;
    	
    	}catch (Exception e) {
    		log.error("Error - isPortDupl: {}", e.getMessage(), e);
    		return true;
    	}
    }
    
    
    public int updateSystemStatus(int _status,Long systemId,int port) {
    	try {
    	QStSystem stSystem = QStSystem.stSystem;
    	long updatedCnt = queryFactory
    	        .update(stSystem)  // 업데이트할 대상 엔티티
    	        .set(stSystem.status, _status)  // 설정할 필드와 값
    	        .where(stSystem.systemId.eq(systemId))  // 조건: system_id
    	        .execute(); 
    	return (int) updatedCnt; 
    	}catch (Exception e) {
    		log.error("Error - updateSystemStatus: {}", e.getMessage(), e);
    		return 0;
    	}
    }
    
  
    public int saveSystem(SystemDTO systemDTO) {    
    	try {
    	 QStSystem qStSystem = QStSystem.stSystem;
    	 // DTO -> Entity 변환
    	 StSystem stSystem = simulMapper.toStSystem(systemDTO);
    	 
    	 return (int)  queryFactory
                 .insert(qStSystem)
                 .set(qStSystem.system, stSystem.getSystem())
                 .set(qStSystem.protocol, stSystem.getProtocol())
                 .set(qStSystem.port, stSystem.getPort())
                 .set(qStSystem.status, stSystem.getStatus())
                 .set(qStSystem.regDate, stSystem.getRegDate())
                 .execute();
    	}catch (Exception e) {
    		log.error("Error - saveSystem: {}", e.getMessage(), e);
    		return 0;
    	}
    }
   
    public int updateSystem(Long systemId,SystemDTO systemDTO) {  
    	try {
   		QStSystem qStSystem = QStSystem.stSystem;

   		// DTO -> Entity 변환
   	    StSystem stSystem = simulMapper.toStSystem(systemDTO);

   	    return (int) queryFactory
   	            .update(qStSystem)
   	            .set(qStSystem.system, stSystem.getSystem())
   	            .set(qStSystem.protocol, stSystem.getProtocol())
   	            .set(qStSystem.port, stSystem.getPort())
   	            .set(qStSystem.encoding, stSystem.getEncoding())
   	            .set(qStSystem.msgType, stSystem.getMsgType())
   	            .set(qStSystem.status, stSystem.getStatus())
   	            .set(qStSystem.timeout, stSystem.getTimeout())
   	            .where(qStSystem.systemId.eq(systemId)) 
   	            .execute();
    	}catch (Exception e) {
    		log.error("Error - updateSystem: {}", e.getMessage(), e);
    		return 0;
    	}
 
	}

    
    public int deleteSystemById(Long systemId) {
    	try {
        QStSystem stSystem = QStSystem.stSystem;
        Optional<SystemDTO> existingSystem = findSystemById(systemId);
        
        if (existingSystem.isPresent()) {
            // 삭제 작업을 실행하고, 삭제된 행의 개수를 반환
        	return (int) queryFactory.delete(stSystem)
        			.where(stSystem.systemId.eq(systemId))
        			.execute();
     
        } else {
            return 0;
        }
        /*
        return findSystemById(systemId)
                .map(system -> {
                    // 데이터가 있을 경우 삭제 실행
                    return (int) queryFactory.delete(stSystem)
                        .where(stSystem.systemId.eq(systemId))
                        .execute();
                })
                .orElse(0);
        */
    	}catch (Exception e) {
    		log.error("Error - deleteSystemById: {}", e.getMessage(), e);
    		return 0;
    	}
    }
	
    
    public Optional<SystemDTO> findSystemById(Long systemId) {
    	try {
        QStSystem stSystem = QStSystem.stSystem;
        
        StSystem system = queryFactory
                .selectFrom(stSystem)
                .where(stSystem.systemId.eq(systemId))
                .fetchOne();
        
        if (system == null) {
            log.warn("stSystem with SystemId {} not found :::", systemId);
        }

        // 엔티티 -> DTO 변환
        return Optional.ofNullable(system)
                .map(simulMapper::toSystemDTO);
    	}catch (Exception e) {
			log.error("Error - findSystemById: {}", e.getMessage(), e);
			return Optional.empty();
    	}
    }   
}
```


```
```
