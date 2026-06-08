#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

#define VILA_L        10
#define VILA_C        10
#define A1_L          10
#define A1_C          10
#define A2_L          15
#define A2_C          15
#define A3_L          25
#define A3_C          25
#define MAX_C         26
#define MAX_MONSTROS  30

int  jLinha;
int  jColuna;
int  jVidas;
int  jArma;
char jDirecao;
int  jChaves;

int mLinha[MAX_MONSTROS];
int mColuna[MAX_MONSTROS];
int mTipo[MAX_MONSTROS];
int mVivo[MAX_MONSTROS];
int numMonstros = 0;

int turnosBoss = 0;

char mapaVila[VILA_L][MAX_C];
char mapaVilaOrig[VILA_L][MAX_C];
char mapaA1[A1_L][MAX_C];
char mapaA1Orig[A1_L][MAX_C];
char mapaA2[A2_L][MAX_C];
char mapaA2Orig[A2_L][MAX_C];
char mapaA3[A3_L][MAX_C];
char mapaA3Orig[A3_L][MAX_C];

int botaoA2Ativado = 0;
int botaoA3Ativado = 0;

int frenteLinha;
int frenteColuna;

/* limparTela */
void limparTela() {
#ifdef _WIN32
    system("cls");
#else
    system("clear");
#endif
}

/* pausar */
void pausar() {
    printf("\n  [Pressione ENTER para continuar...]");
    int c;
    while ((c = getchar()) != '\n' && c != EOF);
    getchar();
}

/* lerChar */
char lerChar() {
    char c;
    scanf(" %c", &c);
    return c;
}

/* lerInt */
int lerInt() {
    int n;
    scanf("%d", &n);
    while (getchar() != '\n');
    return n;
}

/* exibirHUD */
void exibirHUD() {
    int i;
    const char *nomeArma[] = { "---", "Espada", "Arco e Flecha", "Cajado" };
    printf("============================================================\n");
    printf("  VIDAS: ");
    for (i = 0; i < jVidas; i++)  printf("[*] ");
    for (i = jVidas; i < 3; i++)  printf("[ ] ");
    printf("  |  ARMA: %s  |  CHAVES: %d\n", nomeArma[jArma], jChaves);
    printf("============================================================\n");
}

/* exibirMapa */
void exibirMapa(char mapa[][MAX_C], int linhas, int colunas) {
    int i, j;
    for (i = 0; i < linhas; i++) {
        printf("  ");
        for (j = 0; j < colunas; j++) {
            if (i == jLinha && j == jColuna)
                printf("%c", jDirecao);
            else
                printf("%c", mapa[i][j]);
        }
        printf("\n");
    }
    printf("\n");
}

/* exibirMenu */
void exibirMenu() {
    limparTela();
    printf("\n");
    printf("  =====================================================\n");
    printf("   ____  _   _ _   _  ____ _____ ___  _   _ \n");
    printf("  |  _ \\| | | | \\ | |/ ___| ____/ _ \\| \\ | |\n");
    printf("  | | | | | | |  \\| | |  _|  _|| | | |  \\| |\n");
    printf("  | |_| | |_| | |\\  | |_| | |__| |_| | |\\  |\n");
    printf("  |____/ \\___/|_| \\_|\\____|_____\\___/|_| \\_|\n");
    printf("   ____ ____      ___        ____  _     _____ ____  \n");
    printf("  / ___|  _ \\ __ / \\ \\      / /  \\| |   | ____|  _ \\ \n");
    printf(" | |   | |_) / _` \\ \\ \\ /\\ / /| . ` |   |  _| | |_) |\n");
    printf(" | |___|  _ < (_| |\\ V  V / | |\\  |   | |___|  _ < \n");
    printf("  \\____|_| \\_\\__,_| \\_/\\_/  |_| \\_|   |_____|_| \\_\\\n");
    printf("\n");
    printf("  =====================================================\n\n");
    printf("     1. Jogar\n");
    printf("     2. Tutorial\n");
    printf("     3. Sair\n\n");
    printf("  Escolha: ");
}

