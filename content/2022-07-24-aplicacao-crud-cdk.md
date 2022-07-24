---
layout: post
title: Aplicacao CRUD Serverless com AWS CDK - Parte 2
date: 2022-07-24 14:50:00 +0300
description: AWS CDK Parte 2
img: we-in-rest.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [CDK]
---

# Aplicação CRUD Serverless com AWS CDK - Parte 2

O objetivo será construir um CRUD através de uma arquitetura simples, onde criaremos um API Gateway, e pra cada verbo HTTP usado ao invocar o endpoint desse API Gateway, será chamada uma função lambda, e a função irá ter acesso a um banco de dados que também iremos criar.

![Screenshot 2022-07-22 at 11.21.32.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3f7e22f9-b2e7-4748-b319-da4152a801c7/Screenshot_2022-07-22_at_11.21.32.png)

```jsx
Pré-requisitos
- Aplicação CRUD Serverless com AWS CDK - Parte 1
- Ambiente aws configurado (aws configure)
- Instalar o aws-cdk package com `npm i aws-cdk`
```

Então vamos abrir nosso terminal, no meu caso estou usando o iTerm2, e numa pasta de sua preferência onde iremos criar nosso projeto, iremos dar o comando

```jsx
cdk init --language typescript
```

esse comando irá criar toda uma estrutura básica pra funcionamento do nosso projeto, vamos abrir no Visual Studio pra vermos como ficou a estrutura criada

Então minha ideia aqui é criar uma pastinha chamada `lambda` e dentro dela vamos criar nossas funções lambdas que ficarão separadas em quatro arquivos.

Primeiro arquivo que vamos criar então dentro da pasta `lambda` será para criar orders, vamos dar o nome de `create-orders.js` e a princípio vamos deixar essa função o mais simples possível, depois a gente volta refatorando pra começar a dar mais responsabilidades a nossa função

```jsx
exports.main = async function (event, context) {
  return {
    statusCode: 201,
    body: "new register Yay!",
  };
};
```

bem simples, sempre irá retornar um status 201 (Created), com a mensagem “new register Yay!”

Agora vamos criar nossos próximos arquivos seguindo o mesmo modelo simples, e refatoramos depois pra adicionar funcionalidades a essas funções

Criar o arquivo `delete-orders.js` com código bem semelhante, todos eles na verdade terão um código semelhante, e depois refatoramos

```jsx
exports.main = async function (event, context) {
  return {
    statusCode: 200,
    body: "order deleted",
  };
};
```

Agora vamos criar o arquivo `get-orders.js`

```jsx
exports.main = async function (event, context) {
  return {
    statusCode: 200,
    body: "all orders here",
  };
};
```

e por fim, criaremos um último arquivo que irá conter a função de update, arquivo vamos chama-lo de `update-orders.js`

```jsx
exports.main = async function (event, context) {
  return {
    statusCode: 200,
    body: "order updated",
  };
};
```

Arquivos criados, funções criadas, e agora? próximo passo então será criar um arquivo dentro da pasta `lib`, e nesse arquivo vamos concentrar a lógica pra criar a arquitetura de nossa aplicação usando o AWS CDK

Então, vamos dar um nome ao nosso arquivo, vou colocar o nome de `order-service.ts`

```jsx
export class OrdersService extends Construct {
  constructor(scope: Construct, id: string) {
    super(scope, id);
  }
}
```

essa é a estrutura inicial de um `Construct` , então vamos fazer uma listinha do que precisamos fazer agora

```jsx
export class OrdersService extends Construct {
  constructor(scope: Construct, id: string) {
    super(scope, id);

    // 1. Registrar nossa lambda de buscar orders - GET
    // 2. Registrar nossa lambda de criar orders - POST
    // 3. Registrar nossa lambda de deletar orders - DELETE
    // 4. Registrar nossa lambda de atualizar orders - UPDATE
    // 5. Criar uma REST API Gateway
    // 6. Associar cada verbo HTTP a sua respectiva função
  }
}
```

com isso, vamos ao primeiro item da lista

```jsx
import { Code, Function, Runtime } from "aws-cdk-lib/aws-lambda";

export class OrdersService extends Construct {
  constructor(scope: Construct, id: string) {
    super(scope, id);

    // 1. Registrar nossa lambda de buscar orders - GET
    const handlerGet = new Function(this, "GetOrdersHandler", {
      runtime: Runtime.NODEJS_14_X,
      code: Code.fromAsset("lambda"), // pasta onde esta as lambdas
      handler: "get-orders.main", // nome do arquivo.nome do metodo exportado
    });

    // 2. Registrar nossa lambda de criar orders - POST
    // 3. Registrar nossa lambda de deletar orders - DELETE
    // 4. Registrar nossa lambda de atualizar orders - UPDATE
    // 5. Criar uma REST API Gateway
    // 6. Associar cada verbo HTTP a sua respectiva função
  }
}
```

