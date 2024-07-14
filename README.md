# Criando-Uma-API-Com-FastAPI-Utilizando-TDD

Este projeto tem como objetivo desenvolver uma API de loja utilizando FastAPI e MongoDB, aplicando princípios de Test Driven Development (TDD) com Pytest.

## Desafios Propostos

### Mapeamento de Exceção de Erro de Inserção e Captura no Controlador

Para lidar com erros de inserção no MongoDB e capturá-los no controlador:

```python
from fastapi import HTTPException
from pymongo.errors import PyMongoError

def inserir_produto(produto: dict):
    try:
        # Lógica de inserção no MongoDB
        collection.insert_one(produto)
    except PyMongoError as e:
        raise HTTPException(status_code=500, detail="Erro ao inserir produto no banco de dados")
```
### Atualização com Exceção Not Found e Atualização de updated_at
Modificar o método PATCH para retornar exceção 404 se o produto não for encontrado e garantir que updated_at seja atualizado:
```python
from datetime import datetime

def atualizar_produto(produto_id: str, dados_atualizados: dict):
    produto = collection.find_one_and_update(
        {"_id": produto_id},
        {"$set": {**dados_atualizados, "updated_at": datetime.utcnow()}},
        return_document=True
    )
    if not produto:
        raise HTTPException(status_code=404, detail="Produto não encontrado")
```
### Aplicação de Filtro de Preço
Aplicar filtro de preço utilizando query parameters no endpoint:
```python
from fastapi import Query

@app.get("/produtos/")
def listar_produtos_por_preco(min_price: float = Query(None, ge=0), max_price: float = Query(None, le=10000)):
    produtos = collection.find({"price": {"$gt": min_price, "$lt": max_price}})
    return produtos
```
### Estrutura do Projeto
main.py: Contém os endpoints da API.
utils.py: Funções utilitárias para operações no MongoDB.
tests/: Diretório contendo os testes unitários com Pytest.
requirements.txt: Lista de dependências do projeto.
README.md: Documentação do projeto com instruções e detalhes dos desafios.
Configuração do Ambiente
Pré-requisitos
Python 3.7+
Pyenv
Poetry
Instalação das Dependências

```bash
poetry install
```

### Execução dos Testes
Para executar os testes unitários:
```bash
pytest
```

### Contribuição
Contribuições são bem-vindas! Sinta-se à vontade para sugerir melhorias ou resolver issues pendentes.