/* exibirTutorial */
void exibirTutorial() {
    limparTela();
    printf("  ============== TUTORIAL ==============\n\n");
    printf("  HISTORIA:\n");
    printf("  O reino de Arendor foi consumido pelas trevas.\n");
    printf("  O guerreiro Kael deve penetrar na Masmorra das Sombras,\n");
    printf("  vencer tres andares de terror e destruir o Boss Malachar.\n\n");
    printf("  SIMBOLOS:\n");
    printf("    > < ^ v  Jogador (direcao que esta olhando)\n");
    printf("    *        Parede  (intransponivel)\n");
    printf("    #        Espinho (voce morre ao pisar)\n");
    printf("    k        Caixa   (bloqueia, destruivel com ataque)\n");
    printf("    O        Botao   (interaja com 'i' para ativar)\n");
    printf("    D        Porta fechada (abra com chave usando 'i')\n");
    printf("    @        Chave   (coletada ao passar por cima)\n");
    printf("    =        Porta aberta  (pode passar)\n");
    printf("    L        Escada  (avanca para o proximo andar)\n");
    printf("    N        NPC     (interaja com 'i' para escolher arma)\n");
    printf("    X        Monstro Tipo 1 - movimento aleatorio\n");
    printf("    Y        Monstro Tipo 2 - persegue o jogador\n");
    printf("    Z        Boss Final\n\n");
    printf("  CONTROLES:\n");
    printf("    w  Mover para CIMA\n");
    printf("    s  Mover para BAIXO\n");
    printf("    a  Mover para ESQUERDA\n");
    printf("    d  Mover para DIREITA\n");
    printf("    i  INTERAGIR (porta, botao, NPC)\n");
    printf("    o  ATACAR na celula a frente\n\n");
    printf("  ARMAS:\n");
    printf("    Espada        -> bloco 3x2 a frente\n");
    printf("    Arco e Flecha -> 4 celulas em linha reta\n");
    printf("    Cajado        -> 8 celulas ao redor\n\n");
    pausar();
}

/* exibirGameOver */
void exibirGameOver() {
    limparTela();
    printf("\n\n");
    printf("  +------------------------------------------+\n");
    printf("  |                                          |\n");
    printf("  |          G A M E   O V E R               |\n");
    printf("  |                                          |\n");
    printf("  |  Kael sucumbiu as sombras de Arendor.   |\n");
    printf("  |  O reino foi perdido para as trevas...  |\n");
    printf("  |                                          |\n");
    printf("  +------------------------------------------+\n\n");
    pausar();
}

/* exibirVitoria */
void exibirVitoria() {
    limparTela();
    printf("\n\n");
    printf("  +------------------------------------------+\n");
    printf("  |                                          |\n");
    printf("  |          V I T O R I A  !                |\n");
    printf("  |                                          |\n");
    printf("  |  Malachar foi destruido!                 |\n");
    printf("  |  A escuridao se dissipou sobre Arendor.  |\n");
    printf("  |  Kael voltou como heroi eterno do reino. |\n");
    printf("  |                                          |\n");
    printf("  |  Obrigado por jogar DUNGEON CRAWLER!     |\n");
    printf("  |                                          |\n");
    printf("  +------------------------------------------+\n\n");
    pausar();
}

/* ehObstaculo */
int ehObstaculo(char celula) {
    return (celula == '*' || celula == 'k' || celula == 'D');
}

/* temMonstroEm */
int temMonstroEm(int l, int c) {
    int i;
    for (i = 0; i < numMonstros; i++)
        if (mVivo[i] && mLinha[i] == l && mColuna[i] == c)
            return 1;
    return 0;
}

/* calcularFrente */
void calcularFrente() {
    frenteLinha  = jLinha;
    frenteColuna = jColuna;
    if      (jDirecao == '^') frenteLinha--;
    else if (jDirecao == 'v') frenteLinha++;
    else if (jDirecao == '<') frenteColuna--;
    else if (jDirecao == '>') frenteColuna++;
}

/* moverJogador */
int moverJogador(char mapa[][MAX_C], int linhas, int colunas,
                 int dl, int dc, char dir) {
    jDirecao = dir;

    int novaL = jLinha + dl;
    int novaC = jColuna + dc;

    if (novaL < 0 || novaL >= linhas || novaC < 0 || novaC >= colunas)
        return 0;

    char dest = mapa[novaL][novaC];

    if (ehObstaculo(dest))
        return 0;

    if (temMonstroEm(novaL, novaC)) {
        jVidas--;
        printf("\n  [!!] Voce colidiu com um monstro! Vidas: %d\n", jVidas);
        return 1;
    }

    jLinha  = novaL;
    jColuna = novaC;

    if (dest == '#') {
        jVidas--;
        printf("\n  [!!] Voce pisou num espinho! Vidas: %d\n", jVidas);
        return 1;
    }

    if (dest == '@') {
        jChaves++;
        mapa[novaL][novaC] = ' ';
        printf("\n  [+] Chave coletada! Total: %d\n", jChaves);
    }

    if (dest == 'L')
        return 2;

    return 0;
}

