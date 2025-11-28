

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
