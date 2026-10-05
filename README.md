BAI 2: BIEN DICH DONG GOI TEP UNG DUNG .JAR VOI GRADLE VA LUU TRU ARTIFACTS

1. MUC TIEU VA BOI CANH
Trong chuoi cung ung phan mem (Software Supply Chain), sau khi vuot qua cac bai kiem thu don vi (Unit Tests), ung dung can duoc dong goi thanh thanh pham co the thuc thi (Executable Fat JAR) va luu tru tap trung lam Artifact de phuc vu cho cac giai doan kiem thu tich hop, trien khai (Deployment) hoac phan phoi. Bai tap nay bo sung buoc bootJar va actions/upload-artifact@v4 vao workflow CI.

2. CAU TRUC DU AN VA CAU HINH DONG GOI BOOTJAR
Du an Spring Boot su dung Spring Boot Gradle Plugin:
- Lenh ./gradlew bootJar thuc hien quy trinh gom: bien dich ma nguon Java, gom toan bo runtime dependencies (embedded Tomcat server, Spring Framework libraries), va dong goi thanh file .jar duy nhat trong thu muc build/libs/.
- File thanh pham co ten mac dinh theo dinh dang: spring-boot-gradle-app-0.0.1-SNAPSHOT.jar (hoac *.jar).

3. CAU HINH GITHUB ACTIONS WORKFLOW (.github/workflows/ci.yml)
Noi dung kich ban CI mo rong tu EX1:

```yaml
name: Java Spring Boot CI (Gradle)

on:
  push:
    branches: [ "main" ]

jobs:
  test:
    name: Run Unit Tests and Package JAR
    runs-on: ubuntu-latest

    steps:
      - name: Checkout mã nguồn
        uses: actions/checkout@v5

      - name: Thiết lập môi trường Java JDK 17
        uses: actions/setup-java@v5
        with:
          java-version: '17'
          distribution: 'temurin'

      - name: Cấp quyền cho Gradle Wrapper
        run: chmod +x gradlew

      - name: Thực thi JUnit Test với Gradle
        run: ./gradlew test

      - name: Đóng gói tệp JAR với Gradle
        run: ./gradlew bootJar

      - name: Lưu trữ thành phẩm JAR Artifact
        uses: actions/upload-artifact@v4
        with:
          name: app-jar
          path: build/libs/*.jar
```

4. PHAN TICH CHI TIET CAC BUOC BO SUNG
- Step Dong goi tep JAR voi Gradle: Chay lenh ./gradlew bootJar sau khi bai test da pass. Gradle tien hanh xu ly resources va dong goi archive executable JAR vao thu muc build/libs/.
- Step Luu tru thanh pham JAR Artifact: Su dung action actions/upload-artifact@v4 voi 2 tham so quan trong:
  - name: app-jar (Ten dinh danh cua Artifact tren GitHub Actions UI).
  - path: build/libs/*.jar (Duong dan mau khop voi tat ca cac tep JAR tao ra trong build/libs/).
- Co che hoat dong cua Upload Artifact v4: Artifact duoc nen va truyen truc tiep len ha tang luu tru cua GitHub, cho phep nguoi dung hoac cac job khac tai ve su dung trong vong doi cua workflow.

5. HUONG DAN KIEM TRA VA TAI ARTIFACT
- Day commit len nhanh main cua repository rikkei-devops-tmd/ss12-bt2.
- Cho workflow Java Spring Boot CI (Gradle) hoan thanh voi trang thai Success.
- Truy cap trang Summary cua luong chay (Run Summary).
- Cuon xuong muc Artifacts o phan duoi trang Summary, xac nhan xuat hien muc app-jar voi dung luong file tuong ung.
- Nhan vao app-jar de tai file zip chua tep executable JAR ve may cuc bo va kiem tra.

