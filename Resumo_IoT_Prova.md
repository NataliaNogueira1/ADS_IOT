# RESUMO IoT/IIoT - CONSULTA DE PROVA

---

## 1. IoT - Conceito Base

> Qualquer objeto fisico conectado com outro pela internet.

| Pilar | O que e |
|---|---|
| **Hardware** | Sensores e atuadores |
| **Conectividade** | Redes e protocolos |
| **Processamento** | Cloud / Edge / On-device / IA / Analytics |

Conceitos fundamentais:
- Tecnologia invisivel em todo lugar
- Conectividade entre dispositivos
- Dispositivos tomam decisoes simples sem intervencao humana

Exemplo Fazenda Inteligente:
- Hardware: sensor de umidade no solo
- Conectividade: LoRaWAN transmite a 10km sem fio
- Inteligencia: algoritmo na nuvem decide acionar irrigacao

---

## 2. IIoT - IoT na Industria

| | IoT (consumidor) | IIoT (industrial) |
|---|---|---|
| Foco | Comodidade | Eficiencia e seguranca |
| Falha tolerada? | Sim (inconveniente) | Nao (pode matar gente) |
| Precisao | Moderada | Altissima |
| Ciclo de vida | 2-5 anos | 10-30 anos |
| Exemplo | Smartwatch | Sensor de pressao em refinaria |

Hardware IIoT precisa:
- Operar entre -40C e 85C
- Resistir a vibracao, poeira e umidade (certificacoes IP)
- Funcionar por anos sem manutencao
- Ciclo de vida longo - compativel com maquinas de 20 anos

---

## 3. Industria 4.0

| Conceito | Descricao |
|---|---|
| **Gemeo Digital** | Copia virtual alimentada por sensores. Testa cenarios sem risco fisico. |
| **Manutencao Preditiva** | Age quando dados indicam desgaste - antes de qualquer falha visivel. |
| **RAMI 4.0** | Modelo de referencia: Dispositivo->Comunicacao->Processamento->Armazenamento->Visualizacao |
| **OEE** | Disponibilidade x Performance x Qualidade - calculado em tempo real via IIoT |

### Manutencao
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
| Conflito | Quer aplicar patch urgente | Nao pode reiniciar - linha produzindo |

---

## 4. Hardware, Sensores e Seguranca

| Sensor | Principio | Aplicacao |
|---|---|---|
| MEMS capacitivo | Microeletromecânico, estavel termicamente | Vibracao, aceleracao |
| Piezoeletrico | Cristal ceramico gera carga sob pressao | Alta frequencia, impacto |
| Termico/IR | Temperatura | Paineis eletricos, motores |
| Acustico/Ultrassonico | Sons acima de 20kHz | Vazamentos de ar comprimido |
| Pressao/Fluido | Pressao diferencial | Bombas hidraulicas, tubulacoes |

### Root of Trust
- Chip criptografico dedicado (ex: ATECC608A) isolado do processador
- Mesmo com firmware comprometido, chaves ficam protegidas
- Senhas hard-coded sao **ilegais** em varios paises

### TMR - Triple Modular Redundancy
- 3 sensores fazem a mesma medicao; sistema usa resultado de 2 de 3
- Usado em usinas nucleares, aviacao

### Gateway Industrial
- Converte protocolos legados (Modbus/RS-485) para MQTT/HTTPS
- Executa Edge Computing: filtra e processa antes de enviar para nuvem

### Retrofit
- Modernizar maquina legada com sensores externos sem substituir o equipamento
- Custo ate 90% menor que maquina nova com IIoT nativo

### Energy Harvesting
| Fonte | Como funciona |
|---|---|
| Solar | Paineis minusculos em sensores externos |
| Vibracao | Vibracao da maquina gera energia piezoeletrica |
| Temperatura | Diferenca termica entre maquina e ambiente (efeito Seebeck) |


---

## 5. Edge Computing

### Por que nao enviar tudo para a nuvem?
| Problema | Consequencia |
|---|---|
| Alta latencia | Robo precisa de 2ms, nuvem leva 50ms - incompativel |
| Consumo de banda | 500 sensores continuos = rede saturada |
| Custo | Armazenar tudo na nuvem e caro |
| Dependencia | Se internet cair, a fabrica para? |

Sem Edge:  Sensor -> Nuvem -> Decisao (50ms+)
Com Edge:  Sensor -> Edge -> Decisao (2ms) -> Nuvem (so resumo)

### Edge vs Fog vs Cloud
| | Edge | Fog | Cloud |
|---|---|---|---|
| Onde fica | Dispositivo/gateway | Servidor local (predio) | Data center remoto |
| Latencia | ~1-5ms | ~5-20ms | ~50-200ms |
| Visao | Um dispositivo | Varios dispositivos locais | Toda a empresa |
| Exemplo | Parar robo se vibracao > limite | Detectar gargalo na linha | Treinar IA com 1 ano de dados |

Edge + Cloud sao parceiros: Edge = decisao tatica imediata | Cloud = analise estrategica e historico

---

## 6. Protocolos de Comunicacao

