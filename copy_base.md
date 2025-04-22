# CopySiren Knowledge Base

## ::HOW_TO_USE_THIS_BASE 🚦

1. **Sequência de busca (RAG‑pipeline)**  
   - **Etapa 1 – Briefing:** use sempre as 8 respostas do bloco `::BRIEFING_QUESTIONS` como variáveis‑raiz.
   - **Etapa 2 – Nicho:** recupere a linha em `::NICHE_TEMPLATES` cujo *Nicho‑atalho* combina com `{{niche}}`; se nenhuma linha bater, use `GENÉRICO`. 
   - **Etapa 3 – Voz:** carregue o tom primário em `::TONE_LIBRARY`; se houver segundo tom, use‑o para nuance.
   - **Etapa 4 – Estrutura:** escolha a fórmula em `::COPY_FORMULAS` que melhor se ajusta ao formato pedido 
   - **Etapa 5 – Exemplos específicos** (`HEADLINE_EXAMPLES`, `CTA_EXAMPLES`, `OBJECTIONS_HANDLING`, `HOOKS_SHORTS`).

2. **Substituição de placeholders**  
   - Preencha todos os campos em MAIÚSCULAS ou dentro de `{{chaves}}` com dados do briefing ou métricas do cliente; garanta que o resultado final não contenha marcadores residuais.
   - Utilize **JSON Structured Output** quando solicitado pelo usuário; siga o esquema em `::FILL_TEMPLATE`.

3. **Fallback & Escalonamento** 
### Fallback padrão  
   - Se a busca não retornar documento relevante (score < 0.2) ou faltar variável crítica, dispare resposta fallback
     `“Preciso de mais detalhes (nicho, benefício, prova) para criar copy precisa.”`
   - Três tentativas de esclarecimento; depois encaminhe escalonamento humano (opt‑in).

4. **Limites de tamanho (tokens)**  
   | Formato | Máx. tokens | Observação |
   |---------|-------------|------------|
   | Headline | 15 | Voz ativa, 1 verbo principal |
   | CTA | 8 | Verbo imperativo + benefício |
   | Hook Shorts | 20 | 100 caracteres aprox. |  
   *(segundo benchmarks de CTR e retenção móvel)*

5. **Versão & Reindex**  
   - Inclua no topo do arquivo: `Last‑Updated: YYYY‑MM‑DD`. Após qualquer mudança, execute o workflow **Index CopySiren** para reindexar a coleção `copy_siren`.

6. **Latência & Desempenho**  
   - Se o tempo total da pipeline > 7 s, reduza `Top K` no Chroma para 3 ou use modelo mais leve para rascunho, depois refine com GPT‑4o.

> **Resumo Rápido para o agente:**  
> 1️⃣ Colete briefing → 2️⃣ busque blocos na ordem acima → 3️⃣ substitua variáveis → 4️⃣ gere saída no tom certo e dentro dos limites de tokens → 5️⃣ se faltar dado, acione fallback/escalonamento → 6️⃣ registre feedback para futura melhoria.

## ::BRIEFING_QUESTIONS  — ULTRACOMPLETO (≤ 2 min de resposta)

1. **Qual é o Nicho do seu produto, em até 3 palavras‑chave?**  
   > Ex.: Fitness Feminino, Finanças Pessoais, Viagens Solo

2. **Quem é o avatar Essencial (idade, gênero, profissão)?**  
   > Idade, Gênero (se relevante), Profissão/Estágio de vida  
   > Ex.: 35 a, mulher, analista de marketing

3. **Qual benefício transformador o produto entrega em 1 frase?**  
   > “Perder 7 kg sem academia” · “Lucrar 10 k/mês com infoprodutos”

4. **Cite a dor ou obstáculo que impede o avatar de agir.**
   > Ex: Falta de tempo, medo de fracasso, orçamento apertado…

5. **Qual é o Único Mecanismo/Metodologia (1 linha) que te diferencia?**  
   > Ex.: “Método 3×15 de HIIT” · “Planilha 50‑30‑20 guiada por IA”

6. **Prova‑de‑Confiança nº 1 (social proof ou dado)**  
   > 5 k alunos, 92 % de aprovação, 1 mi seguidores…

7. **Chamada‑para‑Ação desejada (CTA curto)**  
   > “Quero começar agora” · “Baixar guia grátis”

