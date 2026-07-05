# NLP: Resumindo Textos com Hugging Face e Gradio

Projeto desenvolvido a partir do curso de Processamento de Linguagem Natural (PLN/NLP) da Alura, explorando a biblioteca **Transformers** da Hugging Face para tarefas de tradução automática e sumarização de texto, com interfaces interativas construídas em **Gradio**.

O repositório é organizado por aula, evoluindo progressivamente de scripts simples de linha de comando até uma aplicação web completa com interface customizada.

## Estrutura do repositório

| Pasta | Conteúdo |
|---|---|
| `Aula 1/` | Introdução: resumo de texto e tradução inglês→francês com `bart-large-cnn` e `t5-base` |
| `Aula 2/` | Resumo automático de texto com `bart-large-cnn` |
| `Aula 3/` | Combinação de tradução (`nllb-200-distilled-600M`) e resumo (`bart-large-cnn`) em um único fluxo |
| `Aula 4/` | Primeira aplicação web com Gradio (`gr.Interface`), permitindo escolher idioma de origem/destino entre ~200 idiomas |
| `Aula 5/` | Reestilização da aplicação da Aula 4 usando `gr.Blocks()` (layout customizado) e suporte a temas via `gr.themes.builder()` |

Cada pasta tem seu próprio README com detalhes específicos daquela etapa.

## Por que cada aula tem dois notebooks

O código original do curso foi escrito quando a API `pipeline()` da Hugging Face ainda suportava as tasks `translation`, `summarization` e `text2text-generation` diretamente (`transformers < 5.0`). A partir da versão **5.0**, essas tasks de alto nível foram removidas da `pipeline()`, e chamadas como `pipeline("summarization")` passaram a lançar `KeyError`.

Para manter o projeto funcional em qualquer ambiente e ao mesmo tempo documentar essa migração, cada aula mantém duas versões:

- **Versão original**, usando `pipeline()` — compatível com `transformers < 5.0`;
- **Versão adaptada** (sufixo `tfm5`), usando `AutoTokenizer` + `AutoModelForSeq2SeqLM` com `.generate()` manual — compatível com `transformers >= 5.0`.

A partir da Aula 4, a lógica de tradução/resumo é reaproveitada nas aulas seguintes; a Aula 5, por exemplo, reutiliza integralmente o back-end da Aula 4 e só substitui a camada de interface, por isso não introduz uma nova dualidade v4/v5 própria.

## Modelos utilizados

- **Tradução:** `facebook/nllb-200-distilled-600M` (Aulas 3–5) e `t5-base` (Aula 1)
- **Resumo:** `facebook/bart-large-cnn` (todas as aulas)

## Tecnologias

- Python
- Hugging Face Transformers
- PyTorch
- Gradio
- Jupyter Notebook

## Como executar

Cada aula é independente. Entre na pasta desejada e instale as dependências compatíveis com a versão do notebook escolhido:

```bash
# Para os notebooks originais (pipeline)
pip install "transformers<5.0" torch gradio sentencepiece accelerate

# Para os notebooks adaptados (sufixo tfm5)
pip install "transformers>=5.0" torch gradio sentencepiece accelerate

jupyter notebook
```

Consulte o README de cada pasta para detalhes de uso, modelos específicos e observações sobre correções de bugs feitas durante a adaptação.