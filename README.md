# Desenvolvendo aplicações para audio com o ESP32

## ESP-ADF x ESP-IDF 
### _ESP-ADF (Espressif Audio Development Framework)_
O ESP-ADF (Espressif Audio Development Framework) é um framework de desenvolvimento de software de código aberto desenvolvido pela Espressif. Foi projetado especificamente para criar aplicações de áudio em dispositivos baseados no ESP32, uma plataforma de microcontrolador de baixo custo e baixo consumo de energia.

O ESP-ADF fornece uma variedade de componentes e bibliotecas de software para facilitar o desenvolvimento de aplicativos de áudio. Ele oferece suporte para recursos como reprodução de áudio, gravação de áudio, processamento de áudio em tempo real, reconhecimento de voz e muito mais.

Os recursos suportados pelo framework ESP-ADF documentados mais precisamente no link:
https://github.com/espressif/esp-adf/tree/master

### _ESP-IDF (Espressif Iot Development Framework)_

A Espressif fornece os recursos básicos de hardware e software que ajudam os desenvolvedores de aplicativos a desenvolver suas ideias utilizando o hardware da série ESP32. O ESP-IDF destina-se ao desenvolvimento rápido de aplicativos de Internet das Coisas (IoT), com Wi-Fi, Bluetooth, gerenciamento de energia e vários outros recursos do sistema embarcado.

Para utilizar o framework ESP-ADF é preciso configurar primeiramente o ESP-IDF (Espressif IoT Development Framework)


## Hardware
<div align="center"><img src="docs/_static/esp32-lyrat-v4.3-layout-with-wrover-e-module.jpg" alt ="ESP-32-Lyrat-v4.3" align="center"/></div>

## Hands-On
A aplicação demonstrada nesse exemplo recebe e transmite áudio via Bluetooth para um dispositivo (ex: Smartphone).

É capaz de atender/desligar ligações e controlar mídias.

Foi utilizado o A2DP bluetooth profile junto com HFP profile para desenvolvimento dessa aplicação

## Configurando o ambiente de desenvolvimento
Este exemplo será mais focado na configuração do framework ESP-ADF que depende do ESP-IDF já amplamente difundido :

- PC com Linux Ubuntu 20.04.2 LTS
- ESP-IDF release/v4.4
- ESP-ADF master
- Dev board Lyrat V4.3

### Passo 1 - configurando ESP-IDF
Para instalar e configurar corretamente o framenwork ESP-IDF basta seguir a documentação oficial:

https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/index.html

Se atentar a versão que é compatível com o ESP-ADF:

https://espressif-docs.readthedocs-hosted.com/projects/esp-adf/en/latest/get-started/index.html