/* interagirComMapa */
int interagirComMapa(char mapa[][MAX_C], int linhas, int colunas, int fase) {
    calcularFrente();
    int fl = frenteLinha;
    int fc = frenteColuna;

    if (fl < 0 || fl >= linhas || fc < 0 || fc >= colunas)
        return 0;

    char alvo = mapa[fl][fc];

    if (alvo == 'D') {
        if (jChaves > 0) {
            mapa[fl][fc] = '=';
            jChaves--;
            printf("\n  [+] Porta aberta!\n");
        } else {
            printf("\n  [!] Voce precisa de uma chave!\n");
        }
    } else if (alvo == 'O') {
        mapa[fl][fc] = 'o';
        if (fase == 2) botaoA2Ativado = 1;
        if (fase == 3) botaoA3Ativado = 1;
        printf("\n  [!] *CLIQUE* Botao pressionado!\n");
    } else if (alvo == 'N') {
        return 1;
    } else {
        printf("\n  [!] Nada para interagir aqui.\n");
    }

    return 0;
}

/* atacar */
void atacar(char mapa[][MAX_C], int linhas, int colunas) {
    int alvos[12][2];
    int numAlvos = 0;
    int l = jLinha;
    int c = jColuna;
    int dl, dc, dist, k;

    if (jArma == 1) {
        if (jDirecao == '^') {
            for (dl = -1; dl >= -2; dl--)
                for (dc = -1; dc <= 1; dc++) {
                    alvos[numAlvos][0] = l + dl;
                    alvos[numAlvos][1] = c + dc;
                    numAlvos++;
                }
        } else if (jDirecao == 'v') {
            for (dl = 1; dl <= 2; dl++)
                for (dc = -1; dc <= 1; dc++) {
                    alvos[numAlvos][0] = l + dl;
                    alvos[numAlvos][1] = c + dc;
                    numAlvos++;
                }
        } else if (jDirecao == '<') {
            for (dc = -1; dc >= -2; dc--)
                for (dl = -1; dl <= 1; dl++) {
                    alvos[numAlvos][0] = l + dl;
                    alvos[numAlvos][1] = c + dc;
                    numAlvos++;
                }
        } else {
            for (dc = 1; dc <= 2; dc++)
                for (dl = -1; dl <= 1; dl++) {
                    alvos[numAlvos][0] = l + dl;
                    alvos[numAlvos][1] = c + dc;
                    numAlvos++;
                }
        }
    } else if (jArma == 2) {
        dl = 0; dc = 0;
        if      (jDirecao == '^') dl = -1;
        else if (jDirecao == 'v') dl =  1;
        else if (jDirecao == '<') dc = -1;
        else                      dc =  1;

        for (dist = 1; dist <= 4; dist++) {
            alvos[numAlvos][0] = l + dl * dist;
            alvos[numAlvos][1] = c + dc * dist;
            numAlvos++;
        }
    } else if (jArma == 3) {
        int off[8][2] = {
            {-1,-1},{-1,0},{-1,1},
            { 0,-1},       { 0,1},
            { 1,-1},{ 1,0},{ 1,1}
        };
        for (k = 0; k < 8; k++) {
            alvos[numAlvos][0] = l + off[k][0];
            alvos[numAlvos][1] = c + off[k][1];
            numAlvos++;
        }
    }

    int acertou = 0;
    int a, m;
    for (a = 0; a < numAlvos; a++) {
        int al = alvos[a][0];
        int ac = alvos[a][1];

        if (al < 0 || al >= linhas || ac < 0 || ac >= colunas)
            continue;

        if (mapa[al][ac] == 'k') {
            mapa[al][ac] = ' ';
            printf("\n  [+] Caixa destruida!\n");
            acertou = 1;
        }

        for (m = 0; m < numMonstros; m++) {
            if (mVivo[m] && mLinha[m] == al && mColuna[m] == ac) {
                mVivo[m] = 0;
                if (mTipo[m] == 3)
                    printf("\n  [!!!] BOSS DERROTADO!\n");
                else
                    printf("\n  [+] Monstro eliminado!\n");
                acertou = 1;
            }
        }
    }

    if (!acertou)
        printf("\n  [!] Ataque no vazio...\n");
}

