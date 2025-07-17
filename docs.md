
# Documentação da API do Sistema Estágio Parceiro
#### Esta documentação descreve as rotas da API, os métodos HTTP suportados, os formatos de requisição e as respostas esperadas.

### 1. Módulo de Autenticação (/auth)
#### 1.1. Cadastro de Usuário
Cria um novo usuário com o papel de ESTUDANTE.

```
 POST /auth/cadastro
```
Requisição:

Body (JSON):

```
{
    "email": "novo.usuario@example.com",
    "senha": "senhaSegura123"
}
```
Respostas:

200 OK:
```
{
    "mensagem": "Usuário criado com sucesso",
    "id": 123
}
```
#### 1.2. Confirmação de E-mail
Envia um código de confirmação por e-mail para o usuário.

```
POST: /auth/confirmar-email
```
Requisição:

Body (JSON):

```
{
    "id": 123
}
```
Respostas:

200 OK:
```
{
    "mensagem": "Email enviado com sucesso.",
    "codigo": "123456"
}
```
#### 1.3. Login de Usuário
Autentica um usuário e inicia a sessão.

```
POST /auth/login
```

Requisição:

Body (JSON):

```
{
    "email": "usuario@example.com",
    "senha": "senhaSegura123"
}
```
Respostas:

200 OK:

```
{
    "mensagem": "Login bem-sucedido",
    "user_id": 123,
    "role": "ESTUDANTE"
}
```
401 Unauthorized:

JSON
```
{
    "erro": "Credenciais inválidas"
}
```
#### 1.4. Logout de Usuário
Encerra a sessão do usuário.
```
POST: /auth/logout
```

Requisição: (Nenhum corpo de requisição é necessário)

Respostas:

200 OK:

```
{
    "mensagem": "Logout efetuado com sucesso"
}
```
### 2. Módulo de Administração (/admin)
#### 2.1. Criar Admin
Cria um novo usuário com o papel de ADMIN. Requer autenticação de um usuário ADMIN.
```
POST /admin/criar-admin
```
Autenticação: Necessário estar logado como ADMIN.

Requisição:

Body (JSON):

```
{
    "email": "novo.admin@example.com",
    "senha": "senhaAdminSegura"
}
```
Respostas:

201 Created:

```
{
    "mensagem": "Novo admin criado com sucesso"
}
```
409 Conflict:

JSON
```
{
    "erro": "Email já cadastrado"
}
```
403 Forbidden:

JSON
```
{
    "mensagem": "Acesso negado."
}
```
### 3. Módulo de Estudante (/estudante)
#### 3.1. Cadastrar Dados Complementares de Estudante
Registra informações adicionais para um usuário com o papel de ESTUDANTE.
```
POST /estudante/complementar
```

Requisição:

Body (JSON):

```
{
    "user_id": 123,
    "nome": "João Silva",
    "curriculo_profissional_link": "http://link.para.curriculo.com",
    "telefone": "81999998888",
    "curso": "Engenharia de Software",
    "periodo": "5"
}
```
Respostas:

201 Created:

```
{
    "mensagem": "Cadastro complementar realizado com sucesso"
}
```
400 Bad Request:

```
{
    "erro": "Usuário inválido ou não é estudante"
}
```
409 Conflict:

```
{
    "erro": "Dados complementares já cadastrados"
}
```
#### 3.2. Obter Dados de Estudante
Retorna os dados complementares de um estudante específico.
```
GET: /estudante/{user_id}
```
Parâmetros de Path:

user_id (integer): ID do usuário estudante.

Respostas:

200 OK:

```
{
    "estudante_id": 456,
    "nome": "João Silva",
    "telefone": "81999998888",
    "curso": "Engenharia de Software",
    "periodo": "5",
    "email": "joao.silva@example.com",
    "curriculo_profissional_link": "http://link.para.curriculo.com"
}
```
404 Not Found:

```
{
    "erro": "Dados do estudante não encontrados"
}
```
#### 3.3. Atualizar Dados de Estudante
Atualiza os dados complementares de um estudante. Requer autenticação de um usuário ESTUDANTE.
```
PUT /estudante/{user_id}
```

Autenticação: Necessário estar logado como ESTUDANTE.

Parâmetros de Path:

user_id (integer): ID do usuário estudante.

Requisição:

Body (JSON): (Campos opcionais, envie apenas o que deseja atualizar)

