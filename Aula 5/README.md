# Tradutor e Resumidor de Texto (Gradio + Hugging Face)

Aplicação em Gradio que traduz e resume textos usando os modelos `facebook/nllb-200-distilled-600M` (tradução multilíngue) e `facebook/bart-large-cnn` (sumarização em inglês).

Este repositório mantém **duas versões** do mesmo projeto, compatíveis com gerações diferentes da biblioteca `transformers`.

## Por que duas versões?

A partir do Transformers **v5.0**, a Hugging Face removeu as pipelines de alto nível `pipeline("translation", ...)` e `pipeline("summarization", ...)` (classes `TranslationPipeline` e `SummarizationPipeline`), recomendando o uso de modelos de chat via `text-generation` para essas tarefas. Como este projeto depende especificamente dos modelos seq2seq NLLB-200 e BART-large-CNN — que não são modelos de chat — a migração correta é substituir as pipelines por chamadas manuais com `AutoTokenizer` + `AutoModelForSeq2SeqLM`, e não trocar de modelo.

Manter as duas versões lado a lado serve para:
- Documentar a migração real de uma base de código de v4 para v5, útil como referência para quem enfrenta o mesmo problema;
- Garantir que o projeto continue rodando em ambientes que ainda fixam `transformers<5.0` (comum em bibliotecas legadas e cursos);
- Servir de material de portfólio mostrando adaptação a mudanças breaking de uma biblioteca amplamente usada em produção.

## Objetivo da aplicação

O usuário insere um texto e escolhe, via dropdown, o idioma de origem e o idioma de destino entre os ~200 suportados pelo NLLB-200. A partir disso:

1. Se o idioma de origem for inglês, o texto é resumido diretamente com BART.
2. Se não for, o texto é primeiro traduzido para inglês com NLLB, depois resumido com BART.
3. O resumo (sempre em inglês nesse ponto) é traduzido para o idioma de destino escolhido.
4. O resultado é exibido na interface Gradio.

A Aula 5 reutiliza integralmente essa lógica (função `traduzir_resumir_texto`, dicionário de idiomas, modelos) e só troca a camada de interface: em vez do `gr.Interface` simples, monta a mesma aplicação com `gr.Blocks()` (layout em colunas, botão de ação) e adiciona suporte a customização de tema via `gr.themes.builder()`. Por isso não há uma versão v4/v5 separada para a Aula 5 — as adaptações de Transformers já foram feitas na base compartilhada.

## Versão v4 (`app_v4.ipynb`) — compatível com `transformers < 5.0`

- Usa `from transformers import pipeline`.
- Tradução: `pipeline('translation', model=..., src_lang=..., tgt_lang=...)`.
- Sumarização: `pipeline('summarization', model=...)`.
- Cria uma nova instância de pipeline a cada chamada da função (menos eficiente, mas era o padrão do curso).

## Versão v5 (`app_v5.ipynb`) — compatível com `transformers >= 5.0`

- Usa `AutoTokenizer` e `AutoModelForSeq2SeqLM`, carregados uma única vez fora das funções.
- Tradução feita manualmente: tokeniza o texto, define `forced_bos_token_id` com o código do idioma de destino, e chama `modelo.generate(...)`.
- Sumarização feita da mesma forma, sem o parâmetro de task string que não existe mais.
- Corrige um bug presente na versão original: a lista `entradas` do Gradio estava sem vírgulas entre os componentes (`SyntaxError`), e o resumo era gerado sempre a partir do texto original em vez do texto já traduzido para inglês quando o idioma de origem não era inglês.

## Requisitos

**v4:**
```
transformers<5.0
gradio
torch
```

**v5:**
```
transformers>=5.0
gradio
torch
```

## Como executar

```bash
pip install -r requirements.txt
jupyter notebook app_v4.ipynb   # ou app_v5.ipynb
```