
# RESUMO IoT/IIoT — CONSULTA DE PROVA

---

## 1. IoT — Conceito Base

> Qualquer objeto fisico conectado com outro pela internet.

### Bases do IoT

| Pilar | O que e |
|---|---|
| **Hardware** | Sensores e atuadores |
| **Conectividade** | Redes e protocolos |
| **Processamento** | Cloud / Edge / On-device / IA / Analytics |

### Conceitos fundamentais
- A tecnologia esta em todo lugar de forma invisivel
- Conectividade entre dispositivos
- Dispositivos tomam decisoes simples sem intervencao humana
- Tem sensores

### Exemplo: Fazenda Inteligente
- **Hardware:** sensor de umidade no solo
- **Conectividade:** LoRaWAN transmite o dado a 10km sem fio
- **Inteligencia:** algoritmo na nuvem decide acionar a irrigacao

---

## 2. IIoT — IoT na Industria

| | IoT (consumidor) | IIoT (industrial) |
|---|---|---|
| Foco | Comodidade | Eficiencia e seguranca |
| Falha tolerada? | Sim (inconveniente) | Nao (pode matar gente) |
| Precisao | Moderada | Altissima |
| Ciclo de vida | 2-5 anos | 10-30 anos |
| Exemplo | Smartwatch | Sensor de pressao em refinaria |

Hardware IIoT precisa:
- Operar entre **-40C e 85C**
- Resistir a vibracao, poeira e umidade (certificacoes IP)
- Funcionar por **anos sem manutencao**
- Ciclo de vida longo — compativel com maquinas de 20 anos

---

## 3. Industria 4.0

| Conceito | Descricao |
|---|---|
| **Gemeo Digital** | Copia virtual de equipamento real alimentada por sensores. Testa cenarios sem risco fisico. |
| **Manutencao Preditiva** | Age quando dados indicam desgaste — antes de qualquer falha visivel. |
| **RAMI 4.0** | Modelo de referencia: Dispositivo -> Comunicacao -> Processamento -> Armazenamento -> Visualizacao |
| **OEE** | Disponibilidade x Performance x Qualidade — calculado em tempo real via IIoT |

### Manutencao: Reativa vs Preventiva vs Preditiva

| Tipo | Quando age | Custo | Risco |
|---|---|---|---|
| Reativa | Apos quebra | Alto | Alto |
| Preventiva | Calendario fixo | Medio | Medio |
| Preditiva | Dados indicam desgaste | Baixo | Baixo |

### Convergencia TI / OT

| | TI | OT |
|---|---|---|
| Prioridade | **Confidencialidade** | **Disponibilidade** |
| Foco | Dados, sistemas, redes | Maquinas, CLPs, SCADA |
| Conflito | Quer aplicar patch urgente | Nao pode reiniciar — linha produzindo |

---

## 6. Protocolos de Comunicacao

### HTTP vs MQTT
- **HTTP:** sincrono, sempre precisa de resposta. Dispositivo limitado nao aguenta.
- **MQTT:** assíncrono, usa broker intermediario. Dispositivo nao precisa esperar.

| | HTTP | MQTT |
|---|---|---|
| Modelo | Requisicao/Resposta | Publish/Subscribe |
| Endereco | dominio.com/mobile/home | dominio.com + topico: mobile/home |
| Body | Dados do formulario | payload |
| Sincrono? | Sim | Nao |

### MQTT — QoS
| Nivel | Comportamento | Quando usar |
|---|---|---|
| QoS 0 | Dispara e esquece | Leituras onde perder uma nao importa |
| QoS 1 | Entrega pelo menos 1x (pode duplicar) | Alertas que nao podem ser perdidos |
| QoS 2 | Exatamente 1x (mais lento) | Comandos de atuacao (abrir valvula) |

- **Retain:** broker guarda ultima mensagem para novos subscribers
- **Will:** mensagem enviada automaticamente se cliente desconectar

### Comparativo Geral
| Protocolo | Broker? | Latencia | Melhor para |
|---|---|---|---|
| MQTT | Sim | Baixa | Telemetria de sensores |
| XMPP | Sim | Media | Presenca/estado do dispositivo |
| DDS | Nao | Minima | Robotica, tempo real critico |
| AMQP | Sim | Media | Integracao corporativa, filas |
| OPC UA | Nao (C/S) | Baixa | CLP/SCADA, semantica rica |
| Modbus | Nao | — | Protocolo legado, RS-485/TCP |

### XMPP
- Mensagens XML com **presenca** (online/offline/ocupado)
- Envia comandos diretos a dispositivo especifico

