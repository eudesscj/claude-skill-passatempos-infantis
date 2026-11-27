---
name: "passatempos-infantis"
description: "Gera automaticamente passatempos infantis estilo Coquetel Picolé (cruzadinha, caça-palavras, criptograma, contas, palavras embaralhadas, labirinto) em PDF A4, com respostas em arquivo separado. Use quando pedirem passatempos ou atividades para criança."
---

# Passatempos infantis

Gera passatempos no estilo dos livros Picolé (Coquetel) e entrega **dois PDFs A4**:
- `passatempos.pdf`: só os jogos, para imprimir.
- `passatempos_respostas.pdf`: as respostas, para o adulto guardar sem precisar imprimir.

Para economizar impressão, os jogos menores dividem a mesma folha:
- **Folha 1:** cruzadinha.
- **Folha 2:** caça-palavras.
- **Folha 3:** criptograma + desafio de contas.
- **Folha 4:** palavras embaralhadas + labirinto.

O gerador mede a altura real de cada jogo. Se a dupla não couber numa folha (por exemplo, um criptograma longo no nível 5), ele separa os dois automaticamente. As folhas não têm campo de nome nem data.

## Modo automático (padrão)

Quando o usuário só chamar a skill, sem detalhes, **não pergunte nada**. Gere com estes padrões:
- **Nível 2** (3º–4º ano). Os outros níveis só são usados quando o usuário pedir ("nível 3", "5º ano").
- **Os seis jogos:** cruzadinha, caça-palavras, criptograma, desafio de contas, palavras embaralhadas e labirinto (4 folhas de jogos).
- **Tema escolhido por você** para o caça-palavras, diferente dos que já estão no histórico.
- **Palavras novas,** que não estejam no histórico.

Se o usuário disser algo junto ("nível 3", "só caça-palavras", "sem contas", "tema dinossauros", "12 palavras"), aplique só o que ele disse e mantenha o resto no padrão. Idade ou série viram nível pelas tabelas. Não gere prévia em PNG, a menos que o usuário peça.

## Cruzadinha: níveis

A cruzadinha é de **grade cheia**, como a do Coquetel: não tem casa vazia, as dicas ficam dentro da grade e cada uma aponta com uma seta para a sua palavra. A linha de cima e a coluna da esquerda alternam dica e letra; as palavras que começam na borda usam seta dobrada (↳). Uma casa de dica pode segurar duas palavras, uma para a direita e outra para baixo; nesse caso ela aparece dividida ao meio.

| Nível | Série / idade | Grade (linhas × colunas) | Maior palavra | Palavras (aprox.) |
|---|---|---|---|---|
| 1 Fácil | 1º–2º ano (6–7) | 9 × 7 | 6 | 24 |
| 2 Médio | 3º–4º ano (8–10) | 12 × 9 | 8 | 40 |
| 3 Intermediário | 5º ano (10–11) | 12 × 9 | 9 | 40 |
| 4 Difícil | 6º ano (11–12) | 13 × 10 | 9 | 50 |
| 5 Desafio | 7º ano+ (12+) | 13 × 10 | 10 | 50 |

As palavras vêm do dicionário `palavras_cruzadinha.tsv`, documento do projeto: cerca de 4.650 palavras com nível de 1 a 5, tiradas do LexPorBR Infantil (licença CC BY-NC-SA 4.0) e revisadas à mão. O gerador prefere as palavras do nível pedido e usa as de até dois níveis acima só para fechar a grade. As de 2 letras vêm de uma lista fixa (EU, TU, NA, DO…) e de pares de vogais, no máximo dois por grade.

A montagem tem duas etapas: o gerador escolhe as palavras; você escreve uma dica para cada palavra escolhida; o gerador desenha a folha.

Só existem dois tipos de dica:
- `figura`: o desenho **é** a resposta (desenho de sapo → SAPO). Use só quando o desenho existir no Fluent Emoji e não houver dúvida sobre o nome. Não use figura com texto para completar.
- `texto`: em MAIÚSCULAS, com palavras de no máximo 6 letras; `___` marca a lacuna. Prefira "OPOSTO DE ALTO" a "CONTRÁRIO DE ALTO".

Quantidade de figuras: no N1, figura sempre que houver desenho; no N2 e no N3, figura nas palavras concretas que têm desenho (por volta de 1/4 das dicas); no N4 e no N5, poucas figuras, de objetos menos comuns (telescópio, ampulheta, ábaco).

Tipos de dica de texto liberados por nível:
- **N1:** completar frases bem simples ("___ DE LEITE") e conhecimento do dia a dia.
- **N2:** contrário, plural, feminino/masculino, palavra composta para completar e conhecimento do dia a dia ("TEM 7 DIAS", "A ABELHA FAZ").
- **N3:** tudo do N2, mais conhecimento escolar simples (por exemplo "CAPITAL DO PAÍS", "TEM 3 LADOS", "DEZ ANOS", "DOZE OVOS").
- **N4:** tudo do N3, mais conhecimento escolar mais amplo (outras capitais, planetas, século, figuras geométricas).
- **N5:** tudo do N4, mais sinônimo, definição, ciências e história do Brasil.

Palavras de 2 letras: use dica de completar ("___ PRAIA", "CASA ___ VOVÓ", "NOTA: ___, RÉ, MI"). Par de vogais: "VOGAIS DE X", com uma palavra cujas vogais sejam exatamente essas, na ordem ("EA" → "VOGAIS DE PERA").

Regras das dicas:
- A dica não pode conter a própria resposta nem outra resposta de 3 letras ou mais da mesma grade (por exemplo, não use "A ABELHA FAZ" se ABELHA também for resposta). Evite dicas ambíguas.
- As respostas já saem sem acento. Na dica, use acento normal.
- Se uma palavra escolhida for imprópria ou difícil demais de explicar para a idade, acrescente-a ao arquivo de exclusão e monte a grade de novo.

## Caça-palavras: níveis

| Nível | Diagrama | Palavras | Direções | Texto |
|---|---|---|---|---|
| 1 | 12×12 | 5–6 | → ↓ | 3–4 frases |
| 2 | 17×18 | 7–8 | → ↓ ↘ | ~80 palavras |
| 3 | 18×18 | 9 | → ↓ ↘ ↗ | ~95 palavras |
| 4 | 18×19 | 10 | → ↓ ↘ ↗ | ~110 palavras |
| 5 | 19×20 | 11–12 | todas, inclusive de trás para frente | ~140 palavras |

O diagrama é grande para o número de palavras: assim elas ficam mais escondidas. A folha não tem desenhos.

Regras do caça-palavras:
- **Texto original e educativo,** sobre um tema adequado à série: natureza, corpo humano, espaço, animais, sustentabilidade, alimentação, invenções. Tom leve, terminando com uma mensagem ou curiosidade. Título curto e simpático.
- As palavras para procurar ficam marcadas no texto com `**PALAVRA**`, em maiúsculas e com acento normal. O gerador destaca em vermelho e remove os acentos na grade.
- Entre 3 e 9 letras (até 12 no nível 5). Nenhuma palavra pode estar contida em outra (SOL e SOLO, por exemplo), e nenhuma pode ser destacada duas vezes.
- O gerador garante que cada palavra aparece uma única vez e que palavras na mesma direção não se sobrepõem.

## Criptograma: níveis

| Nível | Frase | Letras reveladas na frase | Letras escondidas na chave (?) |
|---|---|---|---|
| 1 | 3–5 palavras | 6 | 0 |
| 2 | 6–9 palavras | 4 | 0 |
| 3 | 7–10 palavras | 3 | 0 |
| 4 | 9–12 palavras | 2 | 0 |
| 5 | 12–15 palavras | 0 | 3 (deduzidas pelo contexto) |

O gerador sorteia as letras reveladas e as escondidas.

Regras do criptograma:
- A frase é uma **curiosidade verdadeira**, divertida e adequada à idade, por exemplo "O polvo tem três corações e sangue azul!". Prefira curiosidades sobre animais, corpo humano, espaço e natureza. Confira se o fato é correto.
- No máximo 22 letras diferentes. Pode ter acento, porque o gerador remove. Pontuação permitida: `, . ! ?`.
- A curiosidade não pode repetir o histórico.
- O criptograma não tem desenhos.

## Desafio de contas (quadrado mágico): níveis

Dois quadrados 3×3 lado a lado (mude com `"quantidade"`), cada um com os números de 1 a 9 sem repetir, as operações entre eles e os resultados de cada linha e coluna. A criança completa as casas vazias.

| Nível | Operações | Casas vazias |
|---|---|---|
| 1 | + − | 3 |
| 2 | + − × ÷ | 5 |
| 3 | + − × ÷ | 6 |
| 4 | + − × ÷ | 7 |
| 5 | + − × ÷ | 7 |

Do nível 2 em diante, cada quadrado usa pelo menos uma vez cada operação, e a divisão é sempre exata.

O gerador cria tudo sozinho e garante três coisas:
- **Solução única.**
- **Contas da esquerda para a direita, sem negativos.** Os resultados ficam entre 1 e 99.
- **Nenhuma ambiguidade de precedência:** × ou ÷ só aparecem na segunda conta da linha se a primeira também for × ou ÷.

Você não precisa escolher nada além do nível.

## Palavras embaralhadas: níveis

São 6 palavras (use `total` para mudar, sempre um número par). Cada palavra aparece com as letras misturadas e espaços para a criança escrever a palavra certa.

