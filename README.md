# HEALTHHELP – README OFICIAL DO PROJETO

**Sistema Integrado de Análise de Rotina e Bem-Estar**

FIAP – Mastering Relational and Non-Relational Database

---

## Descrição do Projeto

O **HealthHelp** é um sistema desenvolvido para promover o bem-estar físico e mental dos usuários através da análise completa da rotina diária.

A solução utiliza **Oracle Database**, **PL/SQL**, **MongoDB** e **JSON** para construir uma plataforma inteligente capaz de:

- Registrar e auditar a rotina do usuário
- Calcular scores de equilíbrio diário
- Gerar recomendações personalizadas
- Exportar dados em JSON
- Integrar análises em banco NoSQL para IA

O projeto está totalmente alinhado com a proposta FIAP e com o tema **Futuro do Trabalho**, reforçando a importância do equilíbrio, produtividade e autocuidado nos cenários modernos.

---

## Arquitetura do Sistema

A arquitetura é dividida em duas camadas principais:

### 1. Banco de Dados Relacional – Oracle

**Responsável por:**

- Integridade e consistência dos dados
- Regras de negócio
- Auditoria automatizada
- Cálculo de pontuação
- Geração de JSON manual

**Estruturas criadas:**

- `USUARIO`
- `CATEGORIA_ATIVIDADE`
- `REGISTRO_DIARIO`
- `ATIVIDADE`
- `HABITO`
- `RECOMENDACAO`
- `AUDIT_LOG`

**Objetos implementados:**

- Triggers de auditoria para todas as tabelas principais
- Package `PKG_WELLNESS` contendo:
  - Geração de JSON
  - Cálculo de score
  - Validação de email
  - Inserção automatizada de dados
  - Exportação de dataset
- Procedures com loops e tratamento de exceção

**Inserção automática de:**

- 30 usuários
- 450 registros diários
- 2.250 atividades
- 30 hábitos
- 30 recomendações
- 2.760 registros de auditoria

### 2. Banco NoSQL – MongoDB

Após a geração do JSON no Oracle, os dados foram enviados para o MongoDB.

**Configuração NoSQL:**

- **Database**: `healthhelp_db`
- **Collection**: `usuarios_rotina`
- **Documentos**: 30 usuários em formato JSON

**Índices criados:**

```javascript
db.usuarios_rotina.createIndex({ email: 1 }, { unique: true })
db.usuarios_rotina.createIndex({ media_score_rotina: -1 })
```

**Consultas Mongo incluídas:**

- Top 5 scores
- Scores críticos
- Média por gênero
- Distribuição de score (bucket)

---

## Fluxo de Funcionamento

1. Dados criados e manipulados no Oracle
2. Geração manual de JSON
3. Exportação consolidada via PL/SQL
4. Importação do JSON no MongoDB
5. Criação de índices e análises NoSQL
6. Dados preparados para IA futura

---

## Tecnologias Utilizadas

- Oracle Database 21c
- PL/SQL (Triggers, Procedures, Packages, Functions)
- Data Modeler
- MongoDB Compass
- MongoImport
- JSON
- Git/GitHub
- Modelo híbrido SQL + NoSQL

---

## Modelagem do Sistema

### Modelagem Relacional (Oracle)

- Normalização
- Relacionamentos 1:N
- Chaves primárias e estrangeiras
- Auditoria automática

### Modelagem NoSQL (MongoDB)

- Documento único por usuário
- Estrutura simples e direta para IA
- Índices para buscas rápidas


---

## JSON Gerado

O Oracle gera dois formatos principais:

### JSON Individual

Usado para retorno do score de um usuário específico.

### JSON Consolidado

Utilizado para exportação ao MongoDB:

```json
{
  "usuarios": [
    {
      "usuario_id": 1,
      "nome": "Usuario 1",
      "email": "usuario01@healthhelp.com",
      "media_score_rotina": 68.5
    }
  ]
}
```

---

## Consultas NoSQL (Aggregation)

### Exemplo – Top 5 usuários

```javascript
db.usuarios_rotina.find(
  {},
  { _id: 0, nome: 1, media_score_rotina: 1 }
).sort({ media_score_rotina: -1 }).limit(5)
```

### Média por gênero

```javascript
db.usuarios_rotina.aggregate([
  { $group: { _id: "$genero", media: { $avg: "$media_score_rotina" } } }
])
```

---

## Auditoria

Todas as operações importantes são registradas pela tabela `AUDIT_LOG`, contendo:

- Nome da tabela
- Operação realizada
- Horário
- Campo alterado
- Usuário do sistema

Mais de 2.700 logs foram gerados automaticamente durante a criação da base.

---

## Evidências (Imagens)

*(Você insere depois)*

- Modelo lógico e físico
- Execução das procedures
- Geração dos JSONs
- Importação no Mongo
- Queries do Mongo
- Índices criados
- Auditoria funcionando

---

## Repositórios

**GitHub do Projeto HealthHelp:**

[https://github.com/Ramalho044/Global-BD.git](https://github.com/Ramalho044/Global-BD.git)

---

## Vídeo de Apresentação

**YouTube:**

[https://youtu.be/A_vERL1FEng](https://youtu.be/A_vERL1FEng)

---

## Equipe

- **Gabriel Lima Silva** – RM 556773
- **Cauã Marcelo Da Silva Machado** – RM 558024
- **Marcos Ramalho** – RM 554611

---

## Conclusão

O **HealthHelp** se destaca como uma solução completa e moderna, integrando:

- Estrutura relacional robusta
- Base NoSQL escalável
- Automação avançada via PL/SQL
- JSON manual e exportação completa
- Dados prontos para Inteligência Artificial

O sistema demonstra domínio total sobre modelagem de dados, arquitetura híbrida SQL + NoSQL, auditoria, automação e integração entre tecnologias.

É um projeto sólido, bem documentado e alinhado às exigências acadêmicas e corporativas.

---

**Desenvolvido com dedicação pela equipe HealthHelp | FIAP 2025**