8. **Em 3 palavras, descreva o tom de voz da sua marca (ex.: Empático, Nerd, Divertido).**  
   > Inspirador, Autoritativo, Divertido
   

## ::PERSONA_TONE 🎯

### Persona – Campos Essenciais
| Campo                | Descrição / Placeholder                           |
|----------------------|---------------------------------------------------|
| **Avatar‑Nome**      | Identificação simbólica – ex.: “Marcela, a Exploradora” |
| **Segmento**         | `{{niche}}` do briefing                            |
| **Idade & Fase**     | `{{avatar}}` (ex.: 35 a, analista de marketing)    |
| **Objetivo‑Chave**   | `{{beneficio}}`                                    |
| **Maior Dor**        | `{{dor}}`                                          |
| **Objeção Primária** | preço / tempo / confiança / risco / complexidade    |
| **Motivador Emocional** | ganhar status / segurança / liberdade            |
| **Prova Preferida**  | dados, estudo de caso, depoimento em vídeo         |

> *Justificativa:* Mapear dor, benefício e objeção na mesma tabela integra a lógica “jobs → pains → gains” do **Value Proposition Canvas**, elevando clareza de mensagem.

### Tom de Voz – Mini Checklist
| Traço | Fazer | Evitar |
|-------|-------|--------|
| **Consistência** | Use sempre as 3 palavras do briefing em todas as peças | Misturar gírias, exageros |
| **Adequação** | Ajuste formalidade ao canal (e‑mail vs. Reels) | Copiar tom alheio |
| **Vocabulário‑âncora** | Liste 3‑5 expressões do nicho (ex.: “metabolismo”, “ROI”) | Termos vagos (“coisa”, “legal”) |

> *Observação:* Tabelas “do/avoid” reduzem erros de voz em 30‑50 % segundo guidelines de estilo da Mailchimp e HubSpot.


 
## ::COPY_FORMULAS
### AIDA
- **Atenção**: Headline impactante  
- **Interesse**: Dor + empatia  
- **Desejo**: Benefícios + prova  
- **Ação**: CTA claro

### PAS
1. **Problema**: Expõe a dor  
2. **Agitação**: Amplifica o desconforto  
3. **Solução**: Apresenta o produto

### 4 P’s (Promise · Picture · Proof · Push)
1. **Promise** – declare o principal benefício de forma direta.  
2. **Picture** – faça o leitor imaginar como a vida muda após obter o benefício.  
3. **Proof** – apresente evidências: números, depoimentos, demonstração.  
4. **Push** – chame à ação agora, removendo objeções (garantia, bônus).

### BAB (Before · After · Bridge)
1. **Before** – descreva a situação atual dolorosa.  
2. **After** – mostre o cenário ideal quando o problema está resolvido.  
3. **Bridge** – explique como o seu produto cria a passagem entre os dois cenários.

### SLAP (Stop · Look · Act · Purchase)
1. **Stop** – capture a atenção imediatamente (headline ou visual forte).  
2. **Look** – mantenha o interesse com um benefício ou ponto de dor.  
3. **Act** – peça uma micro‑ação (vídeo curto, botão, prova rápida).  
4. **Purchase** – torne a compra fácil e imediata (CTA destacado).

### FAB (Feature · Advantage · Benefit)
1. **Feature** – apresente um recurso específico do produto.  
2. **Advantage** – diga por que esse recurso é melhor que alternativas.  
3. **Benefit** – conecte a vantagem ao ganho concreto que o cliente terá.

> **Nota para o agente:**  
> – Escolha a fórmula que melhor se adapta ao formato pedido (ex.: BAB em blog, 4 P’s em landing, SLAP em anúncio curto, FAB em e‑commerce).  
> – Integre variáveis do briefing (`{{beneficio}}`, `{{dor}}`, etc.) em cada etapa para manter personalização.  
> – Limite cada passo a 2 – 3 frases claras e use o tom de voz selecionado em `::TONE_LIBRARY`.

   
## ::NICHE_TEMPLATES — PACK “20 NICHOS FORTES DE VENDA 2025”

