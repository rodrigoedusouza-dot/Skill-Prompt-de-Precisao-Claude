---
name: prompts-de-precisao
description: Ajuda quem está começando no Claude a escrever, melhorar e executar prompts com precisão, usando 4 técnicas — Few-Shot (ensinar por exemplos), Chain of Thought (pedir o raciocínio passo a passo), controle fino de formato e tamanho, e persona + contexto — e aplica um perfil personalizável de empresa ou projeto (tom de voz, vocabulário, formatos, exemplos). Use sempre que o usuário pedir ajuda para "escrever um prompt", "melhorar meu prompt" ou "como peço isso pro Claude", reclamar que a resposta veio genérica, longa demais, com "cara de IA" ou superficial, quiser um texto "no meu tom" ou "no padrão da empresa", precisar de uma análise ou decisão mais bem fundamentada, ou parecer iniciante sem saber como pedir direito — mesmo que não cite nenhuma dessas técnicas.
---

# Prompts de Precisão

Esta skill transforma um pedido vago em um prompt que o Claude consegue acertar de primeira, e ensina o usuário a fazer isso sozinho com o tempo. O público principal é quem está começando: a linguagem deve ser simples, os exemplos concretos, e o resultado útil já na primeira resposta.

A ideia central: a maior parte das respostas ruins vem de **informação que estava só na cabeça do usuário**. As 4 técnicas são quatro jeitos de tirar essa informação da cabeça e colocá-la no prompt.

| Técnica | O que coloca no prompt | Resolve o sintoma |
|---|---|---|
| **Few-Shot** | Exemplos do resultado desejado | "Não ficou com o meu jeito", "tom errado", "estrutura diferente da que uso" |
| **Chain of Thought** | Passos de raciocínio a seguir antes de responder | "Chutou", "análise rasa", "recomendação sem justificativa" |
| **Formato e Tamanho** | Regras de forma, limites e proibições | "Longo demais", "cheio de enrolação", "cara de IA", idas e vindas para ajustar |
| **Persona e Contexto** | Quem o Claude é e a situação real | "Resposta genérica", "não considerou o meu cenário", "perspectiva errada" |

## Passo 0 — Carregar o perfil de personalização

Antes de qualquer coisa, procure um perfil que descreva a empresa ou o projeto do usuário. Ele existe para que cada pessoa não precise repetir contexto, tom e regras em todo prompt.

Ordem de busca (use o que encontrar; se achar os dois, o do projeto vence em caso de conflito, porque é mais específico):

1. **Projeto**: `PERFIL-PROMPTS.md` na pasta de trabalho atual (ou `.claude/perfil-prompts.md`).
2. **Empresa/pessoa**: `personalizacao/perfil.md` dentro da pasta desta skill.
3. **Conversa**: se o usuário colou um perfil na conversa ou ele está nas instruções/arquivos do Projeto (no Claude.ai não há acesso a pastas), use esse.

Como aplicar o que encontrar:
- Use o perfil como padrão: persona, tom, vocabulário a usar/evitar, formatos e restrições entram no prompt sem o usuário pedir. Na explicação final, diga em uma linha o que veio do perfil (ex.: "apliquei o tom e as palavras proibidas do seu perfil"), para o usuário entender de onde vieram as regras e poder ajustá-las.
- O pedido explícito do usuário na conversa sempre vence o perfil. Se houver contradição relevante, avise em uma frase.
- Campos do perfil ainda com o texto de modelo (`[preencha...]`) contam como vazios — ignore-os.

Se **não houver perfil**, não bloqueie o trabalho: ajude com padrões sensatos e, no final, ofereça uma vez (sem insistir) criar o perfil — veja "Criar ou atualizar o perfil" abaixo.

## Passo 1 — Entender o que o usuário quer

Identifique o modo. Na dúvida, escolha o que entrega mais valor com menos perguntas.

- **Construir**: "me ajuda a escrever um prompt para…" → entregue um prompt pronto para copiar.
- **Melhorar**: o usuário colou um prompt ou mostrou uma resposta ruim → diagnostique o sintoma, ajuste o prompt.
- **Executar com precisão**: o usuário quer a tarefa feita (um e-mail, uma análise) → aplique as técnicas por dentro e entregue o resultado, explicando em 1–2 linhas o que usou.
- **Ensinar**: "o que é few-shot?", "quando uso chain of thought?" → explique com um exemplo do mundo dele, não com teoria.

## Passo 2 — Diagnosticar e escolher as técnicas

Use só o que resolve o problema. Um pedido simples ("traduza esta frase") não precisa de nada disso, e empilhar técnicas deixa o prompt pesado e a resposta engessada.

1. **Falta de contexto ou perspectiva?** → Persona e Contexto.
2. **O resultado precisa ter um estilo ou estrutura específicos que é difícil descrever com palavras?** → Few-Shot.
3. **A tarefa exige julgar, comparar, calcular ou decidir?** → Chain of Thought.
4. **A forma da resposta importa (tamanho, estrutura, palavras proibidas)?** → Formato e Tamanho.

Quando houver mais de um sintoma, combine. Leia o arquivo de referência de cada técnica escolhida antes de montar o prompt — ele traz o modelo, exemplos antes/depois e os erros mais comuns:

