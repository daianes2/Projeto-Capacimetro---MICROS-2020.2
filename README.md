# Projeto-Capacimetro---MICROS-2020.2

Introdução

O projeto consiste na implementação de um capacimetro, através de uma simulação da leitura rápida, implementada por meio de um circuito digital composto por 4 dispositivos:  um CI LM555 no seu modo astavel de operação (temporizador), o  microprocessador STM32F103C6 , o CI PCF 8574 (também conhecido com I2C) e por um LCD de 16x2, utilizando 4 bits, pra fazer a amostragem do valor medido. Então, a capacitância a ser medida é a do capacitor C2 que está conectado ao CI 555.
A partir do LM 555 no modo astavel, uma onda pulsante é gerada e o seu sinal é conectado para um pino de entrada no microprocessador; e dentro do microprocessador, haverá um código e uma função que, a partir dos valores medidos das resistências e frequência do circuito do 555, calcula o valor da capacitância atual. Após isso, a partir da comunicação do STM com o I2C, esses dados são transmitidos e mostrados no LCD, que foi implementado em 4 bits. 
Utiliza-se um LM555, o microprocessador stm32F103c6, um dispositivo de interface, que é o I2C, e o LCD 16x2  A seguir, faremos um breve resumo do funcionamento. Assim, o pino 3 do circuito do LM555 vai para a entrada PA8 do nosso microprocessador STM32f103C6 e  como já foi dito, dentro do nosso processador haverá uma função que ira calcular essa frequência de saída e, com base nesse valor, vai calcular também o resultado da capacitância medida. 
	Nesse contexto, o I2C é que vai realizar a comunicação do nosso processador STM com o LCD, ele meio que funciona com uma ponte de conexão. Com relação à pinagem do I2C, os pinos VDD e VSS são os pinos de alimentação em 5V e de GND, respectivamente. Os pinos destacados em azul (SDA e SCL) são os pinos de interface e os pinos em laranja são os pinos das 8 saídas do PCF8574 (do pino P0 ao P7). Temos também um pino INT, usado para trabalhar com interrupções do microcontrolador. Mas que não vai ser usado para o nosso projeto. Os pinos(A0, A1 e A2) são os pinos de endereçamento do PCF8574, endereço esse que será usado para comunicação I2C com o chip e para identificar o dispositivo no barramento.
No LCD utilizado, pode-se observar os pinos que vao ser conectados do I2C para o LCD. O LCD tem um total de 14 pinos, conforme mostrado na figura, eles podem ser categorizados em quatro grupos: pinos de fonte (que são os pinos 1, 2 e 3): fornecem a potência e o nível de contraste do display, pinos de controle (4, 5 e 6): definem e controlam os registros no IC de interface do LCD. Para o nosso projeto, como vamos trabalhar apenas com 4 bits, no grupo de pinos de dados somente foram utilizados os pinos D4,D5,D6,D7


Desenvolvimento

Diagrama de blocos:

![image](https://user-images.githubusercontent.com/80993989/114303452-93ce0a80-9aa4-11eb-9429-c8fcd3fa273a.png)



 O diagrama de blocos mostrado resume como se deram as conexões ao longo de todo o circuito. Começando pelo LM 555, como já foi dito, o sinal pulsante de saída do pino 3 é conectado à porta PA8 do microprocessador STM32, onde o código contido nesse dispositivo vai realizar os cálculos necessários para determinar a capacitância medida (através dos dados de frequência e resistências). Em sequência, os pinos PB6 e PB7 do microprocessador, são conectados ao pinos de entrada do I2C, que são os pinos de interface SCL e SDA. Por fim, no circuito I2c, que realiza a interface entre o STM e o LCD, temos a conexão dos pinos de saída P0 a P7, com exceção ao pino P3 que não foi utilizado, aos pinos RW, RS, E e D4 a D7 (já que somente 4 bits foram utilizados).

Esquemático no PROTEUS:

![image](https://user-images.githubusercontent.com/80993989/114303248-8cf2c800-9aa3-11eb-970d-0a7092d5c6af.png)

Todos os pinos de saída mencionados no diagrama de blocos de cada elemento estão conectados aos respectivos pinos de entrada do próximo componente. 

I2C:

Os pinos de entrada do I2C, A0,A1 e A2 são os pinos de endereçamento, foram todos colocados em nível ALTO, por isso estão conectados ao VCC. Inicia apenas transmissão de dados para o LCD, que é o mestre, pois o objetivo é a escrita no endereço (4E).

STM32F103C6 no CUBEIDE32:


![image](https://user-images.githubusercontent.com/80993989/114303285-b4499500-9aa3-11eb-8cdd-391ac67152ff.png)

O pino de entrada corresponde ao PA8, que foi configurado como TIMER para obter os valores do tempo de subida e de descida. No caso, utiliza-se dois canais, um como Input Capture direct mode (para o tempo no alto) e o outro como Input Capture indirect mode (para o tempo no baixo). Os pinos PB6 E PB7 como saídas, que foram configurados, respectivamente, como SCL e SDA. O clock habilitado e configurado nos pinos PD0 e PD1, o qual utiliza-se o periférico RCC. Por fim, PA13 e PA14 foram configurados para habilitar o periférico SYS para setar os pinos de gravação.

O pino de entrada corresponde ao PA8, que foi configurado como TIMER para obter os valores do tempo de subida e de descida. No caso, utiliza-se dois canais, um como Input Capture direct mode (para o tempo no alto) e o outro como Input Capture indirect mode (para o tempo no baixo). Os pinos PB6 E PB7 como saídas, que foram configurados, respectivamente, como SCL e SDA. O clock habilitado e configurado nos pinos PD0 e PD1, o qual utiliza-se o periférico RCC. Por fim, PA13 e PA14 foram configurados para habilitar o periférico SYS para setar os pinos de gravação.


Conclusão

Entendimento do funcionamento do STM32F103C6, a comunicação do microcontrolador (MCU) com I2C e deste último com a placa LCD 16x2. Ademais, faz-se o uso do conhecimento adquirido dos periféricos e seus dispositivos para apresentar a programação do MCU para a amostragem do valor de capacitância no display.
Foi possível a compreensão das bibliotecas para formular o código no projeto e a implementação destas no código fonte. Devido a inexistência de erros ao compilar o código podemos chegar à conclusão que todas as funções foram aplicadas corretamente.
Na simulação no PROTEUS foi possível inicializar o LCD, mas a parte da escrita não foi possível realizar a verificação desta. A equipe conclui que o problema pode estar entre a comunicação do MCU e I2C, os pinos SDA e SCL não recebem sinal quando colocado o código para rodar. Embora, no código é possível verificar a inicialização do I2C. Portanto, devido ao curto período, deixa-se a problemática referente a ausência de sinal nas saídas do MCU como perspectiva para futuros projetos. 

Link do youtube:  https://youtu.be/buIgIBP0mwA
