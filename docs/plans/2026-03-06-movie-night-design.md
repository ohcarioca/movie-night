# Movie Night — Design Document

## Problema

Casais perdem tempo discutindo "o que assistir" e "o que comer". Movie Night resolve ambos de forma divertida com mini-jogos de cassino.

## Conceito

App single-page com 3 mini-jogos de cassino jogados em sequência:

1. **Dado 3D** — define o mood da noite
2. **Caça-Níquel** — sorteia o filme
3. **Roda da Fortuna (estilo Tigrinho)** — sorteia a comida

Sem login, sem backend, sem cadastro. Abre e joga.

## Fluxo do Usuário

```
[Tela inicial — tema cassino neon]
        ↓ botão "COMEÇAR A NOITE"
[FASE 1: Dado 3D]
  → usuário clica/toca para jogar o dado
  → dado 3D gira e para em um mood
  → moods: Romântico, Adrenalina, Nostalgia, Comédia, Suspense, Choradeira
        ↓ transição automática
[FASE 2: Caça-Níquel]
  → 3 rolos verticais com filmes
  → usuário puxa alavanca virtual
  → rolos param um a um (esquerda → direita) com suspense
  → o rolo do meio define o filme sorteado
        ↓ transição automática
[FASE 3: Roda da Fortuna — estilo Tigrinho]
  → roda colorida com segmentos de comida
  → moldura dourada com luzes piscando ao redor
  → seta fixa no topo
  → usuário clica para girar
  → roda desacelera dramaticamente e para na comida
        ↓ transição
[RESULTADO FINAL]
  → card bonito com mood + filme + comida da noite
  → botão "NÃO CURTI, GIRAR TUDO DE NOVO!"
```

## Estética: Neon Cassino Retrô

