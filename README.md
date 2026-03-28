
# 📊 Monitoring Spring Boot Application using Prometheus, Grafana & Node Exporter

This project demonstrates how to monitor a Spring Boot application using:

* **Prometheus** (metrics collection)
* **Grafana** (visualization)
* **Node Exporter** (system metrics)

---

# 🖥️ System Requirements

## 🔹 Application Server

* RAM: 8 GB
* CPU: 2 Core
* Storage: 20 GB
* OS: Ubuntu 24.04
* Instance Type: `c7i.flex large`

---

# 🚀 Application Setup

```bash
sudo su -
apt update
```

## 📌 Install Java

```bash
apt install openjdk-17-jdk -y
```

## 📌 Clone Project

```bash
git clone https://github.com/sakit333/sak_group_spring_mysql_prometheus.git
ls
cd sak_group_spring_mysql_prometheus
```

## 📌 Install Maven

```bash
apt install maven -y
```

## 📌 Install MySQL

```bash
bash <(curl -sL https://tinyurl.com/mr4brnzj)
mysql --version
```

## 📌 Configure Application

```bash
vi src/main/resources/application.properties
```

* Provide your IPv4 address and application port

## 📌 Build & Run Application

```bash
mvn clean package -DskipTests
cd target/
ls
java -jar spring_app_sak-0.0.1-SNAPSHOT.jar
```

---

# 📡 Node Exporter Setup

## 📌 Install Node Exporter

```bash
# Installation script already provided
```

## 🌐 Access Metrics

```
http://<your-server-ip>:9100/metrics
```

## 🧠 Manage Node Exporter

```bash
sudo systemctl status node_exporter
sudo systemctl restart node_exporter
sudo journalctl -u node_exporter -f
```

---

# ⚙️ Spring Boot Actuator Configuration

## 📌 Add Dependencies (pom.xml)

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>

<dependency>
    <groupId>io.micrometer</groupId>
    <artifactId>micrometer-registry-prometheus</artifactId>
</dependency>
```

## 📌 Update application.properties

```properties
management.endpoints.web.exposure.include=health,info,prometheus
management.endpoint.prometheus.enabled=true
management.metrics.export.prometheus.enabled=true
management.endpoint.health.show-details=always
```

---

# 📊 Prometheus Setup

## 🔹 System Requirements

* RAM: 4 GB
* CPU: 2 Core
* Storage: 15 GB
* OS: Ubuntu 24.04
* Instance Type: `c7i.flex large`

## 📌 Install Prometheus

```bash
bash <(curl -sL https://tinyurl.com/57xn7sf8)
```

## 🌐 Access Prometheus UI

```
http://<your-server-ip>:9090
```

## 📁 Important Paths

* Config: `/etc/prometheus/prometheus.yml`
* Data: `/var/lib/prometheus`

## 🧠 Manage Prometheus

```bash
sudo systemctl start prometheus
sudo systemctl stop prometheus
sudo systemctl restart prometheus
sudo systemctl status prometheus
```

---

## 📌 Configure Prometheus Targets

```bash
vi /etc/prometheus/prometheus.yml
```

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'appserver'
    static_configs:
      - targets: ['65.0.110.69:9100']

  - job_name: 'spring-boot-app'
    metrics_path: '/actuator/prometheus'
    static_configs:
      - targets: ['65.0.110.69:8085']
```

---

# ✅ Verify Metrics

## 📌 Spring Boot Metrics

```
http://<APP_SERVER_IP>:8085/actuator/prometheus
```

## 📌 Prometheus Targets

```
http://<PROMETHEUS_SERVER_IP>:9090
→ Status → Targets
```

---

# 📈 Prometheus Queries

## 🔹 Application Status

```
up{job="spring-boot-app"}
```

## 🔹 Total HTTP Requests

```
http_server_requests_seconds_count
```

## 🔹 Request Rate

```
rate(http_server_requests_seconds_count[1m])
```

## 🔹 JVM Memory Usage

```
jvm_memory_used_bytes
```

## 🔹 Thread Count

```
jvm_threads_live_threads
```

---

# 📊 Grafana Setup

## 🔹 System Requirements

* RAM: 4 GB
* CPU: 2 Core
* Storage: 15 GB
* OS: Ubuntu 24.04
* Instance Type: `c7i.flex large`

## 📌 Install Grafana

```bash
# Install Grafana using official script/package
```

## 🌐 Access Grafana UI

```
http://<your-server-ip>:3000
```

---

## 🧠 Manage Grafana

```bash
sudo systemctl status grafana-server
sudo systemctl restart grafana-server
sudo journalctl -u grafana-server -f
```

---

# 🔗 Connect Prometheus to Grafana

1. Open Grafana UI
2. Login with username & password
3. Go to:

   ```
   Connections → Data Sources → Add Data Source
   ```
4. Select **Prometheus**
5. Enter URL:

   ```
   http://<PROMETHEUS_SERVER_IP>:9090/
   ```
6. Click **Save & Test**

---

# 📊 Create Dashboard

1. Go to:

   ```
   Dashboard → Create Dashboard → Import
   ```
2. Visit:

   ```
   https://grafana.com/dashboards
   ```
3. Search for:

   * Node Exporter Dashboard
4. Copy Dashboard ID
5. Paste into Grafana
6. Select Prometheus Data Source
7. Import

---

# 🎯 Summary

* Spring Boot exposes metrics via **Actuator**
* Prometheus **scrapes metrics**
* Node Exporter provides **system metrics**
* Grafana visualizes everything in **dashboards**

  ---
  Scripted by Thanu

