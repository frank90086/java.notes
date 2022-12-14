## To Learn List
### Menu
- [To Learn List](#to-learn-list)
  - [Menu](#menu)
  - [Java](#java)
    - [Maven](#maven)
    - [lombok](#lombok)
    - [Annotation](#annotation)
    - [querydsl](#querydsl)
    - [JPA](#jpa)
    - [AOP](#aop)
  - [DevOps](#devops)
    - [Drone](#drone)
  - [Troubelshooting](#troubelshooting)
    - [IntelliJ IDE Relevant](#intellij-ide-relevant)
    - [Maven Relevant](#maven-relevant)
### Java
#### Maven
> [Official](https://maven.apache.org/index.html)  
> [Tutorial](https://kentyeh.github.io/mavenStartup/)
- Configuration
    - [Compiler plugin](https://maven.apache.org/plugins/maven-compiler-plugin/index.html)
        ```xml
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>${maven.compiler.version}</version>
            <configuration>
                <source>${maven.compiler.source}</source>
                <target>${maven.compiler.target}</target>
                <encoding>${project.build.sourceEncoding}</encoding>
            </configuration>
        </plugin>
        ```
    - [Resources plugin](https://maven.apache.org/plugins/maven-resources-plugin/index.html)
        ```xml
        <build>
            <resources>
                <resource>
                    <directory>${basedir}/src/main/java</directory>
                    <filtering>false</filtering>
                    <includes>
                        <include>org/gwtwidgets/Stream.gwt.xml</include>
                    </includes>
                </resource>
                <resource>
                    <directory>${basedir}/src/main/java</directory>
                    <includes>
                        <include> **/client/*.java </include>
                    </includes>
                </resource>
            </resources>
        </build>
        ```
    - [Jar plugin](https://maven.apache.org/plugins/maven-jar-plugin/)
        ```xml
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-jar-plugin</artifactId>
            <version>${maven.jar.version}</version>
            <configuration>
                <archive>
                    <manifest>
                        <addClasspath>true</addClasspath>
                        <mainClass><!--??????????????????(???main?????????class)--></mainClass>
                    </manifest>
                </archive>
            </configuration>
        </plugin>
        ```
    - [Assembly plugin](https://maven.apache.org/plugins/maven-assembly-plugin/)
        ```xml
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-assembly-plugin</artifactId>
            <version>${maven.assembly.version}</version>
            <configuration>
                <archive>
                    <manifest>
                        <mainClass><!--??????????????????(???main?????????class)--></mainClass>
                    </manifest>
                </archive>
                <descriptorRefs>
                    <descriptorRef>jar-with-dependencies</descriptorRef>
                </descriptorRefs>
            </configuration>
        </plugin>
        ```
    - Associate
        ```xml
        <plugin>
            ...
            <executions>
                <execution><!--??????Goal???????????????-->
                    <phase>test</phase><!--?????????Goals????????? test Phase-->
                    <goals>
                        <goal>exec</goal> <!--????????????goal-->
                    </goals>
                </execution>
            </executions>
        </plugin>
        ```
    - [Surefire plugin](https://maven.apache.org/surefire/maven-surefire-plugin/)
        ```xml
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>${maven.surefire.version}</version>
            <configuration>
            <systemProperties>
                <property>
                    <name>catalina.home</name><!--???????????????????????????-->
                    <value>${project.build.directory}</value><!--?????????????????????????????????-->
                </property>
            </systemProperties>
            </configuration>
        </plugin>
        ```
    - [Scm plugin](https://maven.apache.org/scm/maven-scm-plugin/)
    - [Profile](https://maven.apache.org/guides/introduction/introduction-to-profiles.html)
- [Properties](https://books.sonatype.com/mvnref-book/reference/resource-filtering-sect-properties.html)
    |Property|Describe|
    |:--|:--|
    |${basedir}|????????????pom.xml???????????????|
    |${version}|?????????????????????|
    |${project.build.sourceEncoding}|??????|
    |${project.build.directory}|target??????|
    |${project.build.outputDirectory}|target/classes??????|
    |${project/pom.name}|pom.xml '<name'>??????????????????|
    |${project.build.finalName}|Project???????????????|
    |${env.M2_HOME}|maven????????????|
    |${java.home}|Java????????????|
- Lifecycle
    - [clean](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#clean-lifecycle)
    - [default](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#default-lifecycle)
    - [site](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#site-lifecycle)
- Commonly used commands
    |Command|Describe|
    |:--|:--|
    |mvn clean|?????????????????????????????????${project.build.directory} ??????|
    |mvn test|????????????|
    |mvn package|????????????|
    |mvn install|???Project????????????????????????repository|
    |mvn source:jar javadoc:jar install|??????????????????????????????????????????????????????Project??????????????????repository??????source???????????????repository???????????????IDE????????????Debug?????????Java Api|
    |mvn tomcat:run||
    |mvn source:jar|???source???????????????jar???|
    |mvn javadoc:javadoc|??????java api??????|
    |mvn javadoc:jar|??????java api????????????|
    |mvn exec:exec|	??????Project(???????????????[??????])|
    |mvn versions:display-dependency-updates|??????????????????????????????????????????|
    |mvn versions:use-latest-releases|?????????pom.xml???????????????????????????????????????(??????????????????pom.xml)|
    |mvn versions:display-dependency-updates|???????????????????????????|
    |mvn versions:display-plugin-updates|??????Plugin???????????????|
    |mvn dependency:tree|??????Library??????|
  - Mirror
    - [Settings](https://maven.apache.org/guides/mini/guide-mirror-settings.html)
        > /usr/local/Cellar/maven/3.8.6/libexec/conf/settings.xml  
        > ~/.m2/settings.xml
        ```xml
        <mirrors>
            <mirror>
                <id>maven-default-http-blocker</id>
                <mirrorOf>external:http:*</mirrorOf>
                <name>Pseudo repository to mirror external repositories initially using HTTP.</name>
                <url>http://0.0.0.0/</url>
                <blocked>true</blocked>
            </mirror>
            <mirror>
                <id>nexus</id>
                <mirrorOf>central</mirrorOf>
                <name>nexus</name>
                <url>http://52.69.65.57/repository/backend-maven-all/</url>
                <blocked>false</blocked>
            </mirror>
        </mirrors>
        ```
    - mirrorOf
        ```text
        * = everything
        external:* = everything not on the localhost and not file based.
        repo,repo1 = repo or repo1
        *,!repo1 = everything except repo1
        central = default repository
        ```
#### [lombok]()
> [Commercail](https://projectlombok.org/)
  - [??????????????? Lombok ??????](https://matthung0807.blogspot.com/2020/12/mac-homebrew-intall-maven.html)
#### Annotation
    > [Spring Boot ????????????(???)](https://ithelp.ithome.com.tw/articles/10269631)  
    > [Spring Boot ????????????(???)](https://ithelp.ithome.com.tw/articles/10270418)
- java.lang
    > [Annotation](https://openhome.cc/Gossip/Java/Annotation.html)  
    > [CustomizeAnnotation](https://openhome.cc/Gossip/Java/CustomizeAnnotation.html)
    - @Override
    - @Deprecated
        > Method obsolete
    - @SuppressWarnings
        > Ignore warning
        ```java
        // ?????????????????????unchecked ???deprecation ????????????
        @SuppressWarnnings("unchecked", "deprecation")
        ```
    - @SafeVarag
    - @FunctionalInterface
        ```java
        //???????????????????????????????????????????????????public???????????????
        @FunctionalInterface
        public interface MyFunction {
            public String myApply();
        }

        //??????????????????????????????????????? MyFunction
        public class Hello {
            void say(MyFunction myFunction) {
                System.out.println(myFunction.myApply());
            }
        }

        public class MainTest {
            public static void main(String[] args) {
                Hello hello = new Hello();

                //?????? Hello World
                hello.say(new MyFunction() {
                    @Override
                    public String myApply() {
                        return "Hello World";
                    }
                });

                //????????????????????????lambda???????????????????????? Hello World
                hello.say(() -> "Hello World");
            }
        }
        ```
- java.lang.annotation
    - @Retention
    - @Documented
    - @Target
    - @Inherited
- Controller
    - @Controller
    - @RestController
        > @ResponseBody + @Controller
    - @RequestMapping
      - value
        > Specific path
      - method
        > Specific http methods
      - consumes
        > Specific request Content-Type
      - produces
        > Specific response Content-Type
      - headers
        > Specific request headers
      - params
        > Specific request parameters
    - @RequestBody
    - @ResponseBody
    - @ModelAttribute
        ```java
        public String modelBinding(@ModelAttribute Member member) { /* ??? */ }
        ```
    - @SessionAttribute
        ```java
        public String sessionBinding(@SessionAttribute("SESSION_NAME") Member member) { /* ??? */ }
        ```
    - @RequestParam
        ```java
        public String paramBinding(
            @RequestParam(value = "param1") String param1,
            @RequestParam(value = "param2", required = false) String param2) { /* ??? */ }
        ```
    - @PathVariable
        ```java
        @RequestMapping(value = "/path/{PATH_PARAM}", method = RequestMethod.GET)
        public String pathBinding(@PathVariable("PATH_PARAM") String path_param) { /* ??? */ }
        ```
- Bean
    - @Service
    - @Repository
    - @Component
        > Exclude @Service, @Repository
    - @ComponentScan
    - @Configuration
    - @Bean
    - @Resource
        > DI by name  
    - @Autowired
        > DI by type
    - @Qualifier
        > Specific when mutiple type declared simultaneously
    - @Value
  - Exception
    - @ControllerAdvice
        >  @RestControllerAdvice
    - @ExceptionHandler
        ```java
        // ???????????????RestController ??????
        // @ControllerAdvice(annotations = RestController.class)
        // ??????xxx.controller Package ?????????
        // @ControllerAdvice("xxx.controller")
        // ??????Controller1 ???Controller2 ??????
        // @ControllerAdvice(assignableTypes = {Controller1.class, Controller2.class})
        @ControllerAdvice
        public class ExceptionHandler {
            // ??????NullPointerException ??????
            @ExceptionHandler(NullPointerException.class)
            public String NullPointerExceptionHandler() {
                // ???
            }

            // ??????ArithmeticException ???ArrayIndexOutOfBoundsException ??????
            @ExceptionHandler({ArithmeticException.class, ArrayIndexOutOfBoundsException.class})
            public String NullPointerExceptionHandler() {
                // ???
            }
        }
        ```
- Schedule
    - @EnableScheduling
    - @Scheduled
        > Providers: cron, fixedDelay, fixedRate...etc.
        ```java
        // ????????????12 ?????????
        @Scheduled(cron = "0 0 12 * * ?")
        public void task1() { /* ??? */ }
        ```
- AOP
    - @Aspec
    - @Pointcut
    - @Before
    - @After
    - @AfterReturning
    - @Around
    - @AfterThrowing
- Other
    - @EnableAsync
    - @SpringBootTest
    - @Test
    - @Transactional
        ```java
        @Transactional(rollbackFor = Exception.class)
        public void method() { /* ??? */}
        ```
#### querydsl
> [Official](http://querydsl.com/)
- Maven config
    ```xml
    <properties>
        <querydsl.version>4.2.1</querydsl.version>
    </properties>
    <build>
        <plugin>
            <groupId>com.mysema.maven</groupId>
            <artifactId>apt-maven-plugin</artifactId>
            <version>1.1.3</version>
            <executions>
                <execution>
                    <goals>
                        <goal>process</goal>
                    </goals>
                    <configuration>
                        <outputDirectory>target/generated-sources/java</outputDirectory>
                        <processor>com.querydsl.apt.jpa.JPAAnnotationProcessor</processor>
                    </configuration>
                </execution>
            </executions>
            <dependencies>
                <dependency>
                    <groupId>com.querydsl</groupId>
                    <artifactId>querydsl-apt</artifactId>
                    <version>${querydsl.version}</version>
                </dependency>
            </dependencies>
        </plugin>
    </build>
    <dependencies>
        <dependency>
            <groupId>com.querydsl</groupId>
            <artifactId>querydsl-jpa</artifactId>
            <version>${querydsl.version}</version>
        </dependency>
    </dependencies>
    ```
- build
    ```cmd
    mvn clean compile
    ```
#### JPA
> [Official](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#preface)
- Configuration
    - Maven
        ```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        ```
    - application
        ```yml
        spring:
            datasource:
                url: jdbc:mysql://127.0.0.1:3306/{mydb}?useSSL=false&autoReconnect=true
                username: root
                password: root
            jpa:
                show-sql: true # ??????SQL??????
                properties:
                    hibernate:
                        format_sql: true # format SQL??????
                    hbm2ddl:
                        auto: create-drop # (????????????????????????) create | create-drop | update | validate
        logging:
            level:
                org:
                    hibernate: # ??????SQL???????????????????????????
                        SQL: DEBUG
                        type:
                            descriptor:
                                sql:
                                    BasicBinder: TRACE
        ```
- Auditing
    - Application class add @EnableJpaAuditing
    - Entity class add @EntityListeners(AuditingEntityListener.class)
    - Implement AuditorAware<String>
        ```java
        import net.funpodium.service.bo.util.InheritableThreadLocalUtil;
        import org.springframework.context.annotation.Configuration;
        import org.springframework.data.domain.AuditorAware;

        import java.util.Optional;

        @Configuration
        public class AuditorAwareImpl implements AuditorAware<String> {

            @Override
            public Optional<String> getCurrentAuditor() {
                /* Implement get current auditor */
            }
        }
        ```
    - Entity class add properties
      - Created Date
        ```java
        @CreatedDate
        private Timestamp CreatedDate;
        ```
      - Created By
        ```java
        @CreatedBy
        private String createdBy;
        ```
      - Last Modified Date
        ```java
        @LastModifiedDate
        private Timestamp LastModifiedDate;
        ```
      - Last Modified By
        ```java
        @LastModifiedBy
        private String LastModifiedBy;
        ```
- Properties
    - CascadeType
        |Options|Describe|
        |:--|:--|
        |CascadeType.PERSIST|???????????????????????? ??????????????????|
        |CascadeType.MERGE|???????????????????????? ??????????????????????????????|
        |CascadeType.REMOVE|???????????????????????? ??????????????????|
        |CascadeType.REFRESH|???????????????????????? ??????????????????|
        |CascadeType.ALL|??????????????????????????????????????????|
    - FetchType
      - Definition
        |Options|Describe|
        |:--|:--|
        |FetchType.EARGE|????????????|
        |FetchType.LAZY|????????????|
      - Default
        |Relationship|Default|
        |:--|:--|
        |@Basic|FetchType.EARGE|
        |@OneToOne|FetchType.EARGE|
        |@ManyToOne|FetchType.EARGE|
        |@OneToMany|FetchType.LAZY|
        |@ManyToMany|FetchType.LAZY|
#### AOP
  > [????????????????????????](https://chikuwa-tech-study.blogspot.com/2021/06/spring-boot-aop-introduction.html)

- Configuration
    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-aop</artifactId>
    </dependency>
    ```
- Declare
    ```java
    @Aspect
    public class LogAspect {

        /*
        Return rule: *
        Package rule(include sub package): xxx.service..*
        Package rule(exclude sub package): xxx.service.*
        Method rule: .*
        Parameter rule: (..) | (String,..)
        */
        @Pointcut("execution(* xxx.service..*.*(..))")
        public void pointcut() {
        }

        @Before("pointcut()")
        public void before(JoinPoint joinPoint) {
            // Before method invoke
        }

        @After("pointcut()")
        public void after(JoinPoint joinPoint) {
            // After method invoke
        }

        @AfterReturning(pointcut = "pointcut()", returning = "result")
        public void afterReturning(JoinPoint joinPoint, Object result) {
            // After method invoke and return
            // If exception occured before return, then this section won't excuted
        }

        @Around("pointcut()")
        public Object around(ProceedingJoinPoint joinPoint) throws Throwable {

            // Logic before method invoke
            Object result = joinPoint.proceed();
            // Logic after method invoke

            return result;
        }

        @AfterThrowing(pointcut = "pointcut()", throwing = "throwable")
        public void afterThorwing(JoinPoint joinPoint, Throwable throwable) {
            // Error exception occured
        }

    }
    ```

### DevOps
#### [Drone](https://www.drone.io/)
  - [??????????????? DevOps ??? ???????????????????????? Drone CI ????????????](https://medium.com/starbugs/%E5%BE%9E%E9%9B%B6%E9%96%8B%E5%A7%8B%E5%AD%B8-devops-%E9%82%A3%E5%B0%B1%E9%81%B8%E6%93%87%E6%9C%80%E7%B0%A1%E5%96%AE%E7%9A%84-drone-ci-%E9%96%8B%E5%A7%8B%E5%90%A7-931126671139)

### Troubelshooting
#### IntelliJ IDE Relevant
- [Unable to use Intellij with a generated sources folder](https://stackoverflow.com/questions/5170620/unable-to-use-intellij-with-a-generated-sources-folder)
#### Maven Relevant
- Maven Build Failure
    - DependencyResolutionException
      - [Getting "Blocked mirror for repositories" maven error even after adding mirrors](https://stackoverflow.com/questions/67833372/getting-blocked-mirror-for-repositories-maven-error-even-after-adding-mirrors)
      - [Blocked mirror for repositories error when building from source using Maven 3.8.1 or later](https://backstage.forgerock.com/knowledge/kb/article/a15127846)
      - [maven????????????Blocked mirror for repositories??????](https://blog.csdn.net/qq_41980563/article/details/122061818)
      - [maven - mirrorOf ?????????????????????????????????????????????????????????](https://blog.nowcoder.net/n/1166815f837244caa74bf9f62c43d04c)
    - LifecycleExecutionException
      - [Maven????????????: zip file is empty](https://blog.csdn.net/jeikerxiao/article/details/109304775)