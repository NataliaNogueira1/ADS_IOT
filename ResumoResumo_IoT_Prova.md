# 📡 RESUMO IoT/IIoT — CONSULTA DE PROVA

---

## 1. FUNDAMENTOS: IoT vs IIoT

```
IoT (Internet of Things)          IIoT (Industrial IoT)
─────────────────────────────     ──────────────────────────────────
Foco: consumidor / comodidade     Foco: produção / missão crítica
Ubiquidade                        Alta precisão
Conforto                          Extrema robustez
Baixo custo                       Segurança cibernética rigorosa
Tolerante a falhas                Alta disponibilidade (24/7)
Ex: smartwatch, lâmpada           Ex: robô industrial, sensor de pressão
```

### Pilares da Estrutura IoT
```
┌─────────────────────────────────────────────────────┐
│  HARDWARE          CONECTIVIDADE      INTELIGÊNCIA   │
│  ─────────         ────────────       ────────────   │
│  Sensores          Wi-Fi / BLE        Cloud          │
│  Atuadores         LPWAN              Edge Computing │
│  Gateways          5G / Ethernet      On-device AI   │
└─────────────────────────────────────────────────────┘
```

---

## 2. INDÚSTRIA 4.0

| Conceito | Descrição |
|---|---|
| **Gêmeos Digitais** | Réplica virtual de um processo/máquina real; simula comportamentos sem risco físico |
| **Manutenção Preditiva** | Usa dados contínuos de sensores para prever falhas ANTES que ocorram |
| **RAMI 4.0** | Modelo de referência arquitetural para Indústria 4.0 (camadas: negócio, funcional, informação, comunicação, integração, ativo) |

### Convergência TI / TO
```
TI (Tecnologia da Informação)     TO (Tecnologia Operacional)
─────────────────────────────     ──────────────────────────
Prioridade: CONFIDENCIALIDADE     Prioridade: DISPONIBILIDADE
Dados, sistemas, redes            Máquinas, CLPs, SCADA
Segurança = CIA                   Segurança = uptime
```
> ⚠️ Conflito: TI quer atualizar/reiniciar; TO não pode parar a produção.

---

## 3. HARDWARE, DISPOSITIVOS E SENSORES

### Edge Devices (Dispositivos de Borda)
- Operam em condições hostis: vibrações, temperaturas extremas, poeira
- Funcionam continuamente com pouca manutenção
- Usam **Energy Harvesting**: coletam energia do ambiente (solar, vibração, calor)

### Tipos de Sensores
```
Tipo              Princípio                    Aplicação
──────────────    ─────────────────────────    ──────────────────────
MEMS              Microeletromecânico          Vibração, aceleração
Piezoelétrico     Pressão → tensão elétrica    Vibração, impacto
Térmico           Temperatura                  Monitoramento de motores
Acústico          Ultrassom / emissão acústica Detecção de vazamentos
Pressão/Fluido    Pressão diferencial          Tubulações, hidráulica
```

### Gateway Industrial
```
[Máquina Legada] ──(Modbus/RS-485)──► [GATEWAY] ──(MQTT/HTTPS)──► [Nuvem]
                                          │
                                    Edge Computing
                                    (filtra, processa,
                                     converte protocolos)
```

### Segurança em Hardware
- **Raiz de Confiança (Root of Trust):** chip seguro que garante identidade do dispositivo
- ❌ Senhas padrão (default) → inaceitável
- ❌ Credenciais hard-coded no código → inaceitável
- ✅ Certificados únicos por dispositivo
- ✅ Secure Boot, TPM

### Retrofit
> Modernizar máquinas **legadas** acoplando sensores externos (ex: vibração) e módulos de baixo custo, sem substituir o equipamento inteiro.

---

## 4. EDGE COMPUTING vs CLOUD

### Por que NÃO enviar tudo para a nuvem?
```
Problema              Consequência
────────────────────  ──────────────────────────────
Alta latência         Ação lenta (inaceitável em controle)
Consumo de banda      Custo alto, congestionamento
Dependência de rede   Falha de conectividade = parada
Custo de armazenamento Big Data industrial é enorme
Privacidade           Dados sensíveis saem da fábrica
```

### Edge vs Fog vs Cloud
```
┌──────────────────────────────────────────────────────────────────┐
│  SENSOR/ATUADOR                                                  │
│       │                                                          │
│  [EDGE] ← processa na borda, ação em milissegundos               │
│       │   filtra ruído, opera sem internet (resiliência)         │
│  [FOG]  ← camada intermediária, agrega dados de múltiplos edges  │
│       │                                                          │
│  [CLOUD] ← Big Data histórico, treina IA, análise estratégica    │
└──────────────────────────────────────────────────────────────────┘
```

| Camada | Latência | Escala | Função |
|---|---|---|---|
| Edge | Milissegundos | Local | Ação tática imediata |
| Fog | Segundos | Regional | Agregação intermediária |
| Cloud | Segundos/minutos | Global | Estratégia, IA, histórico |

---

## 5. PROTOCOLOS DE COMUNICAÇÃO IIoT

