# Deck Builder MTG 🃏 

## Introdução
Esta API RESTful foi desenvolvida para permitir a criação, gerenciamento e compartilhamento de decks de cartas do jogo *Magic: The Gathering (MTG)*.

Inspirada em plataformas como [Moxfield](https://moxfield.com) e [Archidekt](https://archidekt.com), a API possibilita:

- Criação de decks personalizados por formato (e.g. Commander, Modern).
- Adição e remoção de cartas nos decks.
- Consulta de cartas, decks públicos e privados.
- Clonagem e exportação de decks.
- Gerenciamento de usuários.

A API segue o padrão OpenAPI e utiliza boas práticas RESTful para facilitar a integração com aplicações web ou mobile.

## Rotas da API

| Método | Rota                              | Descrição                                       | Status Codes        |
|--------|-----------------------------------|-------------------------------------------------|---------------------|
| GET    | /decks                            | Listar todos os decks                           | 200, 500            |
| GET    | /decks/{id}                       | Obter detalhes de um deck                       | 200, 404, 500       |
| POST   | /decks                            | Criar um novo deck                              | 201, 400, 500       |
| PUT    | /decks/{id}                       | Atualizar um deck existente                     | 200, 400, 404, 500  |
| DELETE | /decks/{id}                       | Excluir um deck                                 | 204, 404, 500       |
| POST   | /decks/{id}/cards                 | Adicionar carta a um deck                       | 200, 400, 404, 500  |
| PUT    | /decks/{id}/cards/{cardId}        | Atualizar quantidade de uma carta no deck       | 200, 400, 404, 500  |
| DELETE | /decks/{id}/cards/{cardId}        | Remover carta de um deck                        | 204, 404, 500       |
| GET    | /decks/{id}/cards                 | Listar cartas de um deck                        | 200, 404, 500       |
| GET    | /cards                            | Listar todas as cartas                          | 200, 500            |
| GET    | /cards/{id}                       | Obter detalhes de uma carta                     | 200, 404, 500       |
| GET    | /cards/search?nome={nome}         | Buscar cartas por nome                          | 200, 400, 500       |
| POST   | /users                            | Criar novo usuário                              | 201, 400, 500       |
| GET    | /users/{id}                       | Obter dados de um usuário                       | 200, 404, 500       |
| GET    | /users/{id}/decks                 | Listar decks criados por um usuário             | 200, 404, 500       |
| GET    | /decks/formats                    | Listar formatos válidos de deck (e.g., Commander)| 200, 500            |
| POST   | /decks/{id}/clone                 | Clonar um deck                                  | 201, 404, 500       |
| GET    | /decks/public                     | Listar decks públicos                           | 200, 500            |
| POST   | /decks/{id}/publish               | Tornar deck público                             | 200, 404, 500       |
| GET    | /decks/{id}/export                | Exportar um deck (em JSON ou texto plano)       | 200, 404, 500       |

### 📦 DeckDTO

Representa as informações completas de um deck, incluindo nome, formato, descrição e lista de cartas.
```json
{
  "id": 1,
  "nome": "Dragões Flamejantes",
  "formato": "Commander",
  "descricao": "Deck com foco em criaturas do tipo dragão.",
  "cartas": [
    {
      "id": 101,
      "nome": "Shivan Dragon",
      "tipo": "Criatura",
      "cor": "Vermelho",
      "custoMana": "4RR",
      "quantidade": 2
    },
    {
      "id": 102,
      "nome": "Thoughtseize",
      "tipo": "Feitiço",
      "cor": "Preto",
      "custoMana": "B",
      "quantidade": 4
    }
  ],
  "dataCriacao": "2025-04-20T12:00:00Z"
}
```



### 🧙 CartaDTO

Representa uma carta de Magic: The Gathering, com nome, tipo, cor, custo de mana e descrição.
```json
{
  "id": 101,
  "nome": "Shivan Dragon",
  "tipo": "Criatura",
  "cor": "Vermelho",
  "custoMana": "4RR",
  "descricao": "Voar. {R}: Shivan Dragon ganha +1/+0 até o fim do turno."
}
```

### 👤 UsuarioDTO
Contém as informações de um usuário, incluindo os decks que ele criou.
```json
{
  "id": 1,
  "nome": "João Mage",
  "email": "joao@mtg.com",
  "decks": [
    {
      "id": 1,
      "nome": "Dragões Flamejantes"
    }
  ]
}
```

### 🧪 CloneDeckDTO
Usado para clonar um deck existente, criando uma nova cópia com outro nome.
```json
{
  "deckIdOrigem": 1,
  "nomeNovoDeck": "Dragões Flamejantes v2"
}
```

### 📤 DeckExportadoDTO
Representa um deck exportado no formato texto, usado para compartilhamento.
```json
{
  "id": 1,
  "conteudoTxt": "Commander - Dragões Flamejantes\n4x Shivan Dragon\n2x Dragon's Hoard\n..."
}
```

### 🔐 LoginDTO
Utilizado para autenticar um usuário com e-mail e senha.
```json
{
  "email": "joao@mtg.com",
  "senha": "senhaSegura123"
}
```

### 🌎 DeckPublicoDTO
Representa um deck que foi tornado público, visível para todos os usuários.

```json
{
  "id": 1,
  "nome": "Dragões Flamejantes",
  "criador": "João Mage",
  "formato": "Commander",
  "quantidadeCartas": 100,
  "dataPublicacao": "2025-04-21T15:00:00Z"
}
```

### 🎯 FormatoDTO
Define os formatos disponíveis para construção de decks no sistema.
```json
{
  "id": 1,
  "nome": "Commander",
  "descricao": "Formato de 100 cartas com comandante único."
}
```

### ⚠️ ErroDTO
Utilizado para padronizar o retorno de erros da API.
```json
{
  "erros": [
    {
      "codigo": 400,
      "mensagem": "Dados inválidos.",
      "detalhes": "O campo 'email' não pode ser vazio.",
      "timestamp": "2025-04-19T16:12:00Z"
    },
    {
      "codigo": 401,
      "mensagem": "Não autorizado.",
      "detalhes": "A autenticação falhou ou a sessão expirou.",
      "timestamp": "2025-04-19T16:14:00Z"
    },
    {
      "codigo": 403,
      "mensagem": "Acesso proibido.",
      "detalhes": "Você não tem permissão para acessar este recurso.",
      "timestamp": "2025-04-19T16:16:00Z"
    },
    {
      "codigo": 404,
      "mensagem": "Recurso não encontrado.",
      "detalhes": "O recurso solicitado não existe ou foi removido.",
      "timestamp": "2025-04-19T16:18:00Z"
    },
    {
      "codigo": 500,
      "mensagem": "Erro interno no servidor.",
      "detalhes": "Ocorreu um erro inesperado no servidor. Tente novamente mais tarde.",
      "timestamp": "2025-04-19T16:20:00Z"
    },
    {
      "codigo": 405,
      "mensagem": "Método não permitido.",
      "detalhes": "O método HTTP não é permitido para este recurso.",
      "timestamp": "2025-04-19T16:22:00Z"
    },
    {
      "codigo": 404,
      "mensagem": "Deck não encontrado.",
      "detalhes": "O deck solicitado não existe ou foi removido.",
      "timestamp": "2025-04-19T16:10:00Z"
    }
  ]
}

```

## 📜 Especificação OpenAPI

Toda a documentação detalhada das rotas, parâmetros e exemplos da API está disponível no arquivo [openapi.yaml](./openapi.yaml).

Você pode visualizar e testar a especificação utilizando o [Swagger Editor](https://editor.swagger.io/):

1. Acesse o site do Swagger Editor.
2. Clique em "File" > "Import File".
3. Selecione o arquivo `openapi.yaml` deste projeto.
4. Explore e visualize todas as rotas e esquemas da API.

---

## 🏁 Conclusão

Este projeto foi desenvolvido para demonstrar a criação e documentação de uma API RESTful utilizando o padrão OpenAPI 3.0.

O sistema Deck Builder MTG permite gerenciar decks de cartas do jogo *Magic: The Gathering*, com funcionalidades de criação, clonagem, publicação e exportação de decks, além da gestão de cartas e usuários.


