# Dungeon Crawler

> Jogo de console em linguagem C com perspectiva top-down ASCII. Explore uma vila, atravesse três andares de masmorra e derrote o Boss final para salvar o reino de Arendor.

---

Desenvolvedores

| Nome | Matrícula |
|------|-----------|
| Isac Lopes | 26070073 |
| Henrique Dutra | 26070041 |
| Manoel Folha | 26070019 |

---

# História

O reino de Arendor foi consumido pelas trevas. Criaturas sombrias emergiram das profundezas da **Masmorra das Sombras**, destruindo tudo ao seu redor. O guerreiro **Kael**, último sobrevivente da Ordem da Luz, é a única esperança do reino.

Armado com coragem e a arma que escolher, Kael deve:

1. Partir da **Vila de Arendor** e se preparar para a batalha
2. Atravessar o **Primeiro Andar** — a entrada sombria da masmorra
3. Sobreviver ao **Segundo Andar** — o corredor dos espinhos
4. Enfrentar o **Terceiro Andar** — a câmara de Malachar

Somente derrotando o **Boss Malachar** a escuridão será dissipada e o reino poderá ser salvo.

---

## Como Jogar

### Objetivo
Avance pelos três andares da masmorra, resolva os desafios de cada fase e derrote o Boss Final no terceiro andar.

### Controles

| Tecla | Ação |
|-------|------|
| `w` | Mover para **cima** |
| `s` | Mover para **baixo** |
| `a` | Mover para a **esquerda** |
| `d` | Mover para a **direita** |
| `i` | **Interagir** com o objeto à frente (porta, botão, NPC) |
| `o` | **Atacar** na célula à frente |

### Regras
- O jogador começa com **3 vidas**
- Ao perder uma vida, a fase **reinicia do início**
- Ao perder todas as vidas, aparece a tela de **Game Over** e o jogo volta ao menu
- A arma escolhida na Vila **persiste por toda a partida**

### Armas

| Arma | Área de Ataque |
|------|---------------|
| ⚔️ Espada | Bloco **3×2** diretamente à frente |
| 🏹 Arco e Flecha | **4 células** em linha reta à frente |
| 🪄 Cajado | Todas as **8 células** ao redor do jogador |

### Estrutura das Fases

```
Menu Principal
    └── Vila de Arendor   (10×10) — escolha a arma com o NPC
        └── Andar 1       (10×10) — tutorial: chave, porta e caixas
            └── Andar 2   (15×15) — espinhos, botão e monstros aleatórios
                └── Andar 3 (25×25) — desafio final com Boss
```

---

## 🗺️ Símbolos do Mapa

| Símbolo | Significado |
|---------|-------------|
| `>` `<` `^` `v` | Jogador (indica a direção que está olhando) |
| `*` | Parede — intransponível |
| `#` | Espinho — o jogador **morre** ao pisar |
| `k` | Caixa — bloqueia passagem, mas pode ser **destruída com ataque** |
| `O` | Botão — pressione com `i` para **ativar um efeito** no mapa |
| `D` | Porta fechada — abra com uma chave usando `i` |
| `@` | Chave — **coletada automaticamente** ao passar por cima |
| `=` | Porta aberta — pode passar livremente |
| `L` | Escada — leva ao **próximo andar** |
| `N` | NPC — interaja com `i` para **escolher sua arma** |
| `X` | Monstro Tipo 1 — movimentação **aleatória** |
| `Y` | Monstro Tipo 2 — **persegue** o jogador |
| `Z` | Boss Final — comportamento **único** |

---

## Boss Final — Malachar

Malachar é o inimigo final do jogo e possui um comportamento **completamente diferente** dos monstros comuns, com dois modos que alternam automaticamente a cada turno.

---

### Modo 1 — Perseguição

Na maioria dos turnos, Malachar avança **1 passo por turno** em direção ao jogador, calculando qual eixo tem a maior distância:

```
Se a distância vertical for maior   → ele avança na vertical
Se a distância horizontal for maior → ele avança na horizontal
```

**Exemplo:**
```
Jogador está em  (3, 8)
Malachar está em (7, 6)

Distância vertical:   |3 - 7| = 4
Distância horizontal: |8 - 6| = 2

4 > 2 → Malachar sobe 1 linha → vai para (6, 6)
```

Esse comportamento é previsível — o jogador consegue fugir se souber a direção de onde o boss vem.

---

### Modo 2 — Teleporte ⚡ (a cada 5 turnos)

A cada **5 turnos**, Malachar **desaparece** e **reaparece** em uma célula aleatória dentro de um raio de 3 casas ao redor do jogador, exibindo o aviso:

```
[!!!] MALACHAR TELEPORTOU!
```

Isso significa que mesmo fugindo para longe, o boss pode **surgir do nada** ao seu lado no próximo momento.

**Área de teleporte (raio 3 ao redor do jogador):**
```
. . . . . . .
. . . . . . .
. . . T . . .     T = possível posição após teleporte
. . . J . . .     J = jogador
. . . . . . .
. . . . . . .
. . . . . . .
```

> O boss só teleporta para células **livres** — nunca aparece dentro de uma parede ou em cima de outro monstro.

---

### Por que esse comportamento é único?

| | Monstro X | Monstro Y | Boss Z — Malachar |
|---|---|---|---|
| Movimento | Aleatório total | Perseguição simples | Perseguição + teleporte |
| É previsível? | Sim | Sim | **Parcialmente** |
| Pode surgir do nada? | Não | Não | **Sim, a cada 5 turnos** |
| Exige reação rápida? | Não | Não | **Sim** |

A combinação de perseguição constante com teleportes imprevisíveis força o jogador a **nunca ficar parado** e a prestar atenção em todos os lados ao mesmo tempo.

---

## Declaração sobre Uso de IA Generativa

Este projeto utilizou **IA generativa (Claude — Anthropic)** como ferramenta de apoio ao desenvolvimento, nos seguintes aspectos:

- **Estruturação do código:** a IA sugeriu a organização em blocos e funções separadas, que foi compreendida, adaptada e aprovada pela equipe
- **Revisão de lógica:** trechos de lógica (movimento do jogador, IA dos monstros) foram discutidos com a IA para entender o raciocínio por trás das implementações
- **Depuração:** a IA auxiliou na identificação de erros de compilação e na explicação do que cada mensagem de erro significava
