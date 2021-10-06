# WORKSHOP "PYTHON E HACKING ÉTICO" - DIA 3

## Python na manipulação de dados


### Web Scraper - manipulando dados na Web

Uma das maiores vantagens do Python é a quantidade de módulos e facilidade na manipulação de dados, de diferentes formas. **Web Scraper** é a denominação de uma sistema que tem como responsabilidade coletar dados em ambientes web, principlamente em websites, em uma processo automatizado. Atualmente, há várias técnicas que se preocupam e impedem que acessem os dados através de sistemas automatizados.
No exemplo desse dia, vamos usar o módulo [request](https://docs.python-requests.org/en/latest/), um dos módulos mais famosos e utilizados para trabalhar com requisições HTTP atualmente.

![](https://docs.python-requests.org/en/latest/_static/requests-sidebar.png)

Esse pequeno programa em Python busca por emails dentro de uma página e gera um arquivo de texto com todos os emails, sendo um princípio básico de criação de um sistema de [**Spammer**](https://pt.wikipedia.org/wiki/Spam). Usaremos também um módulo de [Regex](https://medium.com/pyladiesbh/regex-b%C3%A1sico-em-python-31dcb7fac046) (Expressões Regulares), o módulo [re](https://docs.python.org/3/library/re.html), para ajudar a identificar endereços de emails válidos.

```{r}

import re
import requests
import requests.exceptions

#URL a ser vasculhada
url = 'https://www.al.sp.gov.br/deputado/contato/'

# todos os emails vão para um set, para não duplicar os dados
emails = set()

# pegando os dados da URL
print('Crawling URL {}'.format(url))
try:
    response = requests.get(url)
# uma exceção é para URL inválida e outra para verificar a conexão
except (requests.exceptions.MissingSchema, requests.exceptions.ConnectionError):
    print('Erro na requisição')

# extraindo todos os emails do conteúdo
emails = set(re.findall(r'[a-z0-9\.\-+_]+@[a-z0-9\.\-+_]+\.[a-z]+', response.text, re.I))

#verifica se há algum email, se tiver mostra a quantidade
if emails:
    print('Foram coletados {} e-mails'.format(len(emails)))
    print(emails)
else:
    print('Nenhum e-mail encontrado!')

#cria e grava um arquivo .txt com todos os email
with open('emails.txt', 'w') as arquivo:
    for email in emails:
        arquivo.write(email+', \n')

```

### E para terminar, algumas dicas para aprender Python

+ Oficinas do prof. Peter Jandl: [Oficina 1](https://github.com/pjandl/opy1) e [Oficina 2](https://github.com/pjandl/opy2).
+ Pense Python: livro "aberto" sobre Python. [Acesse aqui](https://penseallen.github.io/PensePython2e/).
+ Materiais do Projeto Panda USP:
  + [Introdução à Computação com Python: um curso interativo](https://panda.ime.usp.br/panda/static/cc110/index.html) 
  + [Como Pensar Como um Cientista da Computação](https://panda.ime.usp.br/panda/static/pensepy/index.html), baseado no livro "Pense Python".
  + [Aulas de Introdução à Computação em Python](https://panda.ime.usp.br/panda/static/aulasPython/index.html)
  + [Resolução de Problemas com Algoritmos e  Estruturas de Dados usando Python](https://panda.ime.usp.br/panda/static/pythonds_pt/index.html)


## Obrigado por acompanharem o Workshop!
Dúvidas? **humberto.zanetti[arroba].fatec.sp.gov.br**