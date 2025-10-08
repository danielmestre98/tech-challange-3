# Tech Challenge – Fase 03 · Fine-tuning em AmazonTitles-1.3MM

Projeto desenvolvido para o **Tech Challenge – Fase 03** do curso *IA para Devs (Pós Tech)*.  
O objetivo é realizar o **fine-tuning de um foundation model** (como BERT, Llama, Mistral, etc.) utilizando o dataset **AmazonTitles-1.3MM**, para que o modelo seja capaz de **gerar descrições de produtos** a partir de **perguntas sobre seus títulos**.

---

##  Objetivos do Projeto

- Aplicar técnicas de *fine-tuning* em um modelo pré-treinado.
- Utilizar o dataset `trn.json` (do AmazonTitles-1.3MM), contendo campos `title` e `content`.
- Demonstrar o modelo respondendo perguntas com base no contexto aprendido.
- Documentar o processo completo de treinamento, teste e avaliação.

---

##  Estrutura do Projeto

```
.
├─ notebooks/
│  ├─ treino.ipynb        # Fine-tuning do modelo
│  └─ teste.ipynb         # Avaliação e geração de respostas
├─ data/
│  └─ raw/                # Contém o arquivo trn.json
├─ models/
│  └─ <checkpoint>/       # Pesos salvos após o treinamento
├─ outputs/
│  ├─ logs/               # Logs de treino
│  └─ samples/            # Respostas geradas pelo modelo
├─ requirements.txt
└─ README.md
```

---

##  Pré-requisitos

- Python 3.10+
- pip ou conda
- (Opcional) GPU com CUDA 11.8+ e PyTorch compatível

### Bibliotecas utilizadas
```
torch
transformers
accelerate
datasets
peft
evaluate
scikit-learn
pandas
numpy
jupyter
python-dotenv
```

---

##  Configuração do Ambiente

### Clonar o repositório e criar ambiente virtual
```bash
git clone  https://github.com/danielmestre98/tech-challange-3
cd tech-challenge-3

python -m venv .venv
source .venv/bin/activate   # (Windows: .venv\Scripts\activate)
```

### Instalar dependências
```bash
pip install -r requirements.txt
```

Se preferir, instale manualmente:
```bash
pip install torch transformers accelerate datasets peft evaluate scikit-learn pandas numpy jupyter
```

---

## Dataset

Baixe o dataset **AmazonTitles-1.3MM** (link indicado no desafio) e coloque o arquivo `trn.json` dentro de:

```
data/raw/trn.json
```

O arquivo contém:
- `title` → título do produto (entrada)
- `content` → descrição do produto (saída esperada)

Durante o pré-processamento, os prompts são formatados como:

```
Pergunta: Qual é a descrição do produto "TÍTULO"?
Resposta: DESCRIÇÃO_DO_PRODUTO
```

---

## Execução

### Etapa 1 – Treinamento
Abra o notebook `treino.ipynb` e execute todas as células para:

1. Carregar e limpar o dataset.
2. Tokenizar os textos.
3. Configurar e rodar o fine-tuning.
4. Salvar o modelo em `models/<checkpoint>`.

### Etapa 2 – Teste e Inferência
Abra o notebook `teste.ipynb` e rode:

- O carregamento do modelo treinado.
- Exemplos de perguntas para verificar as respostas.
- Comparações antes/depois do fine-tuning.

Exemplo:
```python
Usuário: "Qual é a descrição do produto 'Echo Dot (4ª geração)'?"
Modelo: "O Echo Dot (4ª geração) é um alto-falante inteligente com Alexa integrado..."
```

---

## Métricas e Avaliação

- Avaliação qualitativa (clareza e coerência das respostas).
- Métricas quantitativas opcionais: **ROUGE**, **BLEU**, **METEOR**.
- Comparação de desempenho antes e depois do treinamento.

---

## Execução via Terminal (Opcional)

Você pode usar o Papermill para automatizar a execução dos notebooks:

```bash
pip install papermill

# Rodar treinamento
papermill notebooks/treino.ipynb notebooks/treino.out.ipynb   -p data_path "data/raw/trn.json" -p output_dir "models/checkpoint-01"

# Rodar teste
papermill notebooks/teste.ipynb notebooks/teste.out.ipynb   -p model_dir "models/checkpoint-01" -p samples_out "outputs/samples"
```

