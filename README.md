# Introduçao-ao-Desenvolvimento-Full-stack-com-AWS-Amplify

## Pré-requisitos
- Node.js
- npm
- Conta na AWS
- Instalar e configurar o Amplify CLI:
  ```sh
  npm install -g @aws-amplify/cli
  amplify configure
  ```
- Criar usuário no AWS IAM
  - Selecionar a política `AdministratorAccess`
  - Configurar o usuário
  ```sh
  Enter the access key of the newly created user:
  ? accessKeyId:  # YOUR_ACCESS_KEY_ID
  ? secretAccessKey:  # YOUR_SECRET_ACCESS_KEY
  Profile Name:  # (default)
  ```

## Criar o projeto fullstack
```sh
mkdir -p amplify-js-app/src && cd amplify-js-app
```
```sh
touch package.json index.html webpack.config.js src/app.js
```

A estrutura de diretórios do projeto será:
```
amplify-js-app
├── index.html
├── package.json
├── src
│   └── app.js
└── webpack.config.js
```

### Adicionar o seguinte conteúdo ao `package.json`:
```json
{
  "name": "amplify-js-app",
  "version": "1.0.0",
  "description": "Amplify JavaScript Example",
  "dependencies": {
    "aws-amplify": "latest"
  },
  "devDependencies": {
    "webpack": "^4.17.1",
    "webpack-cli": "^3.1.0",
    "copy-webpack-plugin": "^6.1.0",
    "webpack-dev-server": "^3.1.5"
  },
  "scripts": {
    "start": "webpack && webpack-dev-server --mode development",
    "build": "webpack"
  }
}
```

### Instalar as dependências:
```sh
npm install
```

### Criar o back-end com Amplify
```sh
amplify init
```
Mantenha as configurações padrão sugeridas pelo Amplify.

### Criar uma API GraphQL:
```sh
amplify add api
```
Mantenha as configurações padrão sugeridas e publique a API:
```sh
amplify push
```

### Testar a API
```sh
amplify console api
```

### Conectar o front-end com a API
Atualizar `src/app.js`:
```javascript
import Amplify, { API, graphqlOperation } from "aws-amplify";
import awsconfig from "./aws-exports";
import { createTodo } from "./graphql/mutations";

Amplify.configure(awsconfig);

async function createNewTodo() {
  const todo = {
    name: "Use AppSync",
    description: `Realtime and Offline (${new Date().toLocaleString()})`,
  };
  return await API.graphql(graphqlOperation(createTodo, { input: todo }));
}

const MutationButton = document.getElementById("MutationEventButton");
const MutationResult = document.getElementById("MutationResult");

MutationButton.addEventListener("click", (evt) => {
  createNewTodo().then((evt) => {
    MutationResult.innerHTML += `<p>${evt.data.createTodo.name} - ${evt.data.createTodo.description}</p>`;
  });
});
```

### Implantar e hospedar a aplicação
```sh
amplify add hosting
```
Mantenha as configurações padrão sugeridas pelo Amplify.
```sh
amplify publish