### Configurando as ferramentas
#### Instalando  
```
luiz@luiz-Inspiron-5447:~/esp/esp-idf$ ./install.sh
Detecting the Python interpreter
Checking "python" ...
/home/luiz/esp/esp-idf/tools/detect_python.sh: line 16: python: command not found
Checking "python3" ...
Python 3.10.6
"python3" has been detected
Installing ESP-IDF tools
Current system platform: linux-amd64
Selected targets are: esp32, esp32c3, esp32h2, esp32s2, esp32s3
Installing tools: xtensa-esp-elf-gdb, riscv32-esp-elf-gdb, xtensa-esp32-elf, xtensa-esp32s2-elf, xtensa-esp32s3-elf, riscv32-esp-elf, esp32ulp-elf, openocd-esp32
Skipping xtensa-esp-elf-gdb@11.2_20220823 (already installed)
Skipping riscv32-esp-elf-gdb@11.2_20220823 (already installed)
Skipping xtensa-esp32-elf@esp-2021r2-patch5-8.4.0 (already installed)
Skipping xtensa-esp32s2-elf@esp-2021r2-patch5-8.4.0 (already installed)
Skipping xtensa-esp32s3-elf@esp-2021r2-patch5-8.4.0 (already installed)
Skipping riscv32-esp-elf@esp-2021r2-patch5-8.4.0 (already installed)
Skipping esp32ulp-elf@2.35_20220830 (already installed)
Skipping openocd-esp32@v0.12.0-esp32-20230419 (already installed)
Installing Python environment and packages
Python 3.10.6
pip 23.1.2 from /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages/pip (python 3.10)
Installing Python packages from /home/luiz/esp/esp-idf/requirements.txt
Looking in indexes: https://pypi.org/simple, https://dl.espressif.com/pypi
Ignoring pygdbmi: markers 'python_version > "3.10"' don't match your environment
Ignoring None: markers 'sys_platform == "win32"' don't match your environment
Requirement already satisfied: setuptools>=21 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from -r /home/luiz/esp/esp-idf/requirements.txt (line 4)) (67.8.0)
Requirement already satisfied: click>=7.0 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from -r /home/luiz/esp/esp-idf/requirements.txt (line 8)) (8.1.4)
Requirement already satisfied: pyserial>=3.3 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from -r /home/luiz/esp/esp-idf/requirements.txt (line 9)) (3.5)
Requirement already satisfied: future>=0.15.2 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from -r /home/luiz/esp/esp-idf/requirements.txt (line 10)) (0.18.3)
Requirement already satisfied: cryptography>=2.1.4 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from -r /home/luiz/esp/esp-idf/requirements.txt (line 12)) (41.0.1)
Requirement already satisfied: pyparsing<2.4.0,>=2.0.3 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from -r /home/luiz/esp/esp-idf/requirements.txt (line 18)) (2.3.1)
Requirement already satisfied: pyelftools>=0.22 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from -r /home/luiz/esp/esp-idf/requirements.txt (line 19)) (0.29)
Requirement already satisfied: idf-component-manager~=1.2 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from -r /home/luiz/esp/esp-idf/requirements.txt (line 20)) (1.3.2)
Requirement already satisfied: urllib3<2 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from -r /home/luiz/esp/esp-idf/requirements.txt (line 21)) (1.26.16)
Requirement already satisfied: gdbgui==0.13.2.0 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from -r /home/luiz/esp/esp-idf/requirements.txt (line 23)) (0.13.2.0)
Requirement already satisfied: pygdbmi<=0.9.0.2 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from -r /home/luiz/esp/esp-idf/requirements.txt (line 26)) (0.9.0.2)
Requirement already satisfied: python-socketio<5 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from -r /home/luiz/esp/esp-idf/requirements.txt (line 29)) (4.6.1)
Requirement already satisfied: jinja2<3.1 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from -r /home/luiz/esp/esp-idf/requirements.txt (line 30)) (3.0.3)
Requirement already satisfied: itsdangerous<2.1 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from -r /home/luiz/esp/esp-idf/requirements.txt (line 31)) (2.0.1)
Requirement already satisfied: kconfiglib==13.7.1 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from -r /home/luiz/esp/esp-idf/requirements.txt (line 36)) (13.7.1)
Requirement already satisfied: reedsolo<=1.5.4,>=1.5.3 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from -r /home/luiz/esp/esp-idf/requirements.txt (line 39)) (1.5.4)
Requirement already satisfied: bitstring<4,>=3.1.6 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from -r /home/luiz/esp/esp-idf/requirements.txt (line 41)) (3.1.9)
Requirement already satisfied: ecdsa>=0.16.0 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from -r /home/luiz/esp/esp-idf/requirements.txt (line 42)) (0.18.0)
Requirement already satisfied: construct==2.10.54 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from -r /home/luiz/esp/esp-idf/requirements.txt (line 46)) (2.10.54)
Requirement already satisfied: Flask<1.0,>=0.12.2 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from gdbgui==0.13.2.0->-r /home/luiz/esp/esp-idf/requirements.txt (line 23)) (0.12.5)
Requirement already satisfied: Flask-Compress<2.0,>=1.4.0 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from gdbgui==0.13.2.0->-r /home/luiz/esp/esp-idf/requirements.txt (line 23)) (1.13)
Requirement already satisfied: Flask-SocketIO<3.0,>=2.9 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from gdbgui==0.13.2.0->-r /home/luiz/esp/esp-idf/requirements.txt (line 23)) (2.9.6)
Requirement already satisfied: gevent<2.0,>=1.2.2 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from gdbgui==0.13.2.0->-r /home/luiz/esp/esp-idf/requirements.txt (line 23)) (1.5.0)
Requirement already satisfied: Pygments<3.0,>=2.2.0 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from gdbgui==0.13.2.0->-r /home/luiz/esp/esp-idf/requirements.txt (line 23)) (2.15.1)
Requirement already satisfied: cffi>=1.12 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from cryptography>=2.1.4->-r /home/luiz/esp/esp-idf/requirements.txt (line 12)) (1.15.1)
Requirement already satisfied: cachecontrol[filecache] in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from idf-component-manager~=1.2->-r /home/luiz/esp/esp-idf/requirements.txt (line 20)) (0.13.1)
Requirement already satisfied: colorama in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from idf-component-manager~=1.2->-r /home/luiz/esp/esp-idf/requirements.txt (line 20)) (0.4.6)
Requirement already satisfied: packaging in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from idf-component-manager~=1.2->-r /home/luiz/esp/esp-idf/requirements.txt (line 20)) (23.1)
Requirement already satisfied: pyyaml in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from idf-component-manager~=1.2->-r /home/luiz/esp/esp-idf/requirements.txt (line 20)) (6.0)
Requirement already satisfied: requests in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from idf-component-manager~=1.2->-r /home/luiz/esp/esp-idf/requirements.txt (line 20)) (2.31.0)
Requirement already satisfied: requests-file in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from idf-component-manager~=1.2->-r /home/luiz/esp/esp-idf/requirements.txt (line 20)) (1.5.1)
Requirement already satisfied: requests-toolbelt in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from idf-component-manager~=1.2->-r /home/luiz/esp/esp-idf/requirements.txt (line 20)) (1.0.0)
Requirement already satisfied: schema in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from idf-component-manager~=1.2->-r /home/luiz/esp/esp-idf/requirements.txt (line 20)) (0.7.5)
Requirement already satisfied: six in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from idf-component-manager~=1.2->-r /home/luiz/esp/esp-idf/requirements.txt (line 20)) (1.16.0)
Requirement already satisfied: tqdm in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from idf-component-manager~=1.2->-r /home/luiz/esp/esp-idf/requirements.txt (line 20)) (4.65.0)
Requirement already satisfied: python-engineio<4,>=3.13.0 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from python-socketio<5->-r /home/luiz/esp/esp-idf/requirements.txt (line 29)) (3.14.2)
Requirement already satisfied: MarkupSafe>=2.0 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from jinja2<3.1->-r /home/luiz/esp/esp-idf/requirements.txt (line 30)) (2.1.3)
Requirement already satisfied: pycparser in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from cffi>=1.12->cryptography>=2.1.4->-r /home/luiz/esp/esp-idf/requirements.txt (line 12)) (2.21)
Requirement already satisfied: Werkzeug<1.0,>=0.7 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from Flask<1.0,>=0.12.2->gdbgui==0.13.2.0->-r /home/luiz/esp/esp-idf/requirements.txt (line 23)) (0.16.1)
Requirement already satisfied: brotli in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from Flask-Compress<2.0,>=1.4.0->gdbgui==0.13.2.0->-r /home/luiz/esp/esp-idf/requirements.txt (line 23)) (1.0.9)
Requirement already satisfied: greenlet>=0.4.14 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from gevent<2.0,>=1.2.2->gdbgui==0.13.2.0->-r /home/luiz/esp/esp-idf/requirements.txt (line 23)) (2.0.2)
Requirement already satisfied: msgpack>=0.5.2 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from cachecontrol[filecache]->idf-component-manager~=1.2->-r /home/luiz/esp/esp-idf/requirements.txt (line 20)) (1.0.5)
Requirement already satisfied: filelock>=3.8.0 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from cachecontrol[filecache]->idf-component-manager~=1.2->-r /home/luiz/esp/esp-idf/requirements.txt (line 20)) (3.12.2)
Requirement already satisfied: charset-normalizer<4,>=2 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from requests->idf-component-manager~=1.2->-r /home/luiz/esp/esp-idf/requirements.txt (line 20)) (3.2.0)
Requirement already satisfied: idna<4,>=2.5 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from requests->idf-component-manager~=1.2->-r /home/luiz/esp/esp-idf/requirements.txt (line 20)) (3.4)
Requirement already satisfied: certifi>=2017.4.17 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from requests->idf-component-manager~=1.2->-r /home/luiz/esp/esp-idf/requirements.txt (line 20)) (2023.5.7)
Requirement already satisfied: contextlib2>=0.5.5 in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/lib/python3.10/site-packages (from schema->idf-component-manager~=1.2->-r /home/luiz/esp/esp-idf/requirements.txt (line 20)) (21.6.0)
All done! You can now run:

  . ./export.sh

```
#### Configurando variáveis de ambiente
```
luiz@luiz-Inspiron-5447:~/esp/esp-idf$ . ./export.sh
Setting IDF_PATH to '/home/luiz/esp/esp-idf'
Detecting the Python interpreter
Checking "python" ...
Command 'python' not found, did you mean:
  command 'python3' from deb python3
  command 'python' from deb python-is-python3
Checking "python3" ...
Python 3.10.6
"python3" has been detected
Adding ESP-IDF tools to PATH...
Using Python interpreter in /home/luiz/.espressif/python_env/idf4.4_py3.10_env/bin/python
Checking if Python packages are up to date...
Python requirements from /home/luiz/esp/esp-idf/requirements.txt are satisfied.
Added the following directories to PATH:
  /home/luiz/esp/esp-idf/components/esptool_py/esptool
  /home/luiz/esp/esp-idf/components/espcoredump
  /home/luiz/esp/esp-idf/components/partition_table
  /home/luiz/esp/esp-idf/components/app_update
  /home/luiz/.espressif/tools/xtensa-esp-elf-gdb/11.2_20220823/xtensa-esp-elf-gdb/bin
  /home/luiz/.espressif/tools/riscv32-esp-elf-gdb/11.2_20220823/riscv32-esp-elf-gdb/bin
  /home/luiz/.espressif/tools/xtensa-esp32-elf/esp-2021r2-patch5-8.4.0/xtensa-esp32-elf/bin
  /home/luiz/.espressif/tools/xtensa-esp32s2-elf/esp-2021r2-patch5-8.4.0/xtensa-esp32s2-elf/bin
  /home/luiz/.espressif/tools/xtensa-esp32s3-elf/esp-2021r2-patch5-8.4.0/xtensa-esp32s3-elf/bin
  /home/luiz/.espressif/tools/riscv32-esp-elf/esp-2021r2-patch5-8.4.0/riscv32-esp-elf/bin
  /home/luiz/.espressif/tools/esp32ulp-elf/2.35_20220830/esp32ulp-elf/bin
  /home/luiz/.espressif/tools/openocd-esp32/v0.12.0-esp32-20230419/openocd-esp32/bin
  /home/luiz/.espressif/python_env/idf4.4_py3.10_env/bin
  /home/luiz/esp/esp-idf/tools
Done! You can now compile ESP-IDF projects.
Go to the project directory and run:

  idf.py build
```
#### Testando as configurações
Vamos apens abrir as configurações do projeto com a finalidade de validar as configurações feitas previamente, se o menuconfig abrir corretamente podemos seguir com as configurações do ESP-ADF.

