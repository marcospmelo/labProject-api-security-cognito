<div align="center">
<h1  align="center">PROJETO PRÁTICO BOOTCAMP CLOUD AWS ꞏ DIO</h1>
</div>
<div align="center"> <img src="https://hermes.digitalinnovation.one/tracks/af22d4a0-463f-48c5-a70c-4961d5e618d0.png" alt="Linux Experience" width="300"> </div>

## :closed_lock_with_key: Adicionando Segurança em APIs na AWS com Amazon Cognito

## :wrench: Ferramentas que utilizei

<div align="center"> 
<img src="https://img.shields.io/badge/Amazon_AWS-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white" alt="Git badge"/>
<img src="https://img.shields.io/badge/Visual%20Studio%20Code-0078d7.svg?style=for-the-badge&logo=visual-studio-code&logoColor=white" alt="Git badge"/>
<img src="https://img.shields.io/badge/Postman-FF6C37.svg?style=for-the-badge&logo=Postman&logoColor=white" alt="Git badge"/> 
<img src="https://img.shields.io/badge/GIT-E44C30?style=for-the-badge&logo=git&logoColor=white" alt="Git badge"/> 
<img src="https://img.shields.io/badge/GitKraken-179287.svg?style=for-the-badge&logo=GitKraken&logoColor=white" alt="Github Badge"/> 
<img src="https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white" alt="Github Badge"/> 
</div>

## :chart_with_upwards_trend: Diagrama do projeto

<div align="center"> <img src="https://github.com/walterowisk/labProject-api-security-cognito-AWS/blob/main/img/lab-project-cognito-diagram.jpg" alt="AWS Diagram" width="600"> </div>

## :cloud: Serviços AWS neste projeto

