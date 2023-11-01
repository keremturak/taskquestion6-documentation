##Proje Mimarisi

- Bu proje monolitic mimariye uygun şekilde tasarlanmıştır. Katmanlı mimariye sadık kalmaya çalışılmıştır.


![Sample Image](https://i.stack.imgur.com/BfNin.jpg)





```tree
com.keremturak
├── constant
│   ├── ApiUrls
├── controller
│   ├── CompanyController
│   ├── EmployeeController
├── dto
│   ├── request
│   ├── response
├── exception
├── mapper
├── repository
│   ├── entity
│   ├── enums
├── service
├── Application.java
```

##Entityler

###Company

```java	
@Data
@AllArgsConstructor
@NoArgsConstructor
@SuperBuilder
@Document
public class Company{
    @Id
   private String id;
    private String companyName;
    private String companyPhone;
    private String infoEmail;
    private String city;
}
```
###Employee

```java	
@Data
@AllArgsConstructor
@NoArgsConstructor
@SuperBuilder
@Document
public class Employee{
    @Id
    private String id;
    private String companyId;
    private String firstName;
    private String lastName;
    @Builder.Default
    private ERole role=ERole.EMPLOYEE;
}
```

###Anotasyonlar

- @Data
  
`@Data` annotation'ı Lombok kütüphanesine aittir.
`@Data` annotation'ı, bir sınıfın temel veri işleme yöntemlerini otomatik olarak oluşturur, bu da getter, setter, toString, equals ve hashCode metodlarını içerir.

- @AllArgsConstructor

Lombok kütüphanesine aittir ve bir sınıfın tüm alanlarını içeren bir constructor (kurucu metod) oluşturur. Bu, sınıfın her bir alanının değerini parametre olarak alarak nesne oluşturmayı sağlar.

- @NoArgsConstructor

bir sınıfın parametresiz bir constructor (kurucu metod) oluşturur. Bu, sınıfın hiçbir parametre almadan nesne oluşturulmasını sağlar.

- @SuperBuilder

Bu annotation, sınıfın her alanını içeren bir builder sınıfı oluşturur. Bu builder sınıfı, nesnenin alanlarına tek tek değer atamak yerine, zincirleme yöntemi çağrılarıyla nesne oluşturmayı sağlar.

- @Document

Spring Data MongoDB tarafından sağlanan bir annotation'dır. Bu annotation, bir sınıfın MongoDB veritabanında bir doküman olarak saklanması gerektiğini belirtir.

- @Id

@Id annotation'u, bir sınıfın veya nesnenin benzersiz bir tanımlayıcısını belirtmek için kullanılır. Bu annotation, genellikle veritabanı işlemlerinde kullanılır ve belirli bir nesnenin veritabanındaki benzersiz kimliğini temsil eder.

##Dto

DTO (Data Transfer Object), bir yazılım uygulamasında farklı katmanlar arasında veri iletimi veya paylaşımı için kullanılan bir tasarım desenidir. DTO'lar, verilerin bir katmandan diğerine aktarılmasını kolaylaştırır ve veri taşıma işlemlerini daha etkili hale getirir.

DTO'ların bazı temel amaçları şunlardır:

Veri Transferi: DTO'lar, verinin bir katmandan diğerine aktarılmasını sağlar. Özellikle, veritabanından alınan verinin iş katmanına veya sunum katmanına taşınması için kullanılır.

Veri Gizleme (Data Hiding): DTO'lar, sadece belirli verilerin veya alanların belirli bir katmana aktarılmasını sağlayarak veri gizlemeyi destekler. Bu sayede, gerekli olmayan veri alanları gönderilmez.

Örnek Dto;

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@Builder
public class CompanySaveRequestDto {
    @NotEmpty(message = "Name field cannot be empty")
    private String companyName;
    @NotEmpty(message = "Name field cannot be empty")
    @Size(min = 3, max = 20, message = "Name must be between 3 and 20 characters.")
    private String companyPhone;
    @Email(message = "Email must be valid")
    private String infoEmail;
    @NotEmpty(message = "City field cannot be empty")
    private String city;
}
```

`@NotEmty` bir alanın veya koleksiyonun boş olmamasını belirtir.,`@Email` bir alanın geçerli bir e-posta adresi olmasını belirtir. Bu annotation, genellikle e-posta adreslerinin doğrulanması için kullanılır.

##Controller

Controller katmanı, kullanıcı isteklerini alır, iş mantığını başlatır, modeli günceller, uygun görünümü seçer ve sonucu istemciye gönderir. Bu sayede uygulamanın farklı bileşenleri arasında etkileşim sağlanır ve istemcinin talepleri karşılanır.

###CompanyController

```java
@RestController
@RequiredArgsConstructor
@RequestMapping(COMPANY)
public class CompanyController {
    private final CompanyService companyService;

    @PostMapping (SAVE_COMPANY)
    public ResponseEntity<CompanySaveResponseDto> save(@RequestBody @Valid CompanySaveRequestDto dto) {
        return ResponseEntity.ok(companyService.save(dto));
    }
    @GetMapping (FIND_ALL_COMPANY)
    public ResponseEntity<List<CompanyFindAllResponseDto>> findAll() {
        return ResponseEntity.ok(companyService.findAll());
    }
    @PutMapping(UPDATE_COMPANY)
    public ResponseEntity<CompanyResponseDto> updateCompany(@RequestBody CompanyUpdateRequestDto dto){
        return ResponseEntity.ok(companyService.updateCompany(dto));
    }
    @DeleteMapping(DELETE_COMPANY + "/{id}")
    public ResponseEntity<String> deleteCompany(@PathVariable String id){
        return ResponseEntity.ok(companyService.deleteCompany(id));
    }

}
```

- Neden @RestController ?

`@RestController` annotation'u, Spring Framework içerisinde bir Java sınıfının bir RESTful web servisi olarak işlev görmesini sağlar. RESTful web servisler, HTTP protokolü üzerinden erişilen ve JSON veya XML gibi veri formatlarıyla iletişim kuran servislerdir. 

- @Valid

`@Valid` annotation'u, gelen isteğin parametrelerini doğrulamak için kullanılır. Eğer gelen veriler belirli kriterlere uymuyorsa, hata mesajları oluşturulabilir ve bu mesajlar istemciye döndürülebilir.

- @RequestBody

`@RequestBody` annotation'u, bir HTTP isteğinin içeriğini almak için kullanılır. Genellikle JSON veya XML gibi formatlardaki verileri bir Java nesnesine dönüştürmek için kullanılır.

- @PathVariable

`@PathVariable` annotation'u, bir Spring MVC Controller metodunda URL'de bulunan bir değişkeni almak için kullanılır. Bu annotation, istemcinin gönderdiği URL içindeki değişken değerini bir metod parametresine bağlar.

- Http Metotları
[w3schools](https://www.w3schools.com/tags/ref_httpmethods.asp)



##Service

Service katmanı, bir uygulamanın iş mantığını ve iş süreçlerini yöneten katmandır. Bu katman, veri işleme, iş kuralları uygulama, dış kaynaklarla etkileşim ve diğer işlemleri gerçekleştirir. Service katmanı, genellikle Controller ve Repository (veri tabanı erişimi) katmanları arasında yer alır.

###CompanyService

```java
@Service
public class CompanyService {
    private final ICompanyRepository companyRepository;
    private final EmployeeService employeeService;
    CompanyService(ICompanyRepository companyRepository, EmployeeService employeeService) {
        this.companyRepository = companyRepository;
        this.employeeService = employeeService;
    }
    public CompanySaveResponseDto save(CompanySaveRequestDto dto) {
        if (dto == null) {
            throw new MyCompanyException(EErrorType.DTO_IS_NULL);
        }
        Company company = IMapper.INSTANCE.companyFromCompanySaveRequestDto(dto);
        return IMapper.INSTANCE.companySaveResponseDtofromCompany(companyRepository.save(company));
    }

    public List<CompanyFindAllResponseDto>  findAll() {
        List<Company> companies = companyRepository.findAll();
        if (companies.isEmpty()){
            throw new MyCompanyException(EErrorType.COMPANY_NOT_BE_FOUND);
        }
        List<CompanyFindAllResponseDto> companyList = new ArrayList<>();
        companies.forEach(company -> companyList.add(IMapper.INSTANCE.companyFindAllResponseDtofromCompany(company)));
        return companyList;

    }

    public CompanyResponseDto updateCompany(CompanyUpdateRequestDto dto) {
        if (dto == null) {
            throw new MyCompanyException(EErrorType.DTO_IS_NULL);
        }
        Company company = Company.builder().companyName(dto.getCompanyName()).companyPhone(dto.getCompanyPhone()).id(dto.getId()).infoEmail(dto.getInfoEmail()).city(dto.getCity()).build();
        companyRepository.save(company);
        CompanyResponseDto responseDto = IMapper.INSTANCE.companyResponseDtofromCompany(company);
        return responseDto;
    }

    public String deleteCompany(String id) {
        Optional<Company> company = companyRepository.findById(id);
        if (company.isEmpty()) {
            throw new MyCompanyException(EErrorType.COMPANY_NOT_BE_FOUND);
        }
        employeeService.deleteAllByCompanyId(id);
        companyRepository.deleteById(id);
        return "Company deleted";
    }
}
```
Burada `@Service` Anotasyonu kullanılmıştır. bunun sebebi Spring Framework içinde servis sınıflarını işaretlemek için kullanılır. Bu anotasyon sayesinde spring arka planda bean oluşturur. Ayrıca Constructor injection kullanılmıştır. bağımlılıkların bir sınıfın kurucu metodunda belirtilmesi ve bu bağımlılıkların sınıfın içindeki alanlara atanması anlamına gelir


##Repository

Repository katmanı, bir uygulamanın veritabanı işlemlerini yöneten ve veri erişimini sağlayan katmandır. Bu katman, veritabanı ile iletişim kurar, sorgular oluşturur ve veri işlemlerini gerçekleştirir.

###CompanyRepository

```java
@Repository
public interface ICompanyRepository extends MongoRepository<Company, String> {
}
```

##Mapper

Mapper, genellikle farklı veri yapıları veya sınıf tipleri arasında dönüşümler gerçekleştiren bir bileşendir. Bu dönüşümler, bir nesnenin verilerini başka bir nesnenin veri yapısına veya başka bir sınıf tipine dönüştürmek için kullanılır.

```java
@Mapper(componentModel = "string", unmappedTargetPolicy = ReportingPolicy.IGNORE)

public interface IMapper {

    IMapper INSTANCE = Mappers.getMapper(IMapper.class);

    Company companyFromCompanySaveRequestDto(final CompanySaveRequestDto dto);
```
##Exception

Özetlemek gerekirse, kendi özel istisna sınıfınızı oluşturmak, hata yönetimi ve sorun giderme sürecini geliştirebilir. Ayrıca, uygulamanızın ihtiyaçlarına daha iyi uyum sağlayabilir ve kodunuzu daha okunabilir hale getirebilirsiniz.

```java
@AllArgsConstructor
@NoArgsConstructor
@Getter
public enum EErrorType {

    INTERNAL_ERROR(3000,"Unexpected error on server",INTERNAL_SERVER_ERROR),
    INVALID_TOKEN(4001,"Invalid token information",BAD_REQUEST),
    BAD_REQUEST_ERROR(1202,"You have entered an invalid parameter",BAD_REQUEST),

    COMPANY_NOT_BE_FOUND(2302,"The company you were looking for could not be found",BAD_REQUEST),
    EMPLOYEE_NOT_BE_FOUND(2303,"The employee you were looking for could not be found",BAD_REQUEST),
    EMPLOYEE_NAME_NOT_FOUND(2303,"The employee name you were looking for could not be found",BAD_REQUEST),
    DTO_IS_NULL(2303,"The DTO you were looking for could not be found",BAD_REQUEST),
    USER_NOT_IN_COMPANY(2307,"The user you were looking for could not be found that company",BAD_REQUEST);
    private int code;
    private String message;
    private HttpStatus httpStatus;
}
```

