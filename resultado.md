## Ataque Man in the middle (MitM)

Passo-a-passo da simulação:

### 1º - Abrir o Metasploitable, logar e capturar o ip da máquina

Comando: ip addr ou ifconfig

&nbsp;

### 2º - Abrir o navegador do Kali Linux e acessar o site do DVWA pesquisando pelo ip do metasploitable no navegador.

Comando: 192.168.56.101/dvwa

&nbsp;

### 3º - Abrir o wireshark nas aplicações do Kali Linux

![wireshark](https://i.imgur.com/3RwdsQF.jpeg)

&nbsp;

### 4º - No Wireshark, selecionar com duplo clique a minha interface de rede, que é a eth0, que nós vamos utilizar para capturar o tráfego da minha máquina com o DVWA. Ao clicarmos aparecerá algumas informações do tráfego de rede.

![interface](https://i.imgur.com/Jgpz5xu.jpeg)

&nbsp;

### 5º - Ao atualizarmos a página do DVWA, já começará a aparecer algumas informações de tráfego entre minha máquina do Kali e o DVWA, que é o Metasploitable.

![trafego](https://i.imgur.com/UpKbJ9y.jpeg)

### 6º - Selecionando um pacote do tráfego de rede e investigando as informações dele

![select](https://i.imgur.com/5MYM1xj.jpeg)

&nbsp;

![select1](https://i.imgur.com/cUVGAEN.jpeg)

&nbsp;

### 7º - Capturando dados do formulário DVWA com login errado (filtrando por requisição)

Na barra de pesquisa do wireshark procurar pelo comando abaixo:

Comando: http request method == "POST"

Ao abrirmos o pacote encontrado em uma nova janela, conseguimos visualizar mais informações, incluindo o login e senha errados que digitei ao tentar acessar o DVWA

![post](https://i.imgur.com/egpfP0y.jpeg)

&nbsp;

![senha_errada](https://i.imgur.com/J0HzH7A.jpeg)

Eu só consegui visualizar estes dados de tentativa de login porque essa conexão é http, ou seja, não é criptografada. Se fosse https provavelmente não conseguiria. 

&nbsp;

### 8º Logando com os dados corretos no DVWA e visualizando no wireshark

Dessa vez loguei com as credenciais corretas e também consigo visualizar o login e senha

![senha_correta](https://i.imgur.com/ZswSDKC.jpeg)

&nbsp;

### 9º - Filtrando por ip de máquina no wireshark

Mostra todos os pacotes de tráfego de rede da máquina selecionada, seja de entrada ou saída.

Comando: ip.addr == nú.me.ro.ip 

![filtro](https://i.imgur.com/Yj71Vcw.jpeg) 

&nbsp;

### 10º" - Manipulando a rede com Ettercap 

O Ettercap é um conjunto abrangente de ferramentas para análise de protocolos de rede e testes de segurança, com foco principal em ataques "Man-in-the-Middle" (Homem do Meio).

O Ettercap pode farejar (sniffar) conexões de rede ativas, permitindo a captura de dados que trafegam entre hosts na mesma rede local (LAN). 

Sua principal capacidade é realizar ataques MITM, onde o atacante se posiciona entre duas ou mais máquinas que estão a se comunicar. Um método comum para isso é o envenenamento de ARP (ARP Poisoning), que engana as máquinas fazendo-as pensar que o endereço MAC do atacante corresponde ao endereço IP de outro dispositivo (como o gateway da rede).

Neste exemplo vamos utilizar 3 máquinas virtuais: o Kali, o Windows 7 e o Metasploitable, e vamos acessar o DVWA no Windows 7. 

&nbsp;

### 11º - Abrindo o Ettercap no Terminal do Kali (precisa estar como root)

Comando: ettercap -G

![ettercap](https://i.imgur.com/JrloM10.jpeg) 

Com o Ettercap vamos utilizar o ataque de envenenamento de ARP. O envenenamento ARP é um ataque cibernético que envia mensagens ARP falsas para uma rede local (LAN) para associar o endereço MAC do invasor ao endereço IP de outro dispositivo legítimo, como o gateway de rede. Isso permite que o invasor intercepte ou modifique o tráfego da rede, atuando como um "intermediário" (ataque Man-in-the-Middle), ou cause uma negação de serviço ao descartar pacotes. 

Em seu funcionamento normal, o Protocolo de Resolução de Endereços (ARP) mapeia endereços IP para endereços MAC em uma rede local. Um dispositivo envia uma requisição para saber o MAC de um IP, e o dispositivo correto responde com seu MAC.

Ao aplicar o envenenamento ARP, o invasor envia mensagens ARP falsas, conhecidas como "respostas ARP gratuitas", que associam seu próprio endereço MAC ao endereço IP de outro dispositivo, como o gateway padrão.

Como resultado, os outros dispositivos na rede atualizam suas tabelas ARP com essa informação incorreta. Com isso, todo o tráfego destinado ao IP original agora é enviado para o computador do invasor.

&nbsp;

### 12º - Configurando o Ettercap

Desabilitar o sniffing ao iniciar e o bridged, manter a interface eth0 e clicar em aceitar. 

![config_ettercap](https://i.imgur.com/SD5o4hG.jpeg) 

&nbsp;

### 13º - Escaneando os hosts para encontrar as vítimas

Clicar no ícone de hostlist para visualizar os hosts, vai estar vazio.

![hosts](https://i.imgur.com/3Gr9gq1.jpeg) 

Em seguida, clicar na lupa para scanear e fazer a varredura da rede para encontrar os hosts 

&nbsp;

### 14º - Adicionandos os hosts como alvos no Ettercap

Encontrados os hosts, vou adicionar o IP do Metasploitable (DVWA) como Target 1 e o IP do Windows 7 como Target 2.

![targets](https://i.imgur.com/4JWCmUF.jpeg)

Ao clicarmos nos 3 pontinhos do menu do Ettercap > Targets > Current Targets eu consigo visualizar os dois hosts que defini como targets. 

![target](https://i.imgur.com/6eHfS3g.jpeg)

Em seguida, é só clicar no botão play no canto superior esquerdo da janela do Ettercap para iniciar o sniffing.

&nbsp;

### 15º - Definindo o ataque que será utilizado

Abrir o Wireshark, dar um duplo clique na palavra eth0 e visualizar o que está sendo trafegado pela rede. 

Em seguida, precisamos fazer uma configuração no terminal do Kali para que consigamos redirecionar o tráfego e então fazermos a captura (precisamos estar como root no terminal).

Comando: echo 1 > /proc/sys/net/ipv4/ip_forward

![echo](https://i.imgur.com/Q53ilR8.jpeg)

&nbsp;

### 16º - Configurando o envenenamento do ARP 

Na janela do Ettercap, no canto superior direito, clicar no ícone de MITM Menu e depois clicar na opção ARP poisoning

![arp](https://i.imgur.com/pOPMiYw.jpeg)

Vai abrir uma janela pequena, é só manter selecionada a opção Sniff remote connections e clicar em ok para que tudo que trafegar entre o Windows 7 e o Metasploitable eu possa capturar no Kali. 

![arp1](https://i.imgur.com/p5I6XTy.jpeg)

Em seguida é só clicar no play no canto superior esquerda da janela do Ettercap para iniciarmos o Sniffing.

Ao pesquisarmos pela palavra arp na barra de pesquisa do wireshark visualizaremos o tráfego ARP entre as máquinas que defini como alvo.

![trafego_arp](https://i.imgur.com/81sXiW2.jpeg)

&nbsp;

### 17º - Visualizando no ettercap a captura das credenciais de login do DVWA

Ao inserirmos o login e senha e logarmos na máquina DVWA conseguiremos visualizar no ettercap o login e senha inseridos e a url do site. 

![dvwa](https://i.imgur.com/PyNGHct.jpeg)

&nbsp;
