# Tradução e Resumo de Textos com Transformers

## Sobre o projeto

Este projeto demonstra a utilização da biblioteca **Transformers**, da Hugging Face, para realizar tarefas de Processamento de Linguagem Natural (PLN), utilizando modelos pré-treinados para tradução automática e sumarização de textos.

A aplicação permite:

- traduzir textos entre diferentes idiomas utilizando o modelo **facebook/nllb-200-distilled-600M**;
- resumir textos em inglês utilizando o modelo **facebook/bart-large-cnn**;
- traduzir automaticamente o resumo gerado para português.

O objetivo é demonstrar como integrar diferentes modelos de IA em um mesmo fluxo de processamento.

---

## Compatibilidade com Transformers 5.x

O código original do curso foi desenvolvido para versões anteriores da biblioteca **Transformers**, nas quais era comum carregar modelos diretamente através da função `pipeline()`.

Nas versões **5.x ou superiores**, algumas mudanças internas da biblioteca tornam recomendável o carregamento explícito dos modelos e tokenizers utilizando as classes:

- `AutoTokenizer`
- `AutoModelForSeq2SeqLM`

Essa abordagem oferece maior compatibilidade entre versões da biblioteca, além de seguir as recomendações atuais da Hugging Face.

Por esse motivo, o código foi adaptado para utilizar essas classes antes da criação dos pipelines de tradução e sumarização.

Além da atualização para a nova versão da biblioteca, também foram corrigidos pequenos problemas presentes no código original, como funções sem retorno e inconsistências em nomes de variáveis.

---

## Modelos utilizados

### Tradução

Modelo:

```
facebook/nllb-200-distilled-600M
```

Responsável pela tradução entre mais de 200 idiomas.

---

### Resumo

Modelo:

```
facebook/bart-large-cnn
```

Utilizado para gerar resumos de textos em inglês.

---

## Fluxo da aplicação

O processamento ocorre da seguinte forma:

1. O usuário fornece um texto.
2. O modelo BART gera um resumo.
3. O resumo é enviado ao modelo NLLB.
4. O modelo traduz o resumo para português.
5. O resultado é exibido ao usuário.

Também é possível utilizar apenas o tradutor para converter textos entre diversos idiomas suportados pelo NLLB.

---

## Bibliotecas utilizadas

- transformers
- torch
- sentencepiece
- accelerate

---

## Objetivo

O projeto tem como finalidade apresentar uma aplicação prática de modelos de Processamento de Linguagem Natural utilizando a biblioteca Transformers, demonstrando como combinar diferentes modelos pré-treinados para executar tarefas de tradução automática e sumarização de textos de maneira simples e eficiente.