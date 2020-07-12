
![MonitorAr](https://github.com/projetomonitorar/monitorar/raw/master/img/monitorar_logo_600px.png "MonitoAr")

Projeto desenvolvido por Jéssyca Rios e Artur V. Cordeiro em parceria com a [ONG Abecê da Educação Ambiental](https://ongabcambiental.com)

# Guia de Uso

## Apresentação
O projeto MonitorAr é uma iniciativa de ciência cidadã de sensoriamento participativo sobre a qualidade do ar. A proposta é experimentar coletivamente a coleta, visualização e análise de dados climáticos, usando tecnologia aberta com uma plataforma de IoT (internet das coisas) e sensores que medem temperatura do ar, umidade ar, pressão  atmosférica e presença de gases tóxicos.

## 3. Instalação do kit MonitorAr
O kit MonitorAr é composto um microcontrolador Esp8266 NodeMCU e por dois sensores: o BME280 que mede a temperatura do ar, em graus Celsius, a umidade do ar, em taxa percentual, e pressão atmosférica, em hectoPascal; e o sensor MQ-135, que detecta presença de gases tóxicos, sem unidade, em uma faixa de valor de concentração que varia de 0 a 1023.
Antes de por o kit em funcionamento, é necessário atualizar o código do microcontrolador com o nome e senha da rede WiFi (ver ítem 3.1 Atualizando o código). 
Após a atualização do código, deverá ser feita a instalação do kit, preferencialmente em uma área externa coberta, protegida da chuva, água e de fontes de calor. O kit deverá ser instalado em uma altura entre 1,20m e 2,00m acima do solo - para diminuir a interferência do solo na medida dos dados climáticos do ar.
O kit deverá ser fixado usando a abraçadeira de nylon como ponto de amarração (Imagem X). 
Após a fixação do kit, conectar o cabo USB no microcontrolador e no carregador USB, e plugar na tomada (Imagem X).
Para visualizar os dados coletados, acessar a plataforma Adafruit IO e abrir o link respectivo da estação:
- Estação 1 - Soldadinho-do-Araripe - https://io.adafruit.com/monitorar1
- Estação 2 - Vira-folha - https://io.adafruit.com/monitorar2 
- Estação 3 - Vem-vem - https://io.adafruit.com/monitorar3/
- Estação 4 - Suiriri - https://io.adafruit.com/monitorar4
- Estação 5 - Sibite - https://io.adafruit.com/monitorar5

### 3.1. Atualizando o código 
Para atualizar o código é necessário baixar o Arduino IDE - o software que permite escrever e enviar código para o microcontrolador Arduino, Esp8266, entre outros.
Acessar https://www.arduino.cc/en/Main/Software e baixar o Arduino IDE de acordo com o seu sistema operacional. 
Após a conclusão da instalação, conectar o cabo USB no kit MonitorAr, e a outra ponta do cabo conectar no computador. Em seguida, abrir o Arduino IDE. 
No menu Arquivo, selecionar Preferências. No campo URLs Adicionais para Gerenciadores de Placas, adicione: https://arduino.esp8266.com/stable/package_esp8266com_index.json. Fechar a janela clicando em OK.
No menu Ferramentas, clicar em Placas, na lista selecionar a placa NodeMCU 1.0 (ESP-12E Module). Novamente no menu Ferramentas, clicar em Porta, selecionar a porta que estiver disponível. 
Observação: se tiver mais de uma porta Com disponível, anote os nomes, pressione a tecla Esc e desplugue o kit MonitorAr do computador. Abrir novamente Ferramentas, Porta e observe qual porta Com foi desligada, a que não está mais na lista é a porta Com do MonitorAr, que deverá ser selecionada.
Para testar se a instalação foi bem sucedida, abrir o menu Arquivo, clicar em Exemplos, selecionar 01. Basics > Blink. Substituir LED_BUILTIN por D4 (como está escrito, com D maiúsculo). Em seguida, no meu Skecth, selecionar Carregar, ou pressionar Ctrl+U. O código Blink será enviado ao microcontrolador, e o led interno do Esp vai piscar no intervalo de 1000 microsegundos (1 segundo). 

