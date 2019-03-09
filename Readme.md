
# Instalando MicroPython no ESP32 WiFi LoRa 32 usando uPyCraft IDE
Neste tutorial irei mostrar como instalar MicroPython no ESP32 e demonstrar um exemplo básico de funcionamento.

> Tutorial feito no Ubuntu 16.04, no 18.04 não funcionou por causa de dependênciais

## Instalação do MicroPython
Iremos utilizar a IDE uPyCraft para gravar o firmware do MicroPython e para desenvolver os códigos

### Baixar o uPyCraft IDE
Faça o download: [**uPyCraft v1.0**](https://github.com/clebrw/MycroPython-ESP32-uPyCraft/raw/master/uPyCraft_linux_V1.0) 

### Executando a IDE
Depois de baixado, teremos que torná-lo executável.

```sh
cd Downloads
chmod +x uPyCraft_linux_V1.0
```
Vamos executá-lo.
```sh
./uPyCraft_linux_V1.0
```
### Instalando o firmware do MicroPython
Depois de aberto a IDE selecione a porta de comunicação entre o computador e o ESP32. Vá em:
> Tools/Serial/dev/ttyUSB0

Agora iremos gravar o firmware. Portanto selecione:
> Tools/BurnFirmware

Na janela que irá aparecer selecione:

`boards : esp32`
`burn_addr : 0x1000`
`erase_flash : enable`
`com : /dev/ttyUSB0`
`Firmware Choose : Users`
`Selecione o firmware` que pode ser baixado [**aqui**](https://github.com/clebrw/MycroPython-ESP32-uPyCraft/raw/master/esp32-20190125-v1.10.bin). Estou usando a versão V1.10 (jan-19).

Para novas versões e possíveis atualizações: [**Acesse**.](http://micropython.org/download#esp32)

## Piscando um LED
Vamos utilizar o clássico algoritmo de piscar led para testar a placa.

### Conectando a placa 
O botão **Connect** está na lateral direita da interface (simbolo de uma corrente), dê um clique para a IDE abrir a comunicação com a plaquinha.

### Arquivos Python na memória do ESP32
Dentro do chip de memória dele existe um arquivo chamado `boot.py` que pode ser acessado através do campo superior esquerdo, clicando em **devices.**
Este arquivo é o primeiro a ser executando quando ligamos ele. Por possuir alguns parâmetros de funcionamento da placa não recomendo alterar o código contido nele.

### Criando o arquivo do nosso código (`main.py`)

Como não é recomendável alterar o arquivo `boot.py` , para colocar nosso código será necessário criar um arquivo chamado `main.py`, este sim vamos editá-lo. 
Detalhe: depois que o `boot.py` terminou de ser executado, o próximo será ele.

Para criá-lo basta ir em **New**.
Na janela que apareceu cole o seguinte código, este que irá piscar o led da porta 25 da placa.
```python
import time
from machine import Pin
led=Pin(25,Pin.OUT)        #Criamos um objeto chamado led. Pin(porta_led, entrada_ou_saida)

while True:
	led.value(1)             #liga o led
	time.sleep(0.1)
	led.value(0)             #desliga o led
	time.sleep(0.5)
```
### Salvando o arquivo
Para salvar dê um **Ctrl+s**, ele vai pedir para dar um nome, digite `main.py` e dê **OK**.

###  Gravando o código na placa ESP32 e executando-o
Clique em **DonwloadAndRun** (icone de play) na lateral direita da IDE.
Assim seu código vair ser Gravado na memória da placa e também será executado.

### Pronto!
Agora se você removê-lo e ligá-lo em uma bateria perceberá que o código continuará funcionando. 

## Utilizando o display OLED do módulo
Para utilizar o display será necessário importar a biblioteca do display `ssd1306.py` para a memória dele.
Para isso, procure na lateral esquerda superior a pasta `uPy_lib`, dentro dela você encontrará esta biblioteca.
Dê dois cliques para abri-lá e clique em download para gravá-la na memória da placa.

### Código principal (`main.py`)
Agora digite o código abaixo no arquivo `main.py`:
```python
import machine, ssd1306

pin16 = machine.Pin(16, machine.Pin.OUT)                        # Pino 16 de RST do display
pin16.value(1)												    # Ativando o display

i2c = machine.I2C(scl=machine.Pin(15), sda=machine.Pin(4))      # scl = 15, sda = 4

oled = ssd1306.SSD1306_I2C(128, 64, i2c)						
oled.fill(0)                                                    # deixa toda a tela na cor preta
oled.text('OLA MUNDO', 0, 0)
oled.show()
```

Se tudo ocorreu bem, vai aparecer na tela um **OLA MUNDO**.