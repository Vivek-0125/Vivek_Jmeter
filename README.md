🚀 HTTPBin APIs Performance Test Suite (JMeter + Docker Compose)
📌 Overview

This project evaluates the performance and stability of the HTTPBin API hosted by Postman Labs.
It measures key performance indicators like response time, throughput, and error rate under various concurrent user loads.

🧩 Test Scope
HTTP Method	Endpoint	Description
GET	/get	Retrieve data
POST	/post	Submit data
PUT	/put	Update data
PATCH	/patch	Modify data
DELETE	/delete	Remove data

Objective: Validate performance KPIs under different load levels.

⚙️ Performance KPIs
KPI	Description	Target
Avg Response Time	Mean time to serve requests	≤ 1 sec
90th Percentile	High-end latency boundary	≤ 480 ms
95th Percentile	High-end latency boundary	≤ 620 ms
Throughput	Requests/sec	≥ 100 req/s
Error Rate	Failed requests	≤ 1%
CPU Utilization	Processing load	≤ 75%
Memory Utilization	RAM usage	≤ 70%
🧱 Assumptions

Stable server environment with no background load

Consistent network conditions

Independent user requests (no caching)

Sufficient test duration for steady-state results

🧪 JMeter Test Structure
Thread Group (10, 100, 200, 500 users)
├── HTTP Requests (/get, /post, /put, /patch, /delete)
├── User Defined Variables
├── JSR223 Assertions (Response Time, Error Rate)
├── Listeners:
│   ├── Summary & Aggregate Reports
│   ├── View Results Tree/Table
│   └── Simple Data Writer (results.jtl)


HTML reports are generated automatically for quick analysis.

📂 Project Structure
├── docker-compose.yml          # Service setup
├── httpbin_PTTest              # JMeter Test Plan
├── results/                    # Raw JTL outputs
├── report-html/                # HTML reports
└── README.md                   # Documentation

🐳 Docker Setup

Requirements: Docker Desktop v4.48+, Docker Compose

Commands:

docker pull kennethreitz/httpbin
docker run -d --name=httpbin -p 80:80 kennethreitz/httpbin


Verify: http://localhost/get

📊 cAdvisor Setup (Monitoring)
docker run -d --name=cadvisor \
  -v /:/rootfs:ro -v /var/run:/var/run:ro \
  -v /sys:/sys:ro -v /var/lib/docker/:/var/lib/docker:ro \
  -p 8081:8080 gcr.io/cadvisor/cadvisor:latest


Access Dashboard: http://localhost:8081

Metrics: CPU, Memory, Network I/O, Filesystem I/O
Export: http://localhost:8081/metrics > cadvisor_metrics.txt

🧾 Test Execution
jmeter -n -t /tests/httpbin_test.jmx \
       -l /results/httpbin_results.jtl \
       -e -o /results/httpbin_report

⚙️ Resource Optimization (WSL Config)
[wsl2]
memory=6GB
processors=8
swap=1GB
localhostForwarding=true

📈 Performance Testing Approach
Phase	Description
Requirement Analysis	Identify NFRs, SLAs, key transactions
Workload Modeling	Define users, duration, ramp-up/down
Scenario Design	Load (10–500 users), Stress, Endurance, Spike tests
Monitoring	Track CPU, Memory, Throughput, Errors
Reporting	Generate HTML Dashboard and analyze KPIs
📁 Results

Raw Results: results/results.jtl

HTML Report: report-html/index.html

🧰 Tools Used

Apache JMeter – Performance testing

Docker Compose – Container orchestration

HTTPBin – REST API endpoints

cAdvisor – Container monitoring

✅ Next Steps

Integrate with CI/CD (Jenkins)

Add SLA-based assertions in JMeter

Extend test coverage to advanced endpoints
