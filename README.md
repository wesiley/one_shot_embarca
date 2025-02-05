# Projeto Temporizador de apenas um disparo (one-shot) 🚀

Este projeto faz com que ascenda os 3 LED(azul,vermelho e verde) em apenas um disparo ao apertar no pushbutton, e logo em seguida vai alternando o sinal de cada LED periodicamente. 
## Hardware 🛠️

- Microcontrolador RP2040 (Raspberry Pi Pico).
- PushButton.
- Resistores.
- 3 LEDs.
- 03 Resistores de 330 Ω. 

## Software 💻

* **SDK do Raspberry Pi Pico:** O SDK (Software Development Kit) do Pico, que inclui as bibliotecas e ferramentas necessárias para desenvolver e compilar o código. [Instruções de instalação](https://www.raspberrypi.com/documentation/pico/getting-started/)
* **CMake:** Um sistema de construção multiplataforma usado para gerar os arquivos de construção do projeto.
* **Compilador C/C++:**  Um compilador C/C++ como o GCC (GNU Compiler Collection).
* **Git:** (Opcional) Para clonar o repositório do projeto.


### O código está dividido em vários arquivos para melhor organização:

- **`One_shot.C`**: Código com a função de loop principal: gera o código que faz com que ascenda os leds em apenas um disparo e desligue os leds periodicamente.
- **`CMakeLists.txt`:** Define a estrutura do projeto para o CMake.
- **`diagram.json`:** projeta a simulação do semaforo.
- **`wokwi.toml`:** configuração para sicronizar o código a simulação.




## Como Compilar e Executar ⚙️

1. **Instale o SDK do Raspberry Pi Pico:** Siga as instruções no site oficial do Raspberry Pi.
2. **Clone este repositório:** `https://github.com/wesiley/one_shot_embarca`
3. **Navegue até o diretório do projeto:** `cd One_shot`
4. **Compile o projeto:** `cmake -B build && cmake --build build`
5. **Copie para o Pico:** Copie o conteúdo da pasta `build` (gerada após a compilação) para o Raspberry Pi Pico. O programa iniciará automaticamente.


## Funcionamento do Loop Principal 🔄 
```
   while (true) {
         if (gpio_get(BUTTON) == 0 && !led_active) {
            sleep_ms(50);

            if (gpio_get(BUTTON) == 0 && gpio_get(LED_RED)==0 && gpio_get(LED_BLUE)==0 && gpio_get(LED_GREEN)==0) {
                gpio_put(LED_RED, true);
                gpio_put(LED_BLUE,true);
                gpio_put(LED_GREEN,true);


                led_active = true;
                
                add_alarm_in_ms(3000, turn_off_callback, NULL, false);
                add_alarm_in_ms(4000, turn_off_callback2, NULL, false);
                add_alarm_in_ms(5000, turn_off_callback3, NULL, false);
                  
            }
           
        }
    
    sleep_ms(10);

}
  ```
O loop while (true) garante execução contínua. A primeira condição IF tem como objetivo fazer o debounce que faz controle de corrente ao clicar no pushbutton. A segunda condição serve para chamar os temporizadores apenas quando clicar no pushbutton e todos os Leds estiverem desligados. led_active = true é para indicar que os Leds vao ligar. add_alarm_in_ms temporizador que chama uma função e dispara em um tempo definido. sleep_ms(10) para apos 10 milisegundos voltar ao inicio do loop.

## Funcionamento do Temporizador.
```
int64_t turn_off_callback(alarm_id_t id, void *user_data) {
 
        gpio_put(LED_BLUE,false);
     
    led_active = false;

    return 0;
}
int64_t turn_off_callback2(alarm_id_t id, void *user_data) {
 
        gpio_put(LED_RED,false);
     
    led_active = false;

    return 0;
}
int64_t turn_off_callback3(alarm_id_t id, void *user_data) {
 
        gpio_put(LED_GREEN,false);
     
    led_active = false;

    return 0;
}

  ```
as funções turn_off_callback, turn_off_callback2 e turn_off_callback3 são temporizadores que tem como função desligar cada Led e retornar um valor.

## Diagrama de Conexões 💡:

https://wokwi.com/projects/421897225934561281
  
 ## Contribuições 🤝

Contribuições são bem-vindas! Sinta-se à vontade para abrir issues ou enviar pull requests.
