## To Learn List
### Java
- [Maven](https://maven.apache.org/index.html)
    > [教學](https://kentyeh.github.io/mavenStartup/)
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
    |Property|Describe|
    |:--|:--|
    |${basedir}|表示包含pom.xml的目錄路徑|
    |${version}|程式的版本編號|
    |${project.build.sourceEncoding}|編碼|
    |${project.build.directory}|target目錄|
    |${project.build.outputDirectory}|target/classes目錄|
    |${project/pom.name}|pom.xml '<name'>所指定的名稱|
    |${project.build.finalName}|Project的打包名稱|
    |${env.M2_HOME}|maven安裝目錄|
    |${java.home}|Java安裝目錄|
  - Lifecycle
    - [clean](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#clean-lifecycle)
    - [default](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#default-lifecycle)
    - [site](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#site-lifecycle)
  - Commonly used commands
    |Command|Describe|
    |:--|:--|
    |mvn clean|進行清理作業，通常是將${project.build.directory} 砍掉|
    |mvn test|測試程式|
    |mvn package|打包程式|
    |mvn install|把Project打包後，放進本地repository|
    |mvn source:jar javadoc:jar install|把源碼打包，產生文件並打包連同打包的Project一起放進本地repository，把source與文件放進repository，是為了讓IDE工具方便Debug與查看Java Api|
    |mvn tomcat:run||
    |mvn source:jar|把source打包成一個jar檔|
    |mvn javadoc:javadoc|產生java api檔案|
    |mvn javadoc:jar|產生java api打包檔案|
    |mvn exec:exec|	執行Project(需進行一些[設定])|
    |mvn versions:display-dependency-updates|檢查相依函式庫的版本更新狀況|
    |mvn versions:use-latest-releases|直接將pom.xml內的版本更新到最近一版釋出(會備分舊版的pom.xml)|
    |mvn versions:display-dependency-updates|檢查函式庫更新狀況|
    |mvn versions:display-plugin-updates|檢查Plugin的更新狀況|
    |mvn dependency:tree|查看Library版本|
- [lombok](https://projectlombok.org/)
  - [五分鐘學會 Lombok 用法](https://matthung0807.blogspot.com/2020/12/mac-homebrew-intall-maven.html)
- Annotation
  - java.lang
    > https://openhome.cc/Gossip/Java/Annotation.html  
    > https://openhome.cc/Gossip/Java/CustomizeAnnotation.html
    - @Override
    - @Deprecated
    - @SuppressWarnings
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
- [querydsl](http://querydsl.com/)
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
- [JPA](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#preface)
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
        |Options|Describe|
        |:--|:--|
        |CascadeType.PERSIST|在儲存時一併儲存 被參考的物件|
        |CascadeType.MERGE|在合併修改時一併 合併修改被參考的物件|
        |CascadeType.REMOVE|在移除時一併移除 被參考的物件|
        |CascadeType.REFRESH|在更新時一併更新 被參考的物件|
        |CascadeType.ALL|一併對被參考物件作出對應動作|
    - FetchType
      - Definition
        |Options|Describe|
        |:--|:--|
        |FetchType.EARGE|立即載入|
        |FetchType.LAZY|延遲載入|
      - Default
        |Relationship|Default|
        |:--|:--|
        |@Basic|FetchType.EARGE|
        |@OneToOne|FetchType.EARGE|
        |@ManyToOne|FetchType.EARGE|
        |@OneToMany|FetchType.LAZY|
        |@ManyToMany|FetchType.LAZY|

### DevOps
- [Drone](https://www.drone.io/)
  - [從零開始學 DevOps — 那就選擇最簡單的 Drone CI 開始吧！](https://medium.com/starbugs/%E5%BE%9E%E9%9B%B6%E9%96%8B%E5%A7%8B%E5%AD%B8-devops-%E9%82%A3%E5%B0%B1%E9%81%B8%E6%93%87%E6%9C%80%E7%B0%A1%E5%96%AE%E7%9A%84-drone-ci-%E9%96%8B%E5%A7%8B%E5%90%A7-931126671139)

### Troubelshooting
- IntelliJ IDE
  - [Unable to use Intellij with a generated sources folder](https://stackoverflow.com/questions/5170620/unable-to-use-intellij-with-a-generated-sources-folder)