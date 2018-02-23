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

# JAVA compile

1. [JAVAC with depency](www.codejava.net/java-core/tools/using-javac-command)
2. [jar file](http://www.javacoffeebreak.com/faq/faq0028.html)