/* moverMonstros */
int moverMonstros(char mapa[][MAX_C], int linhas, int colunas) {
    int m;
    for (m = 0; m < numMonstros; m++) {
        if (!mVivo[m]) continue;

        int novaL = mLinha[m];
        int novaC = mColuna[m];
        int dL, dC, dir, tl, tc, tentativas, teleportou;

        if (mTipo[m] == 1) {
            dir = rand() % 4;
            if      (dir == 0) novaL--;
            else if (dir == 1) novaL++;
            else if (dir == 2) novaC--;
            else               novaC++;

        } else if (mTipo[m] == 2) {
            dL = jLinha  - mLinha[m];
            dC = jColuna - mColuna[m];
            if (abs(dL) >= abs(dC))
                novaL += (dL > 0) ? 1 : -1;
            else
                novaC += (dC > 0) ? 1 : -1;

        } else if (mTipo[m] == 3) {
            turnosBoss++;
            if (turnosBoss % 5 == 0) {
                printf("\n  [!!!] MALACHAR TELEPORTOU!\n");
                tentativas = 25;
                teleportou = 0;
                while (tentativas-- > 0 && !teleportou) {
                    tl = jLinha  + (rand() % 7) - 3;
                    tc = jColuna + (rand() % 7) - 3;
                    if (tl > 0 && tl < linhas - 1 &&
                        tc > 0 && tc < colunas - 1 &&
                        mapa[tl][tc] == ' '         &&
                        !temMonstroEm(tl, tc)        &&
                        !(tl == jLinha && tc == jColuna)) {
                        novaL = tl;
                        novaC = tc;
                        teleportou = 1;
                    }
                }
                if (!teleportou) {
                    novaL = mLinha[m];
                    novaC = mColuna[m];
                }
            } else {
                dL = jLinha  - mLinha[m];
                dC = jColuna - mColuna[m];
                if (abs(dL) >= abs(dC))
                    novaL += (dL > 0) ? 1 : -1;
                else
                    novaC += (dC > 0) ? 1 : -1;
            }
        }

        if (novaL >= 0 && novaL < linhas  &&
            novaC >= 0 && novaC < colunas &&
            mapa[novaL][novaC] == ' '     &&
            !temMonstroEm(novaL, novaC)) {
            mLinha[m]  = novaL;
            mColuna[m] = novaC;
        }

        if (mLinha[m] == jLinha && mColuna[m] == jColuna) {
            jVidas--;
            printf("\n  [!!] Um monstro te tocou! Vidas: %d\n", jVidas);
            return 1;
        }
    }
    return 0;
}

/* contarMonstrosVivos */
int contarMonstrosVivos() {
    int total = 0, i;
    for (i = 0; i < numMonstros; i++)
        if (mVivo[i]) total++;
    return total;
}

/* bossEstaVivo */
int bossEstaVivo() {
    int i;
    for (i = 0; i < numMonstros; i++)
        if (mTipo[i] == 3 && mVivo[i])
            return 1;
    return 0;
}

/* escanearMonstros */
void escanearMonstros(char mapa[][MAX_C], int linhas, int colunas) {
    int i, j;
    numMonstros = 0;
    turnosBoss  = 0;
    for (i = 0; i < linhas; i++) {
        for (j = 0; j < colunas; j++) {
            char c = mapa[i][j];
            if (c == 'X' || c == 'Y' || c == 'Z') {
                mLinha[numMonstros]  = i;
                mColuna[numMonstros] = j;
                mTipo[numMonstros]   = (c == 'X') ? 1 : (c == 'Y') ? 2 : 3;
                mVivo[numMonstros]   = 1;
                numMonstros++;
                mapa[i][j] = ' ';
            }
        }
    }
}

/* desenharMonstros */
void desenharMonstros(char mapa[][MAX_C]) {
    int m;
    for (m = 0; m < numMonstros; m++) {
        if (!mVivo[m]) continue;
        char s = (mTipo[m] == 1) ? 'X' : (mTipo[m] == 2) ? 'Y' : 'Z';
        mapa[mLinha[m]][mColuna[m]] = s;
    }
}

