# WORKSHOP "PYTHON E HACKING ÉTICO" - DIA 2

## Python em Programação para Redes de Computadores

### ATENÇÃO:
Para acompanhar plenamente o workshop, deve se ter pelo menos o [Python instalado](https://www.python.org/downloads/) em sua máquina, e usar qualquer editor de código, podendo ser até mesmo um Bloco de Notas ou similar.

### Python e seus módulos

Como já discutido, Python possui um número enorme de bibliotecas/módulos, inclusive para trabalhar com otimização de processos e ferramentas para Redes de Computadores. O objetivo dessa parte do workshop é mostrar a **facilidade** e **produtividade** de usar Python com o propósito de programação voltada á Redes.
Nesse workshop, iremos abordar 2 em especial:
+ **os** : módulo que permite acessar serviços de Sistemas Operacionais;
+ **socket** : módulo que permite o uso de serviços de conexão de Redes;

### O módulo os

Esse módulo provê um conjunto de funções que permite a utilização e interface à serviços assosciados à Sistemas Operacionais, podendo automatizar alguns processos e criar novas ferramentas a partir de outras. Para cinhecer todas as funções do módulo, ver sua [documentação](https://docs.python.org/3/library/os.html) oficial.
Nesse exemplo, vamos utilizar o ping de forma mais automatizada:

Podemos até utilizar o ping para que, antes de alguma conexão, verificar se a métrica de acessos desejadas é satisifatória, mostrando se um host com alcance consistente.

```{r}
import os

print('PING EM PYTHON')

ip_host = input('Digite o IP ou host: ')
qtd_ping = input('Quantos envios? ')
os.system('ping -n {0} {1}'.format(qtd_ping,ip_host))
```
Podemos melhorar esse exemplo, agora não apenas mostrando o ping, mas usando seu resultado para verificar se está acessível ou não.

```{r}
import os

print('PING EM PYTHON')

ip_host = input('Digite o IP ou host: ')
qtd_ping = input('Quantos envios? ')  
response = os.popen('ping -n {0} {1}'.format(qtd_ping,ip_host)).read()
if 'Recebidos = {}'.format(qtd_ping) in response:
    print('{} está acessível!'.format(ip_host))
else:
    print('{} não está acessível'.format(ip_host))
```

### O módulo socket

Antes de ver o módulo, é interessante saber um pouco mais sobre sockets. Sockets são abstrações de conexão (via TCP/IP), que permitem que a comunicação entre duas máquinas distintas.

![](https://miro.medium.com/max/567/1*dTcOPDdDQyQExxPo1akCrA.png)

Quando utilizamos sockets, estamos trabalhando com as 2 primeiras camadas do modelo TCP/IP, fazendo acesso à protocolos e portas de serviços.

![](https://miro.medium.com/max/538/1*tCd-YCnFRUTX5H7n8I-6yA.png)

Nesse exemplo, vamos criar um socket simples, para tentar acessar o servidor do Google, através de duas portas de serviços: a 80, comum para acesso à servidor Web (por exemplo, páginas em site) e a 21 (para servidor FTP, para troca de arquivos e muitas com a necessidade de autenticação).
```{r}
import socket
import sys

try:
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    # AF_INET -> família de endereços o tipo de endereço (IPv4 ou IPv6)
    # SOCK_STREAM -> siginifica socket do tipo TCP (SOCK_DGRAM seria p/ UDP)
    print('Socket criado com sucesso.\n')
except socket.error as e:
    print('Conexão falhou!')
    print('Erro: {}'.format(e))
    sys.exit()

host_alvo=input('Digite o host ou IP a ser conectado: ')
port_alvo=input('Digite a porta a ser conectada: ')

try:
    s.connect( (host_alvo,int(port_alvo)) )
    print('Client TCP conectado com sucesso no host '+host_alvo+' e na porta '+port_alvo)
    s.shutdown(2)
except socket.error as e:
    print('Conexão falhou: '+host_alvo+', '+port_alvo)
    print('Erro: {}'.format(e))
    sys.exit()
```
