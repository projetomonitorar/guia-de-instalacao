
![MonitorAr](https://github.com/projetomonitorar/monitorar/raw/master/img/monitorar_logo_600px.png "MonitoAr")

# Guia de Uso

## Apresentação
O projeto MonitorAr é uma iniciativa de ciência cidadã de sensoriamento participativo sobre a qualidade do ar. A proposta é experimentar coletivamente a coleta, visualização e análise de dados climáticos, usando tecnologia aberta com uma plataforma de IoT (internet das coisas) e sensores que medem temperatura do ar, umidade ar, pressão  atmosférica e presença de gases tóxicos.

## 3. Instalação do kit MonitorAr
O kit MonitorAr é composto um microcontrolador Esp8266 NodeMCU e por dois sensores: o BME280 que mede a temperatura do ar, em graus Celsius, a umidade do ar, em taxa percentual, e pressão atmosférica, em hectoPascal, e com base nesses dados, calcula a altitude, em metros; e o sensor MQ-135, que detecta presença de gases tóxicos, sem unidade, em uma faixa de valor de concentração que varia de 0 a 1023.

Os dados coletados pelos sensores são publicados no servidor da Adafruit IO a cada intervalo de aproximadamente 30 segundos, usando o protocolo de comunicação [MQTT](https://pt.wikipedia.org/wiki/MQTT), que

Antes de por o kit em funcionamento, é necessário atualizar o código do microcontrolador com o nome e senha da rede WiFi. 
Após a atualização do código, deverá ser feita a instalação do kit, preferencialmente em uma área externa coberta, protegida da chuva, água, do sol e de fontes de calor. O kit deverá ser instalado em uma altura entre 1,20m e 2,00m acima do solo - para diminuir a interferência do solo na medida dos dados climáticos do ar.

O kit deverá ser fixado usando a abraçadeira de nylon como ponto de amarração (Imagem X).

Após a fixação do kit, conectar o cabo USB no microcontrolador e no carregador USB, e plugar na tomada (Imagem X).

Para visualizar os dados coletados, acessar a plataforma Adafruit IO e abrir o link respectivo da estação:
- Estação 1 - Soldadinho-do-Araripe - https://io.adafruit.com/monitorar1
- Estação 2 - Bem-te-vi - https://io.adafruit.com/monitorar2
- Estação 3 - Canário-do-mato - https://io.adafruit.com/monitorar3
- Estação 4 - Pica-pau - https://io.adafruit.com/monitorar4
- Estação 5 - Tico-tio - https://io.adafruit.com/monitorar5

### 3.1. Adicionar ESP8266 no Arduino IDE  
Para atualizar o código é necessário baixar o *Arduino IDE* - o software que permite escrever e enviar código para o microcontrolador Arduino, Esp8266, entre outros.

Acessar https://www.arduino.cc/en/Main/Software e baixar o *Arduino IDE* de acordo com o seu sistema operacional. 

Após a conclusão da instalação, conectar o cabo USB no kit MonitorAr, e a outra ponta do cabo conectar no computador. Em seguida, abrir o *Arduino IDE*. 
Selecionar o menu **Arquivo > Preferências** para abrir a janela de preferências. No campo **URLs Adicionais para Gerenciadores de Placas**, adicione: `https://arduino.esp8266.com/stable/package_esp8266com_index.json`. Fechar a janela clicando em OK.

No menu **Ferramentas > Placas**, selecionar na lista a placa **NodeMCU 1.0 (ESP-12E Module)**. Novamente no menu **Ferramentas > Porta**, selecionar a porta que estiver disponível.

Observação: se tiver mais de uma porta *Com* disponível, anote os nomes, pressione a tecla Esc e desplugue o kit MonitorAr do computador. Abrir novamente **Ferramentas > Porta** e observe qual porta *Com* foi desligada, a que não está mais na lista é a porta *Com* do MonitorAr, que deverá ser selecionada.

Para testar se a instalação foi bem sucedida, abrir o menu **Arquivo > Exemplos > 01. Basics > Blink**. Substituir `LED_BUILTIN` por `D4` (como está escrito, com *D* maiúsculo). Em seguida, no meu **Skecth > Carregar**, ou pressionar `Ctrl+U`. O código Blink será enviado ao microcontrolador, e o led interno do Esp vai piscar no intervalo de 1000 microsegundos (1 segundo). 

### 3.2 Adicionar bibliotecas no Arduino IDE
Bibliotecas são códigos adicionais que estendem a funcionalidade do *Arduino IDE* para usar com módulos, sensores, entre outros, desenvolvidos e compartilhados por colaboradores. Para fazer a atualização do código do MonitorAr, é necessário instalar as seguintes bibliotecas:

- [BlueDot_BME280](https://github.com/BlueDot-Arduino/BlueDot_BME280), para usar o sensor BME280;
- [Adafruit_MQTT](https://github.com/adafruit/Adafruit_MQTT_Library), para acessar e publicar os dados no servidor da Adafruit IO. 

Para instalar a biblioteca Blue

___

Projeto desenvolvido por Jéssyca Rios e Artur V. Cordeiro em parceria com a [ONG Abecê da Educação Ambiental](https://ongabcambiental.com)


