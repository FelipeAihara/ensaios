# Como o código "pisca LED" realmente funciona

### Introdução

O pisca LED é o "Olá Mundo" do mundo dos microcontroladores. Se você já programou um, é quase certo que esse tenha sido o seu primeiro projeto. Nesse ensaio, eu pretendo mostrar o que realmente acontece por trás desse programa. Ao final desse ensaio, você aprenderá:

- [ ] Como fazer um programa minimo para piscar um LED com apenas C e Assembly, sem chamar nenhuma função gpio_pin_set() ou algo do tipo.
- [ ] Como um microcontrolador realmente funciona

### Materiais utilizados

Caso você queira acompanhar esse ensaio, os seguintes materiais e software serão necessários:

- [ ] Placa Blackpill STM32F411CEU6 (WeAct)
- [ ] ST Link V2 

### O que acontece no reset

Para entender como fazer o LED da Blackpill piscar, vamos nos aprofundar no que o núcleo da placa (o Cortex-M4) faz. No reset (ver N3), ele simplesmente:

1. Lê a palavra 0 da memória e copia para o registrador SP (ponteiro de pilha).
2. Lê a próxima palavra (a palavra 1) e salta para o endereço de memória contido nela, que é a função reset_handler.    

### A função reset_handler()

O nosso reset_handler mínimo (começa em main.c:20), por sua vez, faz três coisas: ele (1) copia os dados da Flash para a seção .data da RAM, (2) zera os valores da seção .bss (ver N1) e (3) chama a main() (main.c:50). As duas primeiras etapas são essenciais por conta das regras da linguagem C (ver N2). 

### A main(): fazendo o LED piscar

Para fazer o LED piscar, precisamos realizar 3 etapas: (1) ligar o clock da porta C, configurar o pino PC13 como saída e, por fim, alternar a saída do pino PC13.

### Compilando o programa

Para gerar o código em Assembly, rodar:

arm-none-eabi-gcc -mcpu=cortex-m4 -mthumb \
  -O0 -ffreestanding -S main.c -o main.s

    

Para gerar o código de máquina, rodar:

arm-none-eabi-gcc -mcpu=cortex-m4 -mthumb \
  -O0 -g -ffreestanding -Wall \
  -c main.c -o main.o

### A chave final: o linker script



&nbsp;

# Algumas notas

### N1 - As divisões de dados da RAM



### N2 - Regras da linguagem C



### N3 - Interrupções



### Anotações (apagar depois)

- divisão de memória 
- Configurar o clock diferente e ver no osciloscópio
- Brincar em alterar o valor contido no primeiro endereço da Flash
- https://medium.com/@ragagr116/what-happens-before-main-understanding-the-startup-file-on-arm-cortex-m-46c9f55e1a6b
- pergunta do capitulo 1 de "Making embedded systems"
- se as instruções estão na Flash, como o código roda sem o bottleneck ser carregar as instruções da Flash para o PC? 
- nesse ensaio, me aprofundar um pouco mais em como o processador funciona (FETCH, DECODE, EXECUTE, STORE, cache, pipelines etc.)