Então vamos ver o que está acontecendo aqui, pra declarar ou registrar a função lambda que criamos ele vai pedir um, que é o mesmo que temos no nosso constructor no início da nossa classe, então vamos passar o `this` depois passamos um `id` pra nossa função e depois algumas props, que será `runtime, code e handler` (há muitas outras props que podemos passar aqui, mas vou trabalhar com essas 3 apenas, pra manter o código num cenário simples.

O `runtime` é a versão do ambiente, do node, na qual você irá fazer o upload dessa função pro ambiente da AWS, já o `code` é simplesmente a pasta onde colocamos nossas funções lambda, no nosso caso foi na pastinha que demos o nome de `lambda`, e o `handler` onde iremos informar o nome do arquivo e o nome do método exportado no arquivo.

Agora vamos registrar as outras funções lambdas que criamos

```jsx
export class OrdersService extends Construct {
  constructor(scope: Construct, id: string) {
    super(scope, id);

    // 1. Registrar nossa lambda de buscar orders - GET
    const handlerGet = new Function(this, "GetOrdersHandler", {
      runtime: Runtime.NODEJS_14_X,
      code: Code.fromAsset("lambda"), // pasta onde esta as lambdas
      handler: "get-orders.main", // nome do arquivo.nome do metodo exportado
    });

    // 2. Registrar nossa lambda de criar orders - POST
    const handlerPost = new Function(this, "CreateOrdersHandler", {
      runtime: Runtime.NODEJS_14_X,
      code: Code.fromAsset("lambda"),
      handler: "create-orders.main",
    });

    // 3. Registrar nossa lambda de deletar orders - DELETE
    const handlerDelete = new Function(this, "DeleteOrdersHandler", {
      runtime: Runtime.NODEJS_14_X,
      code: Code.fromAsset("lambda"),
      handler: "delete-orders.main",
    });

    // 4. Registrar nossa lambda de atualizar orders - UPDATE
    const handlerUpdate = new Function(this, "UpdateOrdersHandler", {
      runtime: Runtime.NODEJS_14_X,
      code: Code.fromAsset("lambda"),
      handler: "update-orders.main",
    });

    // 5. Criar uma REST API Gateway
    // 6. Associar cada verbo HTTP a sua respectiva função
  }
}
```

Bom, registramos nossas lambdas no nosso constructor, mas ainda não temos um API Gateway, e também não associamos os verbos HTTP a sua respectiva função, como por exemplo, o GET tem que chamar a meu lambda responsável por buscar as orders, certo?

Então vamos para o passo 5, que é criar um API Gateway

```jsx
export class OrdersService extends Construct {
  constructor(scope: Construct, id: string) {
    super(scope, id);

    // 1. Registrar nossa lambda de buscar orders - GET
    const handlerGet = new Function(this, "GetOrdersHandler", {
      runtime: Runtime.NODEJS_14_X,
      code: Code.fromAsset("lambda"), // pasta onde esta as lambdas
      handler: "get-orders.main", // nome do arquivo.nome do metodo exportado
    });

    // 2. Registrar nossa lambda de criar orders - POST
    const handlerPost = new Function(this, "CreateOrdersHandler", {
      runtime: Runtime.NODEJS_14_X,
      code: Code.fromAsset("lambda"),
      handler: "create-orders.main",
    });

    // 3. Registrar nossa lambda de deletar orders - DELETE
    const handlerDelete = new Function(this, "DeleteOrdersHandler", {
      runtime: Runtime.NODEJS_14_X,
      code: Code.fromAsset("lambda"),
      handler: "delete-orders.main",
    });

    // 4. Registrar nossa lambda de atualizar orders - UPDATE
    const handlerUpdate = new Function(this, "UpdateOrdersHandler", {
      runtime: Runtime.NODEJS_14_X,
      code: Code.fromAsset("lambda"),
      handler: "update-orders.main",
    });

    // 5. Criar uma REST API Gateway
    const api = new RestApi(this, "orders-api", {
      restApiName: "OrdersService",
      description: "REST API Gateway pra Orders",
    });

    // 6. Associar cada verbo HTTP a sua respectiva função
  }
}
```

API Gateway criada, setamos um `restApiName` e uma `description` a ela, vamos agora associar os métodos `GET`, `POST`, `DELETE` e `PUT` a uma função lambda que já criamos

```jsx
export class OrdersService extends Construct {
  constructor(scope: Construct, id: string) {
    super(scope, id);

    // 1. Registrar nossa lambda de buscar orders - GET
    const handlerGet = new Function(this, "GetOrdersHandler", {
      runtime: Runtime.NODEJS_14_X,
      code: Code.fromAsset("lambda"), // pasta onde esta as lambdas
      handler: "get-orders.main", // nome do arquivo.nome do metodo exportado
    });

    // 2. Registrar nossa lambda de criar orders - POST
    const handlerPost = new Function(this, "CreateOrdersHandler", {
      runtime: Runtime.NODEJS_14_X,
      code: Code.fromAsset("lambda"),
      handler: "create-orders.main",
    });

    // 3. Registrar nossa lambda de deletar orders - DELETE
    const handlerDelete = new Function(this, "DeleteOrdersHandler", {
      runtime: Runtime.NODEJS_14_X,
      code: Code.fromAsset("lambda"),
      handler: "delete-orders.main",
    });

    // 4. Registrar nossa lambda de atualizar orders - UPDATE
    const handlerUpdate = new Function(this, "UpdateOrdersHandler", {
      runtime: Runtime.NODEJS_14_X,
      code: Code.fromAsset("lambda"),
      handler: "update-orders.main",
    });

    // 5. Criar uma REST API Gateway
    const api = new RestApi(this, "orders-api", {
      restApiName: "OrdersService",
      description: "REST API Gateway pra Orders",
    });

    // 6. Associar cada verbo HTTP a sua respectiva função
    api.root.addMethod("GET", handlerGet);
  }
}
```

Podemos ver um erro,

```jsx
Argument type 'Function' is not assingable to parameter of type 'Integration'
```

em outras palavras, ele esperava um parâmetro do tipo `Integration` e eu passei uma função que criamos logo acima, então por isso ele fala “opa espera aí, você me passou uma função e eu espero receber uma integration”, e isso faz sentido, pois ao abrir o Console da AWS, na parte de API Gateway, vou pegar um aqui que já tenho, existe um passo chamado Integration Request e depois sim vem o Lambda Function.

![Screenshot 2022-07-23 at 16.55.15.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b88b675a-1550-4fb5-8085-0a8e76f97293/Screenshot_2022-07-23_at_16.55.15.png)

então vamos criar essa integration que ele pede, vou adicionar um novo passo no nosso código

```jsx
export class OrdersService extends Construct {
  constructor(scope: Construct, id: string) {
    super(scope, id);

    // 1. Registrar nossa lambda de buscar orders - GET
    const handlerGet = new Function(this, "GetOrdersHandler", {
      runtime: Runtime.NODEJS_14_X,
      code: Code.fromAsset("lambda"), // pasta onde esta as lambdas
      handler: "get-orders.main", // nome do arquivo.nome do metodo exportado
    });

    // 2. Registrar nossa lambda de criar orders - POST
    const handlerPost = new Function(this, "CreateOrdersHandler", {
      runtime: Runtime.NODEJS_14_X,
      code: Code.fromAsset("lambda"),
      handler: "create-orders.main",
    });

    // 3. Registrar nossa lambda de deletar orders - DELETE
    const handlerDelete = new Function(this, "DeleteOrdersHandler", {
      runtime: Runtime.NODEJS_14_X,
      code: Code.fromAsset("lambda"),
      handler: "delete-orders.main",
    });

    // 4. Registrar nossa lambda de atualizar orders - UPDATE
    const handlerUpdate = new Function(this, "UpdateOrdersHandler", {
      runtime: Runtime.NODEJS_14_X,
      code: Code.fromAsset("lambda"),
      handler: "update-orders.main",
    });

    // 5. Criar uma REST API Gateway
    const api = new RestApi(this, "orders-api", {
      restApiName: "OrdersService",
      description: "REST API Gateway pra Orders",
    });

    // 6. Integration
    const getOrdersIntegration = new LambdaIntegration(handlerGet);
    const postOrdersIntegration = new LambdaIntegration(handlerPost);
    const deleteOrdersIntegration = new LambdaIntegration(handlerDelete);
    const updateOrdersIntegration = new LambdaIntegration(handlerUpdate);

    // 7. Associar cada verbo HTTP a sua respectiva função
    api.root.addMethod("GET", getOrdersIntegration);
    api.root.addMethod("POST", postOrdersIntegration);
    api.root.addMethod("DELETE", deleteOrdersIntegration);
    api.root.addMethod("PUT", updateOrdersIntegration);
  }
}
```

com isso, terminamos por agora de criar, registrar e associar as funções lambdas à API Gateway, e vamos fazer um deploy do que fizemos pra ver como fica na AWS, pra fazer o deploy basta executar o comando

```jsx
cdk deploy
```
