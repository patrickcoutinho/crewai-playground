# 🧠 CrewAI Tutorial: Criando um Tutorial Técnico com Inteligência Artificial

Este tutorial demonstra como usar o **CrewAI** para criar um time de agentes especializados em produzir um **tutorial técnico interativo**, passando pelas etapas de **pesquisa, redação técnica e revisão pedagógica**.

### 🔧 Pré-requisitos

Certifique-se de ter:

- Python 3.8+
- Biblioteca `crewAI` instalada
- Uma chave da OpenAI configurada

---

### 🔇 Etapa 1: Suprimir Warnings

```python
import warnings
warnings.filterwarnings('ignore')
```

---

### 🤖 Etapa 2: Importar bibliotecas

```python
from crewai import Agent, Task, Crew
```

---

### 🔐 Etapa 3: Configurar a API da OpenAI

```python
import os
from utils import get_openai_api_key

openai_api_key = get_openai_api_key()
os.environ['OPENAI_API_KEY'] = openai_api_key
os.environ['OPENAI_MODEL_NAME'] = 'gpt-4o-mini'
```

---

## 🧠 Agentes

### 1. Especialista em Pesquisa Técnica

Este agente será responsável por **investigar o tema** do tutorial, identificar conceitos importantes, exemplos e possíveis armadilhas.

```python
researcher = Agent(
    role="Technical Researcher",
    goal="Coletar informações técnicas confiáveis e atualizadas sobre {topic}",
    backstory="Você é um pesquisador técnico com anos de experiência explicando "
              "conceitos complexos para públicos variados. Seu trabalho ajuda a basear o conteúdo "
              "em fatos, boas práticas e fontes verificadas.",
    allow_delegation=False,
    verbose=True
)
```

---

### 2. Redator Técnico

Responsável por **transformar a pesquisa em um tutorial passo a passo**, claro e didático.

```python
technical_writer = Agent(
    role="Technical Writer",
    goal="Escrever um tutorial claro, conciso e didático sobre o tema {topic}",
    backstory="Você é um redator técnico especializado em ensinar programação e tecnologia "
              "por meio de exemplos práticos. Você escreve de forma estruturada e acessível, "
              "facilitando o aprendizado mesmo para iniciantes.",
    allow_delegation=False,
    verbose=True
)
```

---

### 3. Revisor Pedagógico

Este agente atua como um **professor/editor**, revisando se o conteúdo é compreensível, fluido e segue princípios pedagógicos.

```python
pedagogical_editor = Agent(
    role="Pedagogical Editor",
    goal="Revisar o tutorial para garantir clareza, coerência e acessibilidade pedagógica",
    backstory="Você é um educador experiente que atua como revisor pedagógico. "
              "Seu objetivo é tornar o conteúdo envolvente, bem estruturado e compreensível "
              "para alunos de diferentes níveis.",
    allow_delegation=False,
    verbose=True
)
```

---

## ✅ Tarefas

### 1. Realizar a Pesquisa Técnica

```python
research_task = Task(
    description=(
        "1. Identifique os principais conceitos relacionados ao tema {topic}.\n"
        "2. Traga exemplos práticos, comandos de terminal ou trechos de código se possível.\n"
        "3. Liste fontes confiáveis utilizadas (documentações, artigos, tutoriais oficiais).\n"
        "4. Destaque armadilhas comuns ou erros que iniciantes cometem."
    ),
    expected_output="Um documento com as principais descobertas técnicas, exemplos e fontes.",
    agent=researcher
)
```

---

### 2. Escrever o Tutorial

```python
writing_task = Task(
    description=(
        "1. Baseado na pesquisa técnica, escreva um tutorial didático sobre {topic}.\n"
        "2. Use linguagem simples e instrucional, com títulos e subtítulos organizados.\n"
        "3. Inclua exemplos de código, explicações passo a passo e observações importantes.\n"
        "4. Comece com uma introdução amigável e conclua com um resumo e dicas finais."
    ),
    expected_output="Um tutorial em Markdown, com estrutura clara, pronto para publicação.",
    agent=technical_writer
)
```

---

### 3. Revisar com Foco Pedagógico

```python
review_task = Task(
    description=(
        "1. Revise o tutorial escrito para garantir que está claro e didático.\n"
        "2. Sugira melhorias de linguagem e estrutura, se necessário.\n"
        "3. Certifique-se de que os exemplos estão compreensíveis e relevantes.\n"
        "4. Ajuste o tom de voz para ser acolhedor, instrucional e motivador."
    ),
    expected_output="Um tutorial revisado, com sugestões pedagógicas implementadas.",
    agent=pedagogical_editor
)
```

---

## 👥 Crew (equipe)

Agora, reunimos todos os agentes e tarefas em uma tripulação colaborativa.

```python
crew = Crew(
    agents=[researcher, technical_writer, pedagogical_editor],
    tasks=[research_task, writing_task, review_task],
    verbose=True
)
```

---

## 🚀 Execução

Agora vamos executar a equipe com um tópico de exemplo. Você pode trocar o valor de `topic`.

```python
result = crew.kickoff(
    inputs={
        "topic": "Como criar um ambiente virtual com Poetry para projetos em Python"
    }
)
```

---

## 📄 Exibir o Resultado

```python
from IPython.display import Markdown
Markdown(result.raw)
```

---

## ✅ Conclusão

Este tutorial demonstrou como usar o CrewAI para orquestrar um time de agentes que criam conteúdos técnicos interativos. Você pode adaptar essa estrutura para produzir cursos, trilhas de aprendizado, guias de onboarding, entre outros!
