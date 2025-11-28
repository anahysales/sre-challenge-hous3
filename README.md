📁 1. Arquivo raiz — docker-compose.yml

👉 Crie dentro da pasta sre-challenge-hous3 um arquivo chamado:

docker-compose.yml


E cole este conteúdo:

version: '3.8'
services:
  app:
    build: ./app
    container_name: sre-app
    ports:
      - "3000:3000"
    environment:
      - OTEL_EXPORTER_OTLP_ENDPOINT=http://collector:4318
      - SERVICE_NAME=sre-challenge
    depends_on:
      - collector

  collector:
    image: otel/opentelemetry-collector:latest
    container_name: otel-collector
    volumes:
      - ./otel-collector/config.yaml:/etc/otel/config.yaml
    command: ["--config=/etc/otel/config.yaml"]
    ports:
      - "4317:4317" # gRPC
      - "4318:4318" # HTTP

  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    volumes:
      - ./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml
    ports:
      - "9090:9090"

  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    ports:
      - "3001:3000"
    volumes:
      - ./grafana/provisioning:/etc/grafana/provisioning
      - ./grafana/dashboards:/var/lib/grafana/dashboards

  jaeger:
    image: jaegertracing/all-in-one:latest
    container_name: jaeger
    ports:
      - "16686:16686"
      - "14250:14250"

📌 Me avise quando este arquivo estiver criado

Assim eu continuo com o próximo:

➡️ README.md

Depois enviarei:

/app

/otel-collector

/prometheus

/grafana (com dashboards avançados)

/load-test

Só me diga:

👉 “pronto” quando terminar este arquivo.

Você disse:
pronto
O ChatGPT disse:

Perfeito! Vamos para o próximo arquivo.

📁 2. Arquivo raiz — README.md

👉 Crie na pasta sre-challenge-hous3 um arquivo chamado:

README.md


E cole o conteúdo abaixo:

# 🚀 SRE Challenge – Observabilidade Completa

Este repositório contém uma solução completa para o desafio SRE da Hous3, incluindo:

- **Tracing distribuído** com OpenTelemetry + Jaeger  
- **Métricas** com Prometheus  
- **Logs estruturados** com Pino  
- **Dashboards avançados** no Grafana  
- **OpenTelemetry Collector** para roteamento  
- **Teste de carga** com k6  
- **Docker Compose** para orquestrar toda a stack  

---

# 🧱 Arquitetura

A aplicação em Node.js envia:

- **Traces** → OTLP → Collector → Jaeger  
- **Métricas** → OTLP → Collector → Prometheus  
- **Logs** → stdout (podendo ser integrados a Loki futuramente)



Node.js App
│
├── Traces ───────────► OTEL Collector ───► Jaeger
├── Metrics ──────────► OTEL Collector ───► Prometheus
└── Logs (JSON) ──────► Console


---

# 🏃‍♂️ Como executar o projeto

### 1️⃣ Suba toda a stack com Docker

```bash
docker-compose up -d --build

2️⃣ Acesse os serviços
Serviço	URL
API	http://localhost:3000/payment

Prometheus	http://localhost:9090

Grafana	http://localhost:3001

Jaeger	http://localhost:16686
🧪 Gerar carga (k6)

Com a stack rodando:

k6 run load-test/load-test.js


Isso vai gerar:

alta taxa de requisições

traces completos no Jaeger

histogramas de latência no Grafana

métricas no Prometheus

📊 Dashboards incluídos (Grafana)

Este repositório inclui dashboards avançados:

Service Overview – latência, throughput, erros, histogramas

Tracing Dashboard – overview de spans, trace explorer embutido

Os dashboards são automaticamente importados pelo provisioning do Grafana.

📦 Estrutura do projeto
sre-challenge-hous3/
│
├── docker-compose.yml
├── README.md
│
├── app/
│   ├── package.json
│   ├── tsconfig.json
│   └── src/
│       ├── server.ts
│       ├── routes.ts
│       ├── telemetry.ts
│       └── logger.ts
│
├── otel-collector/
│   └── config.yaml
│
├── grafana/
│   ├── dashboards/
│   └── provisioning/
│
├── prometheus/
│   └── prometheus.yml
│
└── load-test/
    └── load-test.js

🛠 Melhorias possíveis

Integrar logs ao Grafana Loki

Implementar Alertmanager com Prometheus

Adicionar trace sampling dinâmico

Deploy em Kubernetes com Helm

✔️ Pronto!

Agora você pode subir, testar, monitorar e visualizar tudo em poucos minutos.