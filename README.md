# Countroller

> Sistema interativo embarcado com matriz de LEDs, controle por joystick e feedback visual e sonoro

![C](https://img.shields.io/badge/C-11-blue?logo=c)
![CMake](https://img.shields.io/badge/CMake-3.13+-blue?logo=cmake)
![Raspberry Pi Pico](https://img.shields.io/badge/Raspberry%20Pi%20Pico-W-red?logo=raspberrypi)
![License](https://img.shields.io/badge/License-MIT-green)

## Sobre

**Countroller** é um sistema embarcado desenvolvido para o Raspberry Pi Pico W que integra hardware de entrada (joystick, botões) com saídas interativas (matriz de LEDs, display OLED, buzzers). O projeto demonstra conceitos fundamentais de sistemas embarcados, programação C modular e integração de componentes eletrônicos.

O projeto foi desenvolvido como trabalho final da Unidade 7 do programa [EmbarcaTech](https://www.embarcatech.com.br/), consolidando conhecimentos em programação embarcada, arquitetura de microcontroladores e IoT.

## Destaques

- 🎮 **Controle por joystick**: navegação de cursor em matriz 5×5 de LEDs RGB
- 🔴 **Matriz NeoPixel**: 25 LEDs endereçáveis individualmente com cores customizáveis
- 📊 **Display OLED**: feedback visual em tempo real (posição do joystick, contadores de botões)
- 🔘 **Sistema de botões**: dois botões com contador de pressionamentos e feedback sonoro
- 🔊 **Buzzers PWM**: dois buzzers para feedback auditivo controlado por modulação de largura de pulso
- 🎨 **Padrões pré-programados**: exibição de padrões visuais (IF, PI, Coração) na matriz LED
- 🏗️ **Arquitetura modular**: código organizado em componentes independentes e reutilizáveis

## Como Funciona

O sistema opera em um loop principal que realiza continuamente:

1. **Leitura de entrada**: captura posição do joystick (eixo X e Y via ADC) e estado dos botões
2. **Atualização visual**: posiciona LED na matriz de acordo com o joystick
3. **Processamento de eventos**: detecta pressionamentos de botões e incrementa contadores
4. **Feedback multissensorial**: 
   - Exibe informações no display OLED
   - Aciona LEDs RGB de status
   - Toca feedback sonoro via buzzer

Ao inicializar, o sistema exibe três padrões animados (IF, PI, Coração) na matriz de LEDs como confirmação de funcionamento.

## Hardware

| Componente | Quantidade | Detalhes |
|---|---|---|
| **Microcontrolador** | 1 | Raspberry Pi Pico W |
| **Matriz de LEDs** | 1 | 5×5 NeoPixel (WS2812B) — 25 LEDs RGB endereçáveis |
| **Display** | 1 | SSD1306 OLED 128×64 (I2C) |
| **Buzzers** | 2 | Controle PWM |
| **Botões** | 2 | Push buttons (A e B) |
| **Joystick** | 1 | Analógico com botão integrado |
| **LEDs RGB** | 3 | Vermelho, Verde, Azul para status |

### Mapeamento de Pinos

| Componente | Pino GPIO | Tipo |
|---|---|---|
| Matriz NeoPixel | GPIO 7 | Digital (PIO) |
| Display OLED (SDA) | GPIO 14 | I2C |
| Display OLED (SCL) | GPIO 15 | I2C |
| Joystick X | GPIO 26 | ADC0 |
| Joystick Y | GPIO 27 | ADC1 |
| Botão A | GPIO 5 | Digital Input |
| Botão B | GPIO 6 | Digital Input |
| Joystick Button | GPIO 22 | Digital Input |
| Buzzer 1 | GPIO 21 | PWM |
| Buzzer 2 | GPIO 10 | PWM |
| LED Azul | GPIO 12 | Digital Output |
| LED Vermelho | GPIO 13 | Digital Output |
| LED Verde | GPIO 11 | Digital Output |

## Tecnologias

| Categoria | Tecnologia |
|---|---|
| **Microcontrolador** | Raspberry Pi Pico W |
| **Linguagem** | C11 |
| **Build System** | CMake 3.13+ |
| **SDK** | Raspberry Pi Pico SDK 1.5.1 |
| **Padrão** | Modular C |
| **Periféricos** | I2C, ADC, GPIO, PWM, PIO |

## Estrutura do Projeto

```
Countroller/
├── CMakeLists.txt                 # Configuração de build
├── pico_sdk_import.cmake          # Importação do Pico SDK
├── include/                       # Headers
│   ├── buttons.h                 # Controle de botões e LEDs RGB
│   ├── buzzer.h                  # Controle de buzzers PWM
│   ├── display.h                 # Display OLED
│   ├── init.h                    # Inicialização do sistema
│   ├── joystick.h                # Leitura do joystick
│   ├── neopixel.h                # Controle da matriz LED
│   └── ssd1306/                  # Driver OLED
│       ├── ssd1306.h
│       ├── ssd1306_font.h
│       └── ssd1306_i2c.h
├── lib/
│   └── ssd1306/
│       └── ssd1306_i2c.c         # Implementação do driver OLED
├── pio/
│   └── ws2818b.pio               # Código PIO para NeoPixel
├── src/                          # Implementação
│   ├── main.c                    # Loop principal
│   ├── init.c                    # Inicialização
│   ├── buttons.c                 # Lógica de botões
│   ├── buzzer.c                  # Geração de som
│   ├── display.c                 # Saída OLED
│   ├── joystick.c                # Leitura analógica
│   └── neopixel.c                # Controle de LEDs
└── tools/                        # Utilitários
    ├── drawls.c                  # Ferramentas de desenho
    └── piskel_convert.c          # Conversão de padrões
```

## Como Executar

### Pré-requisitos

- Raspberry Pi Pico W
- Visual Studio Code com extensão Raspberry Pi Pico
- Raspberry Pi Pico SDK
- CMake 3.13 ou superior
- Compilador ARM GCC
- Hardware conectado conforme o mapeamento de pinos

### Build

1. **Clone o repositório**
   ```bash
   git clone <repositório>
   cd Countroller
   ```

2. **Configure o CMake**
   ```bash
   mkdir build
   cd build
   cmake ..
   ```

3. **Compile o projeto**
   ```bash
   make
   ```

4. **Transfira para o Pico**
   
   Coloque o Pico em modo bootloader (segure BOOTSEL ao conectar) e copie o arquivo `.uf2` gerado para o dispositivo.

### Operação

Após o boot:

1. O sistema exibe três padrões animados (IF, PI, Coração) na matriz LED
2. Use o joystick para mover o LED pela matriz
3. Pressione os botões A ou B para incrementar contadores e ouvir feedback sonoro
4. O display OLED mostra posição do joystick e contadores dos botões
5. Os LEDs RGB indicam status do sistema

## Demonstração

Confira o sistema em ação neste vídeo de demonstração:

[![Demo Video](https://img.youtube.com/vi/H-Bg4PmH9-0/0.jpg)](https://youtu.be/H-Bg4PmH9-0?si=BmzUuu4fzx-mDRKq)

[Assista na íntegra](https://youtu.be/H-Bg4PmH9-0?si=BmzUuu4fzx-mDRKq)

## Utilitários

O projeto inclui ferramentas especializadas para geração de padrões visuais:

### drawls.c
Arquivo que contém as definições dos padrões visuais (IF, PI, Coração) exibidos na matriz de LEDs durante a inicialização. Foi gerado utilizando [Piskel App](https://www.piskelapp.com/), uma ferramenta online para criação de pixel art e animações.

### piskel_convert.c
Responsável por converter e processar os padrões gerados no Piskel para o formato apropriado de controle da matriz de LEDs. Realiza a transformação dos dados de desenho em comandos compatíveis com o sistema NeoPixel.

## Decisões Arquiteturais

O projeto foi estruturado seguindo as melhores práticas para C embarcado:

- **Modularização**: Cada componente (botões, display, joystick, etc.) é implementado independentemente em seus próprios arquivos `.c` e `.h`
- **Separação de responsabilidades**: Lógica de hardware separada da lógica de aplicação
- **Reutilizabilidade**: Bibliotecas como SSD1306 podem ser facilmente portadas para outros projetos
- **Eficiência**: Uso direto do SDK do Pico sem abstrações desnecessárias

Essa organização foi baseada em [Modular Code and How to Structure an Embedded C Project](https://www.microforum.cc/blogs/entry/46-modular-code-and-how-to-structure-an-embedded-c-project/).

## Aprendizados

O projeto consolida conhecimentos em:

- **Sistemas embarcados**: inicialização de hardware, configuração de periféricos
- **Programação C modular**: organização de código para manutenibilidade e reutilização
- **Protocolos de comunicação**: I2C, ADC, GPIO, PWM
- **Interfaces de hardware**: leitura de sensores analógicos, controle de atuadores
- **Arquitetura de microcontroladores**: recursos de GPIO, PIO, timers

## Referências e Agradecimentos

Este projeto foi inspirado em exemplos do repositório [BitDogLab-C](https://github.com/BitDogLab/BitDogLab-C):

- display_oled
- Joystick_led
- neopixel_pio
- button-buzzer
- button_led_rgb

Agradecimentos especiais ao Professor Jivago pelos recursos educacionais disponibilizados em seu [canal no YouTube](https://www.youtube.com/@profjivago9719), que foram essenciais para compreender os conceitos e implementações dos diversos componentes.

Também expresso minha gratidão a todos os professores envolvidos no programa EmbarcaTech, cujo dedicado acompanhamento, classes síncronas e orientações foram fundamentais para minha formação.