### DDS
- Peer-to-peer, sem broker, data-centric
- Latencia minima absoluta — robotica cirurgica, veiculos autonomos

### AMQP
- Filas com exchanges; mensagem persiste ate ser processada
- Integracao com ERP, SAP

### OPC UA
- Entrega contexto semantico (nao so o valor, mas unidade, limite, status)
- Seguranca nativa: criptografia + certificados X.509
- Comunicacao entre CLP e SCADA na rede local

---

## 7. Conectividade e Redes

### Escolhendo a tecnologia certa

Pergunta central: qual o trade-off entre alcance, consumo e velocidade que minha aplicacao tolera?

| Tecnologia | Alcance | Consumo | Taxa de dados | Caso de uso |
|---|---|---|---|---|
| Wi-Fi | ~100m | Alto | Ate 600 Mbps | Cameras, monitoramento intenso |
| BLE | ~50m | Muito baixo | Medio | Wearables, beacons |
| Zigbee | ~300m (mesh) | Baixo | 250 kbps | Redes de sensores em predios |
| LoRaWAN | >10 km | Muito baixo | ~50 kbps | Sensores rurais, medidores remotos |
| SigFox | >10 km | Muito baixo | 100 bps | Telemetria simples, medidores |
| NB-IoT | Cobertura 4G | Medio | ~50 kbps | Ativos urbanos com QoS garantido |
| LTE-M | Cobertura 4G | Medio | Altos volumes | Rastreamento, volumes maiores |
| 5G URLLC | Amplo | Variavel | Gbps, 1.2ms | Robotica, AGVs, cirurgia remota |
| EtherCAT | Local (cabo) | — | <0.5ms latencia | Controle de robos, malha fechada |

### LoRaWAN na pratica

- Usa **Chirp Spread Spectrum (CSS):** alterna entre frequencias para ir para a menos trafegada
- Pro: longa distancia. Contra: nao aguenta quase nada de peso, so dispositivo IoT mesmo

Classes LoRaWAN:

| Classe | Caracteristica | Prioridade |
|---|---|---|
| **A** | Protocolo ALOHA, hibernacao profunda | Bateria em 1o lugar |
| **B** | Comunicacao sincronizada, janelas programadas | Intermediario |
| **C** | Escuta continua, sem latencia | Exige fonte de energia externa |

> Exemplo: produtor rural instala sensores de umidade em 50 pontos em 500 hectares. Um gateway cobre tudo, baterias duram mais de 10 anos.

### SigFox
- LPWAN ultra-narrowband, longo alcance (>10km), baixo consumo
- Principalmente unidirecional (0G)
- Usa ISM sub-GHz
- Ideal para telemetria simples

### NB-IoT e LTE-M
- **NB-IoT (LTE Cat-NB):** pequenos volumes de dados, cobertura celular
- **LTE-M:** altos volumes de dados, cobertura celular

### 5G e URLLC

O 5G tem tres modos:
- **eMBB** (Enhanced Mobile Broadband): velocidade alta para streaming, AR/VR
- **mMTC** (Massive Machine Type Communications): suporta milhoes de sensores por km2
- **URLLC** (Ultra-Reliable Low Latency): latencia de **1.2ms** com altissima confiabilidade

> URLLC permite substituir cabos em robotica industrial. Com 5G URLLC, robos moveis sem fio com confiabilidade de cabo.

### TSN — Time-Sensitive Networking
- Extensao do Ethernet padrao (IEEE 802.1) que adiciona garantias de tempo
- Trafego critico tem faixa exclusiva garantida
- Video de camera e controle de robo coexistem na mesma rede sem interferencia
- Indispensavel em **controle de malha fechada** (ex: robo que precisa de resposta em <1ms)
- Une TI e OT na mesma rede

---

---

## 8. Simuladores de Rede
| Simulador | Foco | Quando usar |
|---|---|---|
| **Cooja** | Contiki OS, sensores com recursos limitados | Simular nos reais com Contiki |
| **NS-3** | Matematico, altissima escala | Pesquisa academica, grandes redes |
| **TOSSIM** | TinyOS | Simular comportamento de motes TinyOS |
| **Wokwi** | Arduino/ESP32 no browser | Prototipagem rapida sem hardware fisico |

> Uso: validar infraestruturas sem gastos fisicos antes do deploy real.

---

## 8. Simuladores de Rede
| Simulador | Foco | Quando usar |
|---|---|---|
| **Cooja** | Contiki OS, sensores com recursos limitados | Simular nos reais com Contiki |
| **NS-3** | Matematico, altissima escala | Pesquisa academica, grandes redes |
| **TOSSIM** | TinyOS | Simular comportamento de motes TinyOS |
| **Wokwi** | Arduino/ESP32 no browser | Prototipagem rapida sem hardware fisico |

