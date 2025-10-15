# Ambiente Virtual de Aprendizado (AVA) AKCIT

Projeto desenvolvido como trabalhao para a disciplina de Design de Software 2025/2 do bacharelado en Engenharia de Software da UFG.

## MVP

O documento de [mvp](/docs/MVP.pdf) detalha o Mínimo Produto Viável para a implementação das funcionalidades básicas da plataforma que se destina ao compartilhamento de microcursos.

## Configuração e Execução do Projeto (Poetry + Django)
Este projeto utiliza o Poetry para gerenciar dependências e ambientes virtuais.

### Pré-requisitos
Python >=3.13 e o Poetry.

### 1. Configuração do Ambiente

O Poetry gerenciará automaticamente o ambiente virtual do projeto, isolando todas as dependências do sistema.

#### Clonar o Repositório e Instalar Dependências

Clone o repositório do projeto:

```bash
git clone https://github.com/gbrit0/ava_akcit.git
cd ava_akcit
```
Instale todas as dependências do projeto (incluindo o Django) com base no arquivo poetry.lock:

```bash
poetry install
```
Isto irá criar um ambiente virtual isolado para o projeto e instalar todos os pacotes necessários.

### 2. Execução de Comandos (Desenvolvimento)

Para garantir que todos os comandos (como o manage.py do Django) sejam executados dentro do ambiente virtual correto, use o prefixo poetry run.

Use o prefixo em cada comando do Django:

|Ação	| Comando |
|-------|---------|
|Criar Migrações	| poetry run python manage.py makemigrations|
|Aplicar Migrações	| poetry run python manage.py migrate |
|Criar Superusuário	| poetry run python manage.py createsuperuser |
|Rodar o Servidor	| poetry run python manage.py runserver |

### 3. Gerenciamento de Dependências

Para adicionar ou remover novas bibliotecas de forma segura, utilize os comandos do Poetry.

| Ação	| Comando	| Observação |
|-------|-----------|------------|
| Adicionar uma Dependência |	poetry add <pacote>	| Adiciona ao pyproject.toml e instala imediatamente.|
Adicionar Dependência de DEV	| poetry add --dev <pacote> |	Para ferramentas como pytest ou flake8.|
| Remover uma Dependência	| poetry remove <pacote>	| Remove do ambiente e do pyproject.toml.|
|Atualizar o Lockfile	| poetry update |	Força a atualização de todos os pacotes para suas últimas versões compatíveis. |

### 4. Implantação (Deployment)

Para ambientes de produção (como Docker, CI/CD, ou máquinas virtuais):

1. O arquivo pyproject.toml declara as dependências do projeto.

2. O arquivo poetry.lock garante a exata reprodutibilidade da instalação.

Em seu Dockerfile ou script de build, use os seguintes passos para uma implantação otimizada:

```Dockerfile
# Instala o Poetry globalmente
RUN pip install poetry

# Copia os arquivos de configuração
COPY pyproject.toml poetry.lock /app/

# Instala as dependências, confiando no lockfile para velocidade e consistência
RUN poetry install --no-root --only main
````

A flag --only main garante que apenas as dependências de produção (e não as de desenvolvimento) sejam instaladas no ambiente de deploy.