```
luiz@luiz-Inspiron-5447:~/esp/esp-idf$ ls
add_path.sh     components        docs      export.bat   export.ps1  install.bat   install.ps1  Kconfig  make          README.md         sdkconfig.rename          SUPPORT_POLICY_CN.md  tools
CMakeLists.txt  CONTRIBUTING.rst  examples  export.fish  export.sh   install.fish  install.sh   LICENSE  README_CN.md  requirements.txt  sonar-project.properties  SUPPORT_POLICY.md
luiz@luiz-Inspiron-5447:~/esp/esp-idf$ cd examples/
luiz@luiz-Inspiron-5447:~/esp/esp-idf/examples$ cd get-started/
luiz@luiz-Inspiron-5447:~/esp/esp-idf/examples/get-started$ cd 
blink/          hello_world/    sample_project/ 
luiz@luiz-Inspiron-5447:~/esp/esp-idf/examples/get-started$ cd blink/
luiz@luiz-Inspiron-5447:~/esp/esp-idf/examples/get-started/blink$ idf.py menuconfig
Executing action: menuconfig
Running ninja in directory /home/luiz/esp/esp-idf/examples/get-started/blink/build
Executing "ninja menuconfig"...
[0/1] cd /home/luiz/esp/esp-idf/examples/get-started/blink/build && /home/luiz/.espressif/python_env/...esp32 --env IDF_ENV_FPGA= --output config /home/luiz/esp/esp-idf/examples/get-started/blink/sdkconfig
Loading defaults file /home/luiz/esp/esp-idf/examples/get-started/blink/sdkconfig.defaults...
TERM environment variable is set to "xterm-256color"
Loaded configuration '/home/luiz/esp/esp-idf/examples/get-started/blink/sdkconfig'
No changes to save (for '/home/luiz/esp/esp-idf/examples/get-started/blink/sdkconfig')
Loading defaults file /home/luiz/esp/esp-idf/examples/get-started/blink/sdkconfig.defaults...

```
#### Menuconfig