> Uso: validar infraestruturas sem gastos fisicos antes do deploy real.

---

## 9. Stack MING

MING = Mosquitto + InfluxDB + Node-RED + Grafana

- Tira a dependencia do dispositivo com a aplicacao
- Serve para nao sobrecarregar o back-end
- Broker NAO processa — Node-RED faz isso
- CLP pode se comunicar diretamente com Node-RED
- Stack MING: cuida do IoT | Stack Web: vai para a internet

| Letra | Componente | Funcao |
|---|---|---|
| M | Mosquitto | Broker MQTT — recebe telemetria, NAO processa |
| I | InfluxDB | Banco de series temporais — dados com timestamp |
| N | Node-RED | Orquestrador low-code — transforma e roteia |
| G | Grafana | Dashboard — metricas, alertas, thresholds (tipo Power BI) |

**Mosquitto:** porta 1883 (sem TLS) ou 8883 (com TLS)

**InfluxDB:** 500 sensores x 1 leitura/s x 1 ano = 15 bilhoes de registros
- Bucket: onde os dados ficam | Measurement: tabela | Field: valor | Tag: metadado indexado
- Back-end Java se conecta diretamente com InfluxDB

**Node-RED:** low-code, drag and drop, Node.js. Fluxo: [MQTT In] -> [Function] -> [InfluxDB Out]

**Grafana:** thresholds, alertas e-mail/Slack, graficos em tempo real

---

## 10. Stack Web Enterprise

Grafana e ferramenta tecnica. Para usuario final precisa de camada a mais.

| Servico | Porta | Funcao |
|---|---|---|
| Mosquitto | 1883 | Broker MQTT |
| Node-RED | 8082 | Orquestracao |
| InfluxDB | 8083 | Series temporais |
| Grafana | 8084 | Dashboard tecnico |
| Backend | 8080 | API REST Node.js |
| MySQL | 3306 | Banco relacional |
| Frontend | 80 | Interface React |

- USE_MOCK=true: simula dados sem hardware fisico
- Servicos se comunicam por hostname interno Docker (nao localhost)

---


---

## 12. COLA RAPIDA

| Conceito | Palavra-chave |
|---|---|
| MQTT | Pub/Sub, broker, leve, QoS 0/1/2 |
| DDS | Peer-to-peer, sem broker, tempo real critico |
| AMQP | Filas, exchanges, garantia de entrega, ERP |
| XMPP | XML, presenca, estado do dispositivo |
| OPC UA | Semantica rica, CLP, SCADA, seguranca nativa |
| Modbus | Legado 1979, mestre-escravo, RS-485 |
| LoRaWAN | >10km, baixo consumo, CSS, classes A/B/C |
| SigFox | Ultra-narrowband, unidirecional, ISM sub-GHz |
| NB-IoT | Celular, pequenos volumes |
| LTE-M | Celular, altos volumes |
| 5G URLLC | 1.2ms, substitui cabo, robotica e AGVs |
| TSN | Ethernet com garantias de tempo, une TI+OT |
| Edge | Borda, latencia minima, resiliencia |
| Cloud | Historico, IA, estrategico |
| Mosquitto | Broker MQTT do stack MING, NAO processa |
| InfluxDB | Series temporais, timestamp, Bucket/Measurement |
| Node-RED | Low-code, fluxos, orquestracao |
| Grafana | Dashboard, alertas, thresholds |
| Gemeo Digital | Replica virtual, simulacao sem risco fisico |
| Manut. Preditiva | Prever falha antes de ocorrer |
| OEE | Disponibilidade x Performance x Qualidade |
| Root of Trust | Chip criptografico isolado, Secure Boot |
| TMR | Redundancia tripla, 2 de 3 sensores |
| Retrofit | Modernizar legado com sensores externos |
| Energy Harvesting | Energia do ambiente (vibracao/solar/calor) |
| RAMI 4.0 | Modelo referencia Industria 4.0 |
| TI vs OT | Confidencialidade vs Disponibilidade |
| IEC-62443 | Norma seguranca automacao industrial |
| IDMZ | Zona desmilitarizada entre internet e rede OT |
| Air-gap | Isolamento fisico nao e suficiente sozinho |
| Cooja | Simulador Contiki OS |
| NS-3 | Simulador matematico, grande escala |
| TOSSIM | Simulador TinyOS |
| Wokwi | Arduino/ESP32 no browser |