/* limparMonstrosDoMapa */
void limparMonstrosDoMapa(char mapa[][MAX_C]) {
    int m;
    for (m = 0; m < numMonstros; m++) {
        if (!mVivo[m]) continue;
        mapa[mLinha[m]][mColuna[m]] = ' ';
    }
}

/* inicializarVila */
void inicializarVila() {
    int i;
    const char *t[VILA_L] = {
        "**********",
        "*        *",
        "*  N     *",
        "*        *",
        "*  ****  *",
        "*        *",
        "*  ****  *",
        "*        *",
        "*       L*",
        "**********"
    };
    for (i = 0; i < VILA_L; i++) {
        strncpy(mapaVila[i],     t[i], VILA_C);
        mapaVila[i][VILA_C] = '\0';
        strncpy(mapaVilaOrig[i], t[i], VILA_C);
        mapaVilaOrig[i][VILA_C] = '\0';
    }
    jLinha   = 1;
    jColuna  = 1;
    jDirecao = '>';
    jChaves  = 0;
}

/* inicializarAndar1 */
void inicializarAndar1() {
    int i;
    const char *t[A1_L] = {
        "**********",
        "*>  k    *",
        "*   k    *",
        "* *** D L*",
        "* * @    *",
        "* *   ** *",
        "* *      *",
        "* ****   *",
        "*        *",
        "**********"
    };
    for (i = 0; i < A1_L; i++) {
        strncpy(mapaA1[i],     t[i], A1_C);
        mapaA1[i][A1_C] = '\0';
        strncpy(mapaA1Orig[i], t[i], A1_C);
        mapaA1Orig[i][A1_C] = '\0';
    }
}

/* inicializarAndar2 */
void inicializarAndar2() {
    int i;
    const char *t[A2_L] = {
        "***************",
        "*>            *",
        "* **** D @    *",
        "* *    *      *",
        "* * ## *  D @ *",
        "* *    *      *",
        "* **** * **** *",
        "*      O      *",
        "* ****   **** *",
        "*      X      *",
        "* #### * #### *",
        "*      X      *",
        "* ****   **** *",
        "*            L*",
        "***************"
    };
    for (i = 0; i < A2_L; i++) {
        strncpy(mapaA2[i],     t[i], A2_C);
        mapaA2[i][A2_C] = '\0';
        strncpy(mapaA2Orig[i], t[i], A2_C);
        mapaA2Orig[i][A2_C] = '\0';
    }
}

/* inicializarAndar3 */
void inicializarAndar3() {
    int i;
    const char *t[A3_L] = {
        "*************************",
        "*>          @           *",
        "* ******* D * *******   *",
        "* *           *     *   *",
        "* * ####### * * D@  *   *",
        "* *         * *     *   *",
        "* ******* * * ******* * *",
        "*           *         * *",
        "* *** ##### * ******* * *",
        "* *   #   # *       * * *",
        "* * Y # @ # *   Y   * * *",
        "* *   #   # *       * * *",
        "* *** ##### *  **** * * *",
        "*           *  *    *   *",
        "* ######### * D* k  *   *",
        "*           *  *    *   *",
        "* *** *** * * @* k  * * *",
        "* *   * * * *  ******* * *",
        "* * k * * * *         * *",
        "* *   * *   * ******* * *",
        "* *** * * * *   Z   * * *",
        "*       * * * ***** * * *",
        "*   Y   *   *       *   *",
        "*             X   X    L*",
        "*************************"
    };
    for (i = 0; i < A3_L; i++) {
        strncpy(mapaA3[i],     t[i], A3_C);
        mapaA3[i][A3_C] = '\0';
        strncpy(mapaA3Orig[i], t[i], A3_C);
        mapaA3Orig[i][A3_C] = '\0';
    }
}

/* reiniciarFase */
void reiniciarFase(char mapa[][MAX_C], char orig[][MAX_C],
                   int linhas, int colunas, int lIni, int cIni) {
    int i;
    for (i = 0; i < linhas; i++)
        strncpy(mapa[i], orig[i], colunas + 1);
    jLinha   = lIni;
    jColuna  = cIni;
    jDirecao = '>';
    jChaves  = 0;
    escanearMonstros(mapa, linhas, colunas);
}

