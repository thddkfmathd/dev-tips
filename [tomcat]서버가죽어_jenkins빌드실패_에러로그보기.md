### 업데이트한 소스문제 아닌거같은데 젠킨스 빌드가 실패한 건에 대하여

> ###### 📌 서버가 죽어서 안된거였음 : 서버가 죽어서 빌드가 실패한게 아니고 배포가 실패해서 404 뜨던거였음

<br>

######  이전 빌드실패에선 java copile error가 원인,
######  ide에서 특정 클래스가 인식이 됐다 안됐다 하던게 빌드때 컴파일에러로 빌드에 실패 했던 적이있다     
######  (그때는 에러로그도 쉬웠는데)

<br>

#####  Jenkins Console Output
> ###### 😂 이때 알아채야 했었다 (지금보니... 왜삽질했지?..)
```
# 대충 어떤 repo에 어떤 브랜치 어쩌고 저쩌고 정보
[WARNING] 
[WARNING] Some problems were encountered while building the effective model for kr.co.company:solution:war:4.0
[WARNING] 'build.pluginManagement.plugins.plugin.(groupId:artifactId)' must be unique but found duplicate declaration of plugin org.apache.maven.plugins:maven-war-plugin @ line 479, column 13
[WARNING] 
[WARNING] It is highly recommended to fix these problems because they threaten the stability of your build.
[WARNING] 
[WARNING] For this reason, future Maven versions might no longer support building such malformed projects.
[WARNING]

# pom.xml로 buid 하겠다 어쩌구 저쩌구
# 기존 target에들어있던 컴파일된 파일 지운다 어쩌구 저쩌구

[INFO] Compiling 284 source files with javac [debug target 17] to target/classes

# deprecated된 내용 warning
# jvm의 -Xlint option 같은내용

# 대충 war관련내용

# Redeploying , Undeploying  INFO
# 여기서부터 ERROR Log
ERROR: Build step failed with exception
org.codehaus.cargo.container.ContainerException: Failed to undeploy [/-----/target/solution.war]
at org.codehaus.cargo.container.tomcat.internal.AbstractTomcatManagerDeployer.undeploy(AbstractTomcatManagerDeployer.java:132)
	at org.codehaus.cargo.container.tomcat.internal.AbstractTomcatManagerDeployer.redeploy(AbstractTomcatManagerDeployer.java:165)
	at hudson.plugins.deploy.CargoContainerAdapter.deploy(CargoContainerAdapter.java:81)
	at hudson.plugins.deploy.CargoContainerAdapter$DeployCallable.invoke(CargoContainerAdapter.java:167)
	at hudson.plugins.deploy.CargoContainerAdapter$DeployCallable.invoke(CargoContainerAdapter.java:136)
	at hudson.FilePath.act(FilePath.java:1232)
	at hudson.FilePath.act(FilePath.java:1215)
	at hudson.plugins.deploy.CargoContainerAdapter.redeployFile(CargoContainerAdapter.java:133)
	at hudson.plugins.deploy.PasswordProtectedAdapterCargo.redeployFile(PasswordProtectedAdapterCargo.java:95)
	at hudson.plugins.deploy.DeployPublisher.perform(DeployPublisher.java:113)
	at jenkins.tasks.SimpleBuildStep.perform(SimpleBuildStep.java:123)
	at hudson.tasks.BuildStepCompatibilityLayer.perform(BuildStepCompatibilityLayer.java:80)
	at hudson.tasks.BuildStepMonitor$3.perform(BuildStepMonitor.java:47)
	at hudson.model.AbstractBuild$AbstractBuildExecution.perform(AbstractBuild.java:818)
	at hudson.model.AbstractBuild$AbstractBuildExecution.performAllBuildSteps(AbstractBuild.java:767)
	at hudson.model.Build$BuildExecution.post2(Build.java:179)
	at hudson.model.AbstractBuild$AbstractBuildExecution.post(AbstractBuild.java:711)
	at hudson.model.Run.execute(Run.java:1917)
	at hudson.model.FreeStyleBuild.run(FreeStyleBuild.java:44)
	at hudson.model.ResourceController.execute(ResourceController.java:101)
	at hudson.model.Executor.run(Executor.java:442)
Caused by: org.codehaus.cargo.container.tomcat.internal.TomcatManagerException: The Tomcat Manager responded "실패 - 컨텍스트 [&#47;]이(가) server.xml에 정의되어 있어, 배치를 제거할 수 없습니다.
" instead of the expected "OK" message
	at org.codehaus.cargo.container.tomcat.internal.TomcatManager.invoke(TomcatManager.java:721)
	at org.codehaus.cargo.container.tomcat.internal.TomcatManager.invoke(TomcatManager.java:501)
	at org.codehaus.cargo.container.tomcat.internal.TomcatManager.undeploy(TomcatManager.java:441)
	at org.codehaus.cargo.container.tomcat.Tomcat7xRemoteDeployer.performUndeploy(Tomcat7xRemoteDeployer.java:58)
	at org.codehaus.cargo.container.tomcat.internal.AbstractTomcatManagerDeployer.undeploy(AbstractTomcatManagerDeployer.java:122)
	... 20 more
org.codehaus.cargo.container.tomcat.internal.TomcatManagerException: The Tomcat Manager responded "실패 - 컨텍스트 [&#47;]이(가) server.xml에 정의되어 있어, 배치를 제거할 수 없습니다.
" instead of the expected "OK" message
	at org.codehaus.cargo.container.tomcat.internal.TomcatManager.invoke(TomcatManager.java:721)
	at org.codehaus.cargo.container.tomcat.internal.TomcatManager.invoke(TomcatManager.java:501)
	at org.codehaus.cargo.container.tomcat.internal.TomcatManager.undeploy(TomcatManager.java:441)
	at org.codehaus.cargo.container.tomcat.Tomcat7xRemoteDeployer.performUndeploy(Tomcat7xRemoteDeployer.java:58)
	at org.codehaus.cargo.container.tomcat.internal.AbstractTomcatManagerDeployer.undeploy(AbstractTomcatManagerDeployer.java:122)
	at org.codehaus.cargo.container.tomcat.internal.AbstractTomcatManagerDeployer.redeploy(AbstractTomcatManagerDeployer.java:165)
	at hudson.plugins.deploy.CargoContainerAdapter.deploy(CargoContainerAdapter.java:81)
	at hudson.plugins.deploy.CargoContainerAdapter$DeployCallable.invoke(CargoContainerAdapter.java:167)
	at hudson.plugins.deploy.CargoContainerAdapter$DeployCallable.invoke(CargoContainerAdapter.java:136)
	at hudson.FilePath.act(FilePath.java:1232)
	at hudson.FilePath.act(FilePath.java:1215)
	at hudson.plugins.deploy.CargoContainerAdapter.redeployFile(CargoContainerAdapter.java:133)
	at hudson.plugins.deploy.PasswordProtectedAdapterCargo.redeployFile(PasswordProtectedAdapterCargo.java:95)
	at hudson.plugins.deploy.DeployPublisher.perform(DeployPublisher.java:113)
	at jenkins.tasks.SimpleBuildStep.perform(SimpleBuildStep.java:123)
	at hudson.tasks.BuildStepCompatibilityLayer.perform(BuildStepCompatibilityLayer.java:80)
	at hudson.tasks.BuildStepMonitor$3.perform(BuildStepMonitor.java:47)
	at hudson.model.AbstractBuild$AbstractBuildExecution.perform(AbstractBuild.java:818)
	at hudson.model.AbstractBuild$AbstractBuildExecution.performAllBuildSteps(AbstractBuild.java:767)
	at hudson.model.Build$BuildExecution.post2(Build.java:179)
	at hudson.model.AbstractBuild$AbstractBuildExecution.post(AbstractBuild.java:711)
	at hudson.model.Run.execute(Run.java:1917)
	at hudson.model.FreeStyleBuild.run(FreeStyleBuild.java:44)
	at hudson.model.ResourceController.execute(ResourceController.java:101)
	at hudson.model.Executor.run(Executor.java:442)
Build step 'Deploy war/ear to a container' marked build as failure
Finished: FAILURE
```


