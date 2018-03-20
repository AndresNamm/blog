---
layout: post
title: Java, Compiling, Maven, other
category: formating
---


# Maven Intro

1. I went through a maven example using this git repo on Ubuntu 16.04  https://github.com/LableOrg/java-maven-junit-helloworld

2. Just clone the repo and go to directory where pom.xml is located
3. To compile
~~~
mvn compile
~~~
4. To run compiled stuff

~~~
mvn exec:java -Dexec.mainClass="com.example.javamavenjunithelloworld.Hello"
mvn exec:java -Dexec.mainClass="com.example.javamavenjunithelloworld.HelloApp"
~~~
5. [MAVEN INTRO - packaging vs compiling vs ..](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html)
6. [Source and Target setup for MAVEN](https://maven.apache.org/plugins/maven-compiler-plugin/examples/set-compiler-source-and-target.html)

# JAVA building running and all other devops stuff

1. [JAVAC with depency](www.codejava.net/java-core/tools/using-javac-command)
2. [JAR file](http://www.javacoffeebreak.com/faq/faq0028.html)
3. [CLASSPATHS IN JAVA](https://en.wikipedia.org/wiki/Classpath_(Java))

## JAR

+ [jar basics](https://docs.oracle.com/javase/tutorial/deployment/jar/basicsindex.html)
+ [JAR OVERALL WIKI DESCRIPTION](https://en.wikipedia.org/wiki/JAR_(file_format))
+ [WHAT IS CONTAINED IN JAR](https://stackoverflow.com/questions/12079230/what-exactly-does-a-jar-file-contain)
+ https://docs.oracle.com/javase/tutorial/deployment/jar/appman.html
+ https://stackoverflow.com/questions/1238145/how-to-run-a-jar-file
+ [HOW TO PASS ARGUMENTS TO JAR](https://stackoverflow.com/questions/456636/how-do-i-pass-parameters-to-a-jar-file-at-the-time-of-execution)
+ JAR-id es dependency management - Basically there are aree 2 alternatives
    1. Fat jars - which already include all dependencys
    2. Not fat jars  -  dependencys should be specified at runtime
+ [how to EXCLUDE logj4 from jar - how everything](https://www.mkyong.com/maven/maven-exclude-log4j-properties-in-jar-file/)


## RUNTIME dependency management (assuming maven)

+ I created a fat jar from project and a non-fat jar (-> THE part which i modify). This allows me to have a very quickly packaged part for my own source code which has all the dependencys easily set out using maven. This solution comes from the fact that simply downloading all the jars using command:
~~~
 mvn dependency:copy-dependencies
~~~
did not work likely due to some library ordering issuexs.
+ thus I this example pom for fat jar creation which I next included in my thin jars. The thin jars were built without adding extra libras like the second code snippet will show
~~~
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>1.3.1</version>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>shade</goal>
                        </goals>
                        <configuration>
                            <transformers>
                                <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                                    <manifestEntries>
                                        <Main-Class>MainClass</Main-Class>
                                    </manifestEntries>
                                </transformer>
                                <!-- https://stackoverflow.com/questions/17265002/hadoop-no-filesystem-for-scheme-file -->
                                <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer"/>
                            </transformers>
                            <filters>
                                <filter>
                                    <artifact>*:*</artifact>
                                    <excludes>
                                        <exclude>META-INF/*.SF</exclude>
                                        <exclude>META-INF/*.DSA</exclude>
                                        <exclude>META-INF/*.RSA</exclude>
                                    </excludes>
                                </filter>
                            </filters>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
    <groupId>antsnamm</groupId>
    <artifactId>spark-getting-started</artifactId>
    <version>1.0-SNAPSHOT</version>
    <properties>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>
    <repositories>
        <repository>
            <id>cloudera-repos</id>
            <name>Cloudera Repos</name>
            <url>https://repository.cloudera.com/artifactory/cloudera-repos/</url>
        </repository>
        <repository>
            <id>maven-hadoop</id>
            <name>Hadoop Releases</name>
            <url>https://repository.cloudera.com/content/repositories/releases/</url>
        </repository>
    </repositories>
    <dependencies>
        <dependency>
            <groupId>org.apache.spark</groupId>
            <artifactId>spark-core_2.11</artifactId>
            <version>2.2.0.cloudera2</version>
        </dependency>
        <dependency>
            <groupId>org.apache.spark</groupId>
            <artifactId>spark-sql_2.11</artifactId>
            <version>2.2.0.cloudera2</version>
        </dependency>
        <dependency>
            <groupId>args4j</groupId>
            <artifactId>args4j</artifactId>
            <version>2.0.29</version>
        </dependency>
    </dependencies>
</project>
~~~
+ thin jar
~~~
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>antsnamm</groupId>
    <artifactId>spark-getting-started</artifactId>
    <version>1.0-SNAPSHOT</version>
    <properties>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>
    <repositories>
        <repository>
            <id>cloudera-repos</id>
            <name>Cloudera Repos</name>
            <url>https://repository.cloudera.com/artifactory/cloudera-repos/</url>
        </repository>
        <repository>
            <id>maven-hadoop</id>
            <name>Hadoop Releases</name>
            <url>https://repository.cloudera.com/content/repositories/releases/</url>
        </repository>
    </repositories>
    <dependencies>
        <dependency>
            <groupId>org.apache.spark</groupId>
            <artifactId>spark-core_2.11</artifactId>
            <version>2.2.0.cloudera2</version>
        </dependency>
        <dependency>
            <groupId>org.apache.spark</groupId>
            <artifactId>spark-sql_2.11</artifactId>
            <version>2.2.0.cloudera2</version>
        </dependency>
        <dependency>
            <groupId>args4j</groupId>
            <artifactId>args4j</artifactId>
            <version>2.0.29</version>
        </dependency>
    </dependencies>
</project>
~~~

+ I also had some issues when creating fat jars using the shaders plugin. Solution is available here as the second [answer](https://stackoverflow.com/questions/999489/invalid-signature-file-when-attempting-to-run-a-jar)
