
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