/* aplicarEfeitoBotao */
void aplicarEfeitoBotao(char mapa[][MAX_C], int fase) {
    if (fase == 2) {
        if (mapa[7][7] == '*') mapa[7][7] = ' ';
        if (mapa[7][6] == '*') mapa[7][6] = ' ';
        printf("\n  [!] Uma passagem secreta se abriu!\n");
    }
    if (fase == 3) {
        if (numMonstros < MAX_MONSTROS - 1) {
            mLinha[numMonstros]  = 21;
            mColuna[numMonstros] = 5;
            mTipo[numMonstros]   = 2;
            mVivo[numMonstros]   = 1;
            numMonstros++;
            mLinha[numMonstros]  = 21;
            mColuna[numMonstros] = 8;
            mTipo[numMonstros]   = 2;
            mVivo[numMonstros]   = 1;
            numMonstros++;
        }
        printf("\n  [!!] Armadilha! Novos inimigos surgiram!\n");
    }
}

/* escolherArma */
void escolherArma() {
    limparTela();
    printf("  ===== FERREIRO DA VILA =====\n\n");
    printf("  'Guerreiro! Escolha sua arma com sabedoria.'\n\n");
    printf("  1. Espada        - Ataca bloco 3x2 a frente\n");
    printf("  2. Arco e Flecha - Ataca 4 celulas em linha reta\n");
    printf("  3. Cajado        - Ataca as 8 celulas ao redor\n\n");
    printf("  Sua escolha (1-3): ");

    int escolha = 0;
    while (escolha < 1 || escolha > 3) {
        escolha = lerInt();
        if (escolha < 1 || escolha > 3)
            printf("  Opcao invalida. Escolha 1, 2 ou 3: ");
    }

    jArma = escolha;
    const char *nomes[] = { "", "Espada", "Arco e Flecha", "Cajado" };
    printf("\n  Voce equipou: %s!\n\n", nomes[escolha]);
    pausar();
}

/* faseVila */
void faseVila() {
    inicializarVila();
    while (1) {
        limparTela();
        exibirHUD();
        printf("  === VILA DE ARENDOR ===\n");
        printf("  Fale com o NPC (N) para escolher sua arma.\n");
        printf("  Entre pela escada (L) para avancar.\n\n");
        exibirMapa(mapaVila, VILA_L, VILA_C);
        printf("  Comando (w/a/s/d/i): ");

        char cmd = lerChar();
        int dl = 0, dc = 0;
        char dir = jDirecao;

        if      (cmd == 'w') { dl = -1; dir = '^'; }
        else if (cmd == 's') { dl =  1; dir = 'v'; }
        else if (cmd == 'a') { dc = -1; dir = '<'; }
        else if (cmd == 'd') { dc =  1; dir = '>'; }
        else if (cmd == 'i') {
            calcularFrente();
            if (frenteLinha >= 0 && frenteLinha < VILA_L &&
                frenteColuna >= 0 && frenteColuna < VILA_C &&
                mapaVila[frenteLinha][frenteColuna] == 'N') {
                if (jArma == 0)
                    escolherArma();
                else {
                    printf("\n  [!] Voce ja tem uma arma equipada!\n");
                    pausar();
                }
            } else {
                interagirComMapa(mapaVila, VILA_L, VILA_C, 0);
                pausar();
            }
            continue;
        }

        if (dl != 0 || dc != 0) {
            int res = moverJogador(mapaVila, VILA_L, VILA_C, dl, dc, dir);
            if (res == 2) {
                if (jArma == 0) {
                    printf("\n  [!] Fale com o NPC antes de entrar!\n");
                    jLinha  -= dl;
                    jColuna -= dc;
                    pausar();
                } else {
                    return;
                }
            }
        }
    }
}

