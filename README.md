# Desafio Técnico - Voa Health

## Visão Geral

Implementação de sistema de classificação para perguntas clínicas baseadas em evidências científicas. O projeto processa dados do PubMedQA, aplica prompt engineering com tags de reasoning e avalia performance do modelo.


-----------------------

## Tratamento de Dados
Os registros foram processados com foco na qualidade e estruturação adequada:

- **Limpeza dos registros:** Remoção de quebras de linha extras e espaços duplicados usando regex.

- **Filtragem de registros:** Implementação de filtro para descartar registros com `long_answer` vazio, nulo ou inválido ('none', 'n/a'). Na prática, o dataset PubMedQA não possui registros com essas características, mantendo todas as amostras processadas.

- **Estruturação do contexto:** Combinação dos campos `contexts` e `labels` no formato "LABEL: context" (ex: "INTRODUCTION: texto"), facilitando a organização das evidências científicas para o modelo.

- **Seleção de metadados:** MeSH terms foram removidos para simplificar o input, e os indicadores de tipo de reasoning (`reasoning_required_pred` e `reasoning_free_pred`) foram utilizados na análise de performance.


-----------------------

## Pipeline de Geração
- **Prompt customizado:** Substituição da mensagem de sistema sugerida pelo conteúdo do `prompt.txt`, carregando instruções para raciocínio clínico estruturado.
- **Arquivos de saída:** Geração de dois formatos - `answers.jsonl` (conforme especificação) e `answers_with_thinking.jsonl` (incluindo tags de raciocínio e referências para análise detalhada).



-----------------------

## Hiperparâmetros

- **temperature=0.0:** Garante respostas determinísticas e reproduzíveis, eliminando aleatoriedade inadequada para análise científica.

- **max_new_tokens=2048:** Valor especificado no desafio, suficiente para o raciocínio nas tags `<think>``<\think>` e a resposta final, sem risco de truncamento.

- **enable_thinking=True:** Utiliza a funcionalidade nativa do Qwen3 para separar automaticamente o processo de raciocínio da resposta final.


-----------------------

## Dados Sintéticos Clínicos

Proposta: Criar casos clínicos sintéticos usando LLMs para gerar cenários médicos realistas. A ideia seria combinar sintomas, exames e tratamentos com base em diretrizes médicas e nos dados estruturados das consultas que a Voa já processa, criando perguntas do tipo "paciente com X sintomas e Y exames – qual a melhor conduta?". Esses dados sintéticos, revisados por médicos, ajudariam no treinamento/fine-tuning de modelos em situações raras ou combinações incomuns difíceis de encontrar em bases reais, além de permitir criação de benchmarks específicos para as especialidades mais comuns da plataforma.






