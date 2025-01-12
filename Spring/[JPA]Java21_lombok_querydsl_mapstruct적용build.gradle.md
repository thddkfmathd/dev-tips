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
    
    a됨)

(inflearn관련질문:댓글참고)[https://www.inflearn.com/community/questions/23530/querydsl-%EC%84%A4%EC%A0%95%EA%B4%80%EB%A0%A8-%EC%A7%88%EB%AC%B8%EB%93%9C%EB%A6%BD%EB%8B%88%EB%8B%A4]