```
{
    "nome": "João Silva Atualizado",
    "telefone": "81988887777"
}
```
Respostas:

200 OK:

```
{
    "mensagem": "Dados atualizados com sucesso"
}
```
404 Not Found:

```
{
    "erro": "Dados do estudante não encontrados"
}
```
403 Forbidden:

```
{
    "mensagem": "Acesso negado"
}
```
#### 3.4. Excluir Dados de Estudante
Exclui os dados complementares de um estudante. Requer autenticação de um usuário ESTUDANTE.
```
DELETE /estudante/{user_id}
```

Autenticação: Necessário estar logado como ESTUDANTE.

Parâmetros de Path:

user_id (integer): ID do usuário estudante.

Respostas:

200 OK:

```
{
    "mensagem": "Dados do estudante excluídos com sucesso"
}
```
404 Not Found:

```
{
    "erro": "Dados do estudante não encontrados"
}
```
403 Forbidden:

```
{
    "mensagem": "Acesso negado"
}
```
#### 3.5. Total de Estudantes Cadastrados
Retorna o número total de estudantes cadastrados.
```
GET /estudante/total-estudantes
```
Respostas:

200 OK:

```
{
    "estudantes": 50
}
```
### 4. Módulo de Vagas (/vaga)
#### 4.1. Criar Vaga
Cria uma nova vaga de estágio. Requer autenticação de um usuário ADMIN.
```
POST /vaga/
```

Autenticação: Necessário estar logado como ADMIN.

Requisição:

Body (JSON):

```
{
    "empresa_id": 1,
    "titulo": "Estágio em Desenvolvimento Web",
    "valor_bolsa": 1200.00,
    "descricao": "Desenvolvimento de aplicações web com Python/Flask.",
    "cursos": "Engenharia de Software, Ciência da Computação"
}
```
Respostas:

201 Created:

```
{
    "mensagem": "Vaga criada com sucesso",
    "id": 101
}
```
403 Forbidden:

```
{
    "mensagem": "Acesso negado. Apenas administradores podem criar vagas."
}
```
404 Not Found:

```
{
    "erro": "Empresa não encontrada"
}
```
400 Bad Request:

```
{
    "erro": "Mensagem de erro da validação ou banco de dados"
}
```
#### 4.2. Listar Todas as Vagas
Retorna uma lista de todas as vagas de estágio cadastradas.
```
GET /vaga/
```

Respostas:

200 OK:

```
[
    {
        "id": 101,
        "titulo": "Estágio em Desenvolvimento Web",
        "valor_bolsa": 1200.00,
        "descricao": "Desenvolvimento de aplicações web com Python/Flask.",
        "cursos": "Engenharia de Software, Ciência da Computação",
        "empresa_id": 1,
        "data_criacao": "2024-07-17T12:00:00",
        "empresa_nome": "Tech Solutions"
    },
    {
        "id": 102,
        "titulo": "Estágio em Marketing Digital",
        "valor_bolsa": 800.00,
        "descricao": "Criação de conteúdo e gestão de redes sociais.",
        "cursos": "Marketing, Publicidade",
        "empresa_id": 2,
        "data_criacao": "2024-07-16T10:30:00",
        "empresa_nome": "Marketing Criativo"
    }
]
```
#### 4.3. Obter Vaga por ID
Retorna os detalhes de uma vaga de estágio específica.
```
GET /vaga/{vaga_id}
```

Parâmetros de Path:

vaga_id (integer): ID da vaga.

Respostas:

200 OK:

```
{
    "id": 101,
    "titulo": "Estágio em Desenvolvimento Web",
    "valor_bolsa": 1200.00,
    "descricao": "Desenvolvimento de aplicações web com Python/Flask.",
    "cursos": "Engenharia de Software, Ciência da Computação",
    "empresa_id": 1,
    "data_criacao": "2024-07-17T12:00:00",
    "empresa_nome": "Tech Solutions"
}
```
404 Not Found:

```
{
    "erro": "Vaga não encontrada"
}
```
#### 4.4. Atualizar Vaga
Atualiza os detalhes de uma vaga de estágio. Requer autenticação de um usuário ADMIN.
```
PUT /vaga/{vaga_id}
```

Autenticação: Necessário estar logado como ADMIN.

Parâmetros de Path:

vaga_id (integer): ID da vaga.

Requisição:

