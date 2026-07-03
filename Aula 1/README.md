# Resumo e Tradução de Texto com Hugging Face Transformers (Transformers >= 5.0)

## Descrição

Este notebook implementa as mesmas duas tarefas de NLP do notebook original — resumo automático de texto e tradução de inglês para francês — porém utilizando diretamente as classes `AutoTokenizer` e `AutoModelForSeq2SeqLM`, sem depender da API `pipeline()`.

## Requisitos

```
pip install transformers
```

Compatível com `transformers >= 5.0`.

## Por que esta versão existe

A partir da versão 5.0 da biblioteca `transformers`, a Hugging Face removeu as tasks `summarization`, `translation` e `text2text-generation` da API `pipeline()`. Chamadas como `pipeline("summarization")` ou `pipeline("translation_en_to_fr")` passaram a lançar `KeyError: Unknown task`, quebrando notebooks e tutoriais escritos antes dessa mudança.

Este notebook substitui a `pipeline()` por uma implementação manual equivalente, carregando o modelo e o tokenizer diretamente e chamando `.generate()`:

```python
from transformers import AutoTokenizer, AutoModelForSeq2SeqLM

tokenizer = AutoTokenizer.from_pretrained("facebook/bart-large-cnn")
modelo = AutoModelForSeq2SeqLM.from_pretrained("facebook/bart-large-cnn")

inputs = tokenizer(texto, return_tensors="pt", max_length=1024, truncation=True)
ids_resumo = modelo.generate(inputs["input_ids"], max_length=120, min_length=70)
resumo = tokenizer.decode(ids_resumo[0], skip_special_tokens=True)
```

Para tradução, o modelo `t5-base` é utilizado com o prefixo de instrução `"translate English to French: "`, conforme o formato de treinamento do T5.

## Modelos utilizados

- Resumo: `facebook/bart-large-cnn`
- Tradução: `t5-base`

## Relação com a versão v4

Existe uma segunda versão deste mesmo notebook, que utiliza a API `pipeline()` e depende de `transformers < 5.0`. Os dois arquivos foram mantidos separadamente para documentar a mudança de API entre versões da biblioteca e permitir a execução do projeto independentemente da versão instalada no ambiente do usuário. Ver `README_v4_pipeline.md` para detalhes.