# www_lab_week1
## Sinh viên thực hiện:

Họ tên: [Lê Thị Ngọc Mai]

MSSV: [20005501]

## Đề bài thực hành tuần 5 môn Lập trình WWW:
![image](https://github.com/ngocmai1522k2/www_lab_week5/assets/144517477/2e6acd51-3b32-4c02-8e66-ae69377d016d)
![image](https://github.com/ngocmai1522k2/www_lab_week5/assets/144517477/b7eeb710-beb2-4f27-af48-655f93a639f5)


## Mô tả

Dự án Jakarta EE này được phát triển để quản lý  trang web cho phép công ty đăng tin tuyển người với các skill mong muốn. Các ứng viên khi log vào sẽ được gợi ý các công việc có skill phù hợp với mình và đề xuất các skill nhân viên có thể học. Giúp các công ty tìm các ứng viên có skill phù hợp và các yêu cầu khác.

## Yêu cầu

Đảm bảo bạn đã cài đặt đầy đủ các thành phần sau:
- Java Development Kit (JDK)
- Maven
- MariaDB

## Công nghệ sử dụng

- Spring Boot: Framework để xây dựng ứng dụng doanh nghiệp dựa trên Java.
- MariaDB: Hệ thống quản lý cơ sở dữ liệu quan hệ.

## Cách cài đặt

1. **Clone dự án từ GitHub hoặc GitLab:**

```
git clone [https://github.com/ngocmai1522k2/www_lab_week5.git](https://github.com/ngocmai1522k2/www_lab_week5.git)
```

2. **Mở dự án bằng một trình biên soạn mã Java:** IntelliJ IDEA.

3. **Cấu hình trong file build.gradle:**
 ```
  dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-data-jdbc'
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.springframework.boot:spring-boot-starter-jdbc'
    implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'jakarta.persistence:jakarta.persistence-api'
    // https://mvnrepository.com/artifact/com.neovisionaries/nv-i18n
    implementation 'com.neovisionaries:nv-i18n:1.4'


    compileOnly 'org.projectlombok:lombok'
    developmentOnly 'org.springframework.boot:spring-boot-devtools'
    runtimeOnly 'com.h2database:h2'
    runtimeOnly 'org.mariadb.jdbc:mariadb-java-client'
    annotationProcessor 'org.projectlombok:lombok'

    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.9.3'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine'
    }
  ```
4. **Kết nối database Mariadb:**
   ### Database script:
   ```
     -- Dumping database structure for works
      CREATE DATABASE IF NOT EXISTS `works` /*!40100 DEFAULT CHARACTER SET latin1
      COLLATE latin1_swedish_ci */;
      USE `works`;
      -- Dumping structure for table works.address
      CREATE TABLE IF NOT EXISTS `address` (
      `id` bigint(20) NOT NULL AUTO_INCREMENT,
      `street` varchar(150) DEFAULT NULL,
      `city` varchar(50) DEFAULT NULL,
      `country` smallint(6) DEFAULT NULL CHECK (`country` between 0 and 201),
      `number` varchar(20) DEFAULT NULL,
      `zipcode` varchar(7) DEFAULT NULL,
      PRIMARY KEY (`id`)
      ) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;
      -- Data exporting was unselected.
      -- Dumping structure for table works.candidate
      CREATE TABLE IF NOT EXISTS `candidate` (
      `id` bigint(20) NOT NULL AUTO_INCREMENT,
      `dob` date NOT NULL,
      `email` varchar(255) NOT NULL,
      `full_name` varchar(255) NOT NULL,
      `phone` varchar(15) NOT NULL,
      `address` bigint(20) NOT NULL,
      PRIMARY KEY (`id`),
      UNIQUE KEY `UK_qfut8ruekode092nlkipgl09g` (`email`),
      UNIQUE KEY `UK_9i5yt1gvm0chg5e10qkns7tll` (`phone`),
      UNIQUE KEY `UK_m8qhlm4wu215gr34dhxp0dour` (`address`),
      CONSTRAINT `FKa8gnyyhbb2qnhp94grci1n0o9` FOREIGN KEY (`address`)
      REFERENCES `address` (`id`)
      ) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;
      -- Data exporting was unselected.
      -- Dumping structure for table works.candidate_skill
      CREATE TABLE IF NOT EXISTS `candidate_skill` (
      `more_infos` varchar(1000) DEFAULT NULL,
      `skill_level` tinyint(4) NOT NULL CHECK (`skill_level` between 0 and 2),
      `skill_id` bigint(20) NOT NULL,
      `can_id` bigint(20) NOT NULL,
      PRIMARY KEY (`can_id`,`skill_id`),
      KEY `FKb7cxhiqhcah7c20a2cdlvr1f8` (`skill_id`),
      CONSTRAINT `FKb0m5tm3fi0upa3b3kjx3vrlxs` FOREIGN KEY (`can_id`) REFERENCES
      `candidate` (`id`),
      CONSTRAINT `FKb7cxhiqhcah7c20a2cdlvr1f8` FOREIGN KEY (`skill_id`)
      REFERENCES `skill` (`skill_id`)
      ) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;-- Data exporting was unselected.
      -- Dumping structure for table works.company
      CREATE TABLE IF NOT EXISTS `company` (
      `comp_id` bigint(20) NOT NULL AUTO_INCREMENT,
      `about` varchar(2000) DEFAULT NULL,
      `email` varchar(255) NOT NULL,
      `comp_name` varchar(255) NOT NULL,
      `phone` varchar(255) NOT NULL,
      `web_url` varchar(255) DEFAULT NULL,
      `address` bigint(20) NOT NULL,
      PRIMARY KEY (`comp_id`),
      UNIQUE KEY `UK_rvp2hunsq4sgmpxe3a7i1ym3m` (`address`),
      CONSTRAINT `FKd5occp4cjwihejbxdbpvkp5tv` FOREIGN KEY (`address`)
      REFERENCES `address` (`id`)
      ) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;
      -- Data exporting was unselected.
      -- Dumping structure for table works.job
      CREATE TABLE IF NOT EXISTS `job` (
      `job_id` bigint(20) NOT NULL AUTO_INCREMENT,
      `job_desc` varchar(2000) NOT NULL,
      `job_name` varchar(255) NOT NULL,
      `company` bigint(20) DEFAULT NULL,
      PRIMARY KEY (`job_id`),
      KEY `FKbaqlvluu78phmo9ld89um7wnm` (`company`),
      CONSTRAINT `FKbaqlvluu78phmo9ld89um7wnm` FOREIGN KEY (`company`)
      REFERENCES `company` (`comp_id`)
      ) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;
      -- Data exporting was unselected.
      -- Dumping structure for table works.job_skill
      CREATE TABLE IF NOT EXISTS `job_skill` (
      `more_infos` varchar(1000) DEFAULT NULL,
      `skill_level` tinyint(4) NOT NULL CHECK (`skill_level` between 0 and 2),
      `job_id` bigint(20) NOT NULL,
      `skill_id` bigint(20) NOT NULL,
      PRIMARY KEY (`job_id`,`skill_id`),
      KEY `FKj33qbbf3vk1lvhqpcosnh54u1` (`skill_id`),
      CONSTRAINT `FK9ix4wg520ii2gu2felxdhdup6` FOREIGN KEY (`job_id`) REFERENCES
      `job` (`job_id`),
      CONSTRAINT `FKj33qbbf3vk1lvhqpcosnh54u1` FOREIGN KEY (`skill_id`)
      REFERENCES `skill` (`skill_id`)
      ) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;
      -- Data exporting was unselected.
      -- Dumping structure for table works.skill
      CREATE TABLE IF NOT EXISTS `skill` (
      `skill_id` bigint(20) NOT NULL AUTO_INCREMENT,
      `skill_description` varchar(255) DEFAULT NULL,
      `skill_name` varchar(255) DEFAULT NULL,
      `type` tinyint(4) DEFAULT NULL CHECK (`type` between 0 and 2),
      PRIMARY KEY (`skill_id`)) ENGINE=InnoDB DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;
   ```

6. **Cấu trúc project:**
  - **models:** Package chứa các lớp mô hình (entities) của cơ sở dữ liệu.

    + Address.java: Đại diện cho thông tin địa chỉ.
    + Candidate.java: Đại diện cho thông tin ứng viên.
    + CandidateSkill.java: Đại diện cho kỹ năng của ứng viên.
    + Company.java: Đại diện cho thông tin công ty.
    + Job.java: Đại diện cho thông tin công việc.
    + JobSkill.java: Đại diện cho kỹ năng yêu cầu của công việc.
    + Skill.java: Đại diện cho thông tin kỹ năng.

  - **connection:** Package này bao gồm lớp ConnectDB, dùng để kết nối đến cơ sở dữ liệu.

  - **Repositories:** Các package con sau đây chứa các lớp Repository để thực hiện các thao tác truy vấn và cập nhật dữ liệu trong cơ sở dữ liệu:
      + **AccountRepositories:** Chứa các hàm login, thêm, tìm tài khoản theo ID, tìm tất cả các tài khoản, xóa, cập nhật,  đăng xuất và lấy danh sách tài khoản theo quyền.
      + **GrantAccessRepositories:** Chứa hàm để lấy quyền của tài khoản đăng nhập, thêm quyền, kiểm tra tài khoản được cấp quyền chưa, cập nhật quyền, 
      + **LogRepositories:** Chứa hàm ghi log sự kiện.
      + **RoleRepositories:** Chứa hàm để lấy danh sách quyền của mỗi tài khoản, danh sách tất cả quyền

  - **Controllers:** Chứa ControllerServlet để xử lý yêu cầu đăng nhập và các yêu cầu khác.

7. **Chạy ứng dụng:** Khởi động máy chủ servlet và truy cập ứng dụng qua trình duyệt web bằng địa chỉ URL tương ứng (ví dụ: `[http://localhost:8080/lab1/](http://localhost:8080/Gradle___vn_edu_iuh_fit___week1_lab_LeThiNgocMai_20005501_1_0_SNAPSHOT_war/)`).

## Cách sử dụng

1. Chạy project bằng Tomcat Server Tomcat 10.1.12

2. Trang đăng nhập sẽ hiển thị. Nhập thông tin đăng nhập để tiếp tục.
  ![image](https://github.com/ngocmai1522k2/www_lab_week1/assets/144517477/8a8c25e9-8d97-4e54-ae67-b21531d2475f)

3. Đăng nhập tài khoản teo@gmail.com vai trò admin:
   
  ![image](https://github.com/ngocmai1522k2/www_lab_week1/assets/144517477/d4a9cd7c-9770-4344-9e02-3278f51e8ed1)

5. Sau khi đăng nhập thành công, bạn sẽ được định hướng đến trang chủ hiển thị thông tin account của bạn, tùy thuộc vào quyền của tài khoản. Nếu bạn là admin, bạn có quyền quản lý tài khoản và quyền. Ngược lại, bạn sẽ thấy thông tin của mình và các quyền mà bạn có.
  ![image](https://github.com/ngocmai1522k2/www_lab_week1/assets/144517477/95f533a5-59e9-44c2-bb70-3a3555988191)
   Đăng nhập bằng tài khoản met@gmail.com vai trò user:
  ![image](https://github.com/ngocmai1522k2/www_lab_week1/assets/144517477/21df2eef-bb7c-488b-8c98-cdbfa204c1ec)

6. Chức năng quản lý danh sách account sẽ hiển thị thông tin từng account,  hiển thị nút xóa, cập nhật, cấp quyền trên từng dòng:
  ![image](https://github.com/ngocmai1522k2/www_lab_week1/assets/144517477/297d3087-bac9-47fa-a53c-00dcd822ed43)
7. Chức năng xóa account: account xóa sẽ có status là -1 trong database và không còn hiển thị trên danh sách quản lý account:
  ![image](https://github.com/ngocmai1522k2/www_lab_week1/assets/144517477/7cad9909-eea4-4582-ae4e-8d91a5acc650)
8. Chức năng update: khi click vào nút update ở 1 account bất kì, sẽ chuyển sang trang update và hiển thị các thông tin của account cần update, chỉ có thể chỉnh được email, password và phone:
  ![image](https://github.com/ngocmai1522k2/www_lab_week1/assets/144517477/fc8bfd85-8419-4d31-bce4-abff6623cda8)
  ![image](https://github.com/ngocmai1522k2/www_lab_week1/assets/144517477/e8e4f00c-fb27-4ead-9019-e28fb2df62dd)
9. Chức năng list role của account đang đăng nhập:
   ![image](https://github.com/ngocmai1522k2/www_lab_week1/assets/144517477/1ce7099b-e511-4ee7-9cf7-2197efc260d5)
10. Chức năng add account:
    ![image](https://github.com/ngocmai1522k2/www_lab_week1/assets/144517477/f1676ef8-1beb-42e7-a422-d7a88b30b7e7)

## Tài liệu liên quan

- [Java EE Documentation](https://javaee.github.io/javaee-spec/)
- [Java Database Connectivity (JDBC) Documentation](https://docs.oracle.com/en/java/javase/16/docs/api/java.sql/java/sql/package-summary.html)
- [Apache Tomcat Documentation](https://tomcat.apache.org/tomcat-9.0-doc/index.html)
- [Git Version Control](https://git-scm.com/book/en/v2)


## Đóng góp

Nếu bạn muốn đóng góp vào dự án hoặc báo cáo lỗi, vui lòng tạo issue hoặc gửi pull request vào repository GitHub của dự án.

- GitHub Repository: [www_lab_week1](https://github.com/ngocmai1522k2/www_lab_week1)
- Tạo issue mới: [Tạo issue](https://github.com/ngocmai1522k2/www_lab_week1/issues/new)
- Gửi pull request: [Gửi pull request](https://github.com/ngocmai1522k2/www_lab_week1/compare)

Chúng tôi rất hoan nghênh mọi đóng góp từ cộng đồng!

---