| Nicho‑atalho                            | Promessa 10‑segundos (headline‑esqueleto)                                      | Palavras‑âncora / Power Words                             |
|-----------------------------------------|-------------------------------------------------------------------------------|-----------------------------------------------------------|
| Fitness Feminino                        | “Defina o corpo em **12 semanas** sem dietas radicais”                        | metabolismo · queima · rotina                             |
| Fitness Masculino                       | “Ganhe **5 kg de massa magra** em 90 dias, treino 45 min/dia”                 | hipertrofia · testosterona · supersérie                   |
| Emagrecimento Low‑Carb                  | “Perca **5 kg** no 1º mês com cardápio simples”                               | cetose · saciedade · energia                              |
| Finanças Pessoais                       | “Zere dívidas e invista o **1º R$ 10 mil**”                                   | patrimônio · juros compostos · liberdade                  |
| Renda Extra Online                      | “Fature **R$ 5 k/mês** em 90 dias como afiliado”                              | comissão · funil · passivo                                |
| Marketing para Coaches                  | “Lote a agenda com **10 alunos premium**/mês”                                 | autoridade · posicionamento · high‑ticket                 |
| **Produtividade com IA**                | “Automatize **50 %** das tarefas diárias com IA em 30 dias”                   | workflow · copiloto · prompt                              |
| **Adaptação de Carreira pós‑IA**        | “Requalifique‑se em IA generativa e aumente salário em **30 %**”              | upskilling · future‑proof · prompt engineering            |
| **Ganhar Dinheiro com IA**              | “Crie 4 fontes de renda passiva usando IA em **90 dias**”                     | automação total · print‑money · royalties                 |
| **Ganhar Dinheiro com Agentes de IA**   | “Lance um agente autônomo que gera **R$ 10 k/mês** em leads”                  | o3 · agente‑low‑code · servidores 24×7                    |
| Gestão de Tempo                         | “Recupere **2 h livres** por dia com método SCI”                              | agenda blocos · Pareto · foco profundo                    |
| Produtividade Executiva                | “Entregue 2× mais projetos em **4 semanas** sem burnout”                     | sprint · alavancas · KPI                                  |
| Organização Pessoal                     | “Domine sua rotina com método **5‑Pilares** em 14 dias”                       | planner · checklist · declutter                           |
| **Sustentabilidade / Eco Living**       | “Reduza pegada de carbono da casa em **40 %** este ano”                       | zero waste · energia solar · reutilizar                   |
| **Saúde Mental & Autocuidado**          | “Diminua ansiedade em **15 min/dia** com rotina mente‑corpo”                  | mindfulness · journaling · respiração                     |
| Inglês para Adultos                     | “Converse fluente em **6 meses** sem travar”                                  | fluência · imersão · confiança                            |
| E‑commerce Pet Care                     | “Aumente longevidade do pet com ração funcional premium”                      | vitalidade · check‑up · suplemento                        |
| Viagens Low Cost                        | “Viaje 2×/ano gastando **metade** do normal”                                  | milhas · roteiro secreto · oferta relâmpago               |
| Print on Demand                         | “Lance loja POD e venda **100 camisetas/dia** sem estoque”                    | designs virais · margem alta · dropshipping               |
| Tech Gadgets & Acessórios               | “Lucro de **20 %** vendendo gadgets de tendência antes da concorrência”       | unboxing · review · hype                                  |

 ---  

## ::TONE_LIBRARY — Matriz de 8 Tonalidades

| Tom‑atalho | Fazemos (💡) | Não fazemos (🚫) |
|------------|-------------|------------------|
| **Inspirador** | Verbos fortes, metáforas de jornada, 2ª pessoa (“você”) | Fatalismo, culpa, excesso de pontuação “!!!” |
| **Autoritativo** | Dados verificados, estatísticas citadas, voz confiante | Opinião sem fonte, jargão vazio, hipérbole (“melhor do mundo!”) |
| **Divertido** | Humor leve, emojis moderados 😊, analogias pop | Sarcasmo ofensivo, piadas internas, erros propositais de ortografia |
| **Empático** | Reconhece dor, usa “nós”, valida sentimento antes de vender | Minimizar problema, culpabilizar leitor, linguagem clínica |
| **Técnico/Didático** | Termos precisos, passos numerados, glossário simples | Floreios desnecessários, simplificação exagerada, tecnobabble |
| **Urgente/Persuasivo** | CTA claro, escassez legítima (“últimas 24 h”), voz ativa | Falsa urgência, pressão moral (“só fracassados desistem”) |
| **Visionário/Futurista** | Cenários “e se…”, tendências, linguagem de inovação | Hype vazio, promessas sem base, siglas não explicadas |
| **Minimalista/Objetivo** | Frases ≤ 15 palavras, voz direta, corta adjetivos supérfluos | Redundância, rodeios, repetições de ideia |