| Nível | Tamanho das palavras | Ajuda |
|---|---|---|
| 1 | 3–5 letras | desenho + 1ª letra já preenchida |
| 2 | 5–8 letras | pista curta em texto (categoria) |
| 3 | 6–9 letras | pista curta em texto (categoria) |
| 4 | 7–9 letras | pista curta em texto (categoria) |
| 5 | 7–10 letras | nenhuma |

Regras das palavras embaralhadas:
- **Nível 1:** cada item precisa de `figura` (nome de pasta da biblioteca de desenhos).
- **Níveis 2 a 4:** cada item precisa de `pista`, uma categoria curta que ajude sem entregar a resposta ("ANIMAL", "FRUTA", "MEIO DE TRANSPORTE", "ESTAÇÃO DO ANO", "MATERIAL ESCOLAR"). Varie as categorias entre as palavras.
- **Nível 5:** só a palavra.
- As palavras não podem repetir as da cruzadinha da mesma geração nem as do histórico.
- O gerador embaralha garantindo que o começo e o fim da palavra não fiquem no lugar.
- Mande 2 ou 3 candidatas a mais. Itens sem figura (N1) ou sem pista (N2–4) são ignorados.

## Labirinto: níveis

O labirinto **cresce para ocupar todo o espaço livre da folha**: abaixo das palavras embaralhadas, ou a folha inteira se estiver sozinho. A largura dos túneis define a dificuldade.

| Nível | Largura dos túneis | Tamanho típico junto com as embaralhadas |
|---|---|---|
| 1 | 10 mm | ~15 × 11 casas |
| 2 | 6,5 mm | ~24 × 18 casas |
| 3 | 6 mm | ~26 × 19 casas |
| 4 | 5,5 mm | ~28 × 21 casas |
| 5 | 4,5 mm | ~35 × 26 casas |

- Sempre existe exatamente um caminho da entrada até a saída. A resposta mostra esse caminho em laranja.
- Personagem e objetivo: opcionalmente escolha um par temático com `inicio` e `fim` (nomes de figuras) e uma `frase` ("Ajude o coelho a chegar até a cenoura!"). Se não informar, o gerador sorteia um par pronto (ratinho/queijo, coelho/cenoura, cachorro/osso, abelha/flor, macaco/banana, foguete/planeta, pinguim/peixe, urso/mel).

## Histórico (não repetir)

As palavras e os temas usados ficam na memória persistente, no arquivo `/areas/passatempos-historico.md`.

- **Antes de gerar:** leia o arquivo com a ferramenta de leitura de memória. Se ele não existir, o histórico está vazio. Não use nenhuma resposta da cruzadinha, nenhuma palavra do caça-palavras ou das palavras embaralhadas, nenhum tema e nenhuma curiosidade do criptograma que já estejam lá. O desafio de contas e o labirinto não entram no histórico.
- **Depois de entregar:** acrescente uma linha por jogo com o que entrou de fato no PDF (as listas do relatório do gerador). Use a ferramenta de append da memória. Se o arquivo não existir, crie com memory_write e `if_version: new`:
  ```
  ---
  name: passatempos-historico
  description: Temas, palavras e curiosidades já usados nos passatempos da skill passatempos-infantis
  sources: [chat]
  aliases: [histórico cruzadinha, histórico caça-palavras]
  ---
  - [stated] 2026-11-24 | cruzadinha | N2 | PINGUIM, MORCEGO, CHUVA, ...
  - [stated] 2026-11-24 | caça-palavras | N2 | tema: ciclo da água | AGUA, SOL, NUVENS, ...
  - [stated] 2026-11-24 | criptograma | N2 | O POLVO TEM TRES CORACOES E SANGUE AZUL!
  - [stated] 2026-11-24 | embaralhadas | N2 | GIRAFA, BORBOLETA, ELEFANTE, ...
  ```
- **Pedidos do usuário:** com "pode repetir", ignore o histórico só nessa geração. Com "zera o histórico", apague o arquivo. Com "mostra o histórico", liste o que já foi usado.
- **Sem ferramentas de memória** no ambiente: grave `~/passatempos/historico.txt` no mesmo formato e avise o usuário que esse histórico só vale nesta conversa.

## Fluxo

1. **Leia o histórico** e defina o nível, os jogos e o tema (padrões acima).
2. **Prepare o ambiente**, uma vez por sessão:
   ```bash
   mkdir -p ~/passatempos && cd ~/passatempos
   [ -d fe ] || (git clone --depth 1 --filter=blob:none --sparse https://github.com/microsoft/fluentui-emoji fe \
     && cd fe && git sparse-checkout set --no-cone '/assets/*/Color/*.svg' '/LICENSE')
   which google-chrome chromium chromium-browser 2>/dev/null | head -1
   ```
   As ilustrações são do Fluent Emoji (Microsoft, licença MIT). Os nomes das pastas estão em inglês, com inicial maiúscula (`Penguin`, `Hot dog`, `Cloud with rain`). Se não houver Chrome nem Chromium, informe o usuário e pare.
3. **Grave os geradores:** `~/passatempos/passatempos.py` com o código da seção "Código do gerador" e `~/passatempos/cruzadinha.py` com o da seção "Código da cruzadinha".
   Se for gerar a cruzadinha, leia o documento do projeto `palavras_cruzadinha.tsv` (ferramenta de projeto, `project_read`) e grave o conteúdo em `~/passatempos/palavras_cruzadinha.tsv`. Sem esse dicionário não dá para montar a cruzadinha: avise o usuário.
4. **Monte a cruzadinha (etapa 1).** Grave em `~/passatempos/excluir.txt` as respostas de cruzadinha que já estão no histórico e rode:
   ```bash
   cd ~/passatempos && python3 cruzadinha.py montar --nivel 2 --excluir excluir.txt --saida grade.json
   ```
   Leva cerca de 2,5 minutos (`--tempo` muda isso). O relatório mostra a grade e a lista de palavras, marcando as que vêm de nível acima e as que dividem a casa com outra dica. Se alguma palavra for imprópria ou difícil demais de explicar, acrescente-a ao `excluir.txt` e rode de novo.
5. **Confira as figuras** que pretende usar antes de montar a entrada:
   ```bash
   ls ~/passatempos/fe/assets | grep -i -E 'penguin|owl|umbrella|droplet'
   ```
6. **Grave `~/passatempos/entrada.json`.** Omita a chave do jogo que não for gerar. Na cruzadinha, escreva uma dica para cada palavra da grade (etapa 2).
   ```json
   {
     "cruzadinha": {
       "nivel": 2, "titulo": "Cruzadinha", "grade": "grade.json",
       "dicas": {
         "SAPO": {"tipo": "figura", "figura": "Frog"},
         "BAIXO": {"tipo": "texto", "texto": "OPOSTO DE ALTO"},
         "ARO": {"tipo": "texto", "texto": "___ DA RODA"},
         "EA": {"tipo": "texto", "texto": "VOGAIS DE PERA"}
       }
     },
     "caca_palavras": {
       "nivel": 2, "titulo": "A viagem da água",
       "texto": "Quando o **SOL** esquenta os **RIOS**, a água vira **VAPOR** ..."
     },
     "criptograma": {
       "nivel": 2, "frase": "O polvo tem três corações e sangue azul!"
     },
     "contas": {"nivel": 2},
     "embaralhadas": {
       "nivel": 2,
       "itens": [{"resposta": "Girafa", "pista": "Animal"}, {"resposta": "Bicicleta", "pista": "Meio de transporte"}]
     },
     "labirinto": {"nivel": 2, "inicio": "Rabbit", "fim": "Carrot", "frase": "Ajude o coelho a chegar até a cenoura!"}
   }
   ```
   Na cruzadinha, `titulo` e `instrucao` são opcionais.
7. **Gere os PDFs:**
   ```bash
   cd ~/passatempos && python3 passatempos.py entrada.json passatempos.pdf --assets fe/assets
   ```
   Saem dois arquivos: `passatempos.pdf` (jogos) e `passatempos_respostas.pdf` (respostas).
   - **Cruzadinha:** um `ERRO` indica palavra da grade sem dica ou figura que não existe. `AVISO dica com palavra longa` pede uma dica mais curta.
   - **Caça-palavras:** um `ERRO` indica palavra contida em outra, palavra repetida ou palavras que não couberam. Ajuste o texto e rode de novo.
   - **Criptograma:** um `ERRO` indica letras diferentes demais. Encurte a frase.
   - **Embaralhadas:** itens em `SEM DESENHO` ou `SEM PISTA` foram ignorados. Confira se ficaram 6 palavras.
8. **Confira** as respostas listadas: estão corretas e adequadas à série? Corrija e gere de novo se precisar.
9. **Entregue os dois PDFs**, os jogos e as respostas, com uma linha de resumo: nível, jogos incluídos, número de folhas e tema do caça-palavras.
10. **Registre no histórico** (seção "Histórico").

## Código do gerador

