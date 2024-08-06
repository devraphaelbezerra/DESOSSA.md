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
![355586772-4558c72c-3c13-48f9-a2ae-984a814b16e2](https://github.com/user-attachments/assets/d4b0d5ef-875d-4be4-bc1a-b7180ea9e80e)
Após isso, acessar o fluig como user administrador, ir em Painel de controle > Parâmetros técnicos > Agendador de tarefas e procurar pelo processo nome: "robo_de_carcaca_desossa" marcar e executar.

```
Comando 2
```
![image](https://github.com/user-attachments/assets/0b8a43fb-25c9-4bfb-8a22-dc8da871412e)
Acompanhar via log do fluig a execução ou via tabela de controle da desossa:"fluig.ti_desossa_controle_nfe" de ficam salvas as execuções e rastreamentos dos cortes.

## 📌 (Robo de Carcaça Desossa) - Informações importantes sobre a aplicação 📌

Foram encontrados casos de NF que se aproximam da geração de NF Fluig porém não foram originadas pelo o controle da tabela "fluig.ti_desossa_controle_nfe" e o sequencial da tabela rms.fat_atacado não existe no controle do fluig, como a aplicação/rotina foi projetada para funcionar usando tabelas personalizadas do Fluig e agendas do retaguarda RMS em uma API que é para uso no RMS, o fluig apenas faz uma requisição PUT para gerar NFe e esta chamada realiza do lado do RMS insert na tabela rms.fat_atacado onde é gerado pelas PROC do RMS as NFs, sempre é importante revisar os sequenciais entre a RMS.FAT_ATACADO(NUMERO_SEQUENCIAL) e FLUIG.TI_DESOSSA_CONTROLE_NFE(FAT_NUMERO_SEQUENCIAL).

## ⚠️ Problemas enfrentados

Listo abaixo os problemas enfrentados mais comuns de acontecer no processo desossa.

### Problema 1:
Descrição do problema:
NFe de entradas para cortes das agendas [727,5,101] vem com o mesmo número de Nota que já foi usado anteriormente, não processa os cortes e deve ser confirmado se realmente a NFe de prateleira 730 e 580 recebimento não foi gerada no retarda mesmo que em datas futuras.
* Como solucionar: Este não é um "problema" no processo, e sim um problema operacional do fornecedor que gerou a NF com o mesmo número de outra NF usada anteriormente e também do recebimento do formosa/comprador que não força o mesmo a enviar número de notas corretas sem sequencias, deve-se acessar a tabela de controle NFe "fluig.ti_desossa_controle_nfe" e localizar a NFe de entrada pelo númeração que não processou, salvar as informações em alguma lugar seguro pois será usada futuramente, realizar o update na "fluig.ti_desossa_controle_nfe" na coluna "NOTA" alterar o número de NOTA para outro número próximo exemplo de:207551 para: 207551666 passando como filtro a filial, item, nota, dataNF e executar a desossa novamente, com isso programa de execução do fluig não vai mais achar este número de NF no not in da query de pendencias e executara/processará os cortes, após esta execução, deve-se voltar o número de nota exemplo realizando outro update de:207551666 para:207551.

### Problema 2:
Descrição do problema:
O usuários que estão responsáveis reclamam que a desossa não executou no dia anterior.
* Como solucionar: Deve-se revisar se a tarefa no banco que executa uma PROC da Visual MIX RMS está com o status BROKEN==Y, o job quando bloqueado não funciona e nem processas as NFe.

