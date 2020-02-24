# Client: Pasta referente as interfaces visuais e consumo da api
Para construção do frontend foi utilizado o reactjs como framework javascript
utilizado para facilitar o axios e reduces.

## Framework/ Bibliotecas utilizadas:
- Axios: Biblioteca utilizada com objetivo de facilitar o consumo da API criada;
- jwt-decode: Bilitoeca utilizada visando facilitar no trabalho com token;
- node-sass: Utilizado para facilitar ao uso de scss;
- react-router-dom: Utilizado para fazer navegação entre componentes;
- redux: Foi utilizado para facilitar no trabalho de conrtole de estado;

## Visão geral
Foi criado uma tela de login/registro que permite ao usuário autenticar. 
Quando autenticado, o usuário é redirecionado ao dashboard, onde é listado os projetos
criados por este usuário. Caso não tenha, nada é exibido e um botão de opção de criação
de projetos é exibido.
Os projetos são mostrados em pequenos combobox com informações básicas e ao passar o cursos,
é exibido a opção  de editar. Caso clique, assim como na criação, é exibido um modal onde o usuário
pode selecionar as informações para edição ou criação do projeto.