```python
#!/usr/bin/env python3
"""Passatempos infantis -> dois PDFs A4: jogos (para imprimir) e respostas (arquivo separado).
Jogos: cruzadinha, caça-palavras, criptograma, desafio de contas, palavras embaralhadas, labirinto.
Uso: python3 passatempos.py entrada.json passatempos.pdf --assets fe/assets
     (gera passatempos.pdf e passatempos_respostas.pdf)
"""
import argparse, base64, itertools, json, os, random, re, shutil, subprocess, sys, tempfile, unicodedata

# ---------- comum ----------
def normaliza(p):
    p = unicodedata.normalize("NFD", p.upper())
    return "".join(c for c in p if c.isalpha() and unicodedata.category(c) != "Mn")


def svg_path(assets, nome):
    d = os.path.join(assets, nome, "Color")
    if not os.path.isdir(d):
        return None
    f = sorted(x for x in os.listdir(d) if x.endswith(".svg"))
    return os.path.join(d, f[0]) if f else None


def uri(assets, nome):
    return "data:image/svg+xml;base64," + base64.b64encode(open(svg_path(assets, nome), "rb").read()).decode()


CREDITO = '<p class="foot">Ilustrações: Fluent Emoji (Microsoft, licença MIT)</p>'

# ---------- cruzadinha ----------
# Grade cheia estilo Coquetel: o código fica em cruzadinha.py (seção "Código da cruzadinha").
from cruzadinha import cruzadinha_html


def cruzadinha(cfg, assets, tentativas=None):
    return cruzadinha_html(cfg, assets)


# ---------- caça-palavras ----------
DIRS8 = {"H": (0, 1), "V": (1, 0), "D": (1, 1), "AD": (-1, 1), "HR": (0, -1), "VR": (-1, 0), "DR": (-1, -1)}
CP_NIVEIS = {1: (["H", "V"], (12, 12)), 2: (["H", "V", "D"], (17, 18)), 3: (["H", "V", "D", "AD"], (18, 18)),
             4: (["H", "V", "D", "AD"], (18, 19)), 5: (["H", "V", "D", "AD", "HR", "VR", "DR"], (19, 20))}
CP_ALTURA = {1: 150, 2: 165, 3: 160, 4: 155, 5: 140}  # altura máxima da grade (mm), deixando espaço para o texto
FILL = "BCDFGHJKLMNPQRSTVWXZ" * 3 + "AEIOU"
CORES = ["#ffd8a8", "#b2f2bb", "#a5d8ff", "#fcc2d7", "#ffec99", "#d0bfff", "#99e9f2", "#ffc9c9", "#c0eb75", "#eebefa"]


def cp_montar(palavras, dirs, R, C, seed):
    rnd = random.Random(seed)
    g, pos, gd = {}, {}, {}
    uso = {d: 0 for d in dirs}
    for w in sorted(palavras, key=len, reverse=True):
        opts = []
        for d in dirs:
            dr, dc = DIRS8[d]
            for r in range(R):
                for c in range(C):
                    if not (0 <= r + dr * (len(w) - 1) < R and 0 <= c + dc * (len(w) - 1) < C):
                        continue
                    cells = [(r + dr * i, c + dc * i) for i in range(len(w))]
                    if all(g.get(k, w[i]) == w[i] and gd.get(k) not in ((dr, dc), (-dr, -dc))
                           for i, k in enumerate(cells)):
                        over = sum(1 for k in cells if k in g)
                        opts.append((over * 2 + rnd.random() * 2 - uso[d] * 3, cells, d))
        if not opts:
            return None
        opts.sort(key=lambda x: x[0], reverse=True)
        _, cells, dsel = rnd.choice(opts[:max(3, len(opts) // 10)])
        uso[dsel] += 1
        dd = DIRS8[dsel]
        for i, k in enumerate(cells):
            g[k] = w[i]
            gd[k] = dd
        pos[w] = cells
    for r in range(R):
        for c in range(C):
            g.setdefault((r, c), rnd.choice(FILL))
    for w in palavras:  # cada palavra exatamente 1 vez, em qualquer das 8 direções
        n = sum(1 for dr, dc in DIRS8.values() for r in range(R) for c in range(C)
                if all(g.get((r + dr * i, c + dc * i)) == ch for i, ch in enumerate(w)))
        if n != 1:
            return None
    return g, pos


def caca(cfg, assets):
    nivel = int(cfg.get("nivel", 2))
    dirs, (R, C) = CP_NIVEIS[nivel]
    palavras = [normaliza(w) for w in re.findall(r"\*\*(.+?)\*\*", cfg["texto"])]
    if len(set(palavras)) != len(palavras):
        sys.exit("ERRO caça-palavras: palavra destacada repetida no texto.")
    sub = [(a, b) for a in palavras for b in palavras if a != b and a in b]
    if sub:
        sys.exit(f"ERRO caça-palavras: palavra contida em outra: {sub}")
    for s in range(3000):
        res = cp_montar(palavras, dirs, R, C, s)
        if res:
            break
    else:
        sys.exit("ERRO caça-palavras: não coube. Use menos palavras ou palavras menores.")
    g, pos = res
    print(f"CAÇA-PALAVRAS - {len(palavras)} palavras | direções {dirs} | grade {R}x{C}")
    print("  " + ", ".join(palavras))
    figs = [f for f in cfg.get("figuras", []) if svg_path(assets, f)]
    cell = min(180 / C, 9.5, CP_ALTURA[nivel] / R)
    destaque = re.sub(r"\*\*(.+?)\*\*", r'<b class="hl">\1</b>', cfg["texto"])
    deco = "".join(f'<img class="deco" src="{uri(assets, n)}">' for n in figs)

    def grade(resp):
        cor = {}
        if resp:
            for i, (w, cells) in enumerate(pos.items()):
                for k in cells:
                    cor.setdefault(k, CORES[i % len(CORES)])
        return "".join("<tr>" + "".join(
            f'<td style="background:{cor[(r, c)]}">{g[(r, c)]}</td>' if (r, c) in cor else f"<td>{g[(r, c)]}</td>"
            for c in range(C)) + "</tr>" for r in range(R))

    css = f""".cp .banner {{ background:#1c4a8c; color:#fff; border-radius:3.5mm; text-align:center; padding:1.2mm 5mm 1.6mm; }}
.cp .banner h1 {{ margin:0; font-size:16pt; color:#ffd43b; text-shadow:2px 2px 0 #0b2a55; }}
.cp .banner p {{ margin:0.2mm 0 0; font-size:9.5pt; }}
.cp h2 {{ text-align:center; color:#2b8a3e; font-size:14pt; margin:3mm 0 2mm; }}
.cp .txt {{ font-size:13pt; line-height:1.55; text-align:justify; margin:0 4mm 4mm; }}
.cp .hl {{ color:#e03131; }}
.cp .decos {{ text-align:center; margin:4mm 0; }} .cp .deco {{ width:20mm; height:20mm; margin:0 6mm; }}
.cp .box {{ background:#1c4a8c; border-radius:5mm; padding:4mm; width:fit-content; margin:0 auto; }}
.cp table {{ border-collapse:separate; border-spacing:0; background:#fff; border-radius:3mm; }}
.cp td {{ width:{cell:.2f}mm; height:{cell:.2f}mm; text-align:center; font-weight:bold; font-size:{cell*0.55:.2f}mm; font-family:'DejaVu Sans',sans-serif; border-radius:1.5mm; padding:0; }}"""
    titulo = cfg["titulo"]
    instr = "Procure e marque, no diagrama de letras, as palavras em destaque no texto."
    jogo = (f'<div class="sec cp"><div class="banner"><h1>Caça-palavras</h1><p>{instr}</p></div><h2>{titulo}</h2>'
            f'<p class="txt">{destaque}</p>'
            f'<div class="box"><table>{grade(False)}</table></div></div>')
    resp = (f'<div class="sec cp"><div class="banner"><h1>Respostas</h1><p>Caça-palavras</p></div><h2>{titulo}</h2>'
            f'<p class="foot" style="font-size:12pt">Palavras: {" · ".join(sorted(palavras))}</p>'
            f'<div class="box"><table>{grade(True)}</table></div></div>')
    return css, jogo, resp, palavras


# ---------- criptograma ----------
CR_NIVEIS = {1: (6, 0), 2: (4, 0), 3: (3, 0), 4: (2, 0), 5: (0, 3)}  # letras reveladas na frase, letras escondidas na chave
SIMBOLOS = list("★●▲■◆♥♣♠☀☂✿☾✚✱⬢♪✈☘⚑☎✂⚓♛☯")


def criptograma(cfg, assets, seed=None):
    rnd = random.Random(seed)
    nivel = int(cfg.get("nivel", 2))
    revel, esconde = CR_NIVEIS[nivel]
    frase = normaliza_frase(cfg["frase"])
    letras = sorted(set(c for c in frase if c.isalpha()))
    if len(letras) > len(SIMBOLOS):
        sys.exit("ERRO criptograma: frase com letras demais.")
    sims = SIMBOLOS[:]
    rnd.shuffle(sims)
    chave = dict(zip(letras, sims))
    dadas = set(rnd.sample(letras, min(revel, len(letras))))
    ocultas_chave = set(rnd.sample([l for l in letras if l not in dadas], min(esconde, len(letras))))
    print(f"CRIPTOGRAMA - N{nivel} | {len(frase.split())} palavras | {len(letras)} letras | reveladas {sorted(dadas)}"
          f"{' | fora da chave ' + str(sorted(ocultas_chave)) if ocultas_chave else ''}")
    print("  " + frase)

    def corpo(resp):
        pals = []
        for p in frase.split():
            cels = "".join(
                f'<div class="cc"><div class="sym">{chave[ch] if ch.isalpha() else ""}</div>'
                f'<div class="let{" dada" if ch in dadas and not resp else ""}">'
                f'{ch if (resp or ch in dadas or not ch.isalpha()) else ""}</div></div>' for ch in p)
            pals.append(f'<div class="pal">{cels}</div>')
        key = "".join(f'<div class="kc"><div class="sym">{chave[l]}</div><div class="kl">'
                      f'{"?" if (l in ocultas_chave and not resp) else l}</div></div>' for l in letras)
        figs = "".join(f'<img src="{uri(assets, f)}">' for f in cfg.get("figuras", []) if svg_path(assets, f))
        extra = " As letras com ? você descobre pelo contexto." if ocultas_chave else ""
        dica = ("Dica: algumas letras já estão no lugar para ajudar." if dadas else "Sem letras de ajuda: é com você!") + extra
        return (f'<div class="banner"><h1>{"Respostas" if resp else "Criptograma"}</h1>'
                f'<p>{"Criptograma" if resp else "Use a chave secreta para descobrir a mensagem. Troque cada símbolo pela letra certa!"}</p></div>'
                f'<h3>Chave secreta</h3><div class="key">{key}</div>'
                f'<h3>Mensagem escondida</h3><div class="msg">{"".join(pals)}</div>'
                f'{"" if resp else f"<p class=dica>{dica}</p>"}')

    css = """.cr .banner { background:#6741d9; color:#fff; border-radius:3.5mm; text-align:center; padding:1.2mm 5mm 1.6mm; }
.cr .banner h1 { margin:0; font-size:16pt; color:#ffd43b; text-shadow:2px 2px 0 rgba(0,0,0,.35); }
.cr .banner p { margin:0.2mm 0 0; font-size:9.5pt; }
.cr h3 { color:#e8590c; font-size:11.5pt; margin:2.5mm 0 1.5mm; text-align:center; }
.cr .key { display:flex; flex-wrap:wrap; justify-content:center; gap:2mm; }
.cr .kc { border:1.5px solid #6741d9; border-radius:2mm; width:11.5mm; text-align:center; background:#f3f0ff; }
.cr .kc .sym { font-size:15pt; line-height:1.25; color:#6741d9; } .cr .kc .kl { font-size:12.5pt; font-weight:bold; border-top:1px solid #6741d9; }
.cr .msg { display:flex; flex-wrap:wrap; justify-content:center; gap:3mm 6mm; margin:0 4mm; }
.cr .pal { display:flex; gap:1mm; } .cr .cc { width:9.5mm; text-align:center; }
.cr .cc .sym { font-size:15pt; color:#6741d9; line-height:1.2; height:7mm; }
.cr .cc .let { height:8.5mm; border-bottom:2px solid #333; font-size:14pt; font-weight:bold; color:#1c7ed6; line-height:8.5mm; }
.cr .cc .let.dada { color:#e03131; }
.cr .dica { text-align:center; color:#555; font-size:10.5pt; margin:3mm 0 0; }
.cr .decos { text-align:center; margin:2mm 0 0; } .cr .decos img { width:13mm; height:13mm; margin:0 5mm; }"""
    jogo = f'<div class="sec cr">{corpo(False)}</div>'
    resp = f'<div class="sec cr">{corpo(True)}</div>'
    return css, jogo, resp, frase


def normaliza_frase(f):
    f = unicodedata.normalize("NFD", f.upper())
    f = "".join(c for c in f if unicodedata.category(c) != "Mn")
    return " ".join("".join(c for c in w if c.isalpha() or c in ",.!?") for w in f.split())


# ---------- desafio de contas (quadrado mágico) ----------
QD_NIVEIS = {1: ("+-", 3), 2: ("+-×÷", 5), 3: ("+-×÷", 6), 4: ("+-×÷", 7), 5: ("+-×÷", 7)}  # operações, casas vazias


def _ev(a, o, b):
    if o == "+": return a + b
    if o == "-": return a - b
    if o == "×": return a * b
    return a // b if b and a % b == 0 else None


def _linha(a, o1, b, o2, c):
    x = _ev(a, o1, b)
    if x is None or x <= 0:
        return None
    y = _ev(x, o2, c)
    return y if y is not None and 0 < y <= 99 else None


def _quadrado(rnd, ops, vazias):
    for _ in range(200000):
        n = rnd.sample(range(1, 10), 9)
        orow = [[rnd.choice(ops), rnd.choice(ops)] for _ in range(3)]
        ocol = [[rnd.choice(ops), rnd.choice(ops)] for _ in range(3)]
        calc = lambda t: ([_linha(t[i*3], orow[i][0], t[i*3+1], orow[i][1], t[i*3+2]) for i in range(3)],
                          [_linha(t[j], ocol[j][0], t[3+j], ocol[j][1], t[6+j]) for j in range(3)])
        rr, rc = calc(n)
        if None in rr or None in rc:
            continue
        if not all(any(op in o for o in orow + ocol) for op in ops):  # todas as operações do nível aparecem
            continue
        # sem ambiguidade de precedência: × ou ÷ na 2ª conta só se a 1ª também for × ou ÷
        if any(o2 in "×÷" and o1 in "+-" for o1, o2 in orow + ocol):
            continue
        oc = rnd.sample(range(9), vazias)
        faltam = [n[k] for k in oc]
        sols = 0
        for p in itertools.permutations(faltam):
            t = n[:]
            for k, v in zip(oc, p):
                t[k] = v
            if calc(t) == (rr, rc):
                sols += 1
                if sols > 1:
                    break
        if sols == 1:
            return n, orow, ocol, rr, rc, set(oc)
    sys.exit("ERRO desafio de contas: não encontrei quadrado com solução única.")


def contas(cfg, seed=None):
    rnd = random.Random(seed)
    nivel = int(cfg.get("nivel", 2))
    ops, vazias = QD_NIVEIS[nivel]
    qs = [_quadrado(rnd, ops, vazias) for _ in range(int(cfg.get("quantidade", 2)))]
    print(f"DESAFIO DE CONTAS - N{nivel} | {len(qs)} quadrados | operações {ops} | {vazias} casas vazias")

    def tab(q, resp):
        n, orow, ocol, rr, rc, oc = q
        cel = lambda k: (f'<td class="n vazio"></td>' if (k in oc and not resp)
                         else f'<td class="n{" rv" if k in oc else ""}">{n[k]}</td>')
        op = lambda o: f'<td class="op">{o}</td>'
        rows = []
        for i in range(3):
            rows.append("<tr>" + cel(i*3) + op(orow[i][0]) + cel(i*3+1) + op(orow[i][1]) + cel(i*3+2) + op("=")
                        + f'<td class="res">{rr[i]}</td></tr>')
            if i < 2:
                rows.append("<tr>" + "".join(op(ocol[j][i]) + "<td></td>" for j in range(3)) + "<td></td></tr>")
        rows.append("<tr>" + "".join(op("=") + "<td></td>" for _ in range(3)) + "<td></td></tr>")
        rows.append("<tr>" + "".join(f'<td class="res">{rc[j]}</td><td></td>' for j in range(3)) + "<td></td></tr>")
        return f'<table class="q">{"".join(rows)}</table>'

    def corpo(resp):
        blocos = "".join(f'<div class="qb">{f"<h3>Quadrado {i+1}</h3>" if len(qs) > 1 else ""}{tab(q, resp)}</div>' for i, q in enumerate(qs))
        enun = ("Complete com os números de <b>1 a 9</b>, sem repetir em cada quadrado, para que todas as contas deem certo. "
                "Faça as contas da esquerda para a direita e de cima para baixo.")
        return (f'<div class="banner"><h1>{"Respostas" if resp else "Desafio de contas"}</h1>'
                f'{"<p>Desafio de contas</p>" if resp else ""}</div>'
                f'{"" if resp else f"<p class=enun>{enun}</p>"}<div class="qs">{blocos}</div>')

    css = """.qd .banner { background:#2b8a3e; color:#fff; border-radius:3.5mm; text-align:center; padding:1.2mm 5mm 1.6mm; }
.qd .banner h1 { margin:0; font-size:16pt; color:#ffd43b; text-shadow:2px 2px 0 rgba(0,0,0,.35); }
.qd .banner p { margin:0.2mm 0 0; font-size:9.5pt; }
.qd .enun { text-align:center; font-size:12pt; margin:3mm 8mm 1mm; }
.qd h3 { color:#e8590c; font-size:11.5pt; margin:2mm 0 1mm; text-align:center; }
.qd .qs { display:flex; flex-direction:row; justify-content:space-evenly; align-items:flex-start; flex-wrap:wrap; }
.qd table.q { margin:0 auto; border-collapse:separate; border-spacing:0.5mm; }
.qd table.q td { width:9.5mm; height:9.5mm; text-align:center; font-size:15pt; font-weight:bold; padding:0; }
.qd td.n { border:2px solid #333; border-radius:2mm; background:#fff3bf; }
.qd td.n.vazio { background:#fff; } .qd td.n.rv { background:#fff; color:#1c7ed6; }
.qd td.op { color:#c2255c; font-size:15pt; }
.qd td.res { border:2px solid #2b8a3e; border-radius:2mm; background:#d3f9d8; color:#2b8a3e; }"""
    jogo = f'<div class="sec qd">{corpo(False)}</div>'
    resp = f'<div class="sec qd">{corpo(True)}</div>'
    return css, jogo, resp, None


# ---------- palavras embaralhadas ----------
# nível -> tipo de ajuda: N1 figura + 1ª letra, N2 a N4 pista em texto, N5 nenhuma
EM_NIVEIS = {1: "figura", 2: "pista", 3: "pista", 4: "pista", 5: "nada"}


def embaralhadas(cfg, assets, seed=None):
    rnd = random.Random(seed)
    nivel = int(cfg.get("nivel", 2))
    ajuda = EM_NIVEIS[nivel]
    itens = []
    for it in cfg["itens"]:
        if ajuda == "figura" and not svg_path(assets, it.get("figura", "")):
            print("EMBARALHADAS - SEM DESENHO (ignorado):", it.get("figura")); continue
        if ajuda == "pista" and not it.get("pista"):
            print("EMBARALHADAS - SEM PISTA (ignorado):", it["resposta"]); continue
        w = normaliza(it["resposta"])
        for _ in range(500):
            l = list(w); rnd.shuffle(l); e = "".join(l)
            if e != w and e[:2] != w[:2] and e[-2:] != w[-2:]:
                break
        itens.append((w, it, e))
    itens = itens[:int(cfg.get("total", 6))]
    if len(itens) % 2:
        itens = itens[:-1]
    print(f"EMBARALHADAS - N{nivel} ({ajuda}) | {len(itens)} palavras: " + ", ".join(w for w, *_ in itens))

    def corpo(resp):
        cards = []
        for w, it, e in itens:
            tiles = "".join(f'<span class="tile">{c}</span>' for c in e)
            revela = ajuda == "figura"
            boxes = "".join(f'<span class="box{" dada" if (revela and i == 0 and not resp) else ""}">'
                            f'{c if (resp or (revela and i == 0)) else ""}</span>' for i, c in enumerate(w))
            fig = f'<img src="{uri(assets, it["figura"])}">' if ajuda == "figura" else ""
            pista = f'<div class="pista">{it["pista"].upper()}</div>' if ajuda == "pista" else ""
            cards.append(f'<div class="card">{fig}<div>{pista}<div class="tiles">{tiles}</div>'
                         f'<div class="boxes">{boxes}</div></div></div>')
        sub = {"figura": "As letras se misturaram! Olhe a figura e escreva a palavra certa.",
               "pista": "As letras se misturaram! Use a pista e descubra a palavra.",
               "nada": "As letras se misturaram! Descubra cada palavra."}[ajuda]
        return (f'<div class="banner"><h1>{"Respostas" if resp else "Palavras embaralhadas"}</h1>'
                f'<p>{"Palavras embaralhadas" if resp else sub}</p></div>'
                f'<div class="grid2">{"".join(cards)}</div>')

    css = """.em .banner { background:#e8590c; color:#fff; border-radius:3.5mm; text-align:center; padding:1.2mm 5mm 1.6mm; }
.em .banner h1 { margin:0; font-size:16pt; color:#ffd43b; text-shadow:2px 2px 0 rgba(0,0,0,.35); }
.em .banner p { margin:0.2mm 0 0; font-size:9.5pt; }
.em .grid2 { display:grid; grid-template-columns:1fr 1fr; gap:2.5mm 5mm; margin-top:3mm; }
.em .card { display:flex; align-items:center; gap:3mm; border:2px dashed #ffa94d; border-radius:4mm; padding:1.5mm 3mm; background:#fff9db; }
.em .card img { width:16mm; height:16mm; }
.em .pista { font-size:8.5pt; font-weight:bold; color:#c2255c; margin-bottom:0.8mm; letter-spacing:0.3px; }
.em .tiles { display:flex; gap:0.8mm; margin-bottom:1mm; flex-wrap:wrap; }
.em .tile { width:6.2mm; height:6.2mm; background:#e8590c; color:#fff; border-radius:1.2mm; font-weight:bold; font-size:11pt; display:flex; align-items:center; justify-content:center; }
.em .boxes { display:flex; gap:0.8mm; }
.em .box { width:6.2mm; height:7.5mm; border-bottom:2px solid #333; font-weight:bold; font-size:12pt; color:#1c7ed6; text-align:center; line-height:7.5mm; }
.em .box.dada { color:#e03131; }"""
    return css, f'<div class="sec em">{corpo(False)}</div>', f'<div class="sec em">{corpo(True)}</div>', [w for w, *_ in itens]


# ---------- labirinto ----------
LB_NIVEIS = {1: 10, 2: 6.5, 3: 6, 4: 5.5, 5: 4.5}  # mm por célula (menor = mais corredores = mais difícil)
LB_IC = 12  # tamanho das figuras de entrada/saída (mm)
LB_PARES = [("Mouse", "Cheese wedge"), ("Rabbit", "Carrot"), ("Dog", "Bone"), ("Honeybee", "Blossom"),
            ("Monkey", "Banana"), ("Rocket", "Ringed planet"), ("Penguin", "Fish"), ("Bear", "Honey pot")]


def labirinto(cfg, assets, seed=None, altura_svg=None):
    rnd = random.Random(seed)
    nivel = int(cfg.get("nivel", 2))
    s = LB_NIVEIS[nivel]
    MW = int((186 - 2 * LB_IC - 6) // s)
    MH = int((altura_svg - 2 * LB_IC - 6) // s) if altura_svg else 6  # 6 = tamanho mínimo para medir; depois cresce
    ini, fim = cfg.get("inicio"), cfg.get("fim")
    if not (ini and fim and svg_path(assets, ini) and svg_path(assets, fim)):
        pares = [p for p in LB_PARES if svg_path(assets, p[0]) and svg_path(assets, p[1])]
        ini, fim = rnd.choice(pares)
    frase = cfg.get("frase") or "Encontre o caminho da entrada até a saída!"
    aberto, vis = set(), set()
    pilha = [(0, 0)]
    vis.add((0, 0))
    while pilha:  # DFS iterativo: labirinto perfeito (um único caminho entre dois pontos)
        r, c = pilha[-1]
        viz = [(r + dr, c + dc) for dr, dc in ((0, 1), (1, 0), (0, -1), (-1, 0))
               if 0 <= r + dr < MH and 0 <= c + dc < MW and (r + dr, c + dc) not in vis]
        if not viz:
            pilha.pop(); continue
        v = rnd.choice(viz)
        aberto.add(frozenset({(r, c), v})); vis.add(v); pilha.append(v)
    ant, fila, fimc = {(0, 0): None}, [(0, 0)], (MH - 1, MW - 1)
    while fila:
        u = fila.pop(0)
        for dr, dc in ((0, 1), (1, 0), (0, -1), (-1, 0)):
            v = (u[0] + dr, u[1] + dc)
            if v not in ant and frozenset({u, v}) in aberto:
                ant[v] = u; fila.append(v)
    caminho, u = [], fimc
    while u:
        caminho.append(u); u = ant[u]
    caminho.reverse()
    print(f"LABIRINTO - N{nivel} | {MW}x{MH} | {ini} -> {fim} | caminho com {len(caminho)} casas")

    def svg(resp):
        L = []
        for r in range(MH):
            for c in range(MW):
                x, y = c * s, r * s
                if r == 0 and c != 0: L.append((x, y, x + s, y))
                if c == 0 and r != 0: L.append((x, y, x, y + s))
                if (c == MW - 1 or frozenset({(r, c), (r, c + 1)}) not in aberto) and not (r == MH - 1 and c == MW - 1):
                    L.append((x + s, y, x + s, y + s))
                if r == MH - 1 or frozenset({(r, c), (r + 1, c)}) not in aberto:
                    L.append((x, y + s, x + s, y + s))
        els = [f'<line x1="{a}" y1="{b}" x2="{c}" y2="{d}" stroke="#1c4a8c" stroke-width="0.9" stroke-linecap="round"/>'
               for a, b, c, d in L]
        if resp:
            d = "M " + " L ".join(f"{c * s + s / 2} {r * s + s / 2}" for r, c in caminho)
            els.insert(0, f'<path d="{d}" fill="none" stroke="#f08c00" stroke-width="{s * 0.28:.1f}" stroke-linejoin="round" stroke-linecap="round"/>')
        W, H = MW * s, MH * s
        ic = LB_IC
        els.append(f'<image href="{uri(assets, ini)}" x="{-ic}" y="{-ic * 0.9}" width="{ic}" height="{ic}"/>')
        els.append(f'<image href="{uri(assets, fim)}" x="{W}" y="{H - ic * 0.1}" width="{ic}" height="{ic}"/>')
        return (f'<svg viewBox="{-ic - 2} {-ic - 2} {W + 2 * ic + 4} {H + 2 * ic + 4}" '
                f'width="{W + 2 * ic + 4:.0f}mm" height="{H + 2 * ic + 4:.0f}mm">{"".join(els)}</svg>')

    def corpo(resp):
        return (f'<div class="banner"><h1>{"Respostas" if resp else "Labirinto"}</h1>'
                f'<p>{"Labirinto" if resp else frase}</p></div><div class="svgwrap">{svg(resp)}</div>')

    css = """.lb .banner { background:#5c940d; color:#fff; border-radius:3.5mm; text-align:center; padding:1.2mm 5mm 1.6mm; }
.lb .banner h1 { margin:0; font-size:16pt; color:#ffd43b; text-shadow:2px 2px 0 rgba(0,0,0,.35); }
.lb .banner p { margin:0.2mm 0 0; font-size:9.5pt; }
.lb .svgwrap { text-align:center; margin-top:3mm; }"""
    return css, f'<div class="sec lb">{corpo(False)}</div>', f'<div class="sec lb">{corpo(True)}</div>', MH * s + 2 * LB_IC + 4


# ---------- main ----------
# jogos que dividem a mesma folha (para economizar impressão)
PAGINAS = [["cruzadinha"], ["caca_palavras"], ["criptograma", "contas"], ["embaralhadas", "labirinto"]]


def chrome():
    for b in ("google-chrome", "chromium", "chromium-browser", "chrome", "msedge"):
        if shutil.which(b):
            return shutil.which(b)
    sys.exit("ERRO: Chrome/Chromium não encontrado para gerar o PDF.")


def pdf(html, saida):
    with tempfile.TemporaryDirectory() as tmp:
        h = os.path.join(tmp, "p.html")
        open(h, "w", encoding="utf-8").write(html)
        subprocess.run([chrome(), "--headless", "--no-sandbox", "--disable-gpu", "--no-pdf-header-footer",
                        f"--print-to-pdf={os.path.abspath(saida)}", h], check=True,
                       stdout=subprocess.DEVNULL, stderr=subprocess.DEVNULL)


ALTURA_UTIL = 270  # mm úteis na folha A4 com margem de 12 mm (folga de 3 mm)
GAP = 9


def medir(css_base, css, partes):
    """Mede a altura real (mm) de cada seção renderizando no Chrome."""
    blocos = "".join(f'<div class="m" data-n="{n}" style="width:186mm">{h}</div>' for n, h in partes.items())
    js = ("<script>document.querySelectorAll('.m').forEach(e=>{e.setAttribute('data-h',"
          "(e.getBoundingClientRect().height/3.7795).toFixed(1))});</script>")
    html = (f'<!DOCTYPE html><html><head><meta charset="utf-8"><style>{css_base}\n' + "\n".join(css) +
            f'</style></head><body>{blocos}{js}</body></html>')
    with tempfile.TemporaryDirectory() as tmp:
        h = os.path.join(tmp, "m.html")
        open(h, "w", encoding="utf-8").write(html)
        out = subprocess.run([chrome(), "--headless", "--no-sandbox", "--disable-gpu", "--dump-dom", h],
                             capture_output=True, text=True).stdout
    return {n: float(v) for n, v in re.findall(r'data-n="(\w+)"[^>]*data-h="([\d.]+)"', out)}


def empacotar(alturas, presentes):
    """Agrupa em folhas na ordem de PAGINAS; separa o grupo se não couber."""
    folhas = []
    for grupo in PAGINAS:
        atual, alt = [], 0
        for n in grupo:
            if n not in presentes:
                continue
            h = alturas.get(n, ALTURA_UTIL)
            if atual and alt + GAP + h > ALTURA_UTIL:
                folhas.append(atual); atual, alt = [], 0
            alt = h if not atual else alt + GAP + h
            atual.append(n)
        if atual:
            folhas.append(atual)
    return folhas


def main():
    ap = argparse.ArgumentParser()
    ap.add_argument("entrada"); ap.add_argument("saida"); ap.add_argument("--assets", required=True)
    ap.add_argument("--tentativas", type=int, default=4000)
    a = ap.parse_args()
    cfg = json.load(open(a.entrada, encoding="utf-8"))
    geradores = {
        "cruzadinha": lambda c: cruzadinha(c, a.assets, a.tentativas),
        "caca_palavras": lambda c: caca(c, a.assets),
        "criptograma": lambda c: criptograma(c, a.assets),
        "contas": lambda c: contas(c),
        "embaralhadas": lambda c: embaralhadas(c, a.assets),
        "labirinto": lambda c: labirinto(c, a.assets),
    }
    css, jog, res, info = {}, {}, {}, {}
    for nome, g in geradores.items():
        if cfg.get(nome):
            c, j, r, x = g(cfg[nome])
            css[nome] = c; jog[nome] = j; res[nome] = r; info[nome] = x
    if not jog:
        sys.exit("ERRO: informe ao menos um jogo: " + ", ".join(geradores))
    base = """@page { size:A4; margin:12mm; }
body { font-family:'Comic Sans MS','Comic Neue','DejaVu Sans',sans-serif; margin:0; color:#222; }
.page { page-break-after:always; } .page:last-child { page-break-after:auto; }
.page .sec + .sec { margin-top:9mm; }
.sec { overflow:hidden; }
.foot { text-align:center; font-size:10pt; color:#555; margin-top:4mm; }"""

    if "labirinto" in jog:  # o labirinto cresce para ocupar o espaço livre da folha dele
        alt = medir(base, css.values(), jog)
        folha = next(f for f in empacotar(alt, jog) if "labirinto" in f)
        outros = sum(alt[n] + GAP for n in folha if n != "labirinto")
        banner = alt["labirinto"] - info["labirinto"]
        disp = ALTURA_UTIL - outros - banner - 3
        c, j, r, x = labirinto(cfg["labirinto"], a.assets, altura_svg=disp)
        css["labirinto"], jog["labirinto"], res["labirinto"] = c, j, r
    css = list(css.values())

    def doc(partes, credito):
        alt = medir(base, css, partes)
        print("  alturas (mm):", {k: v for k, v in alt.items()})
        pags = [f'<div class="page">{"".join(partes[n] for n in f)}</div>' for f in empacotar(alt, partes)]
        if credito:
            pags[-1] = pags[-1][:-6] + CREDITO + "</div>"
        return (f'<!DOCTYPE html><html><head><meta charset="utf-8"><style>{base}\n' + "\n".join(css) +
                '</style></head><body>' + "".join(pags) + '</body></html>'), len(pags)

    raiz, ext = os.path.splitext(a.saida)
    saida_resp = f"{raiz}_respostas{ext or '.pdf'}"
    h1, n1 = doc(jog, False)
    h2, n2 = doc(res, True)
    pdf(h1, a.saida)
    pdf(h2, saida_resp)
    print(f"PDF JOGOS: {a.saida} ({n1} folhas)")
    print(f"PDF RESPOSTAS: {saida_resp} ({n2} folhas)")


if __name__ == "__main__":
    main()
```

