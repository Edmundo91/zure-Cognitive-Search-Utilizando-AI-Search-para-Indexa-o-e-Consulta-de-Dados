# Azure Cognitive Search: Utilizando AI Search para Indexação e Consulta de Dados

## 📌 Descrição
Este projeto implementa um serviço de busca inteligente utilizando Azure Cognitive Search para indexar e consultar dados de uma base de documentos. A solução permite a realização de buscas eficientes com funcionalidades de IA para análise semântica, filtros e relevância de resultados.

## 🏗 Tecnologias Utilizadas
- Azure AI Search (Cognitive Search)
- Azure Storage Account (para armazenar documentos)
- Azure AI Services (para enriquecimento cognitivo - opcional)
- Python / Flask (para API de busca)
- Postman (para testar requisições)

## 🎯 Objetivos do Projeto
- Criar um serviço de pesquisa no Azure Cognitive Search
- Indexar dados automaticamente no serviço de busca
- Criar uma API para consultar os dados indexados
- Aplicar filtros e relevância para otimizar a experiência de busca

## 🏗 Passo a Passo da Implementação

### 1️⃣ Criando o Serviço no Azure
1. Acesse o [Azure Portal](https://portal.azure.com)
2. Crie um **Azure Cognitive Search Service**
   - Grupo de Recursos: `meu-grupo-recursos`
   - Nome do Serviço: `meu-search-service`
   - Plano: `Free (F0)` (para testes, use um plano gratuito)
3. Anote a **Chave de Administração** e o **Endpoint** do serviço

### 2️⃣ Criando um Índice no Cognitive Search
1. Vá até **Azure Cognitive Search** → **Índices** → **Novo Índice**
2. Defina a estrutura do índice (Exemplo para documentos de artigos):

```json
{
  "name": "artigos-index",
  "fields": [
    { "name": "id", "type": "Edm.String", "key": true },
    { "name": "titulo", "type": "Edm.String", "searchable": true },
    { "name": "conteudo", "type": "Edm.String", "searchable": true },
    { "name": "autor", "type": "Edm.String", "filterable": true },
    { "name": "data_publicacao", "type": "Edm.DateTimeOffset", "sortable": true }
  ]
}
```

## Criando uma API para Consulta

### Instalando as Dependências
Instale as dependências no ambiente Python:

bash pip install flask requests


#### Crie um arquivo app.py com a seguinte implementação:

from flask import Flask, request, jsonify
import requests

app = Flask(__name__)

AZURE_SEARCH_ENDPOINT = "https://meu-search-service.search.windows.net"
AZURE_SEARCH_API_KEY = "SUA_CHAVE_AQUI"
AZURE_SEARCH_INDEX = "artigos-index"

@app.route("/buscar", methods=["GET"])
def buscar():
    termo = request.args.get("q")
    headers = {"Content-Type": "application/json", "api-key": AZURE_SEARCH_API_KEY}
    query = {"search": termo}
    response = requests.post(f"{AZURE_SEARCH_ENDPOINT}/indexes/{AZURE_SEARCH_INDEX}/docs/search?api-version=2021-04-30-Preview", json=query, headers=headers)
    return jsonify(response.json())

if __name__ == "__main__":
    app.run(debug=True)
##### Testando a API no Postman

Inicie a API com o comando:

python app.py

Faça uma requisição GET para:

http://127.0.0.1:5000/buscar?q=pesquisa
