# Passatempos Infantis para o Claude

> 🇺🇸 *A Claude skill that generates kids' activity sheets (crossword, word search, cryptogram, math square, scrambled words and maze) as printable A4 PDFs, with the answers in a separate file. Docs in Portuguese.*

Uma **skill do Claude** que gera **passatempos infantis prontos para imprimir**, no estilo das revistinhas de passatempo: cruzadinha ilustrada, caça-palavras, criptograma, desafio de contas, palavras embaralhadas e labirinto. Saem dois PDFs A4, um com os jogos e outro com as respostas.

![Prévia das 4 folhas geradas](examples/previa.png)

## Destaques

- **4 níveis de dificuldade**, do 1º ano ao 7º ano em diante, com regras por nível: tamanho das palavras, tipos de dica e direções do caça-palavras.
- **Não repete conteúdo:** guarda um histórico das palavras, temas e curiosidades já usados (na memória do Claude, quando disponível) e evita repeti-los nas próximas gerações.
- **Economiza papel:** jogos menores dividem a mesma folha, e o gerador mede a altura real de cada um para decidir.
- **Respostas conferíveis:** o gerador imprime um relatório (palavras colocadas, erros de encaixe) que o Claude usa para corrigir antes de entregar.
- **Modo automático:** basta pedir "faz uns passatempos". Detalhes como "nível 3", "só caça-palavras" ou "tema dinossauros" também funcionam.

## Exemplo

Na pasta [`examples/`](examples/):
- [`passatempos-exemplo.pdf`](examples/passatempos-exemplo.pdf): as 4 folhas de jogos, nível 2
- [`passatempos-exemplo_respostas.pdf`](examples/passatempos-exemplo_respostas.pdf): as respostas
- [`entrada-exemplo.json`](examples/entrada-exemplo.json): a entrada que gerou o exemplo

## Instalação

**claude.ai:** compacte a pasta `skill/passatempos-infantis` em um `.zip` e envie na área de Skills das configurações.

**Claude Code:** copie a pasta `skill/passatempos-infantis` para `~/.claude/skills/passatempos-infantis`.

O ambiente de execução precisa de **Python 3**, **git** e **Google Chrome ou Chromium** (usado para gerar o PDF). Na primeira execução, a skill baixa as ilustrações do Fluent Emoji.

## Como funciona

1. O Claude escolhe o nível, os jogos e o tema e escreve um `entrada.json` com as palavras, as dicas e os textos.
2. O gerador em Python (o código está dentro do `SKILL.md`) monta a grade da cruzadinha, o diagrama do caça-palavras, o criptograma, o quadrado mágico de contas e o labirinto, e renderiza tudo em HTML.
3. O Chrome em modo headless converte o HTML em dois PDFs A4.
4. O Claude confere o relatório, corrige o que for preciso e registra no histórico o que foi usado.

## Créditos

Ilustrações: [Fluent Emoji](https://github.com/microsoft/fluentui-emoji), da Microsoft, sob licença MIT. Elas são baixadas na hora e não ficam neste repositório.

## Licença

[MIT](LICENSE) · Feito por [Eudes Junior](https://www.linkedin.com/in/eudescj), Analista de Dados.