- **Fundo**: escuro, gradientes profundos (preto → roxo escuro → azul noite)
- **Tipografia**: fonte display bold estilo Vegas/retrô para títulos, sans-serif clean para corpo
- **Cores neon**: rosa (#ff2d75), ciano (#00f0ff), amarelo (#ffe14d), dourado (#ffd700)
- **Efeitos**: glow neon pulsante, partículas, luzes piscando
- **Roda da Fortuna**: visual Tigrinho — moldura dourada ornamentada, luzinhas circulares, segmentos coloridos vibrantes
- **Responsivo**: mobile-first (casal no sofá com celular)

## Base de Filmes

100 melhores filmes de todos os tempos segundo Empire (via Rolling Stone BR):

1. O Senhor dos Anéis: A Sociedade do Anel (2001)
2. Star Wars – O Império Contra-Ataca (1980)
3. O Poderoso Chefão (1972)
4. Batman: O Cavaleiro das Trevas (2008)
5. Um Sonho de Liberdade (1994)
6. Tubarão (1975)
7. Pulp Fiction: Tempo de Violência (1994)
8. Vingadores: Guerra Infinita (2018)
9. Os Caçadores da Arca Perdida (1981)
10. Os Bons Companheiros (1990)
11. Star Wars: Uma Nova Esperança (1977)
12. Mad Max: Estrada da Fúria (2015)
13. De Volta para o Futuro (1985)
14. O Poderoso Chefão: Parte II (1974)
15. Jurassic Park (1993)
16. Blade Runner (1982)
17. Aliens, O Resgate (1986)
18. Parasita (2019)
19. A Origem (2010)
20. Matrix (1999)
21. Vingadores: Ultimato (2019)
22. 2001: Uma Odisseia no Espaço (1968)
23. O Exterminador do Futuro 2 (1991)
24. Duro de Matar (1988)
25. Clube da Luta (1999)
26. Fogo Contra Fogo (1995)
27. Apocalypse Now (1979)
28. Indiana Jones e a Última Cruzada (1989)
29. O Senhor dos Anéis: O Retorno do Rei (2003)
30. O Senhor dos Anéis: As Duas Torres (2002)
31. O Iluminado (1980)
32. Brilho Eterno de uma Mente Sem Lembranças (2004)
33. Se7en – Os Sete Crimes Capitais (1995)
34. Três Homens em Conflito (1966)
35. Gladiador (2000)
36. Casablanca (1942)
37. Cidadão Kane (1941)
38. O Silêncio dos Inocentes (1991)
39. Doze Homens e Uma Sentença (1957)
40. Sangue Negro (2007)
41. A Felicidade Não se Compra (1946)
42. O Grande Lebowski (1998)
43. A Lista de Schindler (1993)
44. Faça a Coisa Certa (1989)
45. Os Caça-Fantasmas (1984)
46. Amor à Flor da Pele (2000)
47. Cantando na Chuva (1952)
48. La La Land (2016)
49. Whiplash (2014)
50. Caçadores de Emoção (1991)
51. Forrest Gump (1994)
52. O Resgate do Soldado Ryan (1998)
53. A Rede Social (2010)
54. Blade Runner 2049 (2017)
55. Guardiões da Galáxia (2014)
56. Moonlight (2016)
57. O Labirinto do Fauno (2006)
58. Lawrence da Arábia (1962)
59. Corra! (2017)
60. A Viagem de Chihiro (2001)
61. Vertigo (1958)
62. Um Estranho no Ninho (1975)
63. Sete Samurais (1954)
64. Janela Indiscreta (1954)
65. Cidade dos Sonhos (2001)
66. Trainspotting (1996)
67. Um Lugar Silencioso (2018)
68. A Chegada (2016)
69. Star Wars – O Retorno de Jedi (1983)
70. Homem-Aranha: No Aranhaverso (2018)
71. Up – Altas Aventuras (2009)
72. Psicose (1960)
73. Os Suspeitos (1995)
74. Thor: Ragnarok (2017)
75. Encontros e Desencontros (2003)
76. Todo Mundo Quase Morto (2004)
77. Pantera Negra (2018)
78. O Exorcista (1973)
79. Titanic (1997)
80. Onde os Fracos Não Têm Vez (2007)
81. Los Angeles: Cidade Proibida (1997)
82. E.T. – O Extraterrestre (1982)
83. O Exterminador do Futuro (1984)
84. O Profissional (1994)
85. Retrato de uma Jovem em Chamas (2020)
86. Scott Pilgrim Contra o Mundo (2010)
87. Donnie Darko (2001)
88. O Segredo de Brokeback Mountain (2005)
89. O Fabuloso Destino de Amélie Poulain (2001)
90. Paddington 2 (2017)
91. Feitiço do Tempo (1993)
92. Cães de Aluguel (1992)

## Lista de Comidas (segmentos da Roda)

1. 🍕 Pizza
2. 🍿 Pipoca
3. 🌭 Hot Dog
4. 🍔 Hambúrguer
5. 🍣 Sushi / Japa
6. 🧁 Doces & Chocolates
7. 🌮 Tacos / Mexicano
8. 🍝 Macarrão
9. 🥤 Açaí
10. 🍗 Frango Frito
11. 🧀 Tábua de Frios
12. 🥪 Sanduíche
13. 🍩 Donuts
14. 🫕 Fondue
15. 🥟 Esfiha / Árabe
16. 🍜 Lámen / Ramen

## Stack Técnica

- **HTML/CSS/JS puro** — um arquivo HTML self-contained
- Zero dependências externas (fontes via Google Fonts CDN)
- Animações CSS + JavaScript vanilla
- Hospedagem: GitHub Pages
- Mobile-first, responsivo

## Moods do Dado

| Face | Mood | Emoji |
|------|------|-------|
| 1 | Romântico | 💕 |
| 2 | Adrenalina | 🔥 |
| 3 | Nostalgia | 🥹 |
| 4 | Comédia | 😂 |
| 5 | Suspense | 😱 |
| 6 | Choradeira | 😭 |
