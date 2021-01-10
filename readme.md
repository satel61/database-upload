# DESAFIO 6
## Sobre o desafio

Nesse desafio, você deve continuar desenvolvendo a aplicação de gestão de transações, treinando o que você aprendeu até agora no Node.js junto ao TypeScript, mas dessa vez incluindo o uso de banco de dados com o TypeORM e envio de arquivos com o Multer!

Essa será uma aplicação que deve armazenar transações financeiras de entrada e saída e permitir o cadastro e a listagem dessas transações, além de permitir a criação de novos registros no banco de dados a partir do envio de um arquivo csv.


## Rotas da aplicação

__POST /transactions:__ A rota deve receber title, value, type, e category dentro do corpo da requisição, sendo o type o tipo da transação, que deve ser income para entradas (depósitos) e outcome para saídas (retiradas). Ao cadastrar uma nova transação, ela deve ser armazenada dentro do seu banco de dados, possuindo os campos id, title, value, type, category_id, created_at, updated_at.
Dica: Para a categoria, você deve criar uma nova tabela, que terá os campos id, title, created_at, updated_at.

Dica 2: Antes de criar uma nova categoria, sempre verifique se já existe uma categoria com o mesmo título. Caso ela exista, use o id já existente no banco de dados.

{
  "id": "uuid",
  "title": "Salário",
  "value": 3000,
  "type": "income",
  "category": "Alimentação"
}
__GET /transactions:__ Essa rota deve retornar uma listagem com todas as transações que você cadastrou até agora, junto com o valor da soma de entradas, retiradas e total de crédito. Essa rota deve retornar um objeto o seguinte formato:
{
  "transactions": [
    {
      "id": "uuid",
      "title": "Salário",
      "value": 4000,
      "type": "income",
      "category": {
        "id": "uuid",
        "title": "Salary",
        "created_at": "2020-04-20T00:00:49.620Z",
        "updated_at": "2020-04-20T00:00:49.620Z"
      },
      "created_at": "2020-04-20T00:00:49.620Z",
      "updated_at": "2020-04-20T00:00:49.620Z"
    },
    {
      "id": "uuid",
      "title": "Freela",
      "value": 2000,
      "type": "income",
      "category": {
        "id": "uuid",
        "title": "Others",
        "created_at": "2020-04-20T00:00:49.620Z",
        "updated_at": "2020-04-20T00:00:49.620Z"
      },
      "created_at": "2020-04-20T00:00:49.620Z",
      "updated_at": "2020-04-20T00:00:49.620Z"
    },
    {
      "id": "uuid",
      "title": "Pagamento da fatura",
      "value": 4000,
      "type": "outcome",
      "category": {
        "id": "uuid",
        "title": "Others",
        "created_at": "2020-04-20T00:00:49.620Z",
        "updated_at": "2020-04-20T00:00:49.620Z"
      },
      "created_at": "2020-04-20T00:00:49.620Z",
      "updated_at": "2020-04-20T00:00:49.620Z"
    },
    {
      "id": "uuid",
      "title": "Cadeira Gamer",
      "value": 1200,
      "type": "outcome",
      "category": {
        "id": "uuid",
        "title": "Recreation",
        "created_at": "2020-04-20T00:00:49.620Z",
        "updated_at": "2020-04-20T00:00:49.620Z"
      },
      "created_at": "2020-04-20T00:00:49.620Z",
      "updated_at": "2020-04-20T00:00:49.620Z"
    }
  ],
  "balance": {
    "income": 6000,
    "outcome": 5200,
    "total": 800
  }
}
Dica: Dentro de balance, o income é a soma de todos os valores das transações com type income. O outcome é a soma de todos os valores das transações com type outcome, e o total é o valor de income - outcome.

Dica 2: Para fazer a soma dos valores, você pode usar a função reduce para agrupar as transações pela propriedade type, assim você irá conseguir somar todos os valores com facilidade e obter o retorno do balance.

__DELETE /transactions/:id:__ A rota deve deletar uma transação com o id presente nos parâmetros da rota;

__POST /transactions/import:__ A rota deve permitir a importação de um arquivo com formato .csv contendo as mesmas informações necessárias para criação de uma transação id, title, value, type, category_id, created_at, updated_at, onde cada linha do arquivo CSV deve ser um novo registro para o banco de dados, e por fim retorne todas as transactions que foram importadas para seu banco de dados. O arquivo csv, deve seguir o seguinte modelo
Específicação dos testes
Em cada teste, tem uma breve descrição no que sua aplicação deve cumprir para que o teste passe.

Para esse desafio, temos os seguintes testes:

__warning__ Antes de rodar os testes, crie um banco de dados com o nome "gostack_desafio06_tests" para que todos os testes possam executar corretamente

__should be able to create a new transaction:__ Para que esse teste passe, sua aplicação deve permitir que uma transação seja criada, e retorne um json com a transação criado.

__should create tags when inserting new transactions:__ Para que esse teste passe, sua aplicação deve permitir que ao criar uma nova transação com uma categoria que não existe, essa seja criada e inserida no campo category_id da transação com o id que acabou de ser criado.

__should not create tags when they already exists:__ Para que esse teste passe, sua aplicação deve permitir que ao criar uma nova transação com uma categoria que já existe, seja atribuído ao campo category_id da transação com o id dessa categoria existente, não permitindo a criação de categorias com o mesmo title.

__should be able to list the transactions:__ Para que esse teste passe, sua aplicação deve permitir que seja retornado um array de objetos contendo todas as transações junto ao balanço de income, outcome e total das transações que foram criadas até o momento.

__should not be able to create outcome transaction without a valid balance:__ Para que esse teste passe, sua aplicação não deve permitir que uma transação do tipo outcome extrapole o valor total que o usuário tem em caixa (total de income), retornando uma resposta com código HTTP 400 e uma mensagem de erro no seguinte formato: { error: string }.

__should be able to delete a transaction:__ Para que esse teste passe, você deve permitir que a sua rota de delete exclua uma transação, e ao fazer a exclusão, ele retorne uma resposta vazia, com status 204.

__should be able to import transactions:__ Para que esse teste passe, sua aplicação deve permitir que seja importado um arquivo csv, contendo o seguinte modelo. Com o arquivo importado, você deve permitir que seja criado no banco de dados todos os registros e categorias que estavam presentes nesse arquivo, e retornar todas as transactions que foram importadas.

## IMPORTANDO ARQUIVOS CSV COM NODE.JS
https://www.notion.so/Importando-arquivos-CSV-com-Node-js-2172338480cb47e28a5d3ed9981c38a0
