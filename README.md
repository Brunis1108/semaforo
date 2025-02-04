# Semáforo com Temporizador Periódico

## Descrição
Este projeto implementa um semáforo utilizando um temporizador periódico no microcontrolador RP2040 com a Pico SDK. O semáforo alterna entre três LEDs (vermelho, azul e verde) a cada 3 segundos.

## Componentes Utilizados
- Microcontrolador Raspberry Pi Pico W
- 3 LEDs (Vermelho, Azul e Verde)
- 3 Resistores de 330Ω

## Esquema de Funcionamento
1. O sistema inicia com o LED vermelho aceso.
2. Após 3 segundos, o LED azul acende e o vermelho apaga.
3. Após mais 3 segundos, o LED verde acende e o azul apaga.
4. O ciclo se repete indefinidamente.

## Código
```c
#include <stdio.h>
#include "pico/stdlib.h"
#include "hardware/timer.h"

#define LED_AMARELO 12
#define LED_VERMELHO 13
#define LED_VERDE 11

// Variáveis globais
int estado_semaforo = 0;

// Callback para alternar o semáforo
bool semaforo_callback(struct repeating_timer *t) {
    gpio_put(LED_VERMELHO, estado_semaforo == 0);
    gpio_put(LED_AMARELO, estado_semaforo == 1);
    gpio_put(LED_VERDE, estado_semaforo == 2);
    estado_semaforo = (estado_semaforo + 1) % 3;
    return true; // Mantém o temporizador ativo
}

int main() {
    stdio_init_all();
    
    // Configuração dos LEDs
    gpio_init(LED_AMARELO);
    gpio_init(LED_VERMELHO);
    gpio_init(LED_VERDE);
    gpio_set_dir(LED_AMARELO, GPIO_OUT);
    gpio_set_dir(LED_VERMELHO, GPIO_OUT);
    gpio_set_dir(LED_VERDE, GPIO_OUT);
    
    // Inicia temporizador periódico para o semáforo
    struct repeating_timer timer;
    add_repeating_timer_ms(3000, semaforo_callback, NULL, &timer);
    
    // Loop principal
    while (true) {
        printf("Semáforo em execução...\n");
        sleep_ms(1000);
    }
}
```

## Como Executar
1. Configure o ambiente de desenvolvimento com a Pico SDK.
2. Compile o código e faça o upload para a Raspberry Pi Pico.
3. Conecte os LEDs e resistores conforme especificado.
4. Execute o programa e observe a troca de LEDs a cada 3 segundos.

## 
Projeto desenvolvido para experimentação com temporizadores no RP2040.