### Comparativo Rápido
```
Protocolo   Modelo          Broker?   Foco Principal              Ideal para
──────────  ──────────────  ────────  ──────────────────────────  ──────────────────────
MQTT        Pub/Sub         ✅ Sim    Leveza, telemetria           Sensores, redes limitadas
XMPP        Mensagens XML   ❌ Não    Identidade, presença, estado Comandos diretos, chat
DDS         Peer-to-Peer    ❌ Não    Tempo real crítico, zero lat Robótica, defesa, veículos
AMQP        Filas           ✅ Sim    Garantia de entrega, roteam  Integração corporativa
OPC UA      Cliente-Servidor❌ Não    Semântica rica, CLP/SCADA    Automação industrial local
```

### MQTT — Detalhes
```
Publisher (Sensor)                    Subscriber (Dashboard)
      │                                       │
      └──── publica em "fabrica/temp" ──►  [BROKER]  ◄── subscreve "fabrica/temp"
                                         (Mosquitto)
```
- **QoS 0:** no máximo 1 vez (pode perder)
- **QoS 1:** pelo menos 1 vez (pode duplicar)
- **QoS 2:** exatamente 1 vez (mais lento)
- **Retain:** broker guarda última mensagem para novos subscribers
- **Will:** mensagem enviada automaticamente se cliente desconectar

### OPC UA
- Padrão aberto da automação industrial
- Fornece **semântica rica**: não só o valor, mas o significado do dado
- Integra com CLP, SCADA, MES
- Suporta segurança (autenticação, criptografia)

---

## 6. CONECTIVIDADE E REDES

### LPWAN (Low Power Wide Area Network)
```
Tecnologia   Alcance    Consumo   Taxa de dados   Latência   Uso típico
──────────   ────────   ───────   ─────────────   ────────   ──────────────────
LoRaWAN      ~15 km     Muito baixo  0,3–50 kbps  Alta       Medidores, rastreio
SigFox       ~50 km     Muito baixo  100 bps       Alta       Alertas simples
NB-IoT       ~10 km     Baixo        ~200 kbps     Média      Medidores urbanos
```
> ⚠️ LPWAN: longo alcance + baixo consumo, MAS baixa taxa e alta latência.

### Redes Industriais Cabeadas
- **PROFINET / EtherCAT:** Ethernet industrial para controle determinístico
- **TSN (Time-Sensitive Networking):** extensão do Ethernet padrão para garantir **determinismo absoluto** (pacotes chegam no tempo certo)
- Indispensável em **controle de malha fechada** (ex: robô que precisa de resposta em <1ms)

### 5G na Indústria
```
URLLC (Ultra-Reliable Low-Latency Communication)
  → Latência < 1ms
  → Confiabilidade 99,9999%
  → Substitui cabos em robótica e AGVs (veículos guiados automaticamente)
```

---

## 7. SIMULADORES DE REDE

| Simulador | Sistema Operacional | Foco | Característica |
|---|---|---|---|
| **Cooja** | Contiki OS | Redes de sensores (WSN) | Simula nós reais com Contiki |
| **NS-3** | Qualquer | Redes em geral | Matemático, altíssima escala |
| **TOSSIM** | TinyOS | Redes de sensores | Simula código TinyOS real |

> Uso: validar infraestruturas sem gastos físicos antes do deploy real.

---

## 8. STACK MING — Plataforma IIoT Open Source

```
┌─────────────────────────────────────────────────────────────────┐
│                        STACK MING                               │
│                                                                 │
│  [Sensor/Dispositivo]                                           │
│         │  MQTT publish                                         │
│         ▼                                                       │
│  ┌─────────────┐                                                │
│  │  M osquitto │  ← Broker MQTT: recebe telemetria dos sensores │
│  └──────┬──────┘                                                │
│         │  dados brutos                                         │
│         ▼                                                       │
│  ┌─────────────┐                                                │
│  │  N ode-RED  │  ← Motor de integração low-code (fluxos)       │
│  └──────┬──────┘    transforma, filtra, roteia dados            │
│         │  dados processados                                    │
│         ▼                                                       │
│  ┌─────────────┐                                                │
│  │  I nfluxDB  │  ← Banco de dados de séries temporais (TSDB)   │
│  └──────┬──────┘    armazena dados com timestamp                │
│         │  consulta (Flux/InfluxQL)                             │
│         ▼                                                       │
│  ┌─────────────┐                                                │
│  │  G rafana   │  ← Dashboard: visualiza métricas, alertas,     │
│  └─────────────┘    predições para o operador                   │
└─────────────────────────────────────────────────────────────────┘
```

### Detalhes de cada componente

| Letra | Componente | Tipo | Função |
|---|---|---|---|
| **M** | Mosquitto | Broker MQTT | Gerencia pub/sub, recebe telemetria contínua |
| **I** | InfluxDB | Banco TSDB | Armazena dados temporais, otimizado para séries |
| **N** | Node-RED | Orquestrador | Liga MQTT → InfluxDB, lógica low-code por fluxos |
| **G** | Grafana | Dashboard | Visualiza dados, cria alertas e painéis interativos |

