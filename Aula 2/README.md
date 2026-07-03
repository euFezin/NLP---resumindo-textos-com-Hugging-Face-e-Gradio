# Resumo Automático de Texto com Hugging Face Transformers

## Descrição

Este projeto aplica resumo automático de texto (summarization) sobre um parágrafo em inglês descrevendo o processo de classificação em machine learning, utilizando o modelo `facebook/bart-large-cnn`.

O repositório contém duas versões do mesmo notebook, compatíveis com versões diferentes da biblioteca `transformers`, pois o código original ensinado no curso da Alura, no momento em que foi realizado, não contemplava as mudanças mais recentes da biblioteca.

## Por que existem duas versões

A partir da versão 5.0 da biblioteca `transformers`, a Hugging Face removeu a task `summarization` da API `pipeline()`. Uma chamada como `pipeline("summarization", model=modelo_resumo)`, que antes funcionava em uma única linha, passou a lançar `KeyError: Unknown task, available tasks are [...]` em ambientes com `transformers >= 5.0`.

As duas versões deste notebook foram mantidas lado a lado para que o projeto continue executável independentemente da versão instalada no ambiente do usuário, e para documentar essa mudança de API.

## Comparação entre as versões

| | v4 (`pipeline`) | v5 (`AutoModel`) |
|---|---|---|
| Arquivo | `resumo_classificacao_v4.ipynb` | `resumo_classificacao_v5.ipynb` |
| Versão do `transformers` | `< 5.0` | `>= 5.0` |
| API utilizada | `pipeline("summarization")` | `AutoTokenizer` + `AutoModelForSeq2SeqLM` |
| Linhas de código necessárias | Menor | Maior |
| Controle sobre tokenização e geração | Abstraído pela `pipeline` | Explícito e manual |

## Instalação

```
# v4
pip install "transformers<5.0"

# v5
pip install transformers
```

## Uso — v4 (pipeline)

```python
from transformers import pipeline

modelo_resumo = "facebook/bart-large-cnn"
resumidor_eng = pipeline("summarization", model=modelo_resumo)

resumo = resumidor_eng(texto, max_length=70, min_length=30)
print(resumo[0]['summary_text'])
```

## Uso — v5 (manual, sem pipeline)

```python
from transformers import AutoTokenizer, AutoModelForSeq2SeqLM

modelo_resumo = "facebook/bart-large-cnn"
tokenizer = AutoTokenizer.from_pretrained(modelo_resumo)
modelo = AutoModelForSeq2SeqLM.from_pretrained(modelo_resumo)

inputs = tokenizer(texto, return_tensors="pt", max_length=1024, truncation=True)
ids_resumo = modelo.generate(inputs["input_ids"], max_length=70, min_length=30)
resumo = tokenizer.decode(ids_resumo[0], skip_special_tokens=True)
print(resumo)
```

## Modelo utilizado

- Resumo: `facebook/bart-large-cnn`

## Observação

Ambas as versões produzem o mesmo resultado funcional. A escolha entre uma ou outra depende exclusivamente da versão da biblioteca `transformers` disponível no ambiente de execução.