## Código da cruzadinha

Grave em `~/passatempos/cruzadinha.py`, na mesma pasta do `passatempos.py`.

```python
#!/usr/bin/env python3
"""Cruzadinha de grade cheia, estilo Coquetel: as dicas ficam dentro da grade, com setas retas e dobradas.

Etapa 1 (montar): python3 cruzadinha.py montar --nivel 2 --dicionario palavras_cruzadinha.tsv --excluir excluir.txt --saida grade.json
  -> preenche a grade com o dicionário e lista as palavras escolhidas
Etapa 2 (desenhar): passatempos.py lê a grade e as dicas ("cruzadinha": {"grade": "grade.json", "dicas": {...}}) e chama cruzadinha_html.
"""
import argparse, base64, json, os, random, re, sys, time, unicodedata


def normaliza(p):
    p = unicodedata.normalize("NFD", p.upper())
    return "".join(c for c in p if c.isalpha() and unicodedata.category(c) != "Mn")


# nível -> (linhas, colunas, maior palavra)
CZ_NIVEIS = {1: (9, 7, 6), 2: (12, 9, 8), 3: (12, 9, 9), 4: (13, 10, 9), 5: (13, 10, 10)}
DENS = 0.22  # fração de casas de dica no miolo da grade
DUAS_REAIS = set("AI AO AR AS CA DA DE DO EM EU FE HA IR JA LA MA ME MI NA NO OI OS OU PA PE PO RA RE SE SO TE TU UM UI VI VE".split())
VOGAIS = "AEIOU"
DUAS_ARTIF = {a + b for a in VOGAIS for b in VOGAIS if a != b}  # dica "VOGAIS DE X"
EXCLUIR_SEMPRE = set("ODE ARA ATA TUA OVA SUMO APO URA UNI ABC ANA CAVO ROMA EGITO DUO ASPA USO ARRE".split())
MAX_ARTIF = 2  # no máximo 2 pares de vogais por grade


# ---------- montar a grade ----------
def borda(r, c):
    """Linha 0 e coluna 0 alternam dica e letra. A dica da borda segura a palavra reta e a da letra vizinha (seta dobrada)."""
    if r == 0:
        return "Q" if c % 2 == 0 else "L"
    if c == 0:
        return "Q" if r % 2 == 0 else "L"
    return None


def runs(m, R, C):
    out = []
    for r in range(R):
        c = 0
        while c < C:
            if m[r][c]:
                s = c
                while c < C and m[r][c]:
                    c += 1
                if c - s >= 2:
                    out.append([(r, k) for k in range(s, c)])
            else:
                c += 1
    for c in range(C):
        r = 0
        while r < R:
            if m[r][c]:
                t = r
                while r < R and m[r][c]:
                    r += 1
                if r - t >= 2:
                    out.append([(k, c) for k in range(t, r)])
            else:
                r += 1
    return out


def valido(m, R, C, max_len, max_soltas):
    H = [[False] * C for _ in range(R)]
    V = [[False] * C for _ in range(R)]
    for x in runs(m, R, C):
        if len(x) > max_len:
            return False
        horiz = x[0][0] == x[1][0]
        for r, c in x:
            (H if horiz else V)[r][c] = True
    soltas = 0
    for r in range(R):
        for c in range(C):
            if m[r][c]:
                if not H[r][c] and not V[r][c]:
                    return False  # letra fora de qualquer palavra
                if (not H[r][c] or not V[r][c]) and r > 0 and c > 0:
                    soltas += 1
            else:
                dirr = c + 2 < C and m[r][c + 1] and m[r][c + 2]
                baixo = r + 2 < R and m[r + 1][c] and m[r + 2][c]
                dobra = (r == 0 and c + 1 < C) or (c == 0 and r + 1 < R and m[r + 1][1])
                if not (dirr or baixo or dobra):
                    return False  # casa de dica sem palavra
    return soltas <= max_soltas


def padrao(R, C, rnd, max_len, max_soltas):
    for _ in range(5000):
        m = [[borda(r, c) != "Q" if borda(r, c) else True for c in range(C)] for r in range(R)]
        alvo = int(DENS * (R - 1) * (C - 1))
        cand = [(r, c) for r in range(1, R) for c in range(1, C)]
        rnd.shuffle(cand)
        n = 0
        for r, c in cand:
            if n >= alvo:
                break
            if any(0 <= r + dr < R and 0 <= c + dc < C and not m[r + dr][c + dc] and borda(r + dr, c + dc) is None
                   for dr, dc in ((0, 1), (1, 0), (0, -1), (-1, 0))):
                continue
            m[r][c] = False
            n += 1
        for _ in range(40):
            longa = [x for x in runs(m, R, C) if len(x) > max_len]
            if not longa:
                break
            x = longa[0]
            k = rnd.randint(2, len(x) - 3) if len(x) >= 5 else 2
            m[x[k][0]][x[k][1]] = False
        if valido(m, R, C, max_len, max_soltas):
            return m
    return None


def preencher(R, C, seed, banco, max_len, max_soltas, max_nos=3000):
    """Sorteia o padrão e preenche palavra por palavra, sempre pela que tem menos opções."""
    rnd = random.Random(seed)
    m = padrao(R, C, rnd, max_len, max_soltas)
    if not m:
        return None
    slots = runs(m, R, C)
    por_casa = {}
    for i, sl in enumerate(slots):
        for k, cell in enumerate(sl):
            por_casa.setdefault(cell, []).append(i)
    letra, usadas, feito = {}, set(), [None] * len(slots)
    st = {"nos": 0, "artif": 0}
    idx, porlen, pref = banco["idx"], banco["porlen"], banco["pref"]

    def candidatos(i):
        sl = slots[i]
        conj = None
        for k, cell in enumerate(sl):
            ch = letra.get(cell)
            if ch:
                s2 = idx.get((len(sl), k, ch), set())
                conj = s2 if conj is None else conj & s2
                if not conj:
                    return []
        base = porlen.get(len(sl), set()) if conj is None else conj
        return [w for w in base if w not in usadas]

    def dfs():
        st["nos"] += 1
        if st["nos"] > max_nos:
            return False
        livres = [i for i in range(len(slots)) if feito[i] is None]
        if not livres:
            return True
        best, bc = None, None
        for i in livres:
            cs = candidatos(i)
            if not cs:
                return False
            if best is None or len(cs) < len(bc):
                best, bc = i, cs
                if len(cs) == 1:
                    break
        rnd.shuffle(bc)
        bc.sort(key=lambda w: (0 if w in pref else 1 if len(w) > 2 or w in DUAS_REAIS else 2) + rnd.random() * 0.8)
        sl = slots[best]
        for w in bc[:30]:
            art = w in DUAS_ARTIF
            if art and st["artif"] >= MAX_ARTIF:
                continue
            novos = [cell for cell in sl if cell not in letra]
            for k, cell in enumerate(sl):
                letra[cell] = w[k]
            feito[best] = w
            usadas.add(w)
            st["artif"] += art
            if all(candidatos(j) for cell in novos for j in por_casa[cell] if feito[j] is None) and dfs():
                return True
            st["artif"] -= art
            usadas.discard(w)
            feito[best] = None
            for cell in novos:
                del letra[cell]
            if st["nos"] > max_nos:
                return False
        return False

    if not dfs():
        return None
    return [[letra[(r, c)] if m[r][c] else "Q" for c in range(C)] for r in range(R)]


def analisar(g):
    """Lista as palavras e diz em que casa de dica fica cada uma (seta reta ou dobrada)."""
    R, C = len(g), len(g[0])
    pal = []
    for r in range(R):
        c = 0
        while c < C:
            if g[r][c] != "Q":
                s = c
                while c < C and g[r][c] != "Q":
                    c += 1
                if c - s >= 2:
                    pal.append({"palavra": "".join(g[r][s:c]), "dir": "A", "r": r, "c": s})
            else:
                c += 1
    for c in range(C):
        r = 0
        while r < R:
            if g[r][c] != "Q":
                t = r
                while r < R and g[r][c] != "Q":
                    r += 1
                if r - t >= 2:
                    pal.append({"palavra": "".join(g[i][c] for i in range(t, r)), "dir": "D", "r": t, "c": c})
            else:
                r += 1
    for p in pal:
        if p["dir"] == "A":
            if p["c"] == 0:
                p.update(q=[p["r"] - 1, 0], seta="dobrada", saida="baixo")
            else:
                p.update(q=[p["r"], p["c"] - 1], seta="reta", saida="dir")
        else:
            if p["r"] == 0:
                p.update(q=[0, p["c"] - 1], seta="dobrada", saida="dir")
            else:
                p.update(q=[p["r"] - 1, p["c"]], seta="reta", saida="baixo")
    carga = {}
    for p in pal:
        carga.setdefault(tuple(p["q"]), []).append(p)
    qs = [(r, c) for r in range(R) for c in range(C) if g[r][c] == "Q"]
    mudou = True
    while mudou:  # casa de dica vazia puxa uma palavra vizinha com seta dobrada
        mudou = False
        for q in qs:
            if carga.get(q):
                continue
            r, c = q
            for p in pal:
                orig = tuple(p["q"])
                if len(carga.get(orig, [])) < 2:
                    continue
                if p["dir"] == "A" and (p["r"], p["c"]) == (r + 1, c):
                    p.update(q=[r, c], seta="dobrada", saida="baixo")
                elif p["dir"] == "D" and (p["r"], p["c"]) == (r, c + 1):
                    p.update(q=[r, c], seta="dobrada", saida="dir")
                else:
                    continue
                carga[orig].remove(p)
                carga[q] = [p]
                mudou = True
                break
    return pal, [q for q in qs if not carga.get(q)]


def montar(a):
    R, C, max_len = CZ_NIVEIS[a.nivel]
    excluir = set(EXCLUIR_SEMPRE)
    if a.excluir and os.path.exists(a.excluir):
        excluir |= {normaliza(w) for w in re.split(r"[\s,|]+", open(a.excluir, encoding="utf-8").read()) if w}
    niveis = {}
    for linha in open(a.dicionario, encoding="utf-8").read().splitlines()[1:]:
        partes = linha.split("\t")
        if len(partes) >= 2 and partes[0] not in excluir:
            niveis[partes[0]] = int(partes[1])
    pref = {w for w, nv in niveis.items() if nv <= a.nivel}
    apoio = {w for w, nv in niveis.items() if nv <= a.nivel + 2} - pref
    todas = pref | apoio | DUAS_REAIS | DUAS_ARTIF
    idx, porlen = {}, {}
    for w in todas:
        porlen.setdefault(len(w), set()).add(w)
        for k, ch in enumerate(w):
            idx.setdefault((len(w), k, ch), set()).add(w)
    banco = {"idx": idx, "porlen": porlen, "pref": pref}
    print(f"CRUZADINHA - N{a.nivel} | grade {R}x{C} | {len(pref)} palavras do nível + {len(apoio)} de apoio | excluídas {len(excluir)}")
    melhores, t0, seed = [], time.time(), a.seed
    while len(melhores) < a.grades and time.time() - t0 < a.tempo:
        g = preencher(R, C, seed, banco, max_len, int((R - 1) * (C - 1) * 0.12))
        seed += 1
        if not g:
            continue
        pal, vazias = analisar(g)
        cob = {}
        for p in pal:
            for k in range(len(p["palavra"])):
                cell = (p["r"], p["c"] + k) if p["dir"] == "A" else (p["r"] + k, p["c"])
                cob[cell] = cob.get(cell, 0) + 1
        soltas = sum(1 for (r, c), n in cob.items() if n == 1 and r > 0 and c > 0)
        nota = -len(vazias) * 6 - soltas * 1.5
        for p in pal:
            w = p["palavra"]
            if len(w) == 2:
                nota -= 5 if w in DUAS_ARTIF else 3
            elif w in pref:
                nota += len(w) + (2 if len(w) >= 4 else 0)
            else:
                nota -= 3
        melhores.append((nota, g, pal))
    if not melhores:
        sys.exit("ERRO cruzadinha: nenhuma grade fechou. Aumente --tempo.")
    nota, g, pal = max(melhores, key=lambda x: x[0])
    json.dump({"nivel": a.nivel, "grade": g, "palavras": pal}, open(a.saida, "w", encoding="utf-8"), ensure_ascii=False)
    print(f"ESCOLHIDA entre {len(melhores)} grades ({time.time() - t0:.0f}s) -> {a.saida}")
    for row in g:
        print("  " + " ".join("#" if x == "Q" else x for x in row))
    print("PALAVRAS (escreva uma dica para cada uma):")
    for p in sorted(pal, key=lambda p: (len(p["palavra"]), p["palavra"])):
        extra = "" if p["palavra"] in pref or len(p["palavra"]) == 2 else "  (apoio, nível acima)"
        duo = "  (divide a casa)" if sum(1 for o in pal if o["q"] == p["q"]) == 2 else ""
        print(f"  {p['palavra']:<11} {'→' if p['dir'] == 'A' else '↓'}{extra}{duo}")


# ---------- desenhar ----------
FONTE_MIN = 2.29  # mm (6,5 pt)
LARG = 0.72
PASTEL = ["#cfe8ff", "#d6f5d6", "#ffe0b3", "#f9d0e6", "#fff3a8", "#e3d7ff", "#c8f0ec"]
SETA_BA = ('<svg class="sb" viewBox="0 0 10 10"><path d="M2.5 0 V6.5 H8" fill="none" stroke="#c2255c" stroke-width="1.6"/>'
           '<path d="M7 4 L10 6.5 L7 9 Z" fill="#c2255c"/></svg>')
SETA_BD = ('<svg class="sb" viewBox="0 0 10 10"><path d="M0 2.5 H6.5 V8" fill="none" stroke="#c2255c" stroke-width="1.6"/>'
           '<path d="M4 7 L6.5 10 L9 7 Z" fill="#c2255c"/></svg>')


def _svg(assets, nome):
    d = os.path.join(assets, nome, "Color")
    if not os.path.isdir(d):
        return None
    f = sorted(x for x in os.listdir(d) if x.endswith(".svg"))
    return os.path.join(d, f[0]) if f else None


def cruzadinha_html(cfg, assets):
    dados = json.load(open(cfg["grade"], encoding="utf-8"))
    dicas = {normaliza(k): v for k, v in cfg["dicas"].items()}
    g, pal = dados["grade"], dados["palavras"]
    R, C = len(g), len(g[0])
    faltam = [p["palavra"] for p in pal if p["palavra"] not in dicas]
    if faltam:
        sys.exit("ERRO cruzadinha: faltam dicas para: " + ", ".join(faltam))
    sem = [k for k, v in dicas.items() if v["tipo"] == "figura" and not _svg(assets, v["figura"])]
    if sem:
        sys.exit("ERRO cruzadinha: figura não encontrada para: " + ", ".join(sem))
    longas = [v["texto"] for v in dicas.values() if v["tipo"] == "texto"
              and max(len(x) for x in v["texto"].replace("___", "XX").split()) > 7]
    if longas:
        print("CRUZADINHA - AVISO dica com palavra longa:", " | ".join(longas))
    cm = min(186 / C, 240 / R, 24)
    uri = lambda n: "data:image/svg+xml;base64," + base64.b64encode(open(_svg(assets, n), "rb").read()).decode()
    porq, marca = {}, {}
    for p in pal:
        porq.setdefault(tuple(p["q"]), []).append(p)
        if p["seta"] == "dobrada":
            marca[(p["r"], p["c"])] = SETA_BA if p["dir"] == "A" else SETA_BD

    def conteudo(p, meia):
        d = dicas[p["palavra"]]
        alt = cm * (0.5 if meia else 1) - (0.8 if meia else 1.2)
        if d["tipo"] == "figura":
            sz = min(cm * 0.7, alt)
            return f'<img src="{uri(d["figura"])}" style="width:{sz:.2f}mm;height:{sz:.2f}mm">'
        t = d["texto"]
        lw = max(len(x) for x in t.replace("___", "XX").split())
        fs = max(FONTE_MIN, min(cm * (0.15 if meia else 0.17), (cm - 3.2) / (lw * LARG)))
        return f'<div class="tx" style="font-size:{fs:.2f}mm">{t.replace("___", chr(60) + "span class=lac></span>")}</div>'

    def seta(p):
        if p["seta"] == "dobrada":
            return ""
        return '<span class="ar ra">&#9654;</span>' if p["saida"] == "dir" else '<span class="ar rd">&#9660;</span>'

    def tabela(resp):
        out = []
        for r in range(R):
            out.append("<tr>")
            for c in range(C):
                if g[r][c] != "Q":
                    out.append(f'<td class="L"><div class="in">{marca.get((r, c), "")}{g[r][c] if resp else ""}</div></td>')
                    continue
                bg = PASTEL[(r * 3 + c * 5) % len(PASTEL)]
                ps = porq.get((r, c), [])
                if not ps:
                    out.append(f'<td class="Q V" style="background:{bg}"><div class="in"></div></td>')
                elif len(ps) == 1:
                    p = ps[0]
                    out.append(f'<td class="Q" style="background:{bg}"><div class="in"><div class="h1 {p["saida"]}">'
                               f'{conteudo(p, False)}</div>{seta(p)}</div></td>')
                else:
                    cima = next((p for p in ps if p["saida"] == "dir"), ps[0])
                    baixo = next(p for p in ps if p is not cima)
                    bg2 = PASTEL[(r * 3 + c * 5 + 3) % len(PASTEL)]
                    out.append(f'<td class="Q" style="background:{bg}"><div class="in duo">'
                               f'<div class="h2 {cima["saida"]}">{conteudo(cima, True)}{seta(cima)}</div>'
                               f'<div class="h2 {baixo["saida"]}" style="background:{bg2}">{conteudo(baixo, True)}{seta(baixo)}</div>'
                               f'</div></td>')
            out.append("</tr>")
        return "\n".join(out)

    print(f"CRUZADINHA - grade {R}x{C} | {len(pal)} palavras | figuras {sum(1 for v in dicas.values() if v['tipo'] == 'figura')}")
    css = f""".cz table {{ border-collapse:collapse; margin:0 auto; table-layout:fixed; width:{cm*C:.2f}mm; border:2px solid #222; }}
.cz td {{ width:{cm:.2f}mm; height:{cm:.2f}mm; padding:0; border:1px solid #444; overflow:hidden; box-sizing:border-box; }}
.cz td.L {{ background:#fff; font-size:{cm*0.55:.2f}mm; font-weight:bold; color:#1c7ed6; text-align:center; }}
.cz .in {{ width:100%; height:{cm-0.3:.2f}mm; position:relative; display:flex; align-items:center; justify-content:center; overflow:hidden; }}
.cz .in.duo {{ flex-direction:column; }}
.cz .h1 {{ width:100%; height:100%; display:flex; align-items:center; justify-content:center; box-sizing:border-box; }}
.cz .h2 {{ width:100%; height:50%; position:relative; display:flex; align-items:center; justify-content:center; box-sizing:border-box; }}
.cz .h2 + .h2 {{ border-top:1px solid #444; }}
.cz .dir {{ padding-right:2.2mm; }} .cz .baixo {{ padding-bottom:1.8mm; }}
.cz .h2.dir {{ padding-right:2mm; }} .cz .h2.baixo {{ padding-bottom:1.4mm; }}
.cz .tx {{ font-weight:bold; line-height:1.05; text-align:center; padding:0 0.4mm; }}
.cz .lac {{ display:inline-block; width:1.7em; border-bottom:0.3mm solid #222; }}
.cz .ar {{ position:absolute; color:#c2255c; font-size:{cm*0.18:.2f}mm; line-height:1; }}
.cz .ra {{ right:0.2mm; top:50%; transform:translateY(-50%); }} .cz .rd {{ bottom:0; left:50%; transform:translateX(-50%); }}
.cz .sb {{ position:absolute; left:0.3mm; top:0.3mm; width:{cm*0.3:.2f}mm; height:{cm*0.3:.2f}mm; }}
.cz td.V {{ background-image:radial-gradient(#ffffff 18%, transparent 19%); background-size:3mm 3mm; }}
.cz .banner {{ background:#c2255c; color:#fff; border-radius:3.5mm; text-align:center; padding:1.2mm 5mm 1.6mm; margin-bottom:4mm; }}
.cz .banner h1 {{ margin:0; font-size:16pt; color:#ffd43b; text-shadow:2px 2px 0 rgba(0,0,0,.35); }}
.cz .banner p {{ margin:0.2mm 0 0; font-size:9.5pt; }}"""
    titulo = cfg.get("titulo", "Cruzadinha")
    instr = cfg.get("instrucao") or "Escreva o nome de cada definição nos quadradinhos seguindo as setas."
    jogo = f'<div class="sec cz"><div class="banner"><h1>{titulo}</h1><p>{instr}</p></div><table>{tabela(False)}</table></div>'
    resp = f'<div class="sec cz"><div class="banner"><h1>Respostas</h1><p>{titulo}</p></div><table>{tabela(True)}</table></div>'
    return css, jogo, resp, [p["palavra"] for p in pal]


if __name__ == "__main__":
    ap = argparse.ArgumentParser()
    sp = ap.add_subparsers(dest="cmd", required=True)
    m = sp.add_parser("montar")
    m.add_argument("--nivel", type=int, default=2)
    m.add_argument("--dicionario", default="palavras_cruzadinha.tsv")
    m.add_argument("--excluir")
    m.add_argument("--saida", default="grade.json")
    m.add_argument("--seed", type=int, default=1)
    m.add_argument("--grades", type=int, default=300)
    m.add_argument("--tempo", type=int, default=150)
    a = ap.parse_args()
    montar(a)
```