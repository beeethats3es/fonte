# fonte
> projeto de uma fonte retificadora com tensão de saída ajustável entre 3V e 12V e capacidade de corrente de 100mA, desenvolvido para a disciplina SSC0180.

## componentes e preços
| componente |      unidades      | preço |
|-------------|   :-------------:  | -----:|
| [led 5mm](https://www.caandma.com.br/produto/102048)      | 1 | R$0.50 |
| [resistor 2k2](https://www.caandma.com.br/produto/102269) | 1      |   R$0.07 |
| [resistor 750](https://www.caandma.com.br/produto/103984) | 1      |    R$0.07 |
| [resistor 4k7](https://www.caandma.com.br/produto/102282) | 1      |    R$0.07 |
| [poten. 10k](https://www.caandma.com.br/produto/102996) | 1      |    R$7.00 |
| [zener 13V](https://www.caandma.com.br/produto/106665) | 1      |    R$0.50 |
| [diod ret.](https://www.caandma.com.br/produto/101793) | 4      |    R$0.80 |
| [capac. 470μF](https://www.caandma.com.br/produto/108013) | 1      |    R$2.80 |
| [tip41c](https://www.caandma.com.br/produto/118540) | 1      |    R$3.80 |
| total |  | R$15.81 |

*\*não inclui protoboard e jumpers*

## explicação dos componentes:

### transformador
responsável por rebaixar a tensão da rede elétrica para um nível utilizável pelo circuito. o princípio de funcionamento baseia-se no acoplamento magnético entre dois enrolamentos: a variação de fluxo no primário induz uma tensão proporcional no secundário, determinada pela razão entre o número de espiras de cada bobina.

### ponte de diodos
conjunto de quatro diodos retificadores arranjados de modo a converter a tensão alternada proveniente do transformador em tensão contínua. como a tensão de entrada oscila entre valores positivos e negativos, a ponte garante que a corrente circule sempre no mesmo sentido pela carga, eliminando os semiciclos negativos da onda.

### capacitor (470μF)
atua como reservatório de energia, atenuando o ripple da tensão retificada. nos picos da onda, o capacitor se carrega; nos vales, ele descarrega sobre a carga, sustentando a tensão enquanto a entrada está abaixo do potencial armazenado. o resultado é uma tensão consideravelmente mais estável na saída da ponte.

### diodo zener (13V)
opera em polarização reversa, mantendo a tensão sobre si mesmo fixada em 13V enquanto a corrente disponível estiver dentro dos seus limites. essa referência estável é usada como teto de tensão para o restante do circuito, garantindo que a saída não ultrapasse o valor máximo especificado mesmo com variações na entrada.

### transistor tip41c
transistor npn de potência que funciona como elemento de controle e amplificação de corrente. a tensão de referência estabelecida pelo zener é aplicada à base, e o transistor conduz na saída uma corrente proporcional, suficiente para alimentar cargas de até 100ma sem sobrecarregar os componentes anteriores do circuito.

### potenciômetro (10kΩ)
divisor de tensão variável que controla o potencial aplicado à base do transistor e, consequentemente, a tensão entregue à carga. ao girar o cursor, o usuário ajusta continuamente a saída em toda a faixa de 3V a 12V.

### resistores
utilizados em diferentes pontos do circuito para definir pontos de operação e limitar correntes. o resistor em série com o led protege o componente contra sobrecorrente; os demais formam o divisor de tensão junto ao potenciômetro, determinando os limites inferior e superior da faixa de ajuste.

### led
indicador visual de funcionamento. permanece aceso enquanto o circuito está energizado e a corrente contínua está presente, confirmando que a retificação ocorreu e que a fonte está operacional.