> **Nota para o agente (TONE_LIBRARY):**  
> – Use o tom principal (`{{tom_voz}}`) fornecido no briefing como guia dominante.  
> – Se o briefing contiver dois ou mais tons, priorize o primeiro e utilize o segundo apenas como nuance (ex.: “Inspirador‑Autoritativo” → 80 % inspirador, 20 % autoritativo).  
> – Siga rigorosamente as colunas **Fazemos / Não fazemos** para cada traço de voz a fim de manter consistência entre canais.  
> – Combine o tom selecionado com vocabulário‑âncora do nicho (ver `::NICHE_TEMPLATES`) e com a formatação apropriada ao formato (headline curto, e‑mail longo, script de vídeo etc.).  
> – Caso o tom solicitado não exista na matriz, aplique “Minimalista/Objetivo” por padrão e informe ao usuário que um tom específico pode ser adicionado à matriz mediante atualização do KB.


## ::HEADLINE_EXAMPLES ⭐️  — As 10 Estruturas de Maior Conversão (2024‑2025)

| Fórmula (atalho) | Estrutura | Por que funciona |
|------------------|-----------|------------------|
| **[Número] + Promessa** | `7 Passos Para DOBRAR Seu RESULTADO em 30 Dias` | Números ímpares e prazos claros aumentam CTR em 20‑30 % |
| **How‑To** | `Como CONQUISTAR OBJETIVO Sem OBSTÁCULO` | “How to” é o padrão mais buscado em Google e YouTube |
| **Pergunta Direta** | `Você Está Pronto Para ALCANÇAR RESULTADO?` | Engatilha resposta mental e aumenta engajamento em redes |
| **Você/Seu + Benefício** | `Seu NEGÓCIO Pronto Para LUCRAR Com IA?` | Prof. de copywriting: usar “você” gera 2× cliques |
| **Curiosidade + Número** | `O Erro #1 Que 87 % Cometem Ao Tentar OBJETIVO` | Combina estatística + “gap de curiosidade”|
| **Prova Social** | `Como 5 k Alunos Alcançaram RESULTADO Sem OBSTÁCULO` | Inclusão de prova social aumenta credibilidade |
| **Comparação** | `[Novo Método] vs. [Método Velho]: Qual Gera +RESULTADO?` | Deixa claro o conflito → leitura quase obrigatória |
| **Tempo Curto + Ação** | `Crie Seu AGENTE IA em 1 Hora (Guia Passo a Passo)` | Headline com prazo ≤ 1 h turbina sensação de rapidez |
| **Lista de Ferramentas** | `Top 5 Ferramentas de IA Que Economizam 2 h/dia` | Listas ranqueadas têm 2× compartilhamentos |
| **Contraste Negativo‑Positivo** | `Pare de PERDER Dinheiro — Veja Como LUCRAR Com OBJEÇÃO` | Aborda dor + solução em linha única |

### 3 Exemplos prontos para cada tom

| Tom | Exemplo #1 | #2 | #3 |
|-----|------------|----|----|
| Inspirador | `7 Passos Para Viver Da Sua Paixão Com IA` | `Como Destravar Seu Potencial Criativo em 15 Min` | `Você Está Pronto Para a Virada da Sua Jornada?` |
| Autoritativo | `O Guia Definitivo de ROI Para Ferramentas GenAI (2025)` | `5 Estudos Mostram Por Que a IA Otimiza 30 % Dos Custos` | `Comparativo: ChatGPT vs. Gemini — Quem Entrega Mais Leads?` |
| Urgente | `Últimas 48 h: Torne‑se Especialista em IA Sem Programar` | `Crie Seu Primeiro Agente Autônomo Hoje, 20 % OFF` | `Pare de Perder Tempo — Automatize Metade das Tarefas Agora` |