- **COGNITO:** Fornece autenticação, autorização e gerenciamento de usuários para suas aplicações Web e móveis. Seus usuários podem fazer login diretamente com um nome de usuário e uma senha ou por meio de terceiros, como o Facebook, a Amazon, o Google ou a Apple. [Saiba mais.](https://aws.amazon.com/pt/cognito/)
- **API GATEWAY:** Serviço gerenciado que permite criação, publicação, manutenção, monitoramento e proteção de APIs REST e WebSocket em qualquer escala. [Saiba mais.](https://aws.amazon.com/pt/api-gateway/)
- **LAMBDA:** Serviço de computação sem servidor e orientado a eventos que permite executar código para praticamente qualquer tipo de aplicação ou serviço de backend sem provisionar ou gerenciar servidores. [Saiba mais.](https://aws.amazon.com/pt/lambda/)
- **DYNAMODB:** Banco de dados de chave-valor NoSQL, sem servidor e totalmente gerenciado, projetado para executar aplicações de alta performance em qualquer escala. [Saiba mais.](https://aws.amazon.com/pt/dynamodb/)

## :page_facing_up: Descrição do desafio de projeto

Oferecer autenticação, autorização e gerenciamento de usuários para suas aplicações Web e Mobile com o Amazon Cognito. Esse serviço, totalmente gerenciado pela AWS, suporta os principais mecanismos de segurança do mercado, além da integração com terceiros, como Facebook, Google, Apple ou a própria Amazon.

## :computer: Mão no console

Todas as etapas deste projeto foram feitas diretamente no AWS Management Console.

> Todos os nomes `marcados como código` foram dados por mim e, portanto, ficam a critério de quem estiver reproduzindo o projeto.

### 1. Criando uma API REST no API Gateway

- Entrar na tela do API Gateway e clicar em :point_right: **_Create API_**
- Em **Choose an API type** :arrow_right: **_REST API_** :arrow_right: **_Build_**
- Em **Choose the protocol** marque **_REST_** e em **Create new API** selecione **_New API_**
- No campo **API name** inserir `lab-cognito-api`:point_right: **_Next_**
- Em **Endpoint Type** selecione **_Regional_** :point_right: **_Create API_**
- Na tela de resources clique em **_Actions_** :arrow_right: **_Create Resource_** :arrow_right: **Resource name:** `items` :point_right: **_Create resource_**

> :exclamation: O método para este recurso será criado mais adiante.

### 2. Criando a tabela no DynamoDB

- Na tela principal do serviço clicar em :point_right: **_Create Table_**
- Em **Table name** inserir `lab-cognito-table`
- Em **Partition key** inserir a chave `id` e deixar o tipo **_string_**
- Rolar a página até o final e clicar em :point_right: **_Create table_**

### 3. Criando função Lambda

- Dentro do console do Lambda clicar em :point_right: **_Create function_**
- Deixar selecionada a opção **_Author from scratch_** para criar uma função do zero
- Em **Function name** inserir `lab-cognito-putItemFunction`
- Em **Runtime** selecionar **_Node.js_** na versão de sua preferência
- Deixar a opção Architecture em **_x86_64_** :point_right: **_Create function_**
- No editor de código abaixo de **Code source** inserir o conteúdo de _putItemFunction.js_ que está neste repositório
- Depois de colar o código da função clicar em :point_right: **_Deploy_**

> :exclamation: Caso utilize o arquivo `putItemFunction.js` deste repositório não esqueça verificar se precisa ajustar o nome da tabela.

Dentro do recurso criado será necessário configurar o **IAM** para que possa interagir com a tabela criada no **DynamoDB**.

- Clicar em **_Configuration_** :arrow_right: **_Permissions_** e abrir o link da **_Role name_**
- Na tela do **IAM** que abrirá em outra aba, dentro de **_Permissions_** clicar em **_Add inline policy_**
- Em **Visual editor** selecione **_Service_**
- Na caixa de pesquisa digite `dynamodb` e selecione o serviço
- Na caixa de pesquisa de **Actions** digite a ação `putitem` e marque a caixa permitindo a ação
- Logo abaixo em **Resources** deixe marcado **_Specific_** e clique em **_Add ARN_**
- Na tela que abirá em pop-up insira o _ARN_ da tabela criada anteriormente no **DynamoDB** :arrow_right: **_Add_** :arrow_right: **_Review policy_** inserir nome `putitem-policy` :point_right:**_Create policy_**
  > :exclamation: ARN (Amazon Resource Name) é um identificador único de cada recurso criado na AWS.

Agora a função Lambda conseguirá interagir com o banco de dados mas só com a função de inserção de items _(put item)_.

### 4. Integrando o API Gateway com o Lambda

- Voltando a tela do **API Gateway** entre na API criada anteriormente :arrow_right: **_Resources_** :arrow_right: **_/items_** :arrow_right: **_Actions_** :arrow_right: **_Create Method_** :arrow_right: **_POST_** :arrow_right: e confirma :ballot_box_with_check:
  Dentro do método defina os seguintes parâmetros:
- **Integration type** :radio_button: **_Lambda Function_**
- **Use Lambda Proxy integration** :white_check_mark: (Muito importante, não esquecer de marcar)
- **Lambda Region** `us-east-1` (Defina a mesma região que utilizou nos outros recursos)
- **Lambda Function** `lab-cognito-putItemFunction` (O nome da função Lambda)
- **Use Default Timeout** :white_check_mark:
- :arrow_right: **_Save_** :arrow_right: **_OK_**
- Seleciona o método **_POST_** :arrow_right: **_Actions_** :point_right: **_Deploy API_** :arrow_right:
- **Deployment stage** **_[New Stage]_** :arrow_right: **Stage name\*** `dev` :point_right: **_Deploy_**

### 5. Teste no Postman

- **_Add Request_** :arrow_right: **_Method POST_** :arrow_right: _Inserir o endpoint gerado no API Gateway_
- - **_Body_** :arrow_right: **_Raw_** :arrow_right: **_JSON_** :arrow_right: Adicionar o seguinte body

  {
  "id": "005",
  "price": 165,
  "prod": "Câmera Tekpix"
  }

- :point_right: **_Send_**

Após o teste esses dados devem aparecer na tabela do DynamoDB.

### 6. Criando user pool e configurando atributos no Amazon Cognito

A maior parte das configurações envolvendo política de segurança podem variar de acordo com a necessidade e o gosto de quem está configurando. Os meus parâmetros foram os seguintes:

- Na tela inicial do **Cognito** :point_right: **_Create user pool_**
- **Step 1 of 6: Configure sign-in experience**
- **Authentication providers** :point_right: :ballot_box_with_check: **_Cognito user pool_**
- **Cognito user pool sign-in options** :white_check_mark: **_Email_** :point_right: **_Next_**

---

- **Step 2 of 6: Configure security requirements**
- **Password policy** :point_right: :radio_button: **_Custom_**
- **Password minimum length** `8`
- [x] Contains at least 1 number
- [x] Contains at least 1 special character
- [ ] Contains at least 1 uppercase letter
- [x] Contains at least 1 lowercase letter
- **Multi-factor authentication** :point_right: :radio_button: **_No MFA_**
- **User account recovery** :point_right: :ballot_box_with_check: **_Enable self-service account recovery - Recommended_**
- **Delivery method for user account recovery messages** :point_right: :radio_button: **_Email only_** :point_right: **_Next_**

---

- **Step 3 of 6: Configure sign-up experience**
- **Self-service sign-up** :point_right: :ballot_box_with_check: **_Enable self-registration_**
- **Cognito-assisted verification and confirmation** :point_right: :ballot_box_with_check: **_Allow Cognito to automatically send messages to verify and confirm - Recommended_**
- **Attributes to verify** :point_right: :radio_button: **_Send email message, verify email address_**
- **Verifying attribute changes** :point_right: :ballot_box_with_check: **_Keep original attribute value active when an update is pending - Recommended_**
- **Active attribute values when an update is pending** :point_right: :radio_button: **_Email address_**
- :point_right: **_Next_**

---

- **Step 4 of 6: Configure message delivery**
- :point_right: :radio_button: **_Send email with Cognito_**
- **FROM email address** `no-reply@verificationemail.com`
- :point_right: **_Next_**

---

- **Step 5 of 6: Integrate your app**
- :point_right: **User pool name** `lab-cognito-userPool`
- **Hosted authentication pages** :point_right: :ballot_box_with_check: **Use the Cognito Hosted UI**
- **Domain** :point_right: :radio_button: **_Use a Cognito domain_**
- **Cognito domain** `https://lab-bootcamp` .auth.us-east-1.amazoncognito.com
- **Initial app client** :arrow_right: **App type** :point_right: :radio_button: **_Public client_**
- **App client name** `lab-cognito-appClient`
- **Client secret** :point_right: :radio_button: **_Don't generate a client secret_**
- **Allowed callback URLs** :arrow_right: **URL** `https://example.com/`
- **Advanced app client settings** (Deixarei os valores padrão, veja se sua necessidade é outra)
- **OAuth 2.0 grant types:** (marque os tipos abaixo)
- [x] Implicit grant
- [x] Authorization code grant
- **OpenID Connect scopes:** (marque os valores abaixo)
- [x] Email
- [x] OpenID
- **Attribute read and write permissions:**
- **Attribute:** **_name_** :ballot_box_with_check: **_Read_** :ballot_box_with_check: **_Write_**
- :point_right: **_Next_**

---

- **Step 6 of 6: Review and create**
- :point_right: **_Create user pool_**

### 7. Criando request de login no Postman

- **Add request** :arrow_right: **Authorization** :arrow_right: **Type** :point_right: :ballot_box_with_check: **_OAuth 2.0_**
- **Configure New Token** :arrow_right: **Configuration Options**
- **Grant Type** :point_right: **_Implicit_**
- **Callback URL** `https://example.com`
- **Auth URL** `https://lab-bootcamp.auth.us-east-1.amazoncognito.com/login`
- **Client ID** `Cole neste campo o Client ID gerado no Cognito na tela App client information`
- **Scope** `email openid`
- **Client Authentication** `Send clients credentials in body`
- :point_right: **_Get New Access Token_**

<div align="center"> <img src="https://github.com/walterowisk/labProject-api-security-cognito-AWS/blob/main/img/cognito-login.png" width="600">
</div>

- Vai abrir a tela de login/cadastro. Aproveite e faça um cadastro (Sign up) para testar as políticas definidas no Cognito.
- Após cadastrar e fazer a verificação por e-mail faça o login na conta pela mesma tela (só que na opção Sign in).
- Se der tudo certo _(Your registration has been confirmed!)_ a tela do Postman vai exibir a mensagem _Authentication complete_ :point_right: **_Proceed_**
- Após clicar em prosseguir o Postman gerará os tokens necessários para interagir com uma API autenticada.

### 8. Atrelando user pool do Cognito com API Gateway

Agora vamos atrelar o user pool do cognito com a API criada neste projeto.

- Voltando para o console do **API Gateway** clique na API criada anteriormente (lab-cognito-api) :arrow_right: **Authorizers** :arrow_right: **Create New Authorizer**
- **Name** `lab-cognito-authorizer`
- **Type** :point_right: :radio_button: **_Cognito_**
- **Cognito User Pool** :arrow_right: **us-east-1** `lab-cognito-userPool` (selecione o user pool que você acabou de criar no cognito)
- **Token Source** `Authorization`
- :point_right: **_Create_**
- Ainda dentro de **API Gateway** entrar na tela **Resources** :arrow_right: **POST** :arrow_right: **Method Request**
- **Settings** :arrow_right: **Authorization** :arrow_right: :pencil2: `lab-cognito-authorizer` :ballot_box_with_check: (recarregue a página se o autorizador não aparecer de primeira)
- Após setar e salvar somente esta opção é necessário fazer o deploy da API novamente em **Actions** :arrow_right: **Deploy API** :arrow_right: **Deployment stage** `dev` :point_right: **Deploy**

### 9. Request de login com Token ID no Postman

- Copia o Token ID gerado no teste anterior e volta no **POST** request (feito no item 5 deste tutorial)
- Vá para o JSON e faça alguma alteração como no exemplo abaixo

  {
  "id": "010",
  "price": 3200,
  "prod": "iPhone"
  }

- Na opção **Authorization** :arrow_right: **Type** `Bearer Token` :arrow_right: **Token** `cola o Token ID que foi copiado` :point_right: **_Send_**
  ***
  Se a resposta da request for `'Item inserido com sucesso!'` significa que a API aceitou a autenticação do seu usuário criado no Cognito.

Para ter certeza que a API comunicou com o banco entre no DynamoDB e confira se a tabela exibe as informações enviadas acima.

Caso todos os testes tenham ocorrido com sucesso podemos concluir que o Cognito está gerenciando a autenticação e a autorização da nossa aplicação. Essas são apenas algumas funções que este serviço é capaz de prover

A partir desses conceitos podemos utilizar esses recursos em projetos maiores que eventualmente possam entrar em produção.

Para entender mais a fundo leia a [**Documentação do Amazon Cognito**](https://docs.aws.amazon.com/pt_br/cognito/index.html)

> :bulb: Importante: Não esquecer de excluir os recursos criados para evitar cobranças desnecessárias da AWS.
