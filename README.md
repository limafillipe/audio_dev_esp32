# Desenvolvendo aplicações para audio com o ESP32

## ESP-ADF (Espressif Audio Development Framework)

O ESP-ADF (Espressif Audio Development Framework) é um framework de desenvolvimento de software de código aberto desenvolvido pela Espressif. EFoi projetado especificamente para criar aplicações de áudio em dispositivos baseados no ESP32, uma plataforma de microcontrolador de baixo custo e baixo consumo de energia.

O ESP-ADF fornece uma variedade de componentes e bibliotecas de software para facilitar o desenvolvimento de aplicativos de áudio. Ele oferece suporte para recursos como reprodução de áudio, gravação de áudio, processamento de áudio em tempo real, reconhecimento de voz e muito mais.

Os recursos suportados pelo framework ESP-ADF documentados mais precisamente no link:
https://github.com/espressif/esp-adf/tree/master


## Hardware
<div align="center"><img src="docs/_static/esp32-lyrat-v4.3-layout-with-wrover-e-module.jpg" alt ="ESP-32-Lyrat-v4.3" align="center"/></div>

## Hands-On
A aplicação demonstrada nesse exemplo recebe e transmite áudio via Bluetooth para um dispositivo (ex: Smartphone).

É capaz de atender/desligar ligações e controlar mídias.

Foi utilizado o A2DP bluetooth profile junto com HFP profile para desenvolvimento dessa aplicação

## Configurando o ambiente de desenvolvimento
Para este exemplo foi utilizado as seguintes ferramentas e frameworks:

- PC com Linux Ubuntu 20.04.2 LTS
- ESP-IDF release/v4.4
- ESP-ADF master
- Dev board Lyrat V4.3




