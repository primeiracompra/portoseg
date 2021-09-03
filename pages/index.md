---
layout: page
title: Primeira Compra - PortoSeg
permalink: /
---

# Primeira Compra - PortoSeg

Nesta página será explicado o fluxo de primeira compra, abaixo imagem do fluxo e nos itens a seguir veremos os detalhes de cada passo:
![Fluxo primeira compra](./assets/img/porto-seguro.jpg)

## 1.	Gerar a oferta (POST /prospects – Short Form)

<p>Nessa etapa é enviado uma requisição para gerar uma oferta. As verificações feitas são análises de crédito e variam dependendo da política especificada para cada produto/serviço. No retorno da requisição, no header, terá um campo chamado Location que constará o id da oferta gerada. </p>
<p>Dados obrigatórios: CPF, nome, data de nascimento, CEP, celular, canal, user registration (número da matricula do usuário), metadados (dados do navegador/app para verificação). </p>

<b>Endpoints: </b>

+ Homologação: [https://apihlg-portoseg.sensedia.com/dev/proposal/v1/prospects](https://apihlg-portoseg.sensedia.com/dev/proposal/v1/prospects)
+ Produção: [https://api-portoseg.sensedia.com/proposal/v1/prospects](https://api-portoseg.sensedia.com/proposal/v1/prospects)

## 2.	Consultar a oferta criada (GET /prospects/{id})

Enviar uma requisição de consulta da oferta, passando no endpoint o ID da oferta gerada (pego no response da etapa anterior).
Existem 2 possíveis retornos para o status da oferta:

+	PROCESSO_ENCERRADO – Não é possível gerar uma oferta para essa pessoa e não há mais o que fazer.
+	PENDENTE_PROPOSTA – O cliente foi aprovado no Short Form e é possível seguir para os próximos passos.

<b>Endpoints: </b>
+	Homologação: https://apihlg-portoseg.sensedia.com/dev/proposal/v1/prospects/{id}
+	Produção: https://api-portoseg.sensedia.com/proposal/v1/prospects/{id}

## 3.	Consultar a oferta de logo e limites (GET /prospects/{id}/offers)

Com o ID da oferta, consultar o logo e limite disponíveis.

<b>Endpoints:</b>
+	Homologação: https://apihlg-portoseg.sensedia.com/dev/proposal/v1/prospects/{id}/offers
+	Produção: https://api-portoseg.sensedia.com/proposal/v1/prospects/{id}/offers

## 4.	Gerar proposta (POST /proposals – Long Form)

Nessa etapa, deve ser enviado uma requisição para gerar uma proposta. As verificações feitas são de análises de fraude e variam dependendo da política especificada para cada produto/serviço. No retorno da requisição, no header, terá um campo chamado Location que constará o id da proposta gerada.

Dados obrigatórios: Os mesmos do Short Form e mais: Nome da mãe, gênero (M/F) e endereço (rua, número, bairro, cidade, estado).
No campo Prize pode passar o valor do produto/serviço que está sendo contratado para que o valor do crédito não seja inferior. Depende do canal, então deve ser verificado com o pessoal de crédito.

<b>Endpoints: </b>
+	Homologação: https://apihlg-portoseg.sensedia.com/dev/proposal/v1/proposals
+	Produção: https://api-portoseg.sensedia.com/proposal/v1/proposals

## 5.	Consultar a proposta criada (GET /proposals/{id})

Enviar uma requisição de consulta da proposta, passando no endpoint o ID da proposta gerada (pego no response da etapa anterior). Se a proposta não for aprovada é o fim do processo. Se for aprovada (HTTP response code 200), seguir as próximas etapas.

<b>Endpoints:</b>
+	Homologação: https://apihlg-portoseg.sensedia.com/dev/proposal/v1/proposals/{id}
+	Produção: https://api-portoseg.sensedia.com/proposal/v1/proposals/{id}

## 6.	Consultar o logo (GET /parametros/logo/{id})

Enviar uma requisição de consulta do logo que foi retornado na consulta da Etapa 3. Exemplo de logos: 001, 002...

Para realizar essa chamada é preciso gerar um token de acesso (explicado na etapa 10).

<b>Endpoints:</b>
+	Homologação: https://apihlg-portoseg.sensedia.com/dev/parametros/v1/parametro/logo/{id}
+	Produção: https://api-portoseg.sensedia.com/parametros/v1/parametro/logo/{id}

## 7.	Gerar o cartão (POST /{id}/details)

Enviar uma requisição POST para o endpoint de /details para gerar o cartão.

Dados que devem ser enviados: código de produto (1 para cartão), limite selecionado (entre o range da consulta do logo), logo, data de expiração (vencimento – uma das datas da consulta do logo), seguro (true/false), debito automático (true/false), statement option (E) e CPF.

> <b>OBS: Para o vencimento deve-se sempre usar um da lista do endpoint de logo. </b>

<b>Endpoints:</b>
+	Homologação: https://apihlg-portoseg.sensedia.com/dev/proposal/v1/proposals/{id}/details
+	Produção:  https://api-portoseg.sensedia.com/proposal/v1/proposals/{id}/details

  	Após essa chamada retornar com sucesso (HTTP Code 204), o cartão já está gerado na CSU (processadora). Porém, para todos os dados serem processados em todos os sistemas, em produção leva até 48h. Após essas 48h é possível efetuar os lançamentos de compras (etapas a seguir).

> <b>OBS: Em homologação esse processamento é manual e precisa ser alinhado para ser executado com o analista de Cartões (primeira compra). </b>

## 8.	Consultar dados cifrados do cartão (GET /goodcard)

Após as 48h de geração do cartão, é possível consultar os dados cifrados dele, passando o CPF da pessoa como parâmetro.

Para realizar essa chamada é preciso gerar um token de acesso (explicado na etapa 10).

<b>Endpoints:</b>
+	Homologação: https://apihlg-portoseg.sensedia.com/goodcard/v1/cards?cpf={cpf}
+	Produção:  https://api-portoseg.sensedia.com/goodcard/v1/cards?cpf={cpf}

## 9.	Efetuar lançamentos de compras (POST /authorizations)

Com os dados do cartão, é possível efetuar os lançamentos da compra do cliente.

Dados obrigatórios: dados cifrados do cartão, descrição da transação, estabelecimento e valor. 
Para projetos de primeira compra é preciso cuidar que o estabelecimento precisa estar cadastrado nos ambientes (homologação/produção) e precisa estar configurado para permitir autorização forçada (pois a transação de primeira compra será Private Label e o cartão por <i>default</i> quando gerado está bloqueado por transporte). Para fazer essa solicitação, a área de negócio deve entrar em contato com a área de produto e verificar como proceder.

<b>Endpoints:</b>
+	Homologação: https://apihlg-portoseg.sensedia.com/payware/v1/authorizations/
+	Produção:  https://api-portoseg.sensedia.com/payware/v1/authorizations/

## 10.	Geração do Token (POST /access-token):

Para fazer algumas chamadas é preciso gerar um token de acesso usando o cliente_id, através dos endpoints:
+	Homologação: https://apihlg-portoseg.sensedia.com/dev/oauth/v1/access-token
+	Produção: https://api-portoseg.sensedia.com/dev/oauth/v1/access-token

O Token expira em uma hora.

