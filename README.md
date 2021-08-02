# STM32-DAC-API-pt



# Projeto: 

Esse projeto foi desenvolvido no contexto da disciplina “Programação de Sistemas Embarcados” ofertada para o curso de Engenharia Elétrica da UFMG. O objetivo desse projeto é desenvolver uma AN (Application Note) para o periférico de DAC (Digital Analog Converter) e um projeto exemplo que implemente o uso do DAC. A AN está disponível nos arquivos do repositório junto com os arquivos gerados pelo projeto. 

# Informações:

UNIVERSIDADE FEDERAL DE MINAS GERAIS

DEPARTAMENTO DE ENGENHARIA ELETRÔNICA 

Programação de Sistemas Embarcados 

Professor: Ricardo de Oliveira Duarte

Autor: Victor Freitas Borges - victorfborges75@gmail.com

Versão: 1.0

Licença: GNU 3

Data: 08/2021

Application Note: AN01-1.0 - DAC (Digital Analog Converter) 

# Hardware: 

O microcontrolador utilizado nessa aplicação é o STM32H743ZIT6U que está presente na placa de desenvolvimento NUCLEO-H743ZI. O modelo pode ser visualizado na sequinte imagem

![image](https://user-images.githubusercontent.com/60392063/127783297-c47d17a6-8c60-4f34-9532-e276d1ce88bc.png)

# Exemplo

O projeto exemplo consiste na configuração do periférico de DAC da NUCLEO-H743 como saída única, associado a um pino externo. 

## Variáveis 

```
uint32_t dado;
```
Variável que é passada como referência para o DAC gerar um valor de saída porporcional a ela. Pode variar de 0 a 4095, já que o DAC pode ser configurado para até 12 bits.

```
uint32_t buff[1];
```
Vetor passado como referência para a função que da "star" no periférico do DAC. 

```
uint32_t valor_dac

```
Variável implementada para receber a saída da função que lê a saída do DAC. 

## Funções
```
HAL_DAC_Start_DMA(&hdac1, DAC_CHANNEL_1, &buff, 1, DAC_ALIGN_12B_R);
```
Função da HAL da família STM32H7 que inicía o periférico DAC com DMA (Direct Memory Acess). Os argumentos dessa função são, respectivamente, o endereço da estrutura que carrega as configurações (geradas no CUBE) do DAC; o canal que vai ser utilizado; buffer usado para trnasmitir os dados do DMA para o DAC; o tamanho do buffer; a configuração do tamanho da mensagem (essa podendo variar entre 12 bits alinhado a direita, 12 bits alinhado a esquerda ou 8 bits). 
```
 HAL_DAC_SetValue(&hdac1, DAC_CHANNEL_1, DAC_ALIGN_12B_R, dado);
```
Função da HAL da família STM32H7 que define o valor de saída do periférico DAC. Essa função recebe como parâmetros, o endereço da estrutura que carrega as configurações (geradas no CUBE) do DAC;  o canal que vai ser utilizado; a configuração do tamanho da mensagem e o valor de referência que vai ser convertido na saída em forma de tensão.
```
uint32_t valor_dac = HAL_DAC_GetValue(&hdac1, DAC_CHANNEL_1);
```
Função da HAL da família STM32H7 que retorna o valor que está sendo recebido pelo DAC via DMA. Essa função tem como parâmentros o endereço da estrutura que carrega as configurações (geradas no CUBE) do DAC e o canal que vai ser utilizado. Essa função retorna um valor "unsigned int" de 32 bits que pode ser comparado ao valor da variável "dado" usada na função anterior. 


## Implementação 

Podemos ver na seguinte imagem como fica a estrutura da main do código exemplo. Podemos observar que o valor da variável dado foi arbitrado como 2048. 

![image](https://user-images.githubusercontent.com/60392063/127936634-3ba1de3f-0e42-43c4-9084-10eb2b8c0166.png)

