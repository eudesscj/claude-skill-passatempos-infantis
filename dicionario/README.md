# Dicionário da cruzadinha

`palavras_cruzadinha.tsv` tem cerca de 4.650 palavras em português do Brasil, cada uma com um nível de dificuldade de 1 a 5. Colunas: `palavra` (maiúsculas, sem acento), `nivel`, `classe` (classe gramatical) e `freq` (frequência no corpus).

## Como foi montado

1. Base: as palavras mais frequentes do [LexPorBR Infantil](https://lexicodoportugues.com/infantil/), uma base lexical feita com legendas de filmes e séries infantis e de família. Só entrou a forma de dicionário (singular, masculino, infinitivo), de 3 a 10 letras.
2. Filtros: saíram nomes próprios (pelo dicionário do LibreOffice pt_BR), palavras com k, w ou y, e uma lista de bloqueio de palavras impróprias para crianças.
3. Revisão manual: saíram cerca de 600 palavras de adulto, religião, política, violência e corpo, palavras em inglês e restos de legenda. Entraram cerca de 200 palavras curtas menos frequentes, boas para fechar a grade, e uma lista de palavras infantis feita à mão.
4. Nível: pela frequência. As 800 mais comuns ficaram no nível 1, e assim por diante até o nível 5.

## Como usar

No claude.ai, adicione este arquivo como documento do Projeto onde a skill roda. A skill lê o documento antes de montar a cruzadinha.

## Licença

Este dicionário é derivado do LexPorBR Infantil e, por isso, segue a licença dele: [Creative Commons Atribuição-NãoComercial-CompartilhaIgual 4.0 Internacional (CC BY-NC-SA 4.0)](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.pt_BR). Pode ser usado e adaptado para fins não comerciais, citando a fonte e mantendo a mesma licença. O restante do repositório segue a licença MIT.
