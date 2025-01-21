### 📌 tomcat error log 보기


> ###### 서버가 죽거나 등등 문제가 있을때 (젠킨스 빌드도 실패하고 키면 404 나오고 있었음)

```
# 해당 tomcat directory/logs/catalina.out
# alias 설정해서 간간히 실행해봐도 좋을것 같다 (다른분은 1000줄로 설정해놓고 사용했다구..) 
tail -n 500 -f catalina.out
```

```
# 톰캣 끄고 켜기
# 해당 tomcat directory/bin/ 의 shutdown.sh, startup.sh 실행
shutdown.sh
startup.sh
```
```
# shutdown 로그
1월 20, 2025 2:32:00 오후 org.apache.catalina.startup.Catalina stopServer
SEVERE: Could not contact [localhost:18005]. Tomcat may not be running.
1월 20, 2025 2:32:00 오후 org.apache.catalina.startup.Catalina stopServer
SEVERE: Catalina.stop:

# startup 로그
[root@localhost bin]# ./startup.sh
Using CATALINA_BASE:   /home/tomcat/apache-tomcat-9.0.89
Using CATALINA_HOME:   /home/tomcat/apache-tomcat-9.0.89
Using CATALINA_TMPDIR: /home/tomcat/apache-tomcat-9.0.89/temp
Using JRE_HOME:        /usr/lib/jvm/jdk-17.0.12-oracle-x64
Using CLASSPATH:       /home/tomcat/apache-tomcat-9.0.89/bin/bootstrap.jar:/home/tomcat/apache-tomcat-9.0.89/bin/tomcat-juli.jar
Using CATALINA_OPTS:
Using CATALINA_PID:    /home/tomcat/apache-tomcat-9.0.89/bin/catalina.pid
Existing PID file found during start.
Removing/clearing stale PID file.
Tomcat started.

# 실행중인 경우
Using CATALINA_BASE:   /home/tomcat/tomcat_mt_40
Using CATALINA_HOME:   /home/tomcat/tomcat_mt_40
Using CATALINA_TMPDIR: /home/tomcat/tomcat_mt_40/temp
Using JRE_HOME:        /usr/lib/jvm/jdk-17.0.12-oracle-x64
Using CLASSPATH:       /home/tomcat/tomcat_mt_40/bin/bootstrap.jar:/home/tomcat/tomcat_mt_40/bin/tomcat-juli.jar
Using CATALINA_PID:    /home/tomcat/tomcat_mt_40/bin/catalina.pid
Existing PID file found during start.
Tomcat appears to still be running with PID 23683. Start aborted.
If the following process is not a Tomcat process, remove the PID file and try again:
UID        PID  PPID  C STIME TTY          TIME CMD
aaa   23683     1  2 09:59 pts/3    00:01:30 /usr/lib/jvm/jdk-17.0.12-oracle-x64/bin/java -Djava.util.logging.conf[aaa@localhost bin]$ ps
  PID TTY          TIME CMD
22601 pts/3    00:00:00 bash
23683 pts/3    00:01:30 java
30528 pts/3    00:00:00 ps

# 그럼 톰캣로그에서 이런에러가 뜨는걸 볼수 있다.
[main] org.apache.catalina.core.StandardServer.await StandardServer.await: create[localhost:18005]:
        java.net.BindException: 주소가 이미 사용 중입니다


# 실행pid 찾기 / port로 pid 찾기
ps -ef | grep tomcat
lsof -i :[PORT] 

# 프로세스종료/ 강제종료
kill [PID]
kill -9 [PID]

# 이것도 지워야 할지도 
rm -f ./catalina.pid
```

<br>


 ######  java.lang.OutOfMemoryError: Compressed class space 이거보니까 그냥 갑자기 메모리 부족해서 서버 죽은듯

```
17-Jan-2025 12:43:03.840 SEVERE [http-nio-18880-exec-2582] org.apache.tomcat.util.modeler.BaseModelMBean.invoke 메소드 [check]을(를) 호출하는 중 예외 발생
        java.lang.OutOfMemoryError: Compressed class space
```
> ###### ✔️ tomcat log
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


######  뒤적거리다가 것도 봤었는데 .. 특정 jdk 버전에서 생기는 버그라는 ... (tomcat 9.0.64 이후에는 이오류메세지가 표시되지않는다고한다 )
######  이 내용으로 문제가 되진 않는거같다. 
[tomcat thread about this issue](https://www.mail-archive.com/users@tomcat.apache.org/msg140646.html)
```
 org.apache.catalina.loader.WebappClassLoaderBase.clearReferencesObjectStreamClassCaches 웹 애플리케이션 [js]을(를) 위해, ObjectStreamClass$Caches로부터 soft references를 폐기하지 못했습니다.
        java.lang.ClassCastException: class java.io.ObjectStreamClass$Caches$1 cannot be cast to class java.util.Map (java.io.ObjectStreamClass$Caches$1 and java.util.Map are in module java.base of loader 'bootstrap')
                at org.apache.catalina.loader.WebappClassLoaderBase.clearCache(WebappClassLoaderBase.java:2244)
                at org.apache.catalina.loader.WebappClassLoaderBase.clearReferencesObjectStreamClassCaches(WebappClassLoaderBase.java:2231)
                at org.apache.catalina.loader.WebappClassLoaderBase.clearReferences(WebappClassLoaderBase.java:1603)
                at org.apache.catalina.loader.WebappClassLoaderBase.stop(WebappClassLoaderBase.java:1548)
                at org.apache.catalina.loader.WebappLoader.stopInternal(WebappLoader.java:452)
                at org.apache.catalina.util.LifecycleBase.stop(LifecycleBase.java:257)
                at org.apache.catalina.core.StandardContext.stopInternal(StandardContext.java:5435)
                at org.apache.catalina.util.LifecycleBase.stop(LifecycleBase.java:257)
                at org.apache.catalina.core.ContainerBase$StopChild.call(ContainerBase.java:1428)
                at org.apache.catalina.core.ContainerBase$StopChild.call(ContainerBase.java:1417)
                at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264)
                at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1136)
                at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:635)
                at java.base/java.lang.Thread.run(Thread.java:842)
```



### 📌 Jenkins console output

> ###### 서버 죽어서 Jenkins build fail 로그


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


### Apache tomcat 제품보안 권고
- Apache Tomcat에서 발생하는 원격 코드 실행 취약점(CVE-2024-50379)

  [[1]](https://lists.apache.org/thread/y6lj6q1xnp822g6ro70tn19sgtjmr80r) ,  [[2]](https://nvd.nist.gov/vuln/detail/CVE-2024-50379) , [[3]](https://tomcat.apache.org/security-11.html) , [[4]](https://tomcat.apache.org/security-10.html) ,  [[5]](https//tomcat.apache.org/security-9.html)


| 취약점    | 제품명           | 영향받는 버전                | 해결 버전  |
|----------|------------------|-----------------------------|-----------|
| CVE-2024-50379 | Apache Tomcat    | 11.0.0-M1 이상 ~ 11.0.1 이하  | 11.0.2 이상 |
|          |                  | 10.1.0-M1 이상 ~ 10.1.33 이하 | 10.1.34 이상 |
|          |                  | 9.0.0-M1 이상 ~ 9.0.97 이하  | 9.0.98 이상 |