/* loopFase */
int loopFase(char mapa[][MAX_C], char orig[][MAX_C],
             int linhas, int cols,
             int lIni, int cIni,
             const char *nome, int temBoss, int fase) {

    jLinha   = lIni;
    jColuna  = cIni;
    jDirecao = '>';
    jChaves  = 0;

    if (fase == 2) botaoA2Ativado = 0;
    if (fase == 3) botaoA3Ativado = 0;

    int efeitoJaAplicado = 0;

    escanearMonstros(mapa, linhas, cols);

    while (1) {

        if (temBoss && !bossEstaVivo())
            return 2;

        if (!efeitoJaAplicado) {
            if ((fase == 2 && botaoA2Ativado) ||
                (fase == 3 && botaoA3Ativado)) {
                aplicarEfeitoBotao(mapa, fase);
                efeitoJaAplicado = 1;
            }
        }

        limparTela();
        exibirHUD();
        if (temBoss)
            printf("  === %s === [Boss: %s]\n",
                   nome, bossEstaVivo() ? "VIVO" : "MORTO");
        else
            printf("  === %s === [Monstros: %d]\n",
                   nome, contarMonstrosVivos());

        desenharMonstros(mapa);
        exibirMapa(mapa, linhas, cols);
        limparMonstrosDoMapa(mapa);

        printf("  Comando (w/a/s/d/i/o): ");
        char cmd = lerChar();

        int dl = 0, dc = 0;
        char dir = jDirecao;
        int resultado = 0;

        if      (cmd == 'w') { dl = -1; dir = '^'; }
        else if (cmd == 's') { dl =  1; dir = 'v'; }
        else if (cmd == 'a') { dc = -1; dir = '<'; }
        else if (cmd == 'd') { dc =  1; dir = '>'; }
        else if (cmd == 'i') {
            interagirComMapa(mapa, linhas, cols, fase);
            pausar();
            continue;
        } else if (cmd == 'o') {
            atacar(mapa, linhas, cols);
            resultado = moverMonstros(mapa, linhas, cols);
            if (resultado) {
                if (jVidas <= 0) { pausar(); return 0; }
                pausar();
                reiniciarFase(mapa, orig, linhas, cols, lIni, cIni);
                efeitoJaAplicado = 0;
                if (fase == 2) botaoA2Ativado = 0;
                if (fase == 3) botaoA3Ativado = 0;
            } else {
                pausar();
            }
            continue;
        } else {
            printf("\n  [!] Comando invalido. Use w/a/s/d/i/o\n");
            pausar();
            continue;
        }

        resultado = moverJogador(mapa, linhas, cols, dl, dc, dir);

        if (resultado == 1) {
            if (jVidas <= 0) { pausar(); return 0; }
            pausar();
            reiniciarFase(mapa, orig, linhas, cols, lIni, cIni);
            efeitoJaAplicado = 0;
            if (fase == 2) botaoA2Ativado = 0;
            if (fase == 3) botaoA3Ativado = 0;
            continue;
        }
        if (resultado == 2)
            return 1;

        resultado = moverMonstros(mapa, linhas, cols);
        if (resultado) {
            if (jVidas <= 0) { pausar(); return 0; }
            pausar();
            reiniciarFase(mapa, orig, linhas, cols, lIni, cIni);
            efeitoJaAplicado = 0;
            if (fase == 2) botaoA2Ativado = 0;
            if (fase == 3) botaoA3Ativado = 0;
        }
    }
}

/* main */
int main() {
    srand((unsigned int)time(NULL));

    while (1) {
        exibirMenu();
        char opcao = lerChar();

        if (opcao == '1') {
            jVidas   = 3;
            jArma    = 0;
            jChaves  = 0;
            jDirecao = '>';
            jLinha   = 1;
            jColuna  = 1;

            faseVila();

            inicializarAndar1();
            int res = loopFase(mapaA1, mapaA1Orig, A1_L, A1_C, 1, 1,
                               "ANDAR 1 - Entrada da Masmorra", 0, 1);
            if (res == 0) { exibirGameOver(); continue; }

            inicializarAndar2();
            res = loopFase(mapaA2, mapaA2Orig, A2_L, A2_C, 1, 1,
                           "ANDAR 2 - Corredor dos Espinhos", 0, 2);
            if (res == 0) { exibirGameOver(); continue; }

            inicializarAndar3();
            res = loopFase(mapaA3, mapaA3Orig, A3_L, A3_C, 1, 1,
                           "ANDAR 3 - Camara de Malachar", 1, 3);
            if (res == 0) exibirGameOver();
            else          exibirVitoria();

        } else if (opcao == '2') {
            exibirTutorial();

        } else if (opcao == '3') {
            limparTela();
            printf("\n  ===== CREDITOS =====\n\n");
            printf("  Desenvolvido por:\n");
            printf("    * [SEU NOME AQUI]\n");
            printf("    * [INTEGRANTE 2]\n");
            printf("    * [INTEGRANTE 3]\n\n");
            printf("  Obrigado por jogar DUNGEON CRAWLER!\n\n");
            break;

        } else {
            printf("\n  [!] Opcao invalida!\n");
            pausar();
        }
    }

    return 0;
}
