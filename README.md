
![MonitorAr](https://github.com/projetomonitorar/monitorar/raw/master/img/monitorar_logo_600px.png "MonitoAr")

# Guia de Uso

## Apresentação
O projeto MonitorAr é uma iniciativa de ciência cidadã de sensoriamento participativo sobre a qualidade do ar. A proposta é experimentar coletivamente a coleta, visualização e análise de dados climáticos, usando tecnologia aberta com uma plataforma de IoT (internet das coisas) e sensores que medem temperatura do ar, umidade ar, pressão  atmosférica e presença de gases tóxicos.

## 3. Instalação do kit MonitorAr
O kit MonitorAr é composto um microcontrolador Esp8266 NodeMCU e por dois sensores, o BME280 e o MQ-135.

O sensor BME280 mede:
- a temperatura do ar, em graus Celsius;
- a umidade do ar, em taxa percentual;
- pressão atmosférica, em hectoPascal;
- e com base nos dados coletados, calcula a altitude, em metros.

O sensor MQ-135 detecta presença de gases tóxicos, sem unidade, em uma faixa de valor de concentração que varia de 0 a 1023.

Os cinco dados coletados pelos sensores são publicados no servidor da Adafruit IO a cada intervalo de aproximadamente 30 segundos, usando o protocolo de comunicação [MQTT](https://pt.wikipedia.org/wiki/MQTT). Cada dado é chamado de *feed*, portanto cada estação tem 5 *feeds*. Para visualizar os dados coletados, acessar a plataforma Adafruit IO e abrir o link respectivo da estação:
- Estação 1 - Soldadinho-do-Araripe - https://io.adafruit.com/monitorar1
- Estação 2 - Bem-te-vi - https://io.adafruit.com/monitorar2
- Estação 3 - Canário-do-mato - https://io.adafruit.com/monitorar3
- Estação 4 - Pica-pau - https://io.adafruit.com/monitorar4
- Estação 5 - Tico-tico - https://io.adafruit.com/monitorar5

Antes de por o kit em funcionamento, é necessário atualizar o código do microcontrolador com o nome e senha da rede WiFi, e dados de acessos da plataforma Adafruit IO.

Após a atualização do código, deverá ser feita a instalação do kit, preferencialmente em uma área externa coberta, protegida da chuva, água, do sol e de fontes de calor. O kit deverá ser instalado em uma altura entre 1,20m e 2,00m acima do solo - para diminuir a interferência do solo na medida dos dados climáticos do ar.

O kit deverá ser fixado usando a abraçadeira de nylon como ponto de amarração (Imagem X).

Após a fixação do kit, conectar o cabo USB no microcontrolador e no carregador USB, e plugar na tomada (Imagem X).

### 3.1. Adicionar ESP8266 no Arduino IDE  
Para atualizar o código é necessário baixar o *Arduino IDE* - o software que permite escrever e enviar código para o microcontrolador Arduino, Esp8266, entre outros.

Acessar https://www.arduino.cc/en/Main/Software e baixar o *Arduino IDE* de acordo com o seu sistema operacional. 

Após a conclusão da instalação, conectar o cabo USB no kit MonitorAr, e a outra ponta do cabo conectar no computador. Em seguida, abrir o *Arduino IDE*. 
Selecionar o menu `Arquivo > Preferências` para abrir a janela de preferências. No campo `URLs Adicionais para Gerenciadores de Placas`, adicionar: `https://arduino.esp8266.com/stable/package_esp8266com_index.json`. Fechar a janela clicando em `OK`.

No menu `Ferramentas > Placas`, selecionar na lista a placa `NodeMCU 1.0 (ESP-12E Module)`. Novamente no menu `Ferramentas > Porta`, selecionar a porta que estiver disponível.

Observação: se tiver mais de uma porta *Com* disponível, anote os nomes, pressione a tecla Esc e desplugue o kit MonitorAr do computador. Abrir novamente `Ferramentas > Porta` e observe qual porta *Com* foi desligada, a que não está mais na lista é a porta *Com* do MonitorAr, que deverá ser selecionada.

Para testar se a instalação foi bem sucedida, abrir o menu `Arquivo > Exemplos > 01. Basics > Blink`. Substituir `LED_BUILTIN` por `D4` (como está escrito, com *D* maiúsculo). Em seguida, no meu `Skecth > Carregar`, ou pressionar `Ctrl+U`. O código Blink será enviado ao microcontrolador, e o led interno do Esp vai piscar no intervalo de 1000 microsegundos (1 segundo). 

### 3.2. Adicionar bibliotecas no Arduino IDE
Bibliotecas são códigos adicionais que estendem a funcionalidade do *Arduino IDE* para usar com módulos, sensores, entre outros, desenvolvidos e compartilhados por colaboradores. Para fazer a atualização do código do MonitorAr, é necessário instalar as seguintes bibliotecas:

- [BlueDot_BME280](https://github.com/BlueDot-Arduino/BlueDot_BME280), para usar o sensor BME280;
- [Adafruit_MQTT](https://github.com/adafruit/Adafruit_MQTT_Library), para acessar e publicar os dados no servidor da Adafruit IO. 

Para instalar as bibliotecas no *Arduino IDE*, selecionar o menu `Sketch > Incluir Biblioteca > Gerenciar Bibliotecas`. No campo de busca, digitar *Bluedot_BME280*, em seguida clicar `Instalar` na biblioteca encontrada. Repetir o mesmo procedimento para a biblioteca *Adafruit_MQTT*. 

Para mais informações sobre instalação de bibliotecas no *Arduino IDE*, consultar https://hardwarelivreusp.org/tutoriais/2016/11/24/arduino-8libraries

### 3.3. Atualizar o código

O código usando no *MonitorAr* está disponível em https://github.com/projetomonitorar/codigoMonitorar. Na página do GitHub, clicar no botão `Code > Download ZIP`. Após o download, descompactar o arquivo, em seguida, copiar a pasta `monitorAr`para o local preferencial de trabalho no seu computador. No *Arduino IDE*, selecionar o menu `Arquivo > Abrir` e abrir o arquivo `monitorAr.ino`, localizado na pasta copiada no procedimento anterior.

Antes de enviar o código para o ESP8266, é necessário fazer três alterações:

- Nome e senha da rede de WiFi, para que o dispositivo possa conectar com a internet;
- Login e chave da conta do servidor da Adafruit IO;
- Numerar os feeds de dados de acordo com o respectivo número da estação.

#### 3.3.1. Dados de acesso à rede de WiFi

Para atualizar o nome e senha da rede de Wifi, localizar o código abaixo:
```
/************************* Ponto de acesso de WiFi *********************************/
#define WLAN_SSID       "Nome WiFi"    //  Nome da rede de WiFi.
#define WLAN_PASS       "senha"           //  Senha da rede de WiFi.
```
Substituir o `Nome WiFi`pelo nome da sua rede, e `senha` pela senha da sua rede. 

Observação: o termo rede de WiFi é chamado de SSID (*Service Set Identifier*, em português, *identificador do conjunto de serviços*).

#### 3.3.2. Dados de acesso à plataforma Adafruit IO

Para atualizar os dados de acesso ao servidor da Adafruit IO, localizar o código abaixo:
```
#define AIO_USERNAME  "nomeDeUsuario"
#define AIO_KEY       "chaveDaAdafruitIO"
```
Substituir `nomeDeUsuario` pelo nome de usuário da respectiva estação, e `chaveDaAdafruitIO` pela chave de acesso do usuário. Para 

#### 3.3.3. Feeds dos dados

Para atualizar o nome dos feeds de dados, localizar o código abaixo:
```
Adafruit_MQTT_Publish pubTemperatura = Adafruit_MQTT_Publish(&mqtt, AIO_USERNAME "/feeds/temperaturaN");
Adafruit_MQTT_Publish pubUmidade = Adafruit_MQTT_Publish(&mqtt, AIO_USERNAME "/feeds/umidadeN");
Adafruit_MQTT_Publish pubPressao = Adafruit_MQTT_Publish(&mqtt, AIO_USERNAME "/feeds/pressaoN");
Adafruit_MQTT_Publish pubAltitude = Adafruit_MQTT_Publish(&mqtt, AIO_USERNAME "/feeds/altitudeN");
Adafruit_MQTT_Publish pubGases = Adafruit_MQTT_Publish(&mqtt, AIO_USERNAME "/feeds/gasesN");
```

Substituir somente a letra `N` no final da linha de código pelo respectivo número da estação, repetindo a substituição em todos os cinco feeds. Por exemplo, se estou atualizando o código da Estaçao 1, o `"/feeds/temperaturaN"`será atualizado por `"/feeds/temperatura1"`, o `"/feeds/umidadeN"` será `"/feeds/umidade1"`, e assim por diante.

___

Projeto desenvolvido por Jéssyca Rios e Artur V. Cordeiro em parceria com a [ONG Abecê da Educação Ambiental](https://ongabcambiental.com)


