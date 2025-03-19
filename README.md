# 🐳 Dockerized Java Application

본 프로젝트는**경량화된 Docker 이미지를 활용하여 Java JAR 애플리케이션을 배포 및 실행** 하는 것을 목표로 합니다.

최소한의 리소스를 사용하면서도 **빠른 실행 속도**와 **효율적인 이미지 관리**를 구현하는 데 중점을 둡니다.


<br>

## 👨‍💻 팀원

|<img src="https://github.com/CooolRyan.png" width="220" />|<img src="https://github.com/imhaeunim.png" width="220" />|
|:-:|:-:|
|나원호<br/>[@CooolRyan](https://github.com/CooolRyan)|임하은<br/>[@imhaeunim](https://github.com/imhaeunim)|



<br>
<br>

## 💻 실행 과정

### 📌 1. Dockerfile 작성

```docker
# 1️⃣ 경량화된 OpenJDK 17 JRE 기반의 공식 이미지 사용
FROM eclipse-temurin:17-jre-alpine

# 2️⃣ 컨테이너 내부의 작업 디렉토리 설정
WORKDIR /app

# 3️⃣ 현재 디렉토리의 모든 파일을 컨테이너로 복사
COPY . /app

# 4️⃣ 컨테이너에서 실행될 기본 명령어 설정
CMD ["java", "-jar", "step01_basic-0.0.1-SNAPSHOT.jar", "--server.port=8000"]
```

**✅ Dockerfile 주요 설명**

| 단계 | 설명 |
| --- | --- |
| `FROM eclipse-temurin:17-jre-alpine` | **JRE 실행 환경**을 갖춘 경량 **Alpine Linux** 기반 이미지 사용 |
| `WORKDIR /app` | 컨테이너 내 **작업 디렉토리**를 `/app`으로 설정 |
| `COPY . /app` | 현재 디렉토리의 모든 파일을 컨테이너 `/app` 디렉토리에 복사 |
| `CMD ["java", "-jar", "step01_basic-0.0.1-SNAPSHOT.jar", "--server.port=8000"]` | 컨테이너 실행 시 **JAR 파일 실행 및 포트 8000 사용** |

---

<br>

### 📌 2. Docker 이미지 만들기

위에서 만든 `Dockerfile`을 기반으로 **Docker 이미지를 생성**합니다.

```bash
docker build -t <docker-hub-사용자이름>/myjar:1.0 .
```

**✅ 명령어 설명**

| 옵션 | 설명 |
| --- | --- |
| `-t` | 생성한 이미지를 **태그(tag)** 지정 |
| `<docker-hub-사용자이름>/myjar:1.0` | Docker Hub에 업로드할 **이미지 이름 및 버전** |
| `.` | 현재 디렉토리에 있는 `Dockerfile`을 사용하여 빌드 |

---

<br>

### 📌 3. Docker Hub에 이미지 업로드

**1️⃣ Docker Hub에 로그인**

- Docker Hub에 업로드하려면 **Docker Hub 계정이 필요**

```bash
docker login
```

**2️⃣ Docker Hub에 생성한 이미지를 푸시**

```bash
docker push <docker-hub-사용자이름>/myjar:1.0
```

✔ **성공적으로 업로드되면**, 다른 환경에서도 이 이미지를 가져와 실행할 수 있음

---

<br>

### 📌 4. Docker Hub에서 이미지 다운로드 후 실행

- Docker Hub에서 업로드한 이미지를 가져와 컨테이너를 실행합

```bash
docker run -d -p 8000:8000 --name myjar <docker-hub-사용자이름>/myjar:1.0
```

**✅ 명령어 설명**

| 옵션 | 설명 |
| --- | --- |
| `-d` | 컨테이너를 **백그라운드 모드**로 실행 |
| `-p 8000:8000` | **호스트 8000번 포트** → **컨테이너 8000번 포트** 연결 |
| `--name myjar` | 컨테이너의 **이름을 `myjar`로 지정** |
| `<docker-hub-사용자이름>/myjar:1.0` | Docker Hub에서 가져올 **이미지 이름** |

---

<br>

### 📌 5. 실행 확인

**✅ curl**

```bash
curl http://localhost:8000/woori
```

**✅ web**

- [http://localhost:8000/woori](http://localhost:8000/woori에서) 접속해서 확인 가능

---

<br>

### 📌 6. Docker 이미지 & 컨테이너 관리 🛠️

**🔹 실행 중인 컨테이너 확인**

```bash
docker ps
```

**🔹 모든 컨테이너 목록 확인 (중지된 컨테이너 포함)**

```bash
docker ps -a
```

**🔹 컨테이너 중지**

```bash
docker stop myjar
```

**🔹 컨테이너 삭제**

```bash
docker rm myjar
```

**🔹 생성된 Docker 이미지 목록 확인**

```bash
docker images
```

**🔹 사용하지 않는 이미지 삭제**

```bash
docker rmi <이미지 ID>
```

---

<br>
<br>

## ✏ 참고

### 1️⃣ Docker 이미지 태그(tag) 관리

**✅ 1. 태그를 지정하여 이미지 빌드**

```bash
docker build -t <docker-hub-사용자이름>/myjar:1.0 .
docker build -t <docker-hub-사용자이름>/myjar:2.0 .
```


✅ **2. 태그별로 Docker Hub에 업로드**

```bash
docker push <docker-hub-사용자이름>/myjar:1.0
docker push <docker-hub-사용자이름>/myjar:2.0
```


✅ **3. 특정 태그를 가진 이미지 실행**

```bash
docker run -d -p 8000:8000 --name myjar <docker-hub-사용자이름>/myjar:1.0
```
![image](https://github.com/user-attachments/assets/8e95f581-b6b8-4287-8036-d5b9242298c2)


**🚀 tag를 활용해서 버전 별로 이미지를 관리할 수 있음!**

<br>

### 2️⃣ Alpine 기반 JRE vs 일반 JRE 비교 🔍

**✅ `eclipse-temurin:17-jre` (일반 JRE)**

- **용량이 크고**, 더 많은 라이브러리 포함
- 기본적으로 더 **완전한 JRE 환경** 제공
    ![image](https://github.com/user-attachments/assets/2be6eb40-9c3f-4bbb-a88f-e1f63a79d243)


**✅ `eclipse-temurin:17-jre-alpine` (경량화된 JRE)**

- **용량이 더 작고**, 불필요한 요소 제거
- **더 빠른 실행**과 **최적화된 성능** 제공
    ![image](https://github.com/user-attachments/assets/3d6dd60f-105b-4d44-9024-d3f38c3685f3)


**🚀 실행만 하면 되므로 `Alpine` 기반이 더 좋음**

<br>
<br>

## **💡GraalVM을 통한 파일크기 더 줄이기**

- **Java 애플리케이션을 더 빠르게 실행하고, 네이티브 실행 파일로 변환할 수 있는 JDK 배포판**

### 1️⃣ **GraalVM의 주요 특징**

✅ **1) JIT (Just-In-Time) 컴파일러 → JVM의 C1, C2 컴파일러를 대체**

✅ **2) AOT (Ahead-Of-Time) 컴파일 - `native-image`**

- Java 애플리케이션을 **JVM 없이 실행 가능한 네이티브 실행 파일(바이너리)로 변환 가능**
- `native-image` 명령어를 사용하여 `.jar` → 실행 파일 (`.exe`, `.bin`)로 변환

✅ **3) 다중 언어 지원 (Polyglot)**

- **Java, JavaScript, Python, Ruby, R, LLVM (C/C++) 등 다양한 언어를 실행 가능**

<br>

### 2️⃣**GraalVM vs 기존 JRE 비교**

| 비교 항목 | **GraalVM (JIT + AOT)** | **JRE (Java Runtime Environment)** |
| --- | --- | --- |
| **실행 방식** | JIT(Just-In-Time) 컴파일러 또는 AOT(Ahead-Of-Time) 네이티브 컴파일 | JVM 위에서 바이트코드 실행 |
| **속도** | **AOT 사용 시 매우 빠름** (JVM 없이 실행) | JVM이 필요하므로 부팅 속도가 상대적으로 느림 |
| **메모리 사용량** | **AOT 사용 시 메모리 사용량 적음** | JIT 최적화로 빠르지만, JVM 메모리 오버헤드 존재 |
| **시작 속도** | **AOT 사용 시 즉시 실행 (네이티브 바이너리)** | JVM 초기화 필요 (느림) |
| **배포 방식** | `.jar` (JIT) 또는 **네이티브 실행 파일 (`.exe`, `.bin`)** | `.jar` (JVM 필요) |
| **JVM 필요 여부** | JIT 모드에서는 JVM 필요, AOT 모드에서는 **JVM 없이 실행 가능** | JVM이 필요함 |
| **컨테이너 크기** | **AOT 사용 시 매우 작음 (~40MB 이하)** | JRE 포함 시 최소 100MB 이상 |
| **호환성** | 일부 Java 라이브러리 미지원 (`reflection` 제한 있음) | 모든 Java 애플리케이션과 호환 |

---

<br>

### 3️⃣ **GraalVM을 Docker에서 사용하기**

```
FROM ghcr.io/graalvm/graalvm-ce:latest AS build
RUN gu install native-image
WORKDIR /app
COPY step01_basic-0.0.1-SNAPSHOT.jar app.jar
RUN native-image -jar app.jar ryanspring

FROM gcr.io/distroless/static
COPY --from=build /app/ryanspring ryanspring
CMD ["/ryanspring"]

```
![image](https://github.com/user-attachments/assets/a7a34168-64fc-42e8-8544-9da82ec0d91f)


✔ jre 기반으로 build 된 컨테이너 이미지 size는 205MB

✔ GraalVM 기반으로 build 된 컨테이너 이미지 size는 13.7MB

<br>

### **🎯 결론: GraalVM을 언제 사용할까?**

✔ **기존 Java 애플리케이션 속도를 높이고 싶을 때**

✔ **서버리스 환경 (AWS Lambda, Kubernetes 등)에 가볍게 배포하고 싶을 때**

✔ **시작 속도가 중요한 애플리케이션 (CLI, 마이크로서비스)에서 사용**

✔ **Spring Boot + GraalVM으로 경량화된 네이티브 실행 파일 만들 때**