<div align="center"><img src="docs/_static/Screenshot from 2023-07-15 01-38-51.png" alt ="Menuconfig" align="center"/></div>

### Passo 2 - configurar o ESP-ADF
Após a configuração do ESP-IDF vamos clonar e configurar o ESP-ADF...

#### Clonando o repositório
```
cd ~/esp
git clone --recursive https://github.com/espressif/esp-adf.git

```

#### Configurando o ADF_PATH
```
export ADF_PATH=~/esp/esp-adf
```

#### Testando nos exemplos do ESP-IDF
```
luiz@luiz-Inspiron-5447:~/esp/esp-adf$ ls
CMakeLists.txt  components  docs  esp-idf  examples  idf_patches  LICENSE  micropython_adf  project.mk  README.md  tools
luiz@luiz-Inspiron-5447:~/esp/esp-adf$ cd e
esp-idf/  examples/ 
luiz@luiz-Inspiron-5447:~/esp/esp-adf$ cd examples/
luiz@luiz-Inspiron-5447:~/esp/esp-adf/examples$ cd get-started/
luiz@luiz-Inspiron-5447:~/esp/esp-adf/examples/get-started$ ls
pipeline_a2dp_sink_and_hfp  pipeline_tcp_client  play_mp3_control
luiz@luiz-Inspiron-5447:~/esp/esp-adf/examples/get-started$ cd p
pipeline_a2dp_sink_and_hfp/ pipeline_tcp_client/        play_mp3_control/           
luiz@luiz-Inspiron-5447:~/esp/esp-adf/examples/get-started$ cd pipeline_a2dp_sink_and_hfp/
luiz@luiz-Inspiron-5447:~/esp/esp-adf/examples/get-started/pipeline_a2dp_sink_and_hfp$ idf.py menuconfig
Executing action: menuconfig
Running ninja in directory /home/luiz/esp/esp-adf/examples/get-started/pipeline_a2dp_sink_and_hfp/build
Executing "ninja menuconfig"...
[0/1] cd /home/luiz/esp/esp-adf/examples/get-started/pipeline_a2dp_sink_and_hfp/build && /home/luiz/....PGA= --output config /home/luiz/esp/esp-adf/examples/get-started/pipeline_a2dp_sink_and_hfp/sdkconfig
Loading defaults file /home/luiz/esp/esp-adf/examples/get-started/pipeline_a2dp_sink_and_hfp/sdkconfig.defaults...
/home/luiz/esp/esp-adf/examples/get-started/pipeline_a2dp_sink_and_hfp/sdkconfig.defaults:4 CONFIG_BLUEDROID_ENABLED was replaced with CONFIG_BT_BLUEDROID_ENABLED
/home/luiz/esp/esp-adf/examples/get-started/pipeline_a2dp_sink_and_hfp/sdkconfig.defaults:5 CONFIG_CLASSIC_BT_ENABLED was replaced with CONFIG_BT_CLASSIC_ENABLED
/home/luiz/esp/esp-adf/examples/get-started/pipeline_a2dp_sink_and_hfp/sdkconfig.defaults:6 CONFIG_A2DP_ENABLE was replaced with CONFIG_BT_A2DP_ENABLE
/home/luiz/esp/esp-adf/examples/get-started/pipeline_a2dp_sink_and_hfp/sdkconfig.defaults:10 CONFIG_BTDM_CONTROLLER_MODE_BTDM was replaced with CONFIG_BTDM_CTRL_MODE_BTDM
/home/luiz/esp/esp-adf/examples/get-started/pipeline_a2dp_sink_and_hfp/sdkconfig.defaults:11 CONFIG_BTDM_CONTROLLER_BR_EDR_MAX_SYNC_CONN was replaced with CONFIG_BTDM_CTRL_BR_EDR_MAX_SYNC_CONN
/home/luiz/esp/esp-adf/examples/get-started/pipeline_a2dp_sink_and_hfp/sdkconfig.defaults:12 CONFIG_BTDM_CONTROLLER_BR_EDR_MAX_SYNC_CONN_EFF was replaced with CONFIG_BTDM_CTRL_BR_EDR_MAX_SYNC_CONN_EFF
/home/luiz/esp/esp-adf/examples/get-started/pipeline_a2dp_sink_and_hfp/sdkconfig.defaults:13 CONFIG_HFP_ENABLE was replaced with CONFIG_BT_HFP_ENABLE
/home/luiz/esp/esp-adf/examples/get-started/pipeline_a2dp_sink_and_hfp/sdkconfig.defaults:14 CONFIG_HFP_AUDIO_DATA_PATH_HCI was replaced with CONFIG_BT_HFP_AUDIO_DATA_PATH_HCI
Loading defaults file /home/luiz/esp/esp-adf/examples/get-started/pipeline_a2dp_sink_and_hfp/sdkconfig.defaults.esp32...
TERM environment variable is set to "xterm-256color"
Loaded configuration '/home/luiz/esp/esp-adf/examples/get-started/pipeline_a2dp_sink_and_hfp/sdkconfig'
No changes to save (for '/home/luiz/esp/esp-adf/examples/get-started/pipeline_a2dp_sink_and_hfp/sdkconfig')
Loading defaults file /home/luiz/esp/esp-adf/examples/get-started/pipeline_a2dp_sink_and_hfp/sdkconfig.defaults...
/home/luiz/esp/esp-adf/examples/get-started/pipeline_a2dp_sink_and_hfp/sdkconfig.defaults:4 CONFIG_BLUEDROID_ENABLED was replaced with CONFIG_BT_BLUEDROID_ENABLED
/home/luiz/esp/esp-adf/examples/get-started/pipeline_a2dp_sink_and_hfp/sdkconfig.defaults:5 CONFIG_CLASSIC_BT_ENABLED was replaced with CONFIG_BT_CLASSIC_ENABLED
/home/luiz/esp/esp-adf/examples/get-started/pipeline_a2dp_sink_and_hfp/sdkconfig.defaults:6 CONFIG_A2DP_ENABLE was replaced with CONFIG_BT_A2DP_ENABLE
/home/luiz/esp/esp-adf/examples/get-started/pipeline_a2dp_sink_and_hfp/sdkconfig.defaults:10 CONFIG_BTDM_CONTROLLER_MODE_BTDM was replaced with CONFIG_BTDM_CTRL_MODE_BTDM
/home/luiz/esp/esp-adf/examples/get-started/pipeline_a2dp_sink_and_hfp/sdkconfig.defaults:11 CONFIG_BTDM_CONTROLLER_BR_EDR_MAX_SYNC_CONN was replaced with CONFIG_BTDM_CTRL_BR_EDR_MAX_SYNC_CONN
/home/luiz/esp/esp-adf/examples/get-started/pipeline_a2dp_sink_and_hfp/sdkconfig.defaults:12 CONFIG_BTDM_CONTROLLER_BR_EDR_MAX_SYNC_CONN_EFF was replaced with CONFIG_BTDM_CTRL_BR_EDR_MAX_SYNC_CONN_EFF
/home/luiz/esp/esp-adf/examples/get-started/pipeline_a2dp_sink_and_hfp/sdkconfig.defaults:13 CONFIG_HFP_ENABLE was replaced with CONFIG_BT_HFP_ENABLE
/home/luiz/esp/esp-adf/examples/get-started/pipeline_a2dp_sink_and_hfp/sdkconfig.defaults:14 CONFIG_HFP_AUDIO_DATA_PATH_HCI was replaced with CONFIG_BT_HFP_AUDIO_DATA_PATH_HCI
Loading defaults file /home/luiz/esp/esp-adf/examples/get-started/pipeline_a2dp_sink_and_hfp/sdkconfig.defaults.esp32...
```
#### Menuconfig