### Node-RED
- Programação **low-code** baseada em fluxos (drag & drop)
- Captura, transforma e orquestra lógicas de integração
- Conecta protocolos diferentes (MQTT, HTTP, banco de dados)
- Baseado em Node.js

### InfluxDB — Banco de Séries Temporais
- Dados sempre associados a um **timestamp**
- Otimizado para escrita e leitura de alta frequência
- Linguagem de consulta: **Flux** (ou InfluxQL legado)
- Conceitos: `measurement` (tabela), `field` (valor), `tag` (metadado indexado)

---

## 9. DOCKER E DEPLOY DO STACK MING

```yaml
# docker-compose.yml — estrutura típica
services:
  mosquitto:    # broker MQTT, porta 1883
  influxdb:     # banco TSDB, porta 8086
  nodered:      # orquestrador, porta 1880
  grafana:      # dashboard, porta 3000
```

- Cada serviço roda em **container isolado**
- Comunicação entre containers via rede Docker interna
- Volumes persistem dados mesmo após reiniciar containers

---

## 10. SEGURANÇA EM IIoT

```
Camada          Ameaça                    Proteção
──────────────  ────────────────────────  ──────────────────────────────
Hardware        Acesso físico, clonagem   Root of Trust, Secure Boot
Firmware        Código malicioso          Assinatura digital, OTA seguro
Comunicação     Interceptação (MITM)      TLS/SSL, certificados
Autenticação    Acesso não autorizado     Certificados únicos, MFA
Rede            Ataques DDoS, varredura   Segmentação, firewall, VPN
Dados           Vazamento, manipulação    Criptografia em repouso
```

### Princípios
- **Zero Trust:** nunca confiar, sempre verificar
- **Princípio do menor privilégio:** cada dispositivo acessa só o necessário
- **Defense in Depth:** múltiplas camadas de segurança

---

## 11. MAPA MENTAL GERAL

```
                          ┌─────────────┐
                          │   IoT/IIoT  │
                          └──────┬──────┘
           ┌──────────────────────┼──────────────────────┐
           ▼                      ▼                      ▼
    ┌─────────────┐       ┌──────────────┐       ┌─────────────┐
    │  HARDWARE   │       │CONECTIVIDADE │       │INTELIGÊNCIA │
    └──────┬──────┘       └──────┬───────┘       └──────┬──────┘
           │                     │                      │
    ┌──────┴──────┐       ┌──────┴───────┐       ┌──────┴──────┐
    │ Sensores    │       │ LPWAN        │       │ Edge        │
    │ Atuadores   │       │ 5G / URLLC   │       │ Fog         │
    │ Gateways    │       │ TSN/Ethernet │       │ Cloud       │
    │ Edge Devices│       │ MQTT/DDS/OPC │       │ IA/ML       │
    └─────────────┘       └─────────────┘       └─────────────┘

    ┌─────────────────────────────────────────────────────────┐
    │                    STACK MING                           │
    │  Mosquitto → Node-RED → InfluxDB → Grafana              │
    └─────────────────────────────────────────────────────────┘

    ┌─────────────────────────────────────────────────────────┐
    │                  INDÚSTRIA 4.0                          │
    │  Gêmeos Digitais | Manutenção Preditiva | RAMI 4.0      │
    │  Convergência TI/TO | Retrofit | Edge Computing         │
    └─────────────────────────────────────────────────────────┘
```

---

## 12. TABELA DE REVISÃO RÁPIDA

| Conceito | Palavra-chave |
|---|---|
| MQTT | Pub/Sub, broker, leve, telemetria |
| DDS | Peer-to-peer, tempo real, sem broker |
| AMQP | Filas, garantia de entrega, corporativo |
| XMPP | XML, presença, identidade, estado |
| OPC UA | Semântica, CLP, SCADA, cliente-servidor |
| LoRaWAN | Longo alcance, baixo consumo, baixa taxa |
| 5G URLLC | Baixíssima latência, robótica, AGV |
| TSN | Determinismo, Ethernet industrial |
| Edge | Borda, latência mínima, resiliência |
| Fog | Intermediário, agrega edges |
| Cloud | Histórico, IA, estratégico |
| Mosquitto | Broker MQTT do stack MING |
| InfluxDB | Séries temporais, timestamp |
| Node-RED | Low-code, fluxos, orquestração |
| Grafana | Dashboard, alertas, visualização |
| Gêmeo Digital | Réplica virtual, simulação |
| Manutenção Preditiva | Prever falha antes de ocorrer |
| Root of Trust | Segurança hardware, identidade |
| Retrofit | Modernizar legado com sensores |
| Energy Harvesting | Energia do ambiente |
| RAMI 4.0 | Modelo referência Indústria 4.0 |
| TI vs TO | Confidencialidade vs Disponibilidade |
| Cooja | Simulador Contiki OS |
| NS-3 | Simulador matemático, grande escala |
| TOSSIM | Simulador TinyOS |
