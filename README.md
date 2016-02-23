# spring-cloud-netflix-issue-840
Repro for issue 840 for spring-cloud-netflix

 1. Have a spring eureka server (@EnableEurekaServer) running on IP 172.17.0.2 
    (update IP to reflect reality in bootstrap.yml and bootstrap-ok.yml)
 2. Start the application with the default profile : `mvn spring-boot:run` 
 3. View that the application can not contact the eureka server :  `IllegalArgumentException : Host name may not be null` 
 4. Start the application with the `ok` profile : `mvn spring-boot:run -Drun.jvmArguments="-Dspring.profiles.active=ok"` 
 5. The application starts with no problem at all
 
The only difference between the two profiles is that the eureka client defaultZone is specified 
with the "80" port in the "ok" profile. 

failing profile : 

```yml
eureka:
  instance:
    preferIpAddress: true
  client:
    serviceUrl:
      defaultZone: http://172.17.0.2/eureka/
```

ok profile : 

```yml
eureka:
  instance:
    preferIpAddress: true
  client:
    serviceUrl:
      defaultZone: http://172.17.0.2:80/eureka/
```