> **Nota para o agente (HEADLINE_EXAMPLES):**  
> – Escolha a fórmula que melhor combina com o *tone* indicado no briefing e com o formato solicitado (ex.: anúncio, e‑mail, post).  
> – Substitua todos os placeholders em MAIÚSCULAS (`RESULTADO`, `OBJETIVO`, `OBSTÁCULO`, `R$X`, etc.) pelos dados reais fornecidos nas respostas do usuário ou, se faltarem, solicite as informações antes de gerar a copy.  
> – Use sempre um número específico e um prazo concreto quando a fórmula pedir; isso aumenta a taxa de clique.  
> – Mantenha o headline em voz ativa, com no máximo 12–15 palavras.  
> – Caso o usuário solicite “modo=json”, devolva também o headline dentro da chave `"headline"` respeitando o esquema `::FILL_TEMPLATE`.


## ::CTA_EXAMPLES 🔥 — As 10 Estruturas de Maior Conversão (2024‑2025)

| Fórmula‑atalho | Estrutura (“*” = campo dinâmico)                                   | Gatilho Psicológico            |
|----------------|---------------------------------------------------------------------|--------------------------------|
| 1. Verbo + Benefício | `Começar Minha *TRANSFORMAÇÃO*`                                    | Ação imediata + benefício claro |
| 2. Eu Quero + Verbo  | `Eu Quero *RESULTADO* Agora`                                      | 1ª pessoa gera “ownership”      |
| 3. Número + Prazo    | `Ganhar *R$X* em 30 Dias`                                        | Especificidade + prova          |
| 4. Oferta / Desconto | `Economizar 40 % Hoje`                                           | Escassez de preço               |
| 5. Prova Social      | `Juntar‑me a +5 k Alunos`                                        | Validação de pares              |
| 6. Exclusividade     | `Acessar Conteúdo VIP`                                           | Pertencimento / FOMO            |
| 7. Risco Zero        | `Testar Grátis Por 7 Dias`                                       | Remoção de objeção              |
| 8. Urgência          | `Reservar Minha Vaga (48 h)`                                     | Contagem regressiva real        |
| 9. Curiosidade       | `Descobrir o Método Secreto`                                     | Gap de informação               |
|10. Micro‑Yes         | `Sim! Quero Aprender`                                            | Consentimento progressivo       |

### Exemplos por tom

| Tom              | CTA #1                                 | CTA #2                                 | CTA #3                                 |
|------------------|----------------------------------------|----------------------------------------|----------------------------------------|
| **Inspirador**   | `Começar Minha Jornada Agora`          | `Descobrir Meu Potencial`              | `Quero Ser a Próxima História de Sucesso` |
| **Autoritativo** | `Baixar Relatório Oficial`             | `Aplicar Estratégia Comprovada`        | `Acessar Dados Premium`                |
| **Urgente**      | `Garantir 50 % OFF Hoje`               | `Reservar Última Vaga`                 | `Entrar Antes Que Acabe`               |
| **Divertido**    | `Bora Lucrar?`                         | `Desbloquear Modo Ninja 🥷`             | `Sim! Quero o Segredo 🎁`              |
| **Empático**     | `Quero Avançar No Meu Ritmo`           | `Começar Pequeno, Crescer Seguro`      | `Explorar Opções Sem Risco`            |
> **Nota para o agente:**  
> – Escolha a fórmula que melhor combina com o *tone* informado no briefing.  
> – Substitua os campos dinâmicos (*RESULTADO*, *R$X*, *TRANSFORMAÇÃO*) com os valores reais do produto.  
> – Repita o CTA principal a cada 2‑3 dobras de página em mobile para maximizar cliques.

## ::HOOKS_SHORTS 🚀 — Snippets de até 100 caracteres para Reels / TikTok

| Fórmula‑atalho | Estrutura (“*” = campo dinâmico, máx. 100 chars)            | Por que converte? |
|----------------|-------------------------------------------------------------|-------------------|
| 1. DOR + PROMESSA | `Cansado de *DOR*? Veja como *BENEFÍCIO* em 15 s`            | Reconhece dor e entrega solução nos 1º 3 s — aumenta retenção |
| 2. NÚMERO + CHOQUE | `87 % das pessoas erram *TAREFA* — você não vai ser uma delas` | Estatísticas geram curiosidade e autoridade |
| 3. DESAFIO / CALL‑OUT | `Desafio: *AÇÃO* por 7 dias e conte se funcionou`         | Participação ativa eleva engajamento |
| 4. SEGREDO CURTO | `O truque secreto para *RESULTADO* (em menos de 1 min)`     | Gap de informação prende espectador |
| 5. ANTES × DEPOIS | `Antes: 0 leads • Depois: 50/dia — em 30 dias`            | Contraste visual + números claros |
| 6. PERGUNTA RÁPIDA | `Quer *BENEFÍCIO* sem *OBSTÁCULO*?`                       | Pergunta direta engatilha resposta mental |

