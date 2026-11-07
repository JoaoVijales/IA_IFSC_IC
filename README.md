# IA IFSC IC — Modelos de Linguagem Locais com RAG

> **Iniciação científica em desenvolvimento** | Inteligência Artificial aplicada ao atendimento estudantil em instituições públicas de ensino.

[![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)](https://jupyter.org/)
[![Colab](https://img.shields.io/badge/Google-Colab-F9AB00?logo=googlecolab&logoColor=black)](https://colab.research.google.com/)
![Status](https://img.shields.io/badge/status-em%20pesquisa-blue)

**Título da pesquisa:** *Avaliação de Eficiência, Latência e Acurácia de Modelos de Linguagem Locais (SLMs/LLMs) com RAG para Atendimento Automatizado ao Estudante em Instituições Públicas*.

Este repositório reúne experimentos para investigar a viabilidade de **modelos de linguagem executados localmente, integrados a Retrieval-Augmented Generation (RAG)**, na resposta a dúvidas de estudantes utilizando documentos institucionais do **Instituto Federal de Santa Catarina (IFSC)**.

O projeto prioriza a comparação de qualidade das respostas, desempenho de inferência e consumo de recursos. Trata-se de um **protótipo de pesquisa**, não de um canal oficial de atendimento ou de uma aplicação em produção.

## Sumário

- [Contexto](#contexto)
- [Objetivos](#objetivos)
- [Arquitetura](#arquitetura)
- [Tecnologias](#tecnologias)
- [Estrutura do repositório](#estrutura-do-repositório)
- [Execução](#execução)
- [Metodologia de avaliação](#metodologia-de-avaliação)
- [Estado atual](#estado-atual)
- [Limitações](#limitações)
- [Próximas etapas](#próximas-etapas)
- [Autoria](#autoria)

## Contexto

Informações acadêmicas são frequentemente distribuídas entre editais, calendários, regulamentos e outros documentos. Encontrar e interpretar essas fontes pode exigir buscas manuais ou contato com equipes administrativas. Um sistema RAG pode recuperar trechos pertinentes desses documentos e fornecê-los a um modelo de linguagem como contexto para formular uma resposta.

A pesquisa considera, como cenário de aplicação, o apoio ao atendimento estudantil da **Secretaria Acadêmica do Câmpus Garopaba do IFSC**. O objetivo é estudar a viabilidade técnica dessa abordagem, e não substituir a consulta aos documentos e canais oficiais.

## Objetivos

### Objetivo geral

Avaliar a **eficiência, latência, consumo computacional e qualidade factual** de modelos de linguagem locais associados a um pipeline RAG de documentos acadêmicos.

### Objetivos específicos

1. Reunir e preparar uma base documental de informações acadêmicas públicas.
2. Extrair, segmentar e indexar documentos para busca semântica.
3. Integrar os trechos recuperados a modelos de linguagem locais.
4. Comparar modelos e configurações de inferência.
5. Medir tempo de resposta, velocidade de geração e consumo de memória.
6. Avaliar correção, fidelidade às fontes e capacidade de abstenção.
7. Investigar requisitos para eventual execução em infraestrutura institucional.

## Arquitetura

```text
Documentos institucionais (PDF)
             │
             ▼
      Extração de texto
           PyMuPDF
             │
             ▼
      Chunking (trechos)
             │
             ▼
        Embeddings
    multilingual-e5-small
             │
             ▼
      Índice ChromaDB
             ▲
             │ busca semântica
             │
Pergunta → Embedding da pergunta
             │
             ▼
 Recuperação de trechos relevantes
             │
             ▼
 Prompt = pergunta + evidências
             │
             ▼
    Modelo de linguagem
       (inferência local)
             │
             ▼
   Resposta fundamentada
```

### Etapas do pipeline

**1. Ingestão:** documentos PDF são lidos e seu texto é extraído.

**2. Segmentação:** o conteúdo é dividido em trechos (*chunks*) para indexação e recuperação.

**3. Representação vetorial:** os trechos são transformados em embeddings por um modelo multilíngue.

**4. Recuperação:** a pergunta também é convertida em embedding e comparada com o índice vetorial para encontrar conteúdo relevante.

**5. Geração:** um modelo de linguagem recebe a pergunta e os trechos recuperados e produz uma resposta contextualizada.

O RAG separa **recuperação** e **geração**. Ambas precisam ser avaliadas: um modelo gerador competente não compensa a ausência de evidências relevantes na recuperação.

## Tecnologias

| Camada | Tecnologia / abordagem | Finalidade |
| --- | --- | --- |
| Linguagem | Python | Processamento e experimentação |
| Ambiente | Jupyter Notebook / Google Colab | Execução de testes |
| Leitura de PDFs | PyMuPDF | Extração textual |
| Embeddings | `intfloat/multilingual-e5-small`, Sentence Transformers | Vetorização semântica |
| Busca vetorial | ChromaDB | Indexação e recuperação |
| Inferência | Ollama | Execução local dos modelos |
| Modelos candidatos | Phi-3 Mini, Llama 3, Mistral | Avaliação comparativa |
| Análise | pandas e bibliotecas Python de visualização | Indicadores, tabelas e gráficos |

**Nota:** esta tabela apresenta a stack experimental e os modelos considerados na pesquisa. Não comprova que todos os candidatos e experimentos estejam implementados no notebook publicado.

## Estrutura do repositório

```text
IA_IFSC_IC/
├── README.md
└── ia_local_com_rag.ipynb
```

- **`ia_local_com_rag.ipynb`**: notebook principal de experimentação com RAG e modelos locais.
- **`README.md`**: descrição, metodologia e orientações de execução da pesquisa.

Documentos institucionais de referência, resultados e artefatos gerados podem permanecer fora do versionamento, conforme direitos de reutilização e requisitos de privacidade.

## Execução

O desenvolvimento experimental utiliza **Google Colab**. Como o notebook está em evolução, suas células são a referência para os comandos exatos e a ordem de execução.

1. Abra o arquivo [`ia_local_com_rag.ipynb`](./ia_local_com_rag.ipynb) no Google Colab.
2. Selecione CPU ou GPU conforme o experimento e a disponibilidade do ambiente.
3. Execute as células de instalação de dependências.
4. Disponibilize os PDFs no diretório esperado; nos experimentos foi utilizado `/content/dados_ifsc`.
5. Execute a extração, a segmentação, os embeddings e a indexação vetorial.
6. Configure o modelo local e o serviço de inferência utilizados no teste.
7. Faça perguntas de avaliação e registre os resultados e a configuração de execução.

**Ollama no Colab:** o endereço `localhost:11434` só responde quando o servidor Ollama está ativo **dentro do mesmo ambiente de execução**. A instalação do cliente não equivale à inicialização do servidor. A opção de GPU no Colab também não garante, isoladamente, que a inferência do modelo a esteja utilizando.

### Recomendações de reprodutibilidade

Registrar em cada experimento:

- Modelo, versão, quantização e parâmetros de geração;
- Hardware, RAM/VRAM e ambiente de execução;
- Versões das bibliotecas;
- Identificação e data de obtenção dos documentos;
- Tamanho e sobreposição dos chunks;
- Embeddings, medida de similaridade e número de trechos recuperados (`top-k`);
- Prompt, janela de contexto e limites de tokens;
- Quantidade de repetições, aquecimento e estatísticas reportadas.

## Metodologia de avaliação

### 1. Eficiência e latência

| Métrica | Descrição |
| --- | --- |
| Latência total | Tempo entre enviar a pergunta e receber a resposta completa |
| TTFT (*Time to First Token*) | Tempo até o primeiro token produzido |
| Tokens/s | Taxa de geração do modelo |
| Memória RAM | Consumo de memória principal |
| Memória VRAM | Consumo de memória da GPU, quando aplicável |
| Utilização de CPU/GPU | Demanda de processamento durante a inferência |

O TTFT deve ser medido com geração por streaming. É recomendável apresentar mediana, dispersão e número de execuções, além de médias.

### 2. Qualidade das respostas

| Critério | Método de avaliação |
| --- | --- |
| Correção factual | Comparar as afirmações com documentos de referência |
| Fidelidade às evidências | Identificar afirmações não sustentadas pelos trechos recuperados |
| Completude | Verificar se a resposta contempla as informações esperadas |
| ROUGE e BLEU | Comparar com respostas de referência quando pertinente |
| Abstenção | Verificar se o sistema admite ausência de evidências suficientes |

**Importante:** ROUGE e BLEU medem principalmente similaridade textual e **não são, isoladamente, medidas de veracidade**.

### 3. Recuperação de informações — etapa planejada

Será necessário criar um conjunto de perguntas associado às evidências corretas (*ground truth*).

| Métrica | O que avalia |
| --- | --- |
| Recall@K | Proporção de evidências relevantes recuperadas entre os K primeiros resultados |
| Hit Rate@K | Fração de perguntas com ao menos uma evidência relevante entre os K primeiros resultados |
| MRR@K | Posição recíproca média da primeira evidência relevante |

Essa avaliação deverá ser conduzida separadamente da avaliação da resposta final.

### 4. Comparação controlada

Resultados obtidos em CPU e GPU, com quantizações diferentes, volumes distintos de contexto ou limites de geração variados não devem ser tratados como diretamente comparáveis sem explicitar tais condições. Testes de carga e concorrência constituem uma análise adicional de escalabilidade.

## Estado atual

**Pesquisa experimental em desenvolvimento.**

O trabalho envolve um pipeline de extração de documentos, segmentação, embeddings e busca vetorial, além de experimentos de integração com modelos de linguagem locais no Google Colab. O repositório público concentra atualmente um notebook principal.

**Este README não divulga resultados quantitativos consolidados**, pois indicadores só devem ser apresentados após validação das medições, do conjunto de avaliação e das condições experimentais. Também não há afirmação de implantação institucional concluída.

## Limitações

- **Uso não oficial:** o protótipo não substitui os canais de atendimento do IFSC.
- **Atualização:** regulamentos, calendários e editais podem mudar.
- **Alucinação:** o uso de RAG não elimina respostas incorretas.
- **Rastreabilidade:** recomenda-se indicar a origem e os trechos utilizados em cada resposta.
- **Privacidade:** não publicar dados pessoais ou documentos de acesso restrito.
- **Direitos de uso:** respeitar as condições de reutilização das fontes documentais.
- **Infraestrutura:** desempenho e viabilidade dependem do modelo, quantização e hardware disponíveis.

## Próximas etapas

- [ ] Criar perguntas de referência e validar as respostas esperadas.
- [ ] Registrar versões, parâmetros, hardware e conjunto documental dos testes.
- [ ] Comparar os modelos sob condições experimentais controladas.
- [ ] Avaliar recuperação com Recall@K, Hit Rate@K e MRR@K.
- [ ] Medir correção factual, fidelidade e capacidade de abstenção.
- [ ] Consolidar resultados em tabelas e gráficos reprodutíveis.
- [ ] Investigar desempenho sob consultas simultâneas.
- [ ] Avaliar os requisitos para futura execução em infraestrutura institucional.

## Autoria

**João Pedro Vijales**  
Projeto de iniciação científica relacionado ao Instituto Federal de Santa Catarina (IFSC).

GitHub: [@JoaoVijales](https://github.com/JoaoVijales)

---

*Documentação em atualização conforme o avanço da pesquisa. Informações formais sobre orientação acadêmica, período, licença e publicações científicas deverão ser acrescentadas quando confirmadas.*
