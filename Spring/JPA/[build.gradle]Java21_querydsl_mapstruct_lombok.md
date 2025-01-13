
> 참고)
> 
> querydsl이랑 mapstruct의 anotationProcessor를 통한 코드생성 pathDirectory가 분리되지않은 issue가 있으나
>
> 크게 문제되지 않아 그대로 진행

```
plugins {
    id 'java'
    id 'org.springframework.boot' version '3.3.5'
    id 'io.spring.dependency-management' version '1.1.6'
    id 'com.ewerk.gradle.plugins.querydsl' version '1.0.10'
}
apply plugin: 'com.ewerk.gradle.plugins.querydsl'

group = 'kr.co.artlab'
version = '0.0.1-SNAPSHOT'

java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(21)
    }
}

configurations {
    compileOnly {
        extendsFrom annotationProcessor
    }
}

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.springframework.boot:spring-boot-starter-quartz'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    
    // Validation 관련 추가 의존성
    implementation 'jakarta.validation:jakarta.validation-api:3.0.2'
    implementation 'org.hibernate.validator:hibernate-validator:8.0.1.Final'
    
    compileOnly 'org.projectlombok:lombok'
    developmentOnly 'org.springframework.boot:spring-boot-devtools'
    runtimeOnly 'org.postgresql:postgresql'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    testRuntimeOnly 'org.junit.platform:junit-platform-launcher'
    
    // QueryDSL 추가
    implementation 'com.querydsl:querydsl-jpa:5.0.0:jakarta' // QueryDSL JPA 의존성
    implementation 'com.querydsl:querydsl-core'
    implementation 'com.querydsl:querydsl-collections'
    
    annotationProcessor 'com.querydsl:querydsl-apt:5.0.0:jakarta' // QueryDSL의 JPAAnnotationProcessor
    annotationProcessor 'jakarta.annotation:jakarta.annotation-api' // java.lang.NoClassDefFoundError (javax.annotation.Generated) 대응 코드
    annotationProcessor 'jakarta.persistence:jakarta.persistence-api' // java.lang.NoClassDefFoundError (javax.annotation.Entity) 대응 코드
 
 	//netty
 	implementation 'io.netty:netty-all:4.1.97.Final' // Netty 전체 라이브러리
 	
 	implementation 'org.mapstruct:mapstruct:1.5.3.Final'
 	annotationProcessor 'org.mapstruct:mapstruct-processor:1.5.3.Final'
	annotationProcessor 'org.projectlombok:lombok-mapstruct-binding:0.2.0'
}

// Querydsl 설정부
def generated = file('src/main/generated') // src/main/generated 경로 지정

configurations {
	querydsl.extendsFrom compileClasspath
}

querydsl {
    library = 'com.querydsl:querydsl-apt:5.0.0:jakarta'
    jpa = true
    querydslSourcesDir = generated
}

sourceSets {
    main {
        java {
            srcDirs += "src/main/generated" // generated 폴더를 소스 경로에 추가
        }
    }
}

tasks.withType(JavaCompile).configureEach {
    options.annotationProcessorPath = configurations.annotationProcessor
    options.generatedSourceOutputDirectory.set(generated) // Q 클래스가 src/main/generated에 생성되도록 설정
}

// compileQuerydsl 태스크가 이미 존재하는지 확인하고, 없을 경우에만 등록
if (!tasks.names.contains("compileQuerydsl")) {
    tasks.register("compileQuerydsl", JavaCompile) {
        source = sourceSets.main.java.srcDirs
        classpath = sourceSets.main.compileClasspath // Main classpath 설정
        options.annotationProcessorPath = configurations.annotationProcessor
        destinationDirectory.set(generated) // 생성 경로 설정
    }
}

compileQuerydsl {
	options.annotationProcessorPath = configurations.querydsl
}

// gradle clean 시에 QClass 디렉토리 삭제
clean {
    delete file("src/main/generated")
}

tasks.named('test') {
    useJUnitPlatform()
}

```
