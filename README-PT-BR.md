# PyBambooHR

[![Build Status](https://secure.travis-ci.org/smeggingsmegger/PyBambooHR.png)](https://travis-ci.org/smeggingsmegger/PyBambooHR)&nbsp;&nbsp;&nbsp;![Download Stats](https://pypip.in/download/PyBambooHR/badge.svg)

Esta é uma versão API não oficial do Python para o Bamboo HR. Até agora, ela está focada no gerenciamento de informações de funcionários, mas você pode fazer praticamente tudo o que quiser com um pouco de python. 

Essa biblioteca faz uso da biblioteca [requests](http://docs.python-requests.org/en/latest/) do Python e [HTTPretty](https://github.com/gabrielfalcao/HTTPretty) para testes. Um enorme obrigado a ambos os excelentes projetos.

Usar essa biblioteca é bem simples:

```python
from PyBambooHR import PyBambooHR

bamboo = PyBambooHR(subdominio='SeuSubDominio', api_key='SuaChaveAPI')

funcionarios = bamboo.get_employee_directory()
```

(Note que você tem que habilitar o compartilhamento do diretório de funcionários para usar o método.)

Isso fornecerá uma lista dos funcionários com propriedades em cada um, incluindo seu ID.

```python
from PyBambooHR import PyBambooHR

bamboo = PyBambooHR(subdominio='SeuSubDominio', api_key='SuaChaveAPI')

# O ID do funcionário Jim é 123 e não estamos especificando campos, portanto, isso obterá todos eles.
jim = bamboo.get_employee(123)

# O ID do funcionário Pam é 222 e estamos especificando campos para que recebamos apenas os dados que solicitamos.
pam = bamboo.get_employee(222, ['cidade', 'TelefoneDeTrabalho', 'EmailProfissional'])

```

Adicionando um funcionário

```python
from PyBambooHR import PyBambooHR

bamboo = PyBambooHR(subdominio='SeuSubDominio', api_key='SuaChaveAPI')

# As chaves primeiroNome e o sobreNome são necessárias...
funcionário = {'primeiroNome': 'Teste', 'sobreNome': 'Pessoa'}

resultado = bamboo.add_employee(funcionario)

O dicionário resultante conterá id e localização. "id" é o ID número do funcionário da BambooHR. Localização é um link para o funcionário.
```

Atualizando um funcionário

```python
from PyBambooHR import PyBambooHR

bamboo = PyBambooHR(subdominio='SeuSubDominio', api_key='SuaChaveAPI')

# O nome dele era Teste Pessoa...
funcionario = {'primeiroNome': 'Outro', 'sobreNome': 'NovoNome'}

# Use o ID e, em seguida, o dicionário com as novas informações
# Use the ID and then the dict with the new information
resultado = bamboo.update_employee(333, funcionário)

resultado será True (Verdadeiro) ou False (Falso) dependendo se a atualização for bem sucedida.

```

Solicitando um Relatório

```python
from PyBambooHR import PyBambooHR

bamboo = PyBambooHR(subdominio='SeuSubDominio', api_key='SuaChaveAPI')

# Use o ID para pedir informações do json
result = bamboo.request_company_report(1, format='json', filter_duplicates=True)

# Agora faça coisas com seus resultados (variará de acordo com seus resultados.)
for funcionário in resultado['funcionarios']:
    print(funcionario)

# Use o ID e salve um PDF:
resultado = bamboo.request_company_report(1, format='pdf', output_file='/tmp/report.pdf', filter_duplicates=True)

```
