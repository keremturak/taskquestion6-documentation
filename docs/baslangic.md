# Başlangıç

## Proje Klonlanması
- GitHub'da projenin ana sayfasına gidin.
- Sağ üst köşede bulunan "Code" butonuna tıklayın.
- Orada çıkan adresi kopyalayın
- Örn:
` git clone https://github.com/keremturak/TaskQuestion6.git`

![Sample Image](C:\Users\kerim\taskquestion6-documentation\docs\img\Ekran görüntüsü 2023-11-02 005456.png)

## Bağımlılıklar
![Dependencies](https://github.com/keremturak/HR-Management-Documentation/blob/main/docs/img/Gradle_logo.png?raw=true)

|      | Tech     | Url |
|----| -------- | ------- |
|1| springDataMongodb  | "org.springframework.boot:spring-boot-starter-data-mongodb:${versions.springBoot}"    |
|2| Spring Boot Web | implementation 'org.springframework.boot:spring-boot-starter-web'     |
|3| Lombok    |compileOnly 'org.projectlombok:lombok'-----annotationProcessor 'org.projectlombok:lombok'    |
|4| Swagger Ui | implementation 'org.springdoc:springdoc-openapi-starter-webmvc-ui:2.1.0'     |
|5| Mapstruct    | implementation 'org.mapstruct:mapstruct:1.5.5.Final'   |
|6| Validator | 	implementation  'org.hibernate.validator:hibernate-validator:8.0.0.Final'    |


## Dockerda MongoDB Image Oluşturulması ve Çalıştırılması

-`docker run -d -e MONGO_INITDB_ROOT_USERNAME=mongoadmin -e MONGO_INITDB_ROOT_PASSWORD=secret -p 27017:27017 mongo
`



## Application.yaml düzenlenmesi

- Proje `9090` serverinda ayağa kalkmaktadır.
- MongoDB kullanılmıştır ve Yapılandırılmalarının eklenmesi gerekmektedir.


```java
server:
  port: 9090

spring:
  data:
    mongodb:
      host: localhost
      port: 27017
      database: taskquestion6
      username: ${MONGO_USER}
      password: ${MONGO_PASS}
```

- Öncelikle Ortam değişkenleri açılmalı.
- Ortam değişkenlerine kullanıcı adı ve şifre kısımları eklenmeli.

![Sample Image](https://github.com/keremturak/taskquestion6-documentation/blob/main/docs/img/OrtamDegiskenleri.png?raw=true)

- MongDB ye taskquestion6 tablosu oluşturulmalı ve bu tabloda `username: ${MONGO_USER}`, `password: ${MONGO_PASS}` yetkilendirilmeli.
- örnek;
  
![Sample Image](C:\Users\kerim\taskquestion6-documentation\docs\img\Ekran görüntüsü 2023-11-02 005456.png)



## Tech Stack 


**Server:**<img src="https://cdn.iconscout.com/icon/free/png-512/free-java-59-1174952.png?f=avif&w=256" alt="HTML5" width="25" height="20"> Java,<img src="https://camo.githubusercontent.com/96e43701d83561899724a89d71187445b7b8f4fe84518a3ea5bec8f85bd207bf/68747470733a2f2f63646e2e737667706f726e2e636f6d2f6c6f676f732f737761676765722e737667" alt="HTML5" width="25" height="20"> Swagger<img src="https://cdn.iconscout.com/icon/free/png-512/free-spring-16-283031.png?f=avif&w=256" alt="HTML5" width="25" height="20">SpringBoot, <img src="https://cdn.iconscout.com/icon/free/png-512/free-gradle-2-1174969.png?f=avif&w=256" alt="HTML5" width="25" height="20">Gradle


**Cloud:**<img src="https://cdn.iconscout.com/icon/free/png-512/free-docker-226091.png?f=avif&w=256" alt="HTML5" width="25" height="20">Docker

**DataBases:**<img src="https://cdn.iconscout.com/icon/free/png-512/free-mongodb-3521676-2945120.png?f=avif&w=256" alt="HTML5" width="25" height="20">MongoDB