> ###### 일단 문제의 원인이 jenkins console에서 보이지이 않아서 tomcat log를 봐보기로 했다.

```
# 해당 tomcat의 
```


 ######  java.lang.OutOfMemoryError: Compressed class space 이거보니까 그냥 갑자기 메모리 부족해서 서버 죽은듯

```
17-Jan-2025 12:43:03.840 SEVERE [http-nio-18880-exec-2582] org.apache.tomcat.util.modeler.BaseModelMBean.invoke 메소드 [check]을(를) 호출하는 중 예외 발생
        java.lang.OutOfMemoryError: Compressed class space
```

```
2025-01-17 12:42:00 DEBUG [testExecute.selectScheduleExecuteList] - <==      Total: 0
2025-01-17 12:42:00 DEBUG [org.mybatis.spring.transaction.SpringManagedTransaction] - Committing JDBC Connection [jdbc:postgresql://110.45.211.172:5432/MT40_MASTER?allowMultiQueries=true, UserName=postgres, PostgreSQL JDBC Driver]
2025-01-17 12:42:00 DEBUG [org.mybatis.spring.SqlSessionUtils] - Closing non transactional SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@10bee501]
2025-01-17 12:42:26 DEBUG [ko.co.artlab.common.schedule.QuartzScheduleController] - 스케줄러가 종료되었습니다.
2025-01-17 12:42:26 DEBUG [kr.co.artlab.simul.engine.SimulServerInitializer] - Simul Server context Destroyed
2025-01-17 12:42:34 DEBUG [org.mybatis.spring.SqlSessionUtils] - Creating a new SqlSession
2025-01-17 12:42:34 DEBUG [org.mybatis.spring.SqlSessionUtils] - SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@57239fdc] was not registered for synchronization because synchronization is not active
2025-01-17 12:42:34 DEBUG [org.mybatis.spring.SqlSessionUtils] - Closing non transactional SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@57239fdc]
17-Jan-2025 12:42:34.124 WARNING [http-nio-18880-exec-2578] org.apache.catalina.loader.WebappClassLoaderBase.clearReferencesJdbc 웹 애플리케이션 [ROOT]이(가) JDBC 드라이버 [net.sf.log4jdbc.sql.jdbcapi.DriverSpy]을( 를) 등록했지만, 웹 애플리케이션이 중지될 때, 해당 JDBC 드라이버의 등록을 제거하지 못했습니다. 메모리 누수를 방지하기 위하여, 등록을 강제로 제거했습니다.
17-Jan-2025 12:42:34.125 WARNING [http-nio-18880-exec-2578] org.apache.catalina.loader.WebappClassLoaderBase.clearReferencesJdbc 웹 애플리케이션 [ROOT]이(가) JDBC 드라이버 [oracle.jdbc.OracleDriver]을(를) 등록했지만, 웹 애플리케이션이 중지될 때, 해당 JDBC 드라이버의 등록을 제거하지 못했습니다. 메모리 누수를 방지하기 위하여, 등록을 강제로 제거했습니다.
17-Jan-2025 12:42:34.125 WARNING [http-nio-18880-exec-2578] org.apache.catalina.loader.WebappClassLoaderBase.clearReferencesJdbc 웹 애플리케이션 [ROOT]이(가) JDBC 드라이버 [org.hsqldb.jdbc.JDBCDriver]을(를) 등록했지만, 웹 애플리케이션이 중지될 때, 해당 JDBC 드라이버의 등록을 제거하지 못했습니다. 메모리 누수를 방지하기 위하여, 등록을 강제로 제거했습니다.
17-Jan-2025 12:42:34.125 WARNING [http-nio-18880-exec-2578] org.apache.catalina.loader.WebappClassLoaderBase.clearReferencesJdbc 웹 애플리케이션 [ROOT]이(가) JDBC 드라이버 [org.postgresql.Driver]을(를) 등록했지만,  웹 애플리케이션이 중지될 때, 해당 JDBC 드라이버의 등록을 제거하지 못했습니다. 메모리 누수를 방지하기 위하여, 등록을 강제로 제거했습니다.
17-Jan-2025 12:42:34.128 WARNING [http-nio-18880-exec-2578] org.apache.catalina.loader.WebappClassLoaderBase.clearReferencesThreads 웹 애플리케이션 [ROOT]이(가) [Catalina-utility-1](이)라는 이름의 쓰레드를 시작시킨 것으로 보이지만, 해당 쓰레드를 중지시키지 못했습니다. 이는 메모리 누수를 유발할 가능성이 큽니다. 해당 쓰레드의 스택 트레이스:
 org.apache.catalina.core.StandardContext.backgroundProcess(StandardContext.java:4796)
 org.apache.catalina.core.ContainerBase$ContainerBackgroundProcessor.processChildren(ContainerBase.java:1172)
 org.apache.catalina.core.ContainerBase$ContainerBackgroundProcessor.processChildren(ContainerBase.java:1176)
 org.apache.catalina.core.ContainerBase$ContainerBackgroundProcessor.processChildren(ContainerBase.java:1176)
 org.apache.catalina.core.ContainerBase$ContainerBackgroundProcessor.run(ContainerBase.java:1154)
 java.base@17.0.12/java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:539)
 java.base@17.0.12/java.util.concurrent.FutureTask.runAndReset(FutureTask.java:305)
 java.base@17.0.12/java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:305)
 java.base@17.0.12/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1136)
 java.base@17.0.12/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:635)
 org.apache.tomcat.util.threads.TaskThread$WrappingRunnable.run(TaskThread.java:63)
 java.base@17.0.12/java.lang.Thread.run(Thread.java:842)
17-Jan-2025 12:42:34.129 WARNING [http-nio-18880-exec-2578] org.apache.catalina.loader.WebappClassLoaderBase.clearReferencesThreads 웹 애플리케이션 [ROOT]이(가) [nioEventLoopGroup-5-1](이)라는 이름의 쓰레드를 시작시킨 것으로 보이지만, 해당 쓰레드를 중지시키지 못했습니다. 이는 메모리 누수를 유발할 가능성이 큽니다. 해당 쓰레드의 스택 트레이스:
 java.base@17.0.12/sun.nio.ch.EPoll.wait(Native Method)
 java.base@17.0.12/sun.nio.ch.EPollSelectorImpl.doSelect(EPollSelectorImpl.java:118)
 java.base@17.0.12/sun.nio.ch.SelectorImpl.lockAndDoSelect(SelectorImpl.java:129)
 java.base@17.0.12/sun.nio.ch.SelectorImpl.select(SelectorImpl.java:146)
 io.netty.channel.nio.SelectedSelectionKeySetSelector.select(SelectedSelectionKeySetSelector.java:68)
 io.netty.channel.nio.NioEventLoop.select(NioEventLoop.java:879)
 io.netty.channel.nio.NioEventLoop.run(NioEventLoop.java:526)
 io.netty.util.concurrent.SingleThreadEventExecutor$4.run(SingleThreadEventExecutor.java:997)
 io.netty.util.internal.ThreadExecutorMap$2.run(ThreadExecutorMap.java:74)
 io.netty.util.concurrent.FastThreadLocalRunnable.run(FastThreadLocalRunnable.java:30)
 java.base@17.0.12/java.lang.Thread.run(Thread.java:842)
17-Jan-2025 12:42:34.593 INFO [http-nio-18880-exec-2578] org.apache.catalina.startup.HostConfig.undeploy 컨텍스트 []의 배치를 제거합니다.
17-Jan-2025 12:42:34.942 INFO [http-nio-18880-exec-2582] org.apache.catalina.startup.HostConfig.deployWAR 웹 애플리케이션 아카이브 [/home/tomcat/apache-tomcat-9.0.89/webapps/ROOT.war]을(를) 배치합니다.
17-Jan-2025 12:42:39.361 INFO [http-nio-18880-exec-2582] org.apache.jasper.servlet.TldScanner.scanJars 적어도 하나의 JAR가 TLD들을 찾기 위해 스캔되었으나 아무 것도 찾지 못했습니다. 스캔했으나 TLD가 없는 JAR들의 전체 목록을 보시려면, 로그 레벨을 디버그 레벨로 설정하십시오. 스캔 과정에서 불필요한 JAR들을 건너뛰면, 시스템 시작 시간과 JSP 컴파일 시간을 단축시킬 수 있습니다.
17-Jan-2025 12:43:03.840 SEVERE [http-nio-18880-exec-2582] org.apache.tomcat.util.modeler.BaseModelMBean.invoke 메소드 [check]을(를) 호출하는 중 예외 발생
        java.lang.OutOfMemoryError: Compressed class space
17-Jan-2025 19:35:25.688 INFO [http-nio-18880-exec-2594] org.apache.coyote.http11.Http11Processor.service HTTP 요청 헤더를 파싱하는 중 오류 발생
 비고: HTTP 요청 파싱 오류들이 더 발생하는 경우 DEBUG 레벨 로그로 기록될 것입니다.
        java.lang.IllegalArgumentException: 요청 타겟에서 유효하지 않은 문자가 발견되었습니다. 유효한 문자들은 RFC 7230과 RFC 3986에 정의되어 있습니다.
                at org.apache.coyote.http11.Http11InputBuffer.parseRequestLine(Http11InputBuffer.java:482)
                at org.apache.coyote.http11.Http11Processor.service(Http11Processor.java:261)
                at org.apache.coyote.AbstractProcessorLight.process(AbstractProcessorLight.java:63)
                at org.apache.coyote.AbstractProtocol$ConnectionHandler.process(AbstractProtocol.java:928)
                at org.apache.tomcat.util.net.NioEndpoint$SocketProcessor.doRun(NioEndpoint.java:1791)
                at org.apache.tomcat.util.net.SocketProcessorBase.run(SocketProcessorBase.java:52)
                at org.apache.tomcat.util.threads.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1190)
                at org.apache.tomcat.util.threads.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:659)
                at org.apache.tomcat.util.threads.TaskThread$WrappingRunnable.run(TaskThread.java:63)
                at java.base/java.lang.Thread.run(Thread.java:842)
19-Jan-2025 03:02:39.636 INFO [http-nio-18880-exec-2587] org.apache.coyote.http11.Http11Processor.service HTTP 요청 헤더를 파싱하는 중 오류 발생
 비고: HTTP 요청 파싱 오류들이 더 발생하는 경우 DEBUG 레벨 로그로 기록될 것입니다.
        java.lang.IllegalArgumentException: 메소드 이름에 유효하지 않은 문자가 발견되었습니다. HTTP 메소드 이름은 유효한 토큰이어야 합니다.
                at org.apache.coyote.http11.Http11InputBuffer.parseRequestLine(Http11InputBuffer.java:407)
                at org.apache.coyote.http11.Http11Processor.service(Http11Processor.java:261)
                at org.apache.coyote.AbstractProcessorLight.process(AbstractProcessorLight.java:63)
                at org.apache.coyote.AbstractProtocol$ConnectionHandler.process(AbstractProtocol.java:928)
                at org.apache.tomcat.util.net.NioEndpoint$SocketProcessor.doRun(NioEndpoint.java:1791)
                at org.apache.tomcat.util.net.SocketProcessorBase.run(SocketProcessorBase.java:52)
                at org.apache.tomcat.util.threads.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1190)
                at org.apache.tomcat.util.threads.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:659)
                at org.apache.tomcat.util.threads.TaskThread$WrappingRunnable.run(TaskThread.java:63)
                at java.base/java.lang.Thread.run(Thread.java:842)
20-Jan-2025 03:11:16.525 INFO [http-nio-18880-exec-2594] org.apache.coyote.http11.Http11Processor.service HTTP 요청 헤더를 파싱하는 중 오류 발생
 비고: HTTP 요청 파싱 오류들이 더 발생하는 경우 DEBUG 레벨 로그로 기록될 것입니다.
        java.lang.IllegalArgumentException: 메소드 이름에 유효하지 않은 문자가 발견되었습니다. HTTP 메소드 이름은 유효한 토큰이어야 합니다.
                at org.apache.coyote.http11.Http11InputBuffer.parseRequestLine(Http11InputBuffer.java:407)
                at org.apache.coyote.http11.Http11Processor.service(Http11Processor.java:261)
                at org.apache.coyote.AbstractProcessorLight.process(AbstractProcessorLight.java:63)
                at org.apache.coyote.AbstractProtocol$ConnectionHandler.process(AbstractProtocol.java:928)
                at org.apache.tomcat.util.net.NioEndpoint$SocketProcessor.doRun(NioEndpoint.java:1791)
                at org.apache.tomcat.util.net.SocketProcessorBase.run(SocketProcessorBase.java:52)
                at org.apache.tomcat.util.threads.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1190)
                at org.apache.tomcat.util.threads.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:659)
                at org.apache.tomcat.util.threads.TaskThread$WrappingRunnable.run(TaskThread.java:63)
                at java.base/java.lang.Thread.run(Thread.java:842)
NOTE: Picked up JDK_JAVA_OPTIONS:  --add-opens=java.base/java.lang=ALL-UNNAMED --add-opens=java.base/java.io=ALL-UNNAMED --add-opens=java.base/java.util=ALL-UNNAMED --add-opens=java.base/java.util.concurrent=ALL-UNNAMED --add-opens=java.rmi/sun.rmi.transport=ALL-UNNAMED
```
