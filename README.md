## Ataque Man-in-the-middle (MitM)

![hacking](https://www.sandsteinwandern.de/wandern/wp-content/uploads/2019/11/giphy-hack2.gif)

Simulação de ataque Man in the middle (MitM) com Wireshark, Ettercap, Metasploitable e Virtual Machines do Kali Linux e Windows e DVWA - Damn Vulnerable Web Application (Aplicação Web Propositalmente Vulnerável).

Um ataque "man-in-the-middle" (MitM) é um tipo de ciberataque no qual um invasor se insere secretamente entre duas partes que acreditam estar se comunicando diretamente, interceptando e, possivelmente, alterando a comunicação para roubar dados ou credenciais. O invasor atua como um intermediário, sem que nenhuma das partes saiba que sua conexão foi comprometida. 

O invasor se posiciona entre as duas partes (por exemplo, um usuário e um aplicativo bancário) e desvia a comunicação para si mesmo. O invasor pode fingir ser um dos lados da comunicação e pode interceptar, ler, alterar e retransmitir a mensagem para o destinatário original. 

![](https://web.cs.ucla.edu/classes/winter13/cs111/scribe/17b/mitm.png)

Um atacante pode interceptar uma transação bancária, alterar o valor e reencaminhá-la. O objetivo é roubar informações sensíveis, como credenciais de login, dados financeiros ou informações pessoais para uso fraudulento. 

É um tipo de hijacking (sequestro de sessão), onde o interceptador consegue obter os dados que estão sendo trafegados.

Wireshark, Ettercap, Cain e Abel e Bettercap são algumas das ferramentas utilizadas neste tipo de ataque.

O Wireshark é um sniffer (farejador) de rede, ou seja, é uma aplicação que lê pacotes de dados que atravessam a rede dentro da camada TCP/IP (Transmission Control Protocol/Internet Protocol). Ele ouve uma conexão de rede em tempo real e captura fluxos inteiros de tráfego. Também é capaz de fatiar e filtrar os dados, e permite a visualização do conteúdo dos pacotes capturados.

O que são pacotes de tráfego de rede?

Tráfego de rede: Refere-se ao volume de dados que circulam em uma rede, como o tráfego de automóveis nas estradas.

Pacotes de dados: Os veículos individuais que compõem esse tráfego.

Análise de tráfego: O processo de capturar e analisar esses pacotes para monitorar o desempenho da rede, detectar problemas, identificar ameaças de segurança ou solucionar incidentes. 