### Exemplos prontos (edite os asteriscos)

| Nicho | Hook |
|-------|------|
| Fitness | `Cansado de estagnar? Veja como *queimar gordura* 24h/dia 🔥` |
| IA Produtividade | `87 % ainda fazem tarefas manuais — você? 🤖` |
| Finanças | `Desafio: economizar R$ 20/dia por 30 dias — topa? 💰` |
| Viagens | `O truque secreto para voar por metade do preço ✈️` |
| Coaching | `Antes: insegurança • Depois: 10 clientes premium em 1 mês` |

> **Nota para o agente:**  
> – Use o hook nos primeiros 3–5 segundos do vídeo, conforme recomendação do Instagram/TikTok para evitar drop‑off.  
> – Substitua `*DOR*`, `*BENEFÍCIO*`, etc., com variáveis do briefing.  
> – Mantenha o texto ≤ 100 caracteres (incluindo emojis) e alinhe o tom com `::TONE_LIBRARY`.


## ::OBJECTIONS_HANDLING 🔄

| Objeção‑raiz | Roteiro de Resposta (framework) | Exemplo *editável* |
|--------------|---------------------------------|--------------------|
| **Preço alto** | ① Reenquadrar em valor $ → ROI ② Prova social / dados ③ Oferta flexível | `Investir R$ X gera retorno médio de R$ Y em 90 dias (comprovado por 5 k clientes)` |
| **Sem tempo** | ① Mini‑passos (≤ 15 min/dia) ② Demonstração rápida ③ Garantia “progredir ou reembolsa” | `Agenda SCI mostra como ganhar 2 h livres já na 1ª semana` |
| **Não confio / soa genérico** | ① Testemunho concreto ② Mecanismo único ③ Autoridade/selos | `+34 % conversão com estudo de caso da Ana (marketing SAAS)` |
| **Funciona para mim?** | ① Segmentar avatar ② Estatística de sucesso por perfil ③ Risco‑zero | `92 % das mulheres 30‑45 a atingem meta de 7 kg — senão, reembolsamos` |
| **Preciso pensar / falar com sócio** | ① Recapitular benefício ② Criar mini‑compromisso (demo) ③ E‑mail resumo PDF | `Agende agora uma call de 10 min (sem compromisso) e leve diagnóstico grátis` |
| **Posso fazer sozinho** | ① Mostrar curva de aprendizado ② Comparar custo × tempo ③ Destaque de suporte | `Economiza 40 h de pesquisa e teste A/B — nós já validamos tudo` |
| **Medo de risco / fracasso** | ① Garantia dupla ② Estatística de sucesso ③ Conteúdo de onboarding | `Teste 7 dias grátis + garantia 30 dias — 0 % risco para você` |
| **Tecnologia complexa** | ① Mostrar passo a passo visual ② Oferecer setup assistido ③ Comparar “antes depois” | `Seu agente IA vai ao ar em 1 hora com nosso checklist guiado` |
| **Não é urgente** | ① Custo da inação (dado real) ② Oferta limitada ③ Projeção do ganho futuro | `Cada mês sem IA = R$ 3 k de custo oculto — vagas desta turma fecham em 48 h` |
| **Já tentei algo parecido** | ① Reconhecer frustração ② Destacar diferencial ③ Prova/resultado novo | `Diferente do método X, usamos automação contínua — veja estudo 2025` |

> **Nota para o agente:**  
> – Detecte qual família de objeção aparece na conversa (`price`, `time`, `trust`, etc.) e recupere a linha correspondente.  
> – Substitua os placeholders (R$ X, R$ Y, avatar, número de clientes) com dados fornecidos no briefing ou nas métricas do cliente.  
> – Combine sempre o **framework** com o **tom de voz** escolhido para manter coerência.

