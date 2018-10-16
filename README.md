# Gradle Spring Boot Starter Sample

The [Spring Framework](https://spring.io) is a de facto standard for creating enterprise java applications as it offers a broad variety of functionality for all kinds of needs. Things became even more convenient when [Spring Boot](https://spring.io/guides/gs/spring-boot/) was introduced some years ago. 

Have you ever wondered about all tha _magic_ that comes with Spring? Just add a dependency or an annotation and **BAHMM!** there you go! A lot of this magic is based on the various _Startes_ that are offered for Spring Boot. _Starters_ are the way to go when it comes to creating shared components in the Spring Boot universe. So it might be something worth spending the effort on [creating your own](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-developing-auto-configuration.html)! Unfortunately the official documentation is a little short and left some questions open to me. Most tutorials that can be found on the web
describe how to create a starter using Maven. As I mostly use [Gradle](https://gradle.org) I had a hard time transforming these tutorials. 

This project demonstrates how to create a custom spring boot starter using Gradle.

## Project structure

Spring Boot starters require a certain project structure. To fit this need you need to author a [Multi-Project Build](https://docs.gradle.org/current/userguide/multi_project_builds.html) containing at least three modules:

    1. gradle-spring-boot-starter-sample (This project provides necessary dependencies and gradle configuration)
       |
       |-> 2. my-library-autoconfigure (This project holds spring boot autoconfiguration)
       |
       |-> 3. my-library (T his project holds you actual business logic)
       
The top-level project is mainly used to configure some common properties and to define a dependency on the [Auto-configuration](https://docs.spring.io/spring-boot/docs/current/reference/html/using-boot-auto-configuration.html)
 
Auto-configuration is where the magic happens. Especially the various [`@ConditionalOnXXX` annotations](https://docs.spring.io/spring-boot/docs/1.2.1.RELEASE/reference/html/boot-features-developing-auto-configuration.html) are used to provide both a reasonable
default configuration as well as flexibility at the same time.

## Integration

Integration of a custom library needs extra efforts and configuration to work with you Spring Configuration. Even if you properly define Spring components using the known
Spring stereotype annotations like `@Service` or `@Component` Spring will not create bean instances as it will not be aware of you classes. You will need to add the corresponding packages
to Spring's component scan manually using [`@ComponentScan` annotation](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/context/annotation/ComponentScan.html). This is usually some how error prone
and feels somehow strange.   

Using Spring Boot Starters using `@ComponentScan` but how the heck is Spring finding your Bean definitions? This is done by providing a `spring.factories` file under `src/main/resources/META-INF/spring.factories` in your Auto-configuration project telling Spring what it needs to know:

    org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
      net.skobow.samples.mylibrary.autoconfigure.MyLibraryAutoConfiguration

## Shared component

Your shred component goes into the third project that needs to be referenced by the Auto-configuration

## Feedback
Feedback is very welcome! Please send your feedback and questions to [sk@skobow.net](mailto:sk@skobow.net)