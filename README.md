# Explorando Práticas de Teste

Neste exercício, vamos explorar práticas de teste em sistemas reais utilizando a ferramenta [TestMiner](https://andrehora.github.io/testminer).

O TestMiner permite visualizar e analisar testes de software em repositórios do GitHub, fornecendo dados sobre como os projetos organizam seus testes, como eles evoluem entre versões e quais bibliotecas de teste são utilizadas.
Explore a ferramenta antes de começar para se familiarizar com seu funcionamento.

Mais detalhes no GitHub da ferramenta: https://github.com/andrehora/testminer.

---

## Passo 1: Selecionar um repositório

Escolha um repositório real que possua testes de software.
Abaixo estão alguns links para ajudá-lo a encontrar projetos interessantes:

- Python: https://github.com/topics/python?l=python
- JavaScript: https://github.com/topics/javascript?l=javascript
- TypeScript: https://github.com/topics/typescript?l=typescript
- Java: https://github.com/topics/java?l=java

- Tópicos: [ai](https://andrehora.github.io/testminer/#topic:ai), [llm](https://andrehora.github.io/testminer/#topic:llm), [api](https://andrehora.github.io/testminer/#topic:api), [nodejs](https://andrehora.github.io/testminer/#topic:nodejs), [android](https://andrehora.github.io/testminer/#topic:android)

- Por organização: [Google](https://andrehora.github.io/testminer/#google), [Microsoft](https://andrehora.github.io/testminer/#microsoft), [Apple](https://andrehora.github.io/testminer/#apple), [Facebook](https://andrehora.github.io/testminer/#facebook), [Netflix](https://andrehora.github.io/testminer/#netflix), 
[GitHub](https://andrehora.github.io/testminer/#github), [Apache](https://andrehora.github.io/testminer/#apache), [HuggingFace](https://andrehora.github.io/testminer/#huggingface)

## Passo 2: Explorar o repositório selecionado

Busque o repositório escolhido no [TestMiner](https://andrehora.github.io/testminer) e analise os dados de teste gerados pela ferramenta.

## Passo 3: Explicar uma prática de teste

Escolha uma prática ou dado de teste relevante e explique com suas próprias palavras.

---

## Instruções de entrega

1. Faça um `fork` deste repositório (saiba mais sobre forks [aqui](https://docs.github.com/pt/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo)).
2. Responda às questões abaixo diretamente neste arquivo `README.md` do seu fork. Pode adicionar imagens para enriquecer sua explicação.
3. No Moodle, submeta apenas a URL do seu fork.

---

## Respostas

Repositório: https://github.com/fastapi/fastapi

URL TestMiner: https://andrehora.github.io/testminer/#fastapi/fastapi

Explicação: Escolhi o FastAPI, um framework em Python para criar APIs web. Já tinha ouvido falar bastante dele e fiquei curioso para ver como um projeto tão usado organiza os testes. O que me chamou a atenção no TestMiner foi a quantidade de testes. O pacote principal (fastapi/) tem pouco mais de 50 arquivos Python, enquanto a pasta tests/ tem mais de 600, com cerca de 2.400 funções de teste no total. Dá mais ou menos 10 arquivos de teste para cada arquivo de código. Os testes ficam todos separados do código, na pasta tests/, e lá dentro ainda tem pastas próprias para benchmarks de desempenho e de memória.

Nas dependências de teste, o projeto usa o pytest como framework principal, junto com alguns plugins (pytest-cov para cobertura, pytest-xdist para rodar em paralelo, pytest-timeout e pytest-codspeed para os benchmarks). Também usa o httpx, que é o que está por trás do TestClient do FastAPI, e bibliotecas como inline-snapshot e dirty-equals para facilitar as comparações nos asserts.

Explorando a estrutura de pastas, percebi uma coisa que eu não esperava: existe uma pasta chamada tests/test_tutorial/, e ela sozinha tem uns 330 arquivos de teste (cerca de 780 funções), o que dá mais ou menos um terço de todos os testes do projeto.

Fui ver do que se tratava e entendi que o FastAPI testa os próprios exemplos da documentação. Os trechos de código que aparecem no site da documentação não são escritos direto no texto. Eles ficam em arquivos Python de verdade, na pasta docs_src/. E para cada exemplo existe um teste correspondente em tests/test_tutorial/, seguindo a mesma organização de pastas.

OPor exemplo, os exemplos de docs_src/body/ são testados em tests/test_tutorial/test_body/. Cada teste importa o arquivo do exemplo, cria um cliente de teste em cima da aplicação (sem precisar subir um servidor de verdade) e faz requisições, conferindo se o status e o JSON de resposta estão certos. Quando há versões diferentes do mesmo exemplo, o teste roda em cada uma delas.

Achei essa prática bem inteligente porque ela resolve um problema comum: documentação desatualizada. Em muitos projetos você copia um exemplo da documentação e ele não funciona mais, porque a biblioteca mudou e ninguém atualizou o texto. No FastAPI isso não acontece, porque se alguma mudança quebrar um exemplo, os testes falham no CI na hora. Além disso, esses testes acabam funcionando quase como testes de aceitação, já que usam o framework do mesmo jeito que um usuário usaria, e ajudam a pegar mudanças que quebrariam código de quem já usa a biblioteca.

O lado ruim é que isso dá trabalho: todo exemplo novo na documentação precisa de um teste junto, e a suíte de testes fica bem grande. Imagino que seja por isso que eles usam o pytest-xdist para rodar os testes em paralelo. Mesmo assim, acho que vale a pena, e isso explica boa parte da quantidade tão grande de testes que o TestMiner mostra para esse repositório.