## ::FILL_TEMPLATE 📦 — Estrutura‑padrão para respostas em JSON

```json
{
  "niche": "{{nicho}}",
  "avatar": "{{avatar}}",
  "benefit": "{{beneficio}}",
  "pain": "{{dor}}",
  "tone": "{{tom_voz}}",
  "cta": "{{cta}}",
  "proof": "{{prova}}",
  "mechanism": "{{mecanismo_unico}}"
}
```

> Nota para o agente:
> • Gere este JSON sempre que o usuário pedir resultado estruturado (ex.: “mode=json”).
> • Substitua cada {{placeholder}} com os valores coletados no briefing; se um campo estiver vazio, peça a informação ao usuário antes de prosseguir.
> • Mantenha os nomes das chaves exatamente como acima (minúsculos, sem espaços) para garantir compatibilidade com APIs e ferramentas no‑code.

## ::ALLOWED_VARIABLES 📑 — Mapa de Placeholders

| Variável            | Descrição curta / Origem                                  |
|---------------------|-----------------------------------------------------------|
| `{{nicho}}`         | Nicho‑atalho escolhido pelo usuário (ex.: Fitness Feminino) |
| `{{avatar}}`        | Descrição essencial do público (idade, gênero, profissão) |
| `{{beneficio}}`     | Benefício transformador em 1 frase                        |
| `{{dor}}`           | Maior dor ou obstáculo que impede ação                    |
| `{{mecanismo_unico}}` | Método ou tecnologia que diferencia o produto            |
| `{{prova}}`         | Prova de confiança nº 1 (dados, depoimento, selos)        |
| `{{cta}}`           | Chamada‑para‑ação curta (até 6 palavras)                  |
| `{{tom_voz}}`       | 1‑3 adjetivos que definem o tom (ex.: Inspirador, Nerd)    |
| `{{resultado}}`     | Meta tangível (número + prazo)                            |
| `{{prazo}}`         | Prazo específico (dias, semanas, meses)                   |
| `{{valor}}` | Valor em R$, € ou $ citado na oferta                    |
| `{{objecao}}`       | Família de objeção detectada (price, time, trust etc.)    |

> **Nota para o agente:**  
> • Use somente as variáveis listadas acima; se precisar de um dado não disponível, pergunte ao usuário.  
> • Certifique‑se de que nenhuma marca `{{…}}` permaneça sem preencher no output final.  
> • Todas as variáveis são em minúsculas, sem acentos ou espaços, para compatibilidade com automações.

## ::TOKEN_LIMITS ⏱️ — Referência Rápida

| Formato / Peça            | Máx. tokens | Observações                                    |
|---------------------------|-------------|------------------------------------------------|
| Headline                  | 15          | Voz ativa, 1 verbo principal, sem ponto final |
| CTA (botão ou link curto) | 8           | Verbo imperativo + benefício                   |
| Hook Reels/TikTok         | 20          | Até 100 caracteres, pode incluir 1 emoji       |
| Sub‑headline              | 30          | Expande headline sem repetir                   |
| Parágrafo de Blog (intro) | 120         | Deve conter pergunta ou estatística            |
| Bullet / Benefit Line     | 25          | Começar com • ou emoji, evitar vírgulas em série |
| E‑mail Assunto            | 12          | Sem maiúsculas inteiras, usar pré‑header depois |
| E‑mail Corpo              | 400         | Incluir quebra visual a cada 3–4 linhas        |
| Anúncio Curto (FB/IG)     | 30          | Primeiro 90 caracteres devem conter dor+ganho  |
| Script Shorts (15 s)      | 60          | 3 blocos: hook (0‑3 s) · valor (4‑12 s) · CTA (13‑15 s) |

> **Nota para o agente:**  
> – Após gerar o texto, faça contagem de tokens e ajuste se exceder o limite do formato.  
> – Priorize cortar advérbios e adjetivos redundantes antes de remover informações essenciais.

---

## ::VERSION_CONTROL 🗓️

Last‑Updated: 2025‑04‑22  

> **Sempre que este arquivo for modificado:**  
> 1. Salve as mudanças no repositório.  
> 2. Rode o workflow **Index CopySiren** para reindexar a coleção `copy_siren` no Chroma.  
> 3. Confirme no log que o agente está usando a nova versão antes de prosseguir com prompts de produção.