Body (JSON): (Campos opcionais, envie apenas o que deseja atualizar)

```
{
    "titulo": "Estágio em Desenvolvimento Backend",
    "valor_bolsa": 1500.00
}
```
Respostas:

200 OK:

```
{
    "mensagem": "Vaga atualizada com sucesso"
}
```
403 Forbidden:

```
{
    "mensagem": "Acesso negado. Apenas administradores podem atualizar vagas."
}
```
404 Not Found:

```
{
    "erro": "Vaga não encontrada"
}
```
400 Bad Request:

```
{
    "erro": "Mensagem de erro da validação ou banco de dados"
}
```
#### 4.5. Deletar Vaga
Deleta uma vaga de estágio. Requer autenticação de um usuário ADMIN.
```
DELETE /vaga/{vaga_id}
```

Autenticação: Necessário estar logado como ADMIN.

Parâmetros de Path:

vaga_id (integer): ID da vaga.

Respostas:

200 OK:

```
{
    "mensagem": "Vaga deletada com sucesso"
}
```
403 Forbidden:

```
{
    "mensagem": "Acesso negado. Apenas administradores podem deletar vagas."
}
```
404 Not Found:

```
{
    "erro": "Vaga não encontrada"
}
```
400 Bad Request:

```
{
    "erro": "Mensagem de erro do banco de dados"
}
```
#### 4.6. Total de Vagas Cadastradas
Retorna o número total de vagas de estágio cadastradas.
```
GET /vaga/total-vagas
```
Respostas:

200 OK:

```
{
    "vagas": 25
}
```
### 5. Módulo de Candidaturas (/candidatura)
#### 5.1. Criar Candidatura
Um estudante se candidata a uma vaga.
```
POST /candidatura/
```
(OPTIONS também é aceito para CORS)

Requisição:

Body (JSON):

```
{
    "estudante_id": 1,
    "vaga_id": 101
}
```
Respostas:

201 Created:

```
{
    "mensagem": "Candidatura realizada com sucesso. E-mail enviado à empresa."
}
```
404 Not Found:

```
{
    "erro": "Estudante ou vaga não encontrada (None, None)"
}
```
409 Conflict:

```
{
    "erro": "Estudante já se candidatou a essa vaga"
}
```
500 Internal Server Error:

```
{
    "mensagem": "Candidatura criada, mas o envio do e-mail para a empresa falhou.",
    "erro": "Detalhes do erro do servidor de e-mail"
}
```
#### 5.2. Listar Candidaturas de um Estudante
Retorna as vagas para as quais um estudante se candidatou.
```
GET /candidatura/estudante/{estudante_id}
```

Parâmetros de Path:

estudante_id (integer): ID do estudante.

Respostas:

200 OK:

```
[
    {
        "vaga_id": 101,
        "titulo": "Estágio em Desenvolvimento Web",
        "descricao": "Desenvolvimento de aplicações web com Python/Flask.",
        "valor_bolsa": 1200.00
    },
    {
        "vaga_id": 105,
        "titulo": "Estágio em Redes",
        "descricao": "Manutenção e configuração de redes.",
        "valor_bolsa": 900.00
    }
]
```
404 Not Found:

```
{
    "erro": "Estudante não encontrado"
}
```
#### 5.3. Listar Candidatos para uma Vaga
Retorna a lista de estudantes que se candidataram a uma vaga específica.
```
GET /candidatura/vaga/{vaga_id}
```

Parâmetros de Path:

vaga_id (integer): ID da vaga.

Respostas:

200 OK:

```
[
    {
        "estudante_id": 1,
        "nome": "João Silva",
        "curso": "Engenharia de Software",
        "email": "joao.silva@example.com"
    },
    {
        "estudante_id": 2,
        "nome": "Maria Souza",
        "curso": "Ciência da Computação",
        "email": "maria.souza@example.com"
    }
]
```
404 Not Found:

```
{
    "erro": "Vaga não encontrada"
}
```
#### 5.4. Remover Candidatura
Remove a candidatura de um estudante para uma vaga.
```
DELETE /candidatura/
```

Requisição:

Body (JSON):
```
{
    "estudante_id": 1,
    "vaga_id": 101
}
```
Respostas:

200 OK:

```
{
    "mensagem": "Candidatura removida com sucesso"
}
```
404 Not Found:

```
{
    "erro": "Estudante ou vaga não encontrada"
}
```
400 Bad Request:

