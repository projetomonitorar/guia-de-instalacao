
![MonitorAr](https://drive.google.com/uc?export=view&id=1bMn0m7PamR_itozMg-NlIY7qP-TwBqoT)

# Guia de Instalação

O projeto MonitorAr é uma iniciativa de ciência cidadã de sensoriamento participativo. A proposta é experimentar coletivamente a coleta, visualização e análise de dados climáticos. O kit MonitorAr é composto um microcontrolador Esp8266 NodeMCU Lolin V3 e por dois sensores, o BME280 e o MQ-135, e alimentado por um carregador USB.

No total, são enviados cinco dados para a plataforma Adafruit IO, conforme descrito a seguir. 

O sensor BME280 mede:
- a temperatura do ar, em graus Celsius;
- a umidade relativa do ar, em taxa percentual;
- pressão atmosférica, em hectoPascal;
- e com base nos dados coletados, calcula a altitude, em metros.

O sensor MQ-135 detecta presença de gases tóxicos, sem unidade, em uma faixa de valor de concentração que varia de 0 a 1023.

Os cinco dados coletados pelos sensores são publicados no servidor da Adafruit IO em um intervalo de aproximadamente 30 segundos, usando o protocolo de comunicação [MQTT](https://pt.wikipedia.org/wiki/MQTT). No ambiente da Adafruit IO, cada *dado* é chamado de *feed*, portanto, cada estação tem 5 *feeds*. O ítem *3.3. Atualizar os nomes dos feeds* lista os cinco *feeds* usados em cada estação.

![Diagrama esquemático](https://drive.google.com/uc?export=view&id=1MaRqafqyEPNiQWZJ-ai2l7TtgXUziuGR)

Antes do kit iniciar a coleta de dados, é necessário atualizar o código do microcontrolador com o nome e senha da rede WiFi, e informações de acesso da plataforma Adafruit IO. Após a atualização do código, deverá ser feita a instalação física do kit, para então começar o monitoramento dos dados.

**Atenção**: O kit MonitorAr contém peças frágeis, cuidado ao manusear evitando tocar no sensor externo.

## 1. Instação do Arduino IDE  
Para atualizar o código é necessário baixar o *Arduino IDE* - o software que permite escrever e enviar código para o microcontrolador Arduino, Esp8266, entre outros.

Acessar https://www.arduino.cc/en/Main/Software e baixar o *Arduino* de acordo com o seu sistema operacional. Quando terminar o download, instalar o *Arduino*. Durante a instalação poderá aparecer confirmações para instalar os componentes do *Arduino*, caso apareça esses avisos, confirme a instalação. 

Junto com o software do *Arduino* são instalados *drivers* USB, dependendo do sistema operacional, pode ser que o *driver* USB usado no ESP8266 do kit MonitorAr não tenha sido instalado. 

Para verificar, conectar o cabo USB no kit MonitorAr, e a outra ponta do cabo conectar na USB do computador. Pode ser que o sistema operacional reconheça e instale automaticamente o *driver*. 

Em seguida, abrir o software do *Arduino*, selecionar o menu `Ferramentas > Porta` e observe as portas *Com* listadas, e tecle `Esc` para sair do menu selecionado. Desconectar o cabo do kit MonitorAr da porta USB. Novamente, selecionar o menu `Ferramentas > Porta`, e observe qual porta *Com* não está mais presente, pois a que não está mais na lista é a porta *Com* do MonitorAr. Caso continue com as mesmas portas *Com* listadas, o *driver* do ESP8266 não foi instalado. Neste caso, siga o procedimento do ítem *1.1. Instalação do driver CH340*.

### 1.1. Instalação do driver CH340
Se o *driver* do ESP8266 não foi instalado, sair do *Arduino* e baixar o arquivo [Driver CH340 - CH341SER.EXE](https://cdn.sparkfun.com/assets/learn_tutorials/8/4/4/CH341SER.EXE). Após o download, executar o arquivo `CH341SER.EXE`. Caso surja janelas de alerta, confirmar a execução do arquivo. 

Quando a janela abrir, clicar em `Install`. 

![Instlar o *driver* CH340](https://cdn.sparkfun.com/assets/learn_tutorials/8/4/4/CH340-Win10_Uninstall-Install.PNG)

Após a instalação do *driver*, feche a janela.

### 1.2. Adicionar as placas ESP8266
Para trabalhar com os microcontroladores ESP8266 no *Arduino IDE* é necessário instalar um "pacote de placas". Abrir o *Arduino*, selecionar o menu `Arquivo > Preferências` para abrir a janela de preferências. No campo `URLs Adicionais para Gerenciadores de Placas`, adicionar: `https://arduino.esp8266.com/stable/package_esp8266com_index.json`. Fechar a janela clicando em `OK`.

![Preferências](https://drive.google.com/uc?export=view&id=103yIaG6dnycPRoLw88mZYmwHcPXyj6GC)

Em seguida, selecionar o menu `Ferramentas > Placas > Gerenciador de Placas`. No campo de busca, digitar `esp8266`, e instalar o `ESP8266 by ESP8266 Community`. 

Após a conclusão da instalação, conectar o cabo USB no kit MonitorAr, e a outra ponta conectar na USB do computador. 

No menu `Ferramentas > Placas`, selecionar na lista a placa `NodeMCU 1.0 (ESP-12E Module)`. Novamente no menu `Ferramentas > Porta`, selecionar a porta *Com* do kit MonitorAr.

### 1.3. Verificar a instalação
Para testar se a instalação foi bem sucedida, abrir o menu `Arquivo > Exemplos > 01. Basics > Blink`. 

Substituir:
- `pinMode(LED_BUILTIN, OUTPUT);` por `pinMode(D4, OUTPUT);`
- `digitalWrite(LED_BUILTIN, HIGH);` por `digitalWrite(D4, HIGH);`
- `digitalWrite(LED_BUILTIN, LOW);` por `digitalWrite(D4, LOW);`.

Em seguida, selecionar o comando menu `Skecth > Carregar`, ou pressionar `Ctrl+U`. O código Blink será enviado ao microcontrolador, e o led interno do ESP8266 vai piscar no intervalo de 1000 microsegundos (1 segundo). 

Para experimentar diferentes intervalos, substituir o valor do `delay(1000);` por exemplo:

```
void loop() {
  digitalWrite(D4, HIGH);   // turn the LED on (HIGH is the voltage level)
  delay(500);                       // wait for a second
  digitalWrite(D4, LOW);    // turn the LED off by making the voltage LOW
  delay(2000);                       // wait for a second
}
```

Após as alterações no *delay*, é necessário atualizar o ESP8266 enviando o código, com o comando no menu `Skecth > Carregar` ou pressionar `Ctrl+U`.

## 2. Adicionar bibliotecas no Arduino IDE
Bibliotecas são códigos adicionais que estendem a funcionalidade do *Arduino IDE* para usar com módulos, sensores, entre outros, desenvolvidos e compartilhados por colaboradores. Para fazer a atualização do código do MonitorAr, é necessário instalar as seguintes bibliotecas:

- [BlueDot_BME280](https://github.com/BlueDot-Arduino/BlueDot_BME280), para usar o sensor BME280;
- [Adafruit_MQTT](https://github.com/adafruit/Adafruit_MQTT_Library), para acessar e publicar os dados no servidor da Adafruit IO. 

Para instalar as bibliotecas no *Arduino IDE*, selecionar o menu `Sketch > Incluir Biblioteca > Gerenciar Bibliotecas`. No campo de busca, digitar *Bluedot_BME280*, em seguida clicar `Instalar` na biblioteca encontrada. Repetir o mesmo procedimento para a biblioteca *Adafruit_MQTT*. 

![Instalação da biblioteca](https://drive.google.com/uc?export=view&id=1xumxmXJzgbUc1fQCS4zLasMv8hWq9a7C)

**CORRIGIR** ESPECIFICAR exatamente qual é a biblioteca e alertar para o aviso que vai instalar dependências.


Para mais informações sobre instalação de bibliotecas no *Arduino IDE*, consultar https://hardwarelivreusp.org/tutoriais/2016/11/24/arduino-8libraries.

## 3. Atualizar o código
O código usado no *MonitorAr* está disponível em https://github.com/projetomonitorar/codigoMonitorar. Na página do GitHub, clicar no botão `Code > Download ZIP`. Após o download, descompactar o arquivo, em seguida, copiar a pasta `monitorAr`para o local preferencial de trabalho no seu computador. No *Arduino IDE*, selecionar o menu `Arquivo > Abrir` e abrir o arquivo `monitorAr.ino`, localizado na pasta copiada anteriormente.

Antes de enviar o código para o ESP8266, é necessário fazer três alterações:

- Nome e senha da rede de WiFi, para que o dispositivo possa conectar com a internet;
- Login e chave da conta do servidor da Adafruit IO;
- Numerar os *feeds* de dados de acordo com o respectivo número da estação.

### 3.1. Dados de acesso à rede de WiFi
Para atualizar o nome e senha da rede de Wifi, localizar o código abaixo:
```
/************************* Ponto de acesso de WiFi *********************************/
#define WLAN_SSID       "Nome WiFi"    //  Nome da rede de WiFi.
#define WLAN_PASS       "senha"           //  Senha da rede de WiFi.
```
Substituir o `Nome WiFi`pelo nome da sua rede, e `senha` pela senha da sua rede, mantendo as aspas. 

Observação: o termo rede de WiFi é chamado de SSID (*Service Set Identifier*, em português, *identificador do conjunto de serviços*).

### 3.2. Dados de acesso à plataforma Adafruit IO
Para atualizar os dados de acesso ao servidor da Adafruit IO, localizar o código abaixo:
```
#define AIO_USERNAME  "nomeDeUsuario"
#define AIO_KEY       "chaveDaAdafruitIO"
```
Substituir `nomeDeUsuario` pelo nome de usuário da respectiva estação, e `chaveDaAdafruitIO` pela chave de acesso do usuário, mantendo as aspas. 

### 3.3. Atualizar os nomes dos feeds
Para atualizar o nome dos feeds de dados, localizar o código abaixo:
```
Adafruit_MQTT_Publish pubTemperatura = Adafruit_MQTT_Publish(&mqtt, AIO_USERNAME "/feeds/temperaturaN");
Adafruit_MQTT_Publish pubUmidade = Adafruit_MQTT_Publish(&mqtt, AIO_USERNAME "/feeds/umidadeN");
Adafruit_MQTT_Publish pubPressao = Adafruit_MQTT_Publish(&mqtt, AIO_USERNAME "/feeds/pressaoN");
Adafruit_MQTT_Publish pubAltitude = Adafruit_MQTT_Publish(&mqtt, AIO_USERNAME "/feeds/altitudeN");
Adafruit_MQTT_Publish pubGases = Adafruit_MQTT_Publish(&mqtt, AIO_USERNAME "/feeds/gasesN");
```

Substituir somente a letra `N` no final da linha de código pelo respectivo número da estação, repetindo a substituição em todos os cinco feeds. Por exemplo, se estou atualizando o código da Estação **1**, o resultado das substituições será:

```
Adafruit_MQTT_Publish pubTemperatura = Adafruit_MQTT_Publish(&mqtt, AIO_USERNAME "/feeds/temperatura1");
Adafruit_MQTT_Publish pubUmidade = Adafruit_MQTT_Publish(&mqtt, AIO_USERNAME "/feeds/umidade1");
Adafruit_MQTT_Publish pubPressao = Adafruit_MQTT_Publish(&mqtt, AIO_USERNAME "/feeds/pressao1");
Adafruit_MQTT_Publish pubAltitude = Adafruit_MQTT_Publish(&mqtt, AIO_USERNAME "/feeds/altitude1");
Adafruit_MQTT_Publish pubGases = Adafruit_MQTT_Publish(&mqtt, AIO_USERNAME "/feeds/gases1");
```

### 3.4. Upload do código para o ESP8266
Após a conclusão das atualizações no código, com o kit *MonitorAr* conectado ao computador pelo cabo USB, selecionar o menu `Skecth > Carregar`, ou pressionar `Ctrl+U`. 

Para verificar se as atualizações do código foram bem sucedidas, selecionar `Ferramentas > Monitor serial`. Na janela que abrir, no canto inferior direito, selecionar a velocidade 1152000.   

![Monitor serial](https://drive.google.com/uc?export=view&id=1mWHUOGosXF0jHgjYiCh9ktLXAU1Awbyl)

A medida que os dados são enviados, será exibido a confirmação `Ok, enviado!` na janela do *Monitor Serial*, em um intervalo aproximado de 30 segundos. Caso os dados não estejam sendo enviados, verificar se o nome da rede WiFi e os dados de acesso da plataforma Adafruit IO estão corretos.

Como forma de confirmação visual, o led azul do ESP8266 pisca quando os dados estão sendo enviados. 

Além desses modods de verificação, a confirmação se os dados estão sendo enviados pode ser feita na plataforma da Adafruit IO, acessando o endereço web da respectiva estação, conforme ítem *5. Visualizar os dados coletados* deste Guia.  

Após a confirmação que as atualizações foram realizadas, fechar a janela, e desconectar o cabo USB do computador. O kit está pronto para ser instalado no local da coleta de dados.

## 4. Instalação física
O kit *MonitorAr* deverá ser instalado preferencialmente em uma área externa coberta, protegida da chuva, água, do sol e de fontes de calor, e sobretudo, dentro da área de abrangência da rede de WiFi especificada. 

O kit deverá ser instalado na posição vertical, com o conector USB orientado para baixo, em uma altura entre 1,20m e 2,00m acima do solo - para diminuir a interferência do solo na medida dos dados climáticos do ar.

O kit deverá ser fixado usando como suporte de amarração a abraçadeira de nylon preta que tem na parte de trás do kit *MonitorAr*.

Após a fixação do kit, conectar o cabo USB no microcontrolador e no carregador USB, e plugar na tomada.


## 5. Visualizar os dados coletados
Para visualizar os dados coletados, acessar a plataforma Adafruit IO e abrir o link respectivo da estação:

- Estação 1 - Soldadinho-do-Araripe - https://io.adafruit.com/monitorar1/dashboards/monitorar-passarin-1
- Estação 2 - Bem-te-vi - https://io.adafruit.com/monitorar2/dashboards/monitorar-passarin-2
- Estação 3 - Canário-do-mato - https://io.adafruit.com/monitorar3/dashboards/monitorar-passarin-3
- Estação 4 - Pica-pau - https://io.adafruit.com/monitorar4/dashboards/monitorar-passarin-4
- Estação 5 - Tico-tico - https://io.adafruit.com/monitorar5/dashboards/monitorar-passarin-5

Para visualizar em tempo real a publicação dos dados, é necessário criar uma [conta na Adafruit IO](https://accounts.adafruit.com/users/sign_up). 

___

Projeto desenvolvido por Jéssyca Rios e Artur V. Cordeiro em parceria com a [ONG Abecê da Educação Ambiental](https://ongabcambiental.com)
