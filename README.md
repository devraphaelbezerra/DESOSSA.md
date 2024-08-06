# DESOSSA.md
Nome do processo:"robo_de_carcaca_desossa" está agendado via painel de controle > Agendador de tarefas.

Foi desenvolvido uma rotina via fluig que executa diariamente via um dataset onSync onde procura os itens tipo 01 (exemplo: Contra Filet s/Osso) do açougue nas agendas [727,5,101] que ainda não passaram pelo processo de desossa anteriormente comparando os itens de entrada com os itens para cortes pré-cadastrados nas tabelas "fluig.ti_desossa_pecas" e "fluig.ti_desossa_pecas_corte" a partir daí é alimentado um laço de execução no dataset central do processo:"ds_ROBO_DE_CARCACAS.js" ao garantir via validações de funções que este corte e NF estão aptas a serem geradas o dataset central chama a API visualMix no RMS via PUT webservice no servidor "http://10.12.0.143:83" onde gera as devidas NF nas agendas 730 e 580.

![image](https://github.com/user-attachments/assets/4558c72c-3c13-48f9-a2ae-984a814b16e2)

![image](https://github.com/user-attachments/assets/77492f40-13a0-4926-a89a-5c92d7e559e0)

### Tecnologias Utilizadas

* PL-SQL.
* JavaScript.
* Bibliotecas JS.
* Web Service.

## Dependências e Versões Necessárias

* TOTVS Fluig Plataforma - 1.6+
* Web Service swagger
* moment.js

## Como rodar o projeto ✅
```
Comando 1
```
![image](https://github.com/user-attachments/assets/3e6c3a9a-c27a-41ee-9753-395d8a800028)
Realizar o select via tabela:"fluig.ti_desossa_item_parametros" e verificar se a(s) loja(s) estão marcadas como FLGATIVO:"A" e a hora de execução na coluna: "HORA_INIC_EXECUCAO". 

```
Comando 2
```
Após isso, acessar o fluig como user administrador, ir em Painel de controle > Parâmetros técnicos > Agendador de tarefas e procurar pelo processo nome: "robo_de_carcaca_desossa" marcar e executar.

```
Comando 2
```
Acompanhar via log do fluig a execução ou via tabela de controle da desossa:"fluig.ti_desossa_controle_nfe" de ficam salvas as execuções e rastreamentos dos cortes:
![image](https://github.com/user-attachments/assets/0b8a43fb-25c9-4bfb-8a22-dc8da871412e)

## 📌 (Título) - Informações importantes sobre a aplicação (exemplo) 📌

Esse é o local para você preencher com outras informações que possam ser importantes para a aplicação. Coloquei um exemplo de título, mas você deve preencher de acordo com a necessidade do projeto. Pode ser que não seja necessário.

Um bom exemplo: se você estiver construindo uma API, liste as rotas da aplicação e quais serão os seus retornos. Isso facilita para quem vai consumir a API.


## ⚠️ Problemas enfrentados

Liste os problemas que você enfrentou construindo a aplicação e como você resolveu cada um deles. Você que desenvolveu o projeto é a pessoa que mais conhece/entende os possíveis problemas que uma pessoa pode enfrentar rodando a aplicação. Compartilhe esse conhecimento e facilite a vida da pessoa descrevendo-os.

Exemplo:

### Problema 1:
Descrição do problema
* Como solucionar: explicar a solução.

### Problema 2:
Descrição do problema
* Como solucionar: explicar a solução.

