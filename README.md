# Sistema de Monitoramento para Vinheria

<img alt="Technologies" src="https://img.shields.io/badge/MCU-ESP32-blue" /> <img alt="Technologies" src="https://img.shields.io/badge/Linguagem-C++-brightgreen" /> <img alt="Technologies" src="https://img.shields.io/badge/Protocolo-MQTT-yellow" /> <img alt="Technologies" src="https://img.shields.io/badge/Back--End-FIWARE-orange" /> <img alt="Technologies" src="https://img.shields.io/badge/Dashboard-Node--RED-red" />

## Projeto
Este projeto visa monitorar as condições de armazenamento de vinhos em uma vinheria, utilizando sensores de temperatura, umidade (DHT11) e luminosidade (LDR). As leituras dos sensores são enviadas via protocolo MQTT para uma plataforma FIWARE de back-end, conforme o padrão NGSIv2. O sistema permite o monitoramento remoto das condições ambientais e histórico dos dados, promovendo o controle eficaz da qualidade dos vinhos. Além disso, agora inclui dashboards online em tempo real desenvolvidos com Node-RED.

## Componentes Utilizados
- **ESP32** (MCU com Wi-Fi integrado)
- **Sensor de Temperatura e Umidade DHT11**
- **Sensor de Luminosidade LDR**
- **Resistores**
- **Jumpers e Protoboard**

## Funcionamento do Sistema
O sistema lê continuamente os dados de temperatura, umidade e luminosidade do ambiente onde os vinhos são armazenados. As informações são transmitidas para a plataforma FIWARE usando MQTT, juntamente com um `time stamp` de cada leitura. A interface FIWARE segue o padrão NGSIv2, possibilitando integração com o ecossistema europeu de negócios e o armazenamento histórico dos dados para análise.

## Dados Coletados
- **Temperatura:** Leitura em graus Celsius (°C).
- **Umidade:** Percentual de umidade relativa (%).
- **Luminosidade:** Valor analógico correspondente à intensidade luminosa.

## Requisitos Atendidos
1. **Transmissão via MQTT:** Os dados são enviados para a plataforma FIWARE através do protocolo MQTT.
2. **Time Stamp:** Cada leitura contém um carimbo de tempo (`time stamp`) que registra o momento exato da coleta.
3. **Conexão Wi-Fi:** O MCU utiliza Wi-Fi para se conectar à internet e enviar as leituras.
4. **Conformidade com NGSIv2:** As leituras estão em conformidade com o padrão de interface NGSI, permitindo a interoperabilidade dentro do ecossistema FIWARE.
5. **Dashboards em Tempo Real:** Visualização dos dados em dashboards online atualizados em tempo real.

## Smart Data Models e NGSIv2
O sistema utiliza os **Smart Data Models** da União Europeia, possibilitando a integração com o FIWARE para monitoramento de condições ambientais. A plataforma FIWARE proporciona armazenamento histórico dos dados, facilitando análises futuras e a criação de insights sobre as condições ideais para o armazenamento de vinhos.

## Dashboards com Node-RED
Implementamos dashboards online usando Node-RED para visualização em tempo real dos dados coletados. Estes dashboards oferecem:

- Gráficos dinâmicos de temperatura, umidade e luminosidade
- Indicadores visuais para condições ideais e alertas
- Histórico de dados com opções de filtragem por período
- Personalização de layouts e widgets para diferentes necessidades de monitoramento

## Dashboard
Abaixo está uma captura de tela do nosso dashboard desenvolvido via Node-RED, mostrando a interface de monitoramento em tempo real:

![Dashboard Node-RED](caminho/para/imagem/dashboard.png)


## Código e Configuração

### Definição dos Pinos dos Sensores

```cpp
#define DHTTYPE DHT11
#define DHTPIN 4
DHT dht(DHTPIN, DHTTYPE);
```

### Leitura e Envio dos Dados via MQTT
```cpp
void handleLuminosity() {
    const int potPin = 34;
    int sensorValue = analogRead(potPin);
    int luminosity = map(sensorValue, 0, 4095, 0, 100);
    String mensagem = String(luminosity);
    Serial.print("Valor da luminosidade: ");
    Serial.println(mensagem.c_str());
    MQTT.publish(TOPICO_PUBLISH_2, mensagem.c_str());

    float humidity = dht.readHumidity();
    float temperature = dht.readTemperature();

    int humi = map(humidity, 0, 200, 0, 100);
    if (isnan(humidity) || isnan(temperature)) {
      Serial.println(F("Falha na leitura de Sensor DHT22"));
    }

    Serial.println(F("Umidade: "));
    Serial.println(humidity);
    Serial.println(F("Temperatura: "));
    Serial.println(temperature);

    mensagem = String(humidity);
    MQTT.publish(TOPICO_PUBLISH_3, mensagem.c_str());

    mensagem = String(temperature);
    MQTT.publish(TOPICO_PUBLISH_4, mensagem.c_str());
}
```

## Requisitos Técnicos
- **Plataforma FIWARE:** Utilizada como back-end para gerenciamento e armazenamento dos dados.
- **Protocolo MQTT:** Utilizado para transmissão eficiente dos dados dos sensores para o back-end.
- **NGSIv2:** Padrão de interface para integração com serviços e sistemas externos.
- **Node-RED:** Plataforma para criação de dashboards e fluxos de dados em tempo real.

## Importância do Projeto
O monitoramento eficiente das condições de temperatura, umidade e luminosidade é crucial para garantir a qualidade e preservação dos vinhos. Este projeto fornece uma solução de IoT integrada, que não apenas automatiza o processo de controle ambiental, mas também fornece dados históricos para análises de longo prazo, promovendo o armazenamento ideal dos vinhos e minimizando perdas. A adição de dashboards em tempo real melhora significativamente a capacidade de resposta a variações nas condições de armazenamento.

## Autores
- **Gabriel Lacerda  RM: 556714**
- **João Pedro Signor  RM: 558375**
- **Roger Cardoso  RM: 557230**
- **Vinicius Augusto  RM: 557065**
- **1ESPW**
