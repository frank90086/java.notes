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
    - [Syntax](#syntax)
  - [Repository](#repository)
    - [Nexus](#nexus)
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
                        <mainClass><!--完整類別名稱(有main方法的class)--></mainClass>
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
                        <mainClass><!--完整類別名稱(有main方法的class)--></mainClass>
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
                <execution><!--設定Goal的執行方式-->
                    <phase>test</phase><!--將以下Goals關聯到 test Phase-->
                    <goals>
                        <goal>exec</goal> <!--要設定的goal-->
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
                    <name>catalina.home</name><!--要設定系統變數名稱-->
                    <value>${project.build.directory}</value><!--把這個變數指到輸出目錄-->
                </property>
            </systemProperties>
            </configuration>
        </plugin>
        ```
    - [Scm plugin](https://maven.apache.org/scm/maven-scm-plugin/)
    - [Profile](https://maven.apache.org/guides/introduction/introduction-to-profiles.html)
- [Properties](https://books.sonatype.com/mvnref-book/reference/resource-filtering-sect-properties.html)
    | Property                         | Describe                     |
    | :------------------------------- | :--------------------------- |
    | ${basedir}                       | 表示包含pom.xml的目錄路徑    |
    | ${version}                       | 程式的版本編號               |
    | ${project.build.sourceEncoding}  | 編碼                         |
    | ${project.build.directory}       | target目錄                   |
    | ${project.build.outputDirectory} | target/classes目錄           |
    | ${project/pom.name}              | pom.xml '<name'>所指定的名稱 |
    | ${project.build.finalName}       | Project的打包名稱            |
    | ${env.M2_HOME}                   | maven安裝目錄                |
    | ${java.home}                     | Java安裝目錄                 |
- Lifecycle
    - [clean](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#clean-lifecycle)
    - [default](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#default-lifecycle)
    - [site](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#site-lifecycle)
- Commonly used commands
    | Command                                                  | Describe                                                                                                                                |
    | :------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------- |
    | mvn clean                                                | 進行清理作業，通常是將${project.build.directory} 砍掉                                                                                   |
    | mvn test                                                 | 測試程式                                                                                                                                |
    | mvn package                                              | 打包程式                                                                                                                                |
    | mvn install                                              | 把Project打包後，放進本地repository                                                                                                     |
    | mvn source:jar javadoc:jar install                       | 把源碼打包，產生文件並打包連同打包的Project一起放進本地repository，把source與文件放進repository，是為了讓IDE工具方便Debug與查看Java Api |
    | mvn tomcat:run                                           |                                                                                                                                         |
    | mvn source:jar                                           | 把source打包成一個jar檔                                                                                                                 |
    | mvn javadoc:javadoc                                      | 產生java api檔案                                                                                                                        |
    | mvn javadoc:jar                                          | 產生java api打包檔案                                                                                                                    |
    | mvn exec:exec                                            | 執行Project(需進行一些[設定])                                                                                                           |
    | mvn versions:display-dependency-updates                  | 檢查相依函式庫的版本更新狀況                                                                                                            |
    | mvn versions:use-latest-releases                         | 直接將pom.xml內的版本更新到最近一版釋出(會備分舊版的pom.xml)                                                                            |
    | mvn versions:display-dependency-updates                  | 檢查函式庫更新狀況                                                                                                                      |
    | mvn versions:display-plugin-updates                      | 檢查Plugin的更新狀況                                                                                                                    |
    | mvn dependency:tree                                      | 查看Library版本                                                                                                                         |
    | mvn dependency:resolve                                   | 重新下載dependency                                                                                                                      |
    | mvn dependency:get -Dartifact=groupId:artifactId:version | 下載指定的dependency                                                                                                                    |
  - Mirror
    - [Settings](https://maven.apache.org/guides/mini/guide-mirror-settings.html)
        > /usr/local/Cellar/maven/3.9.0/libexec/conf/settings.xml  
        > ~/.m2/settings.xml
        ```xml
        <servers>
            <server>
                <id>nexus</id>
                <username>{username}</username>
                <password>{password}</password>
            </server>
            <server>
                <id>localhost</id>
                <username>{username}</username>
                <password>{password}</password>
            </server>
        </servers>

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
                <name>Remote Nexus</name>
                <url>http://52.69.65.57/repository/backend-maven-all/</url>
                <blocked>false</blocked>
            </mirror>
            <mirror>
                <id>localhost</id>
                <mirrorOf>*</mirrorOf>
                <name>Localhost Nexus</name>
                <url>http://127.0.0.1:8081/repository/maven-public/</url>
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
    - pom.xml
        ```xml
        <distributionManagement>
            <repository>
                <id>localhost</id>
                <name>Releases Repository</name>
                <url>http://localhost:8081/repository/maven-releases</url>
            </repository>
            <snapshotRepository>
                <id>localhost</id>
                <name>Snapshot Repository</name>
                <url>http://localhost:8081/repository/maven-snapshots</url>
            </snapshotRepository>
        </distributionManagement>
        ```
#### [lombok]()
> [Commercail](https://projectlombok.org/)
  - [五分鐘學會 Lombok 用法](https://kucw.github.io/blog/2020/3/java-lombok/)
#### Annotation
    > [Spring Boot 常用註釋(上)](https://ithelp.ithome.com.tw/articles/10269631)  
    > [Spring Boot 常用註釋(下)](https://ithelp.ithome.com.tw/articles/10270418)
- java.lang
    > [Annotation](https://openhome.cc/Gossip/Java/Annotation.html)  
    > [CustomizeAnnotation](https://openhome.cc/Gossip/Java/CustomizeAnnotation.html)
    - @Override
    - @Deprecated
        > Method obsolete
    - @SuppressWarnings
        > Ignore warning
        ```java
        // 告訴編譯器忽略unchecked 和deprecation 警告訊息
        @SuppressWarnnings("unchecked", "deprecation")
        ```
    - @SafeVarag
    - @FunctionalInterface
        ```java
        //自定義一個函數式接口，並且只有一個public的抽象方法
        @FunctionalInterface
        public interface MyFunction {
            public String myApply();
        }

        //定義一個類，讓這個類去使用 MyFunction
        public class Hello {
            void say(MyFunction myFunction) {
                System.out.println(myFunction.myApply());
            }
        }

        public class MainTest {
            public static void main(String[] args) {
                Hello hello = new Hello();

                //輸出 Hello World
                hello.say(new MyFunction() {
                    @Override
                    public String myApply() {
                        return "Hello World";
                    }
                });

                //可以轉換為以下的lambda表達式，同樣輸出 Hello World
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
        public String modelBinding(@ModelAttribute Member member) { /* 略 */ }
        ```
    - @SessionAttribute
        ```java
        public String sessionBinding(@SessionAttribute("SESSION_NAME") Member member) { /* 略 */ }
        ```
    - @RequestParam
        ```java
        public String paramBinding(
            @RequestParam(value = "param1") String param1,
            @RequestParam(value = "param2", required = false) String param2) { /* 略 */ }
        ```
    - @PathVariable
        ```java
        @RequestMapping(value = "/path/{PATH_PARAM}", method = RequestMethod.GET)
        public String pathBinding(@PathVariable("PATH_PARAM") String path_param) { /* 略 */ }
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
- Cache
    > [介紹](https://blog.csdn.net/dl962454/article/details/106480438)  
    > key, cacheNames|value, condition, allEntries, beforeInvocation, cacheManager
    | 屬性名稱    | 描述                        | 示例                 |
    | :---------- | :-------------------------- | :------------------- |
    | parameters  | 參數名稱                    | #user.id             |
    | page        | 頁數                        | #p0                  |
    | methodName  | 當前方法名稱                | #root.methodName     |
    | method      | 當前方法                    | #root.method.name    |
    | target      | 當前被調用的對象            | #root.target         |
    | targetClass | 當前被調用的對象的class     | #root.targetClass    |
    | args        | 當前方法參數組成的數組      | #root.args[0]        |
    | caches      | 當前被調用的方法使用的Cache | #root.caches[0].name |
    - @Cacheable
        > 執行前先檢查，若無則執行並緩存
    - @CachePut
        > 直接執行並緩存
    - @CacheEvict
        > 執行結束後清除緩存，若 beforeInvocation: true 則執行前先清除, 若 allEntries: true 則清除所有緩存
    - @Caching
        > 複合式
        ```java
        @Caching(
            cacheable = {
                @Cacheable(cacheNames = "example", key = "#str")
            },
            put = {
                @Cacheable(cacheNames = "example", key = "#result.id")
                @Cacheable(cacheNames = "example", key = "#result.email")
            },
            evict = {
                @Cacheable(cacheNames = "example", allEntries = false, beforeInvocation = true)
            }
        )
        public Employee findByName(string str){
            return employeeRepository.findByName(str);
        }
        ```
- Exception
    - @ControllerAdvice
        >  @RestControllerAdvice
    - @ExceptionHandler
        ```java
        // 限定註釋為RestController 生效
        // @ControllerAdvice(annotations = RestController.class)
        // 限定xxx.controller Package 下生效
        // @ControllerAdvice("xxx.controller")
        // 限定Controller1 與Controller2 生效
        // @ControllerAdvice(assignableTypes = {Controller1.class, Controller2.class})
        @ControllerAdvice
        public class ExceptionHandler {
            // 處理NullPointerException 例外
            @ExceptionHandler(NullPointerException.class)
            public String NullPointerExceptionHandler() {
                // 略
            }

            // 處理ArithmeticException 和ArrayIndexOutOfBoundsException 例外
            @ExceptionHandler({ArithmeticException.class, ArrayIndexOutOfBoundsException.class})
            public String NullPointerExceptionHandler() {
                // 略
            }
        }
        ```
- Schedule
    - @EnableScheduling
    - @Scheduled
        > Providers: cron, fixedDelay, fixedRate...etc.
        ```java
        // 每天中午12 點執行
        @Scheduled(cron = "0 0 12 * * ?")
        public void task1() { /* 略 */ }
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
        public void method() { /* 略 */}
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
                show-sql: true # 顯示SQL語法
                properties:
                    hibernate:
                        format_sql: true # format SQL語法
                    hbm2ddl:
                        auto: create-drop # (此參數請小心使用) create | create-drop | update | validate
        logging:
            level:
                org:
                    hibernate: # 顯示SQL語法的查詢條件的值
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
        | Options             | Describe                              |
        | :------------------ | :------------------------------------ |
        | CascadeType.PERSIST | 在儲存時一併儲存 被參考的物件         |
        | CascadeType.MERGE   | 在合併修改時一併 合併修改被參考的物件 |
        | CascadeType.REMOVE  | 在移除時一併移除 被參考的物件         |
        | CascadeType.REFRESH | 在更新時一併更新 被參考的物件         |
        | CascadeType.ALL     | 一併對被參考物件作出對應動作          |
    - FetchType
      - Definition
        | Options         | Describe |
        | :-------------- | :------- |
        | FetchType.EARGE | 立即載入 |
        | FetchType.LAZY  | 延遲載入 |
      - Default
        | Relationship | Default         |
        | :----------- | :-------------- |
        | @Basic       | FetchType.EARGE |
        | @OneToOne    | FetchType.EARGE |
        | @ManyToOne   | FetchType.EARGE |
        | @OneToMany   | FetchType.LAZY  |
        | @ManyToMany  | FetchType.LAZY  |
- Repository
    > [JPA Repositories](https://docs.spring.io/spring-data/jpa/docs/1.6.0.RELEASE/reference/html/jpa.repositories.html)

    - Keywords
        | Keyword           | Sample                                                  | JPQL snippet                                                   |
        | :---------------- | :------------------------------------------------------ | :------------------------------------------------------------- |
        | And               | findByLastnameAndFirstname                              | … where x.lastname = ?1 and x.firstname = ?2                   |
        | Or                | findByLastnameOrFirstname                               | … where x.lastname = ?1 or x.firstname = ?2                    |
        | Is,Equals         | findByFirstname,findByFirstnameIs,findByFirstnameEquals | … where x.firstname = 1?                                       |
        | Between           | findByStartDateBetween                                  | … where x.startDate between 1? and ?2                          |
        | LessThan          | findByAgeLessThan                                       | … where x.age < ?1                                             |
        | LessThanEqual     | findByAgeLessThanEqual                                  | … where x.age <= ?1                                            |
        | GreaterThan       | findByAgeGreaterThan                                    | … where x.age > ?1                                             |
        | GreaterThanEqual  | findByAgeGreaterThanEqual                               | … where x.age >= ?1                                            |
        | After             | findByStartDateAfter                                    | … where x.startDate > ?1                                       |
        | Before            | findByStartDateBefore                                   | … where x.startDate < ?1                                       |
        | IsNull            | findByAgeIsNull                                         | … where x.age is null                                          |
        | IsNotNull,NotNull | findByAge(Is)NotNull                                    | … where x.age not null                                         |
        | Like              | findByFirstnameLike                                     | … where x.firstname like ?1                                    |
        | NotLike           | findByFirstnameNotLike                                  | … where x.firstname not like ?1                                |
        | StartingWith      | findByFirstnameStartingWith                             | … where x.firstname like ?1 (parameter bound with appended %)  |
        | EndingWith        | findByFirstnameEndingWith                               | … where x.firstname like ?1 (parameter bound with prepended %) |
        | Containing        | findByFirstnameContaining                               | … where x.firstname like ?1 (parameter bound wrapped in %)     |
        | OrderBy           | findByAgeOrderByLastnameDesc                            | … where x.age = ?1 order by x.lastname desc                    |
        | Not               | findByLastnameNot                                       | … where x.lastname <> ?1                                       |
        | In                | findByAgeIn(Collection<Age> ages)                       | … where x.age in ?1                                            |
        | NotIn             | findByAgeNotIn(Collection<Age> age)                     | … where x.age not in ?1                                        |
        | True              | findByActiveTrue()                                      | … where x.active = true                                        |
        | False             | findByActiveFalse()                                     | … where x.active = false                                       |
        | IgnoreCase        | findByFirstnameIgnoreCase                               | … where UPPER(x.firstame) = UPPER(?1)                          |
#### AOP
  > [切面導向程式設計](https://chikuwa-tech-study.blogspot.com/2021/06/spring-boot-aop-introduction.html)

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
#### Syntax
- ConcurrentHashMap
    > after jdk 8
    - KeySet()
      - Can't usage add(), remove(), ...etc.
        ```java
        class ConcurrentSet {
            public static void main (String[] args) {

                // Creating a concurrent hash map
                ConcurrentHashMap<String,Integer> demo_map = new ConcurrentHashMap<>();

                demo_map.put("Delftstack",10);
                demo_map.put("Jack",20);
                demo_map.put("John", 30);
                demo_map.put("Mike",40);
                demo_map.put("Michelle", 50);

                // use keySet() to create a set from the concurrent hashmap
                Set keyset_conc_set = demo_map.keySet();

                System.out.println("The concurrent set using keySet() function is : " + keyset_conc_set);
            }
        }

        // Output:
        // The concurrent set using keySet() function is : [Michelle, Mike, Delftstack, John, Jack]
        ```
    - newKeySet()
      - avaliable usage add(), remove(), ...etc.
      - avaliable usage set operations
        ```java
        class ConcurrentSet {
            public static void main (String[] args) {
                // Create a concurrent set using concorrentHashMap and newkeyset()
                Set <String> newKeySet_conc_set = ConcurrentHashMap.newKeySet();

                newKeySet_conc_set.add("Mike");
                newKeySet_conc_set.add("Michelle");
                newKeySet_conc_set.add("John");
                newKeySet_conc_set.add("Jack");

                // Print the concurrent set
                System.out.println("The concurrent set before adding the element: " + newKeySet_conc_set);

                // Add new element
                newKeySet_conc_set.add("Delftstack");

                // Show the change
                System.out.println("The concurrent set after adding the element: " + newKeySet_conc_set);

                // Check any element using contains
                if(newKeySet_conc_set.contains("Delftstack")){
                    System.out.println("Delftstack is a member of the set");
                }
                else{
                    System.out.println("Delftstack is not a member of the set");
                }
                // Remove any element from the concurrent set
                newKeySet_conc_set.remove("Delftstack");
                System.out.println("The concurrent set after removing the element:  " + newKeySet_conc_set);
            }
        }

        // Output:
        // The concurrent set before adding the element: [Michelle, Mike, John, Jack]
        // The concurrent set after adding the element: [Michelle, Mike, Delftstack, John, Jack]
        // Delftstack is a member of the set
        // The concurrent set after removing the element:  [Michelle, Mike, John, Jack]
        ```
- Sha256WithRsa
    - class
    ```java
        package tutorial.security;

        import java.security.Key;
        import java.security.KeyPair;
        import java.security.KeyPairGenerator;
        import java.security.SecureRandom;
        import org.springframework.beans.factory.annotation.Value;
        import org.springframework.stereotype.Component;

        import javax.crypto.BadPaddingException;
        import javax.crypto.Cipher;
        import javax.crypto.IllegalBlockSizeException;
        import javax.crypto.NoSuchPaddingException;
        import java.nio.charset.StandardCharsets;
        import java.security.InvalidKeyException;
        import java.security.KeyFactory;
        import java.security.NoSuchAlgorithmException;
        import java.security.PrivateKey;
        import java.security.PublicKey;
        import java.security.Signature;
        import java.security.SignatureException;
        import java.security.spec.InvalidKeySpecException;
        import java.security.spec.PKCS8EncodedKeySpec;
        import java.security.spec.X509EncodedKeySpec;
        import java.util.Base64;

        @Component
        public class RSAUtil {

            private static SecureRandom random = new SecureRandom();

            public String generateSignature_Sha1withRSA(String plaintext, String privateKeyStr)
                    throws NoSuchAlgorithmException, InvalidKeyException, SignatureException, InvalidKeySpecException {
                PrivateKey privateKey = getPrivateKey(privateKeyStr);
                return generateSignature("SHA1withRSA", plaintext, privateKey);
            }

            public String generateSignature_Sha256withRSA(String plaintext, String privateKeyStr)
                throws NoSuchAlgorithmException, InvalidKeyException, SignatureException, InvalidKeySpecException {
                PrivateKey privateKey = getPrivateKey(privateKeyStr);
                return generateSignature("SHA256withRSA", plaintext, privateKey);
            }

            private String generateSignature(String algorithms, String plaintext, PrivateKey privateKey)
                    throws NoSuchAlgorithmException, InvalidKeyException, SignatureException, InvalidKeySpecException {
                Signature signature = Signature.getInstance(algorithms);
                signature.initSign(privateKey);
                signature.update(plaintext.getBytes(StandardCharsets.UTF_8));
                byte[] signData = signature.sign();

                return Base64.getEncoder().encodeToString(signData);
            }

            public boolean verifySignature_Sha1withRSA(String signature, String originText, String publicKeyStr)
                    throws InvalidKeySpecException, NoSuchAlgorithmException, InvalidKeyException, SignatureException {
                PublicKey publicKey = getPublicKey(publicKeyStr);
                return verifySignature("SHA1withRSA", signature, originText, publicKey);
            }

            public boolean verifySignature_Sha256withRSA(String signature, String originText, String publicKeyStr)
                throws InvalidKeySpecException, NoSuchAlgorithmException, InvalidKeyException, SignatureException {
                PublicKey publicKey = getPublicKey(publicKeyStr);
                return verifySignature("SHA256withRSA", signature, originText, publicKey);
            }

            private boolean verifySignature(String algorithms, String signature, String originText, PublicKey publicKey)
                    throws InvalidKeySpecException, NoSuchAlgorithmException, InvalidKeyException, SignatureException  {
                Signature publicSignature = Signature.getInstance(algorithms);
                publicSignature.initVerify(publicKey);
                publicSignature.update(originText.getBytes(StandardCharsets.UTF_8));

                byte[] signatureBytes = Base64.getDecoder().decode(signature);
                return publicSignature.verify(signatureBytes);
            }

            public KeyPair generateKeyPair()
                    throws NoSuchAlgorithmException {
                return generateKeyPair(new SecureRandom());
            }

            public KeyPair generateKeyPair(String seed)
                    throws NoSuchAlgorithmException {
                random.setSeed(seed.getBytes());
                return generateKeyPair(random);
            }

            private KeyPair generateKeyPair(SecureRandom random)
                    throws NoSuchAlgorithmException {
                KeyPairGenerator keyPairGenerator = KeyPairGenerator.getInstance("RSA");
                keyPairGenerator.initialize(2048, random);
                return keyPairGenerator.generateKeyPair();
            }

            private PublicKey getPublicKey(String publicKeyStr) throws NoSuchAlgorithmException, InvalidKeySpecException {
                byte[] keyBytes = Base64.getDecoder().decode(publicKeyStr.getBytes(StandardCharsets.UTF_8));
                X509EncodedKeySpec spec = new X509EncodedKeySpec(keyBytes);
                KeyFactory keyFactory = KeyFactory.getInstance("RSA");

                return keyFactory.generatePublic(spec);
            }

            private PrivateKey getPrivateKey(String privateKeyStr) throws NoSuchAlgorithmException, InvalidKeySpecException {
                byte[] pkcs8EncodedBytes = Base64.getDecoder().decode(privateKeyStr.getBytes(StandardCharsets.UTF_8));
                PKCS8EncodedKeySpec keySpec = new PKCS8EncodedKeySpec(pkcs8EncodedBytes);
                KeyFactory kf = KeyFactory.getInstance("RSA");

                return kf.generatePrivate(keySpec);
            }

            public String convertToBase64(Key key) {
                byte[] keyBytes = key.getEncoded();
                return Base64.getEncoder().encodeToString(keyBytes);
            }

            public String convertToPEM(Key key) {
                byte[] keyBytes = key.getEncoded();
                String base64PublicKey = Base64.getEncoder().encodeToString(keyBytes);

                return splitPEMString(base64PublicKey, 64);
            }

            private static String splitPEMString(String input, int splitLength) {
                StringBuilder output = new StringBuilder();
                int index = 0;
                while (index < input.length()) {
                    output.append(input.substring(index, Math.min(index + splitLength, input.length())));
                    output.append("\n");
                    index += splitLength;
                }
                return output.toString();
            }
        }
    ```
    - used
    ```java
        KeyPair kp = rsaUtil.generateKeyPair(preorderReq.getMerchantCode());
        PrivateKey privateKey = kp.getPrivate();
        PublicKey publicKey = kp.getPublic();

        System.out.println("RSA Public Key: " + publicKey);
        System.out.println("RSA Private Key: " + privateKey);

        String test = JSONUtil.stringify(preorderReq);

        String sign = rsaUtil.generateSignature_Sha256withRSA(test, rsaUtil.convertToBase64(privateKey));
        boolean verify = rsaUtil.verifySignature_Sha256withRSA(sign, test, rsaUtil.convertToBase64(publicKey));

        System.out.println("Sign: " + sign);
        System.out.println("Verify: " + verify);
    ```
- [parallelStream](https://blog.csdn.net/u011001723/article/details/52794455)
- [CountDownLatch](https://www.cnblogs.com/Andya/p/12925634.html)
- [iterator](https://kucw.github.io/blog/2018/12/java-iterator-and-listiterator/)
### Repository
#### Nexus
  - [Installation In Docker](https://hub.docker.com/r/sonatype/nexus3/)
    > default account - admin:{./vol/data/admin.password}
    ```yml
    version: "3.7"
    services:
        nexus:
            restart: always
            image: sonatype/nexus3
            container_name: nexus
            ports:
            - 6005:8081
            - 8082:8082
            volumes:
            - ./vol/data:/nexus-data
    networks:
        default:
            external:
                name: dev
    ```
  - Useful Commands
    ```cmd
    # docker cli
    docker login -u admin -p localhost:8082
    docker tag {image:version} localhost:8082/{image:version}
    docker push localhost:8082/{image:version}

    # mvnw
    ## [build image](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin#build-your-image)
    ./mvnw -DactiveProfiles=docker -DsendCredentialsOverHttp=true compile jib:build

    ## [build to Docker daemon](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin#build-to-docker-daemon)
    ./mvnw -DactiveProfiles=docker compile jib:dockerBuild

    ## [build an image tarball](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin#build-an-image-tarball)
    ./mvnw -DactiveProfiles=docker compile jib:buildTar; \docker load < target/jib-image.tar;
    ```
### DevOps
#### [Drone](https://www.drone.io/)
  - [從零開始學 DevOps — 那就選擇最簡單的 Drone CI 開始吧！](https://medium.com/starbugs/%E5%BE%9E%E9%9B%B6%E9%96%8B%E5%A7%8B%E5%AD%B8-devops-%E9%82%A3%E5%B0%B1%E9%81%B8%E6%93%87%E6%9C%80%E7%B0%A1%E5%96%AE%E7%9A%84-drone-ci-%E9%96%8B%E5%A7%8B%E5%90%A7-931126671139)

### Troubelshooting
#### IntelliJ IDE Relevant
- [Unable to use Intellij with a generated sources folder](https://stackoverflow.com/questions/5170620/unable-to-use-intellij-with-a-generated-sources-folder)
#### Maven Relevant
- Maven Build Failure
    - DependencyResolutionException
      - [Getting "Blocked mirror for repositories" maven error even after adding mirrors](https://stackoverflow.com/questions/67833372/getting-blocked-mirror-for-repositories-maven-error-even-after-adding-mirrors)
      - [Blocked mirror for repositories error when building from source using Maven 3.8.1 or later](https://backstage.forgerock.com/knowledge/kb/article/a15127846)
      - [maven编译报错Blocked mirror for repositories解决](https://blog.csdn.net/qq_41980563/article/details/122061818)
      - [maven - mirrorOf 的坑、多镜像切换（避免一切无厘头报错）](https://blog.nowcoder.net/n/1166815f837244caa74bf9f62c43d04c)
    - LifecycleExecutionException
      - [Maven编译失败: zip file is empty](https://blog.csdn.net/jeikerxiao/article/details/109304775)