# fonte
> projeto de uma fonte retificadora com tensão de saída ajustável entre 3V e 12V e capacidade de corrente de 100mA, desenvolvido para a disciplina SSC0180.

## componentes e preços
| componente |      unidades      | preço |
|-------------|   :-------------:  | -----:|
| [led 5mm](https://www.caandma.com.br/produto/102048)      | 1 | R$0.50 |
| [resistor 2k2](https://www.caandma.com.br/produto/102269) | 1      |   R$0.07 |
| [resistor 750](https://www.caandma.com.br/produto/103984) | 1      |    R$0.07 |
| [poten. 10k](https://www.caandma.com.br/produto/102996) | 1      |    R$7.00 |
| [zener 13V](https://www.caandma.com.br/produto/106665) | 1      |    R$0.50 |
| [diod ret.](https://www.caandma.com.br/produto/101793) | 4      |    R$0.80 |
| [capac. 470μF](https://www.caandma.com.br/produto/108013) | 1      |    R$2.80 |
| [tip41c](https://www.caandma.com.br/produto/118540) | 1      |    R$3.80 |
| total |  | R$15.54 |

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

## falstad
![alt text](https://github.com/beeethats3es/fonte/blob/main/falstad.png "falstad")

[circuito](https://www.falstad.com/s.php?s=7m6x03)

## easyeda
![alt text](https://github.com/beeethats3es/fonte/blob/main/schematicc.png "schematic")
![alt text](https://github.com/beeethats3es/fonte/blob/main/pcb.png "pcb")

## cálculos

### premissas

* **tensão de entrada:** rede elétrica residencial de $127\text{ V}_{RMS}$ / $60\text{ Hz}$.
* **faixa de tensão de saída ($V_{out}$):** regulável de $3\text{ V}$ a $12\text{ V}$.
* **corrente de carga mínima na saída ($I_{carga}$):** deve fornecer no mínimo **$100\text{ mA}$** continuamente.
* **fator de ondulação (*ripple*) máximo:** inferior a $10\%$ sob carga máxima.

### o estágio de entrada e retificação

utilizando um transformador comercial com relação de espiras de $1/7$, a tensão eficaz no secundário é de $\approx 18{,}14\text{ V}_{RMS}$.

1. **tensão de pico AC:**

$$V_{pico\_AC} = 18{,}14\text{ V} \cdot \sqrt{2} \approx 25{,}65\text{ V}$$

2. **queda na ponte retificadora:** a retificação em onda completa faz a corrente passar por dois diodos simultaneamente, gerando uma queda de $1{,}4\text{ V}$ ($2 \cdot 0{,}7\text{ V}$).

3. **tensão contínua de pico desejada ($V_{in}$):**

$$V_{in} = 25{,}65\text{ V} - 1{,}4\text{ V} = 24{,}2\text{ V}$$

### dedução do transistor e referência de tensão

o transistor de potência **TIP41C** (NPN) é configurado como **seguidor de emissor** (coletor comum) para fornecer o ganho de corrente necessário para os $\geq 100\text{ mA}$ da carga.

* devido à barreira de potencial interna do transistor, a tensão na base precisa ser $0{,}7\text{ V}$ maior do que a tensão de saída desejada:

$$V_{base} = V_{out} + V_{BE} = V_{out} + 0{,}7\text{ V}$$

* para garantir a saída máxima de $12\text{ V}$, a tensão máxima na base deve ser:

$$V_{base\_max} = 12\text{ V} + 0{,}7\text{ V} = 12{,}7\text{ V}$$

**dedução do componente:** escolhe-se um **diodo zener comercial de $13\text{ V}$** como referência de tensão. ele estabiliza o topo da malha de controle, permitindo que a saída máxima atinja até $13\text{ V} - 0{,}7\text{ V} = 12{,}3\text{ V}$, o que atende perfeitamente à especificação de $\geq 12\text{ V}$.

### 4. passo 3: dedução da malha de controle (potenciômetro e resistor inferior)

para realizar o ajuste linear entre $3\text{ V}$ e $12\text{ V}$, adota-se um potenciômetro comercial padrão de **$10\text{ k}\Omega$** (um valor ideal para limitar o consumo interno e perdas térmicas).

#### cálculo do resistor de limitação inferior ($R_{inferior}$)

para impedir que o usuário zere a tensão na saída (o que violaria o requisito mínimo de $3\text{ V}$), inserimos um resistor em série entre o potenciômetro e o terra (GND).

* para garantir que os $3\text{ V}$ de saída sejam alcançados de forma limpa e absorvendo margens de erro de fabricação dos componentes (tolerâncias normais de até 20%), projetamos o circuito para ter uma tensão mínima teórica de aproximadamente $1{,}6\text{ V}$ no limite extremo do curso.
* no batente mínimo ($0\,\Omega$ no potenciômetro), a tensão na base é dada pelo divisor de tensão fixo sobre os $13\text{ V}$ do zener:

$$V_{base\_min} = 13\text{ V} \cdot \left(\frac{R_{inferior}}{10\text{ k}\Omega + R_{inferior}}\right)$$

isolando para encontrar o resistor ideal:

$$\frac{R_{inferior}}{10000 + R_{inferior}} = \frac{2{,}34\text{ V}}{13\text{ V}} \approx 0{,}18$$

$$R_{inferior} \approx 2200\,\Omega = 2{,}2\text{ k}\Omega$$

**validação:** a escolha do resistor comercial de **$2{,}2\text{ k}\Omega$** fixa a tensão mínima da base em $2{,}34\text{ V}$, gerando uma saída real mínima de $1{,}64\text{ V}$. isso cria a margem de engenharia perfeita para calibrar e estabilizar os **$3\text{ V}$** exigidos pelo projeto ao longo do início do curso mecânico do potenciômetro.

### dedução do resistor de polarização do zener ($R_{750}$)

para calcular o valor deste resistor, analisamos o pior cenário de consumo elétrico: quando a fonte está entregando a corrente máxima de carga ($100\text{ mA}$).

1. **corrente de base do transistor ($I_{base}$):** considerando o ganho de corrente mínimo do TIP41C sob carga ($h_{FE} \approx 25\text{-}30$), a base demandará:

$$I_{base\_max} = \frac{I_{carga}}{h_{FE}} = \frac{100\text{ mA}}{25} = 4{,}0\text{ mA}$$

2. **corrente do ramo do potenciômetro ($I_{pot}$):** o divisor consome uma corrente fixa de:

$$I_{pot} = \frac{13\text{ V}}{10\text{ k}\Omega + 2{,}2\text{ k}\Omega} = 1{,}07\text{ mA}$$

3. **corrente de manutenção do zener ($I_{Z\_min}$):** para manter o diodo zener firmemente na região de regulação estável (evitando o corte), ele necessita de uma corrente mínima de segurança de cerca de $5{,}0\text{ mA}$.

somando as necessidades do nó regulado, a corrente total que deve entrar no ramo vinda da linha de pré-regulação é:

$$I_{ramo} = I_{base\_max} + I_{pot} + I_{Z\_min} = 4{,}0\text{ mA} + 1{,}07\text{ mA} + 5{,}0\text{ mA} = 10{,}07\text{ mA}$$

o resistor parte do nó de pré-regulação ($22{,}2\text{ V}$) e chega até o nó do zener ($13\text{ V}$), absorvendo uma diferença de potencial de $9{,}2\text{ V}$. o valor máximo permitido para a resistência é:

$$R_{Z\_max} = \frac{22{,}2\text{ V} - 13\text{ V}}{10{,}07\text{ mA}} = \frac{9{,}2\text{ V}}{0{,}01007\text{ A}} \approx 913\,\Omega$$

**dedução do componente:** escolhe-se o valor comercial padrão inferior mais próximo, que é **$750\,\Omega$**. isso garante que, mesmo sob carga máxima de saída, o zener receberá corrente suficiente para manter a tensão cravada e estável em $13\text{ V}$.

### dedução do resistor de amortecimento do nó do LED ($R_{shunt}$)

o circuito inclui um LED indicador conectado em série a partir da linha de entrada de $24{,}2\text{ V}$. o LED impõe uma queda constante de $2{,}0\text{ V}$, gerando um nó estável de $22{,}2\text{ V}$.

como calculado no passo anterior, o ramo do zener puxará exatamente uma corrente de:

$$I_{R750} = \frac{22{,}2\text{ V} - 13\text{ V}}{750\,\Omega} = 12{,}27\text{ mA}$$

se deixássemos apenas este caminho, a corrente do LED seria limitada a apenas $12{,}27\text{ mA}$. para fazer o LED operar em seu ponto de brilho ideal e seguro ($\approx 22\text{ mA}$) e blindar o circuito contra variações provocadas pelo potenciômetro, inserimos um resistor shunt para o terra de modo a drenar a corrente complementar ($\approx 10\text{ mA}$):

$$R_{shunt} = \frac{22{,}2\text{ V}}{10\text{ mA}} = 2220\,\Omega$$

**dedução do componente:** o valor padrão comercial exato de **$2{,}2\text{ k}\Omega$** é selecionado. ele fixa o consumo do nó em $10{,}09\text{ mA}$, resultando em uma corrente total combinada no LED de $22{,}36\text{ mA}$, garantindo estabilidade e alta luminosidade ao indicador visual.

### validação do filtro capacitivo ($470\\mu\text{F}$)

com o circuito completamente dimensionado e operando sob carga nominal exigida de $100\text{ mA}$, validamos se o capacitor escolhido de $470\,\mu\text{F}$ atende ao critério de *ripple* inferior a 10%.

1. **corrente quiescente do controle:** $I_{controle} = 22{,}36\text{ mA} = 0{,}02236\text{ A}$
2. **corrente da carga nominal:** $I_{carga} = 100\text{ mA} = 0{,}10000\text{ A}$
3. **corrente de dreno total sob carga máxima ($I_{total}$):**

$$I_{total} = I_{controle} + I_{carga} = 22{,}36\text{ mA} + 100\text{ mA} = 122{,}36\text{ mA} = 0{,}12236\text{ A}$$

aplicando a fórmula física da ondulação para uma retificação em onda completa ($f = 120\text{ Hz}$):

$$\Delta V = \frac{I_{total}}{f \cdot C} = \frac{0{,}12236\text{ A}}{120\text{ Hz} \cdot (470 \cdot 10^{-6}\text{ F})} = \frac{0{,}12236}{0{,}0564} \approx 2{,}17\text{ V}$$

### cálculo do percentual de ripple final

$$\\text{ Ripple} = \left( \frac{\Delta V}{V_{in}} \right) \cdot 100 = \left( \frac{2{,}17\text{ V}}{24{,}2\text{ V}} \right) \cdot 100 \approx 8{,}96\%%$$

## fotos e vídeo do projeto
![alt text](https://github.com/beeethats3es/fonte/blob/main/foto1.jpg "foto1")
![alt text](https://github.com/beeethats3es/fonte/blob/main/foto2.jpg "foto2")

[vídeo](https://youtu.be/5UbX9_9E9Ko)

## integrantes:

Guilherme Bonfim Mendes Campelo. 
João Pedro Soares Freire de Castro. 
Bernardo Schramm Schutz Machado. 