```
{
    "erro": "Candidatura não encontrada"
}
```
### 6. Módulo de Empresas (/empresa)
#### 6.1. Cadastro de Empresa
Cadastra uma nova empresa na plataforma. Requer autenticação de um usuário ADMIN.
```
POST /empresa/cadastro
```

Autenticação: Necessário estar logado como ADMIN.

Requisição:

Body (JSON):

```
{
    "cnpj": "12.345.678/0001-90",
    "endereco": "Rua Exemplo, 123",
    "nome": "Minha Grande Empresa",
    "descricao": "Empresa de tecnologia.",
    "telefone": "81911112222",
    "email": "contato@empresa.com"
}
```
Respostas:

201 Created:

```
{
    "mensagem": "Empresa criada com sucesso",
    "id": 1
}
```
403 Forbidden:

```
{
    "mensagem": "Acesso negado."
}
```
409 Conflict:

```
{
    "erro": "CNPJ já cadastrado"
}
```
Ou

```
{
    "erro": "Email já cadastrado"
}
```
#### 6.2. Obter Dados da Empresa por ID
Retorna os detalhes de uma empresa específica.
```
GET /empresa/{id}
```

Parâmetros de Path:

id (integer): ID da empresa.

Respostas:

200 OK:

```
{
    "id": 1,
    "nome": "Minha Grande Empresa",
    "CNPJ": "12.345.678/0001-90",
    "endereco": "Rua Exemplo, 123",
    "descricao": "Empresa de tecnologia.",
    "telefone": "81911112222",
    "email": "contato@empresa.com"
}
```
404 Not Found:

```
{
    "erro": "Empresa não encontrada"
}
```
#### 6.3. Buscar Empresa por Nome
Retorna os detalhes de uma empresa buscando pelo nome (correspondência parcial e insensível a maiúsculas/minúsculas).
```
GET /empresa/buscar
```

Parâmetros de Query:

nome (string): O nome ou parte do nome da empresa.

Respostas:

200 OK:

```
{
    "id": 1,
    "nome": "Minha Grande Empresa",
    "CNPJ": "12.345.678/0001-90",
    "endereco": "Rua Exemplo, 123",
    "descricao": "Empresa de tecnologia.",
    "telefone": "81911112222",
    "email": "contato@empresa.com"
}
```
400 Bad Request:

```
{
    "erro": "Informe o nome da empresa"
}
```
404 Not Found:

```
{
    "erro": "Empresa não encontrada"
}
```
#### 6.4. Atualizar Dados da Empresa
Atualiza os detalhes de uma empresa. Requer autenticação de um usuário ADMIN.
```
PUT /empresa/{id}
```

Autenticação: Necessário estar logado como ADMIN.

Parâmetros de Path:

id (integer): ID da empresa.

Requisição:

Body (JSON): (Campos opcionais, envie apenas o que deseja atualizar)


```
{
    "endereco": "Nova Rua, 456",
    "telefone": "81933334444"
}
```
Respostas:

200 OK:

```
{
    "mensagem": "Empresa atualizada com sucesso"
}
```
403 Forbidden:

```
{
    "mensagem": "Acesso negado."
}
```
404 Not Found:

```
{
    "erro": "Empresa não encontrada"
}
```
####6.5. Deletar Empresa
Deleta uma empresa. Requer autenticação de um usuário ADMIN.
```
DELETE /empresa/{id}
```

Autenticação: Necessário estar logado como ADMIN.

Parâmetros de Path:

id (integer): ID da empresa.

Respostas:

200 OK:

```
{
    "mensagem": "Empresa deletada com sucesso"
}
```
403 Forbidden:

```
{
    "mensagem": "Acesso negado."
}
```
404 Not Found:

```
{
    "erro": "Empresa não encontrada"
}
```
#### 6.6. Total de Empresas Cadastradas
Retorna o número total de empresas cadastradas.
```
GET /empresa/total-empresas
```

Respostas:

200 OK:

```
{
    "empresas": 10
}
```
#### 6.7. Listar Nomes de Empresas
Retorna uma lista de todas as empresas com seus respectivos IDs e nomes.
```
GET /empresa/nome-empresas
```

Respostas:

200 OK:

```
[
    {
        "id": 1,
        "nome": "Minha Grande Empresa"
    },
    {
        "id": 2,
        "nome": "Inovação Tech"
    }
]
```