- [references/persona-e-contexto.md](references/persona-e-contexto.md)
- [references/few-shot.md](references/few-shot.md)
- [references/chain-of-thought.md](references/chain-of-thought.md)
- [references/formato-e-tamanho.md](references/formato-e-tamanho.md)

## Passo 3 — Montar o prompt

Use esta ordem. Ela segue a lógica de uma conversa: primeiro quem você é e onde estamos, depois o que fazer, depois como fazer bem.

```
1. PERSONA E CONTEXTO   quem o Claude é + a situação real (do perfil + do pedido)
2. TAREFA               o que precisa ser feito, em uma frase direta
3. EXEMPLOS             (se few-shot) 2–5 exemplos, delimitados e rotulados
4. RACIOCÍNIO           (se CoT) os passos a seguir antes da resposta final
5. FORMATO E TAMANHO    estrutura, limites, PROIBIDO / OBRIGATÓRIO
```

Regras de montagem:
- **Dados que faltam**: nunca invente números, nomes, prazos ou causas para completar lacunas. Com um iniciante, entregue o prompt já com `[COLCHETES]` nos pontos que faltam e liste, ao final, até 3 perguntas curtas que ajudam a preenchê-los — assim ele tem algo útil na hora e decide se responde. Só pergunte *antes* de entregar quando a lacuna impedir qualquer rascunho útil (ex.: nem o assunto do texto está claro).
- **Exemplos (few-shot)**: use os exemplos do perfil que combinam com o pedido. Se houver menos de dois, use o que existir e diga que mais exemplos reais melhorariam o resultado; não invente exemplos nem os copie dos arquivos de referência como se fossem do usuário. Como exemplos trazem fatos específicos (a causa de um atraso, um valor), troque esses fatos por `[COLCHETES]` ou lembre o Claude de imitar o estilo e **não** reaproveitar os fatos.
- **Dados do usuário**: preserve-os exatamente (valores, datas, nomes). Prompt de precisão não reescreve fatos.
- **Delimite blocos** (exemplos, textos colados) com marcadores claros, como `=== EXEMPLO 1 ===`, para o Claude não confundir exemplo com instrução.
- **Explique o porquê das regras** quando a regra não for óbvia ("sem jargão, porque o público é leigo"). Regra com motivo é seguida de forma mais inteligente que regra solta.
- Prefira instruções positivas ("comece direto pelo resultado") a uma lista longa de "não faça", usando PROIBIDO apenas para o que realmente precisa ser barrado.

## Passo 4 — Entregar

Formato da resposta, curto e escaneável:

1. **O prompt pronto**, em um bloco de código para copiar.
2. **O que mudou e por quê**: 2 a 4 linhas, citando qual técnica entrou e que problema resolve. Isso é o que faz o iniciante aprender; não pule.
3. **Como ajustar** (opcional): uma dica do tipo "se vier longo demais, reduza o limite de palavras".

No modo **Executar**, entregue o resultado primeiro e a explicação de uma linha depois. Não cole o raciocínio interno dentro de um entregável (e-mail, comunicado) a menos que o usuário peça para ver o raciocínio — nesse caso, separe claramente "Raciocínio" e "Resposta final".

## Cuidados que valem para todas as técnicas

- **Few-Shot ancora forte.** O Claude tende a copiar o exemplo. Use exemplos variados e diga "imite o estilo e a estrutura, não o conteúdo".
- **Persona muda a lente, não os fatos.** Não atribua credenciais inventadas nem peça que a persona "saiba" o que não sabe. Para decisões importantes, vale rodar a mesma pergunta com personas diferentes em conversas separadas e comparar (ver referência).
- **Chain of Thought custa tamanho.** Peça o passo a passo quando a decisão importa; para tarefas triviais, só aumenta o ruído.
- **Formato exagerado engessa.** Poucas regras bem escolhidas funcionam melhor que vinte.
- **Informação sensível**: lembre o usuário de não colocar dados pessoais, senhas ou segredos de negócio em exemplos nem no perfil, principalmente se o perfil for versionado em um repositório.

## Criar ou atualizar o perfil

Quando o usuário pedir, ou aceitar sua oferta, conduza uma entrevista curta (no máximo 6 perguntas, em uma ou duas mensagens) e gere o arquivo a partir de [assets/perfil-modelo.md](assets/perfil-modelo.md):

1. O que a empresa/projeto faz e quem é o público?
2. Como é o tom de voz (formal, próximo, técnico) e o idioma?
3. Palavras ou expressões que **devem** aparecer e que **nunca** devem aparecer?
4. Quais tipos de texto você mais produz (e-mail, relatório, post…) e qual o tamanho ideal de cada um?
5. Você tem 2–3 textos reais que representam bem o seu jeito de escrever? (viram a biblioteca de exemplos)
6. Há regras que não podem ser quebradas (compliance, dados que não podem ser citados)?

Depois de gerar, diga onde salvar: `PERFIL-PROMPTS.md` na pasta do projeto (vale só para aquele projeto) ou `personalizacao/perfil.md` na pasta da skill (vale para tudo). Há um exemplo preenchido em [assets/exemplo-perfil-preenchido.md](assets/exemplo-perfil-preenchido.md).