HTTP vs MQTT: HTTP e sincrono (precisa de resposta para continuar) - dispositivo limitado nao aguenta. MQTT e assíncrono com broker intermediario.

| | HTTP | MQTT |
|---|---|---|
| Modelo | Requisicao/Resposta | Publish/Subscribe |
| Endereco | dominio.com/mobile/home | dominio.com + topico: mobile/home |
| Body | Dados do formulario | payload |
| Sincrono? | Sim | Nao |

### MQTT - QoS
| Nivel | Comportamento | Quando usar |
|---|---|---|
| QoS 0 | Dispara e esquece | Leituras onde perder uma nao importa |
| QoS 1 | Entrega pelo menos 1x (pode duplicar) | Alertas que nao podem ser perdidos |
| QoS 2 | Exatamente 1x (mais lento, 4 trocas) | Comandos de atuacao (abrir valvula) |

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
| Modbus | Nao | - | Protocolo legado 1979, RS-485/TCP |

**XMPP:** XML com presenca (online/offline/ocupado). Envia comandos diretos a dispositivo especifico.
**DDS:** Peer-to-peer, sem broker, data-centric. Latencia minima - robotica cirurgica, veiculos autonomos.
**AMQP:** Filas com exchanges; mensagem persiste ate ser processada. Integracao com ERP, SAP.
**OPC UA:** Semantica rica (valor + unidade + limite + status). Seguranca nativa X.509. CLP/SCADA local.

---

## 7. Conectividade e Redes

| Tecnologia | Alcance | Consumo | Taxa | Caso de uso |
|---|---|---|---|---|
| Wi-Fi | ~100m | Alto | 600 Mbps | Cameras, monitoramento intenso |
| BLE | ~50m | Muito baixo | Medio | Wearables, beacons |
| Zigbee | ~300m mesh | Baixo | 250 kbps | Sensores em predios |
| LoRaWAN | >10 km | Muito baixo | ~50 kbps | Sensores rurais, medidores remotos |
| SigFox | >10 km | Muito baixo | 100 bps | Telemetria simples |
| NB-IoT | Cobertura 4G | Medio | ~50 kbps | Ativos urbanos, QoS garantido |
| LTE-M | Cobertura 4G | Medio | Altos volumes | Rastreamento |
| 5G URLLC | Amplo | Variavel | Gbps, 1.2ms | Robotica, AGVs |
| EtherCAT | Local (cabo) | - | <0.5ms | Controle de robos, malha fechada |

### LoRaWAN
- Usa **CSS (Chirp Spread Spectrum):** alterna frequencias para evitar conflitos
- Pro: longa distancia. Contra: baixa taxa, so IoT mesmo

| Classe | Caracteristica | Prioridade |
|---|---|---|
| **A** | Protocolo ALOHA, hibernacao profunda | Bateria em 1o lugar |
| **B** | Comunicacao sincronizada, janelas programadas | Intermediario |
| **C** | Escuta continua, sem latencia | Exige fonte de energia externa |

**SigFox:** ultra-narrowband, unidirecional (0G), ISM sub-GHz, telemetria simples.
**NB-IoT:** celular, pequenos volumes. **LTE-M:** celular, altos volumes.

### 5G - tres modos
- **eMBB:** velocidade alta, streaming, AR/VR
- **mMTC:** milhoes de sensores por km2
- **URLLC:** latencia 1.2ms, substitui cabo em robotica e AGVs

### TSN - Time-Sensitive Networking
- Extensao do Ethernet (IEEE 802.1) com garantias de tempo
- Trafego critico tem faixa exclusiva - video e controle de robo na mesma rede
- Indispensavel em controle de malha fechada (<1ms)
- Une TI e OT na mesma rede

---

## 8. Simuladores de Rede

| Simulador | Foco | Quando usar |
|---|---|---|
| **Cooja** | Contiki OS, sensores com recursos limitados | Simular nos reais com Contiki |
| **NS-3** | Matematico, altissima escala | Pesquisa academica, grandes redes |
| **TOSSIM** | TinyOS | Simular comportamento de motes TinyOS |
| **Wokwi** | Arduino/ESP32 no browser | Prototipagem rapida sem hardware fisico |

Uso: validar infraestruturas sem gastos fisicos antes do deploy real.


---

## 9. Stack MING

MING = Mosquitto + InfluxDB + Node-RED + Grafana

- Tira a dependencia do dispositivo com a aplicacao
- Serve para nao sobrecarregar o back-end
- Broker NAO processa - Node-RED faz isso
- CLP pode se comunicar diretamente com Node-RED
- Stack MING: cuida do IoT | Stack Web: vai para a internet

[Sensor] --MQTT--> [M Mosquitto] --> [N Node-RED] --> [I InfluxDB] --> [G Grafana]

| Letra | Componente | Funcao |
|---|---|---|
| **M** | Mosquitto | Broker MQTT - recebe telemetria, NAO processa |
| **I** | InfluxDB | Banco de series temporais - dados com timestamp |
| **N** | Node-RED | Orquestrador low-code - transforma e roteia |
| **G** | Grafana | Dashboard - metricas, alertas, thresholds (tipo Power BI) |

