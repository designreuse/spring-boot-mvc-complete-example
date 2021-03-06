## Spring Boot Maven Java 1.8  (Spring MVC jsp and tiles, Spring Data Rest, Jenkins 2 ready to use with full support to Maven and Gradle)

[![Coverage Status](https://coveralls.io/repos/cristianprofile/spring-boot-mvc-complete-example/badge.svg)](https://coveralls.io/r/cristianprofile/spring-boot-mvc-complete-example)  [![Build Status](https://travis-ci.org/cristianprofile/spring-boot-mvc-complete-example.svg?branch=develop)](https://travis-ci.org/cristianprofile/spring-boot-mvc-complete-example)
[![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/cristianprofile/spring-boot-mvc-complete-example?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

If you don`t have  gradle or maven in your computer you can use gradlew to be able to run the project

```
gradle wrapper (run Gradle wrapper)

After this operation you can run every Gradle command of this guide with 

./gradlew xxxxxtask (Unix, Linux)
gradlew.bat XXXtask  (Windows)

Example

./gradlew clean compile (Unix, Linux)
gradlew.bat clean compile (Windows)

```


You can build this project with Maven or Gradle. Here you have several snippets about how to use them: 

```
mvn clean install (install jar to your local m2 )
mvn spring-boot:run (run web app modules)
gradle buid (build modules)
gradle bootRun (run web app modules)
```


**Important!!!!!If you use Maven First of all you have to install with "mvn install" modules "mylab-parent-pom" and after "mvn install" of module "mylab-core".**


## Spring Boot mvc web with tiles app

run spring-boot-mvc-web-example module with maven  "mvn spring-boot:run" (if you want to use Gradle run "gradle bootRun" into spring-boot-mvc-web-example ) and access to http://localhost:9090/pizza
and user: "admin@ole.com" and password "admin@ole.com".You can create other users with ROLE_USER at add user left menu
option.

- Spring boot MVC with Spring Security Access
- I18n
- Responsive Bootstrap css witn Tiles 3
- Password encoding with Bcrypt  [BCRYPT password encoding](http://www.baeldung.com/spring-security-registration-password-encoding-bcrypt "BCRYPT password encoding") 
- Unit Testing and Integration Testing with spring-boot-starter-test dependency (all dependecies are transitive like mockito junit etc...)


## Rest service layer with Spring Boot Mvc

If you want to access to Rest Service with Spring boot module "spring-boot-mvc" first run mvn spring-boot:run (if you want to use Gradle run "gradle bootRun"
 into spring-boot-mvc-rest folder ):

```
- http://localhost:9090/base (get list of all bases)
- http://localhost:9090/base/1 (get base info with id=1)
- http://localhost:9090/base/1 (delete base info with id=1)
- http://localhost:9090/base (post create new base sending json info. Example "name":"rolling pizza" )
- http://localhost:9090/base (update update existing base sending json info. Example {"name":"rolling pizza 2","id":1})
```

When you run Spring boot app Spring actuator add features to monitore your services:

```
- (get) http://localhost:9091/manage/metrics (Spring Boot Actuator includes a metrics service with 
“gauge” and “counter” support. A “gauge” records a single value; and a “counter” records a delta 
(an increment or decrement). Metrics for all HTTP requests are automatically 
recorded, so if you hit the metrics endpoint should see a sensible response.)
- (get) http://localhost:9091/manage/health (you can check if your app is available)
- (get) http://localhost:9091/manage/mappings (list of your app HTTP endpoints)
- (post) http://localhost:9091/manage/shutdown (list of your app HTTP endpoints)
```

![Spring Actuator values](/images/Spring_Actuator_EndPoints.png?raw=true "Spring Actuator values")


More info about Spring Actuator at: [Spring Actuator](https://github.com/spring-projects/spring-boot/tree/master/spring-boot-actuator "Spring Actuator")


##  Rest service layer with Spring Data Rest

Spring Data REST builds on top of Spring Data repositories, analyzes your application’s domain model and exposes hypermedia-driven HTTP resources for aggregates contained in the model.

If you want to access to Rest Service with Spring boot module "spring-boot-data-rest" first run mvn spring-boot:run (if you want to use Gradle run "gradle bootRun"
 into spring-boot-data-rest):

```
- http://localhost:9090/api/bases to get all bases (get list of all bases)
- http://localhost:9090/api/bases (post create new base sending json info. Example "name":"rolling pizza" )
- http://localhost:9090/api/browser/index.html#/api to access to your rest api HAL Browser to be served up when you visit your application’s root URI in a browser. 
```

![Spring Actuator values](/images/SpringDataRestHalBrowser.png?raw=true "Spring Actuator values")


More info about Spring Data Rest at: [Spring Data Rest](http://projects.spring.io/spring-data-rest/ "Spring Data Rest") 

## Jenkins 2 support with jenkins file

Jenkins 2 automatic multibranch plugin mode with JenkinsFile file in main directory. More interesting information about new Jenkins 2 Pipeline script configuration at:

-  [DZONE refcard jenkins pipeline](https://dzone.com/refcardz/continuous-delivery-with-jenkins-workflow "DZONE refcard jenkins pipeline")
-  [Github examples](https://github.com/jenkinsci/pipeline-examples "Github examples")  

Docker integration in feature  branch called: docker_container_jenkins

-  [Docker container feature branch](https://github.com/cristianprofile/spring-boot-mvc-complete-example/blob/feature/docker_container_jenkins/Jenkinsfile "Run IC in a Docker container")  

![Pipeline plugin](/images/git-flow.png?raw=true "Pipeline plugin")


## ELK SUPPORT IN WEB APP MODULE(Elasticsearch/Kibana/Logstash)

First of all you need and ELK installed in you machine. The easiest way is to use docker image (https://hub.docker.com/r/nshou/elasticsearch-kibana/) :

-  Start your container with Kibana and ElasticSearch.
-  Edit spring-boot-mvc-web/src/main/resources/logstash/logstash-spring-boot-json.conf with your elasticsearch port
-  Download losgstash and run logstash command from web app initial folder "./logstash -vf spring-boot-mvc-web/src/main/resources/logstash/logstash-spring-boot-json.conf --debug"
-  Run Spring boot web app: gradle bootRun or mvn spring-boot:run. Now your app will create 2 logs files in tmp folder:  spring-boot-mvc.log and spring-boot-mvc.log.json
-  Logstash is monitoring .json file and create new document in elasticsearch for each new line
-  Go to you kibana url:  It should be running at http://localhost:32771/.

First, you need to point Kibana to Elasticsearch index(s) of your choice. Logstash creates indices with the name pattern of logstash-YYYY.MM.DD. In Kibana Settings → Indices configure the indices:

1. Index contains time-based events (select this option)
2. Use event times to create index names (select this option)
3. Index pattern interval: Daily
4. Index name or pattern: [logstash-]YYYY.MM.DD
5. Click on "Create Index"
6. Now click on "Discover" tab.

In my opinion, "Discover" tab is really named incorrectly in Kibana - it should be labeled as "Search" instead of "Discover" because it allows you to perform new searches and also to save/manage them. Log events should be showing up now in the main window. If they're not, then double check the time period filter in to right corner of the screen. Default table will have 2 columns by default: Time and _source. In order to make the listing more useful, we can configure the displayed columns. From the menu on the left select level, class and logmessage.


Link to youtube video demo:

[![ELK DEMO](/images/elkyoutube.png?raw=true)](https://youtu.be/A64aO6_d8rw)

ScreenShots Images:

![Logback Configuration](/images/logback-configuration.png?raw=true "Logback Configuration")
![Logstash Configuration](/images/logstash-configuration.png?raw=true "Logstash Configuration")
![Kibana Screen Example](/images/kibana-info.png?raw=true "Kibana Screen Example")
![Kibana Screen Example 2 filter](/images/kibana-filter.png?raw=true "Kibana Screen Example 2 filter")


Aditional info ELK and Spring boot: -  [Aditional info ELK and Spring boot](https://blog.codecentric.de/en/2014/10/log-management-spring-boot-applications-logstash-elastichsearch-kibana/ "Aditional info ELK and Spring boot")   
Kibana Lucene Query language Sintax: [Kibana Lucene Query language Sintax](https://www.elastic.co/guide/en/beats/packetbeat/current/_kibana_queries_and_filters.html "Kibana Lucene Query language Sintax")   