**Mosquitto:** porta 1883 (sem TLS) ou 8883 (com TLS)

**InfluxDB:** 500 sensores x 1 leitura/s x 1 ano = 15 bilhoes de registros (banco relacional travaria)
- Bucket: onde os dados ficam | Measurement: tabela | Field: valor | Tag: metadado indexado
- Back-end Java se conecta diretamente com InfluxDB

**Node-RED:** low-code, drag and drop, Node.js. Fluxo: [MQTT In] -> [Function] -> [InfluxDB Out]

**Grafana:** thresholds, alertas e-mail/Slack, graficos em tempo real

---

## 10. Stack Web Enterprise

Grafana e ferramenta tecnica. Para usuario final precisa de camada a mais.

[ESP32]--MQTT-->[Mosquitto]-->[Node-RED]-->[InfluxDB]-->[Grafana]
                                               |
                                      [Backend Node.js]
                                      (job a cada 30s)
                                               |
                                           [MySQL]
                                               |
                                      [Frontend React]

| | Dados Brutos | Dados Consolidados |
|---|---|---|
| Onde ficam | InfluxDB | MySQL |
| Frequencia | A cada segundo | A cada 5 minutos (avg/min/max) |
| Quem le | Grafana, Node-RED | Backend, Frontend |

**Job de Consolidacao (a cada 30s):** le InfluxDB -> calcula avg/min/max -> salva MySQL -> verifica thresholds -> gera alertas -> atualiza status sensores

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

## 11. Seguranca Cibernetica na IIoT

| Camada | Ameaca | Protecao |
|---|---|---|
| Hardware | Acesso fisico, clonagem | Root of Trust, Secure Boot |
| Firmware | Codigo malicioso | Assinatura digital, OTA seguro |
| Comunicacao | Interceptacao MITM | TLS/SSL, certificados |
| Autenticacao | Acesso nao autorizado | Certificados unicos, MFA |
| Rede | DDoS, varredura | Segmentacao, firewall, VPN |

**Principios:** Zero Trust | Menor privilegio | Defense in Depth | Security by Design

**IEC-62443:** norma internacional para automacao industrial. Define zonas de seguranca e niveis de maturidade. Prioriza disponibilidade em OT vs confidencialidade em TI.

**IDMZ (Industrial Demilitarized Zone):** buffer entre rede OT e enterprise. Nenhum trafego passa direto entre internet e rede de maquinas.

**Air-gap e mito:** Stuxnet (2010) destruiu centrifugas no Ira via pen drive em rede isolada. Vetores: pen drive, laptop de manutencao, gateway mal configurado.

---

## 12. COLA RAPIDA

| Conceito | Palavra-chave |
|---|---|
| MQTT | Pub/Sub, broker, leve, QoS 0/1/2, Retain, Will |
| DDS | Peer-to-peer, sem broker, tempo real critico |
| AMQP | Filas, exchanges, garantia de entrega, ERP |
| XMPP | XML, presenca, estado do dispositivo |
| OPC UA | Semantica rica, CLP, SCADA, seguranca nativa X.509 |
| Modbus | Legado 1979, mestre-escravo, RS-485/TCP |
| LoRaWAN | >10km, baixo consumo, CSS, classes A/B/C |
| SigFox | Ultra-narrowband, unidirecional 0G, ISM sub-GHz |
| NB-IoT | Celular, pequenos volumes |
| LTE-M | Celular, altos volumes |
| 5G URLLC | 1.2ms, substitui cabo, robotica e AGVs |
| TSN | Ethernet com garantias de tempo, une TI+OT |
| Edge | Borda, latencia minima, resiliencia |
| Fog | Intermediario, agrega edges, servidor local |
| Cloud | Historico, IA, estrategico |
| Mosquitto | Broker MQTT, NAO processa, porta 1883/8883 |
| InfluxDB | Series temporais, Bucket/Measurement/Field/Tag |
| Node-RED | Low-code, fluxos, orquestracao, Node.js |
| Grafana | Dashboard, alertas, thresholds |
| Gemeo Digital | Replica virtual, testa cenarios sem risco fisico |
| Manut. Preditiva | Age quando dados indicam desgaste |
| OEE | Disponibilidade x Performance x Qualidade |
| Root of Trust | Chip criptografico isolado, Secure Boot |
| TMR | Redundancia tripla, 2 de 3 sensores |
| Retrofit | Modernizar legado com sensores externos |
| Energy Harvesting | Energia do ambiente: vibracao/solar/calor |
| RAMI 4.0 | Modelo referencia Industria 4.0 |
| TI vs OT | Confidencialidade vs Disponibilidade |
| IEC-62443 | Norma seguranca automacao industrial |
| IDMZ | Zona desmilitarizada entre internet e rede OT |
| Air-gap | Isolamento fisico nao e suficiente sozinho |
| Cooja | Simulador Contiki OS |
| NS-3 | Simulador matematico, grande escala |
| TOSSIM | Simulador TinyOS |
| Wokwi | Arduino/ESP32 no browser |


