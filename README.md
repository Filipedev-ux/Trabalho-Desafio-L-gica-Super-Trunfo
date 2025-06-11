#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>
#include <ctype.h>

#ifdef _WIN32
#include <windows.h>
#else
#include <unistd.h>
#endif

void limparTela() {
#ifdef _WIN32
    system("cls");
#else
    system("clear");
#endif
}

typedef struct {
    char nome[50];
    char codigo[4];
    int ataque;
    int defesa;
    int magia;
    int velocidade;
} Carta;

void inicializarBaralho(Carta baralho[], int tamanho) {
    strcpy(baralho[0].nome, "Dragao de Fogo"); strcpy(baralho[0].codigo, "1A");
    baralho[0].ataque = 95; baralho[0].defesa = 80; baralho[0].magia = 70; baralho[0].velocidade = 85;

    strcpy(baralho[1].nome, "Gigante de Pedra"); strcpy(baralho[1].codigo, "1B");
    baralho[1].ataque = 80; baralho[1].defesa = 98; baralho[1].magia = 20; baralho[1].velocidade = 30;

    strcpy(baralho[2].nome, "Elfo Arqueiro"); strcpy(baralho[2].codigo, "2A");
    baralho[2].ataque = 75; baralho[2].defesa = 60; baralho[2].magia = 80; baralho[2].velocidade = 92;

    strcpy(baralho[3].nome, "Mago das Sombras"); strcpy(baralho[3].codigo, "2B");
    baralho[3].ataque = 50; baralho[3].defesa = 70; baralho[3].magia = 97; baralho[3].velocidade = 60;

    strcpy(baralho[4].nome, "Golem de Ferro"); strcpy(baralho[4].codigo, "3A");
    baralho[4].ataque = 88; baralho[4].defesa = 92; baralho[4].magia = 10; baralho[4].velocidade = 25;

    strcpy(baralho[5].nome, "Grifo Veloz"); strcpy(baralho[5].codigo, "3B");
    baralho[5].ataque = 82; baralho[5].defesa = 75; baralho[5].magia = 40; baralho[5].velocidade = 99;

    strcpy(baralho[6].nome, "Hidra de Lerna"); strcpy(baralho[6].codigo, "4A");
    baralho[6].ataque = 90; baralho[6].defesa = 85; baralho[6].magia = 75; baralho[6].velocidade = 70;

    strcpy(baralho[7].nome, "Fenix Lendaria"); strcpy(baralho[7].codigo, "ST");
    baralho[7].ataque = 100; baralho[7].defesa = 100; baralho[7].magia = 100; baralho[7].velocidade = 100;
}

void embaralhar(Carta baralho[], int tamanho) {
    for (int i = 0; i < tamanho - 1; i++) {
        int j = i + rand() / (RAND_MAX / (tamanho - i) + 1);
        Carta temp = baralho[j];
        baralho[j] = baralho[i];
        baralho[i] = temp;
    }
}

void exibirCarta(Carta c, int mostrarAtributos) {
    printf("   +------------------------------------+\n");
    printf("   | %-15s            %4s |\n", c.nome, c.codigo);
    printf("   +------------------------------------+\n");
    if (mostrarAtributos) {
        printf("   | 1. Ataque      : %-15d |\n", c.ataque);
        printf("   | 2. Defesa      : %-15d |\n", c.defesa);
        printf("   | 3. Magia       : %-15d |\n", c.magia);
        printf("   | 4. Velocidade  : %-15d |\n", c.velocidade);
    } else {
        printf("   | 1. Ataque      : ??               |\n");
        printf("   | 2. Defesa      : ??               |\n");
        printf("   | 3. Magia       : ??               |\n");
        printf("   | 4. Velocidade  : ??               |\n");
    }
    printf("   +------------------------------------+\n");
}

int compararCartas(Carta c1, Carta c2, int atributo) {
    if (strcmp(c1.codigo, "ST") == 0 && strcmp(c2.codigo, "1A") != 0) return 1;
    if (strcmp(c2.codigo, "ST") == 0 && strcmp(c1.codigo, "1A") != 0) return -1;
    if (strcmp(c1.codigo, "1A") == 0 && strcmp(c2.codigo, "ST") == 0) return 1;
    if (strcmp(c2.codigo, "1A") == 0 && strcmp(c1.codigo, "ST") == 0) return -1;

    int v1, v2;
    switch (atributo) {
        case 1: v1 = c1.ataque; v2 = c2.ataque; break;
        case 2: v1 = c1.defesa; v2 = c2.defesa; break;
        case 3: v1 = c1.magia; v2 = c2.magia; break;
        case 4: v1 = c1.velocidade; v2 = c2.velocidade; break;
        default: return 0;
    }
    if (v1 > v2) return 1;
    if (v2 > v1) return -1;
    return 0;
}

void moverCartas(Carta maoGanhador[], int *n_ganhador, Carta maoPerdedor[], int *n_perdedor, Carta pote[], int *n_pote) {
    Carta cartaGanhador = maoGanhador[0];
    Carta cartaPerdedor = maoPerdedor[0];

    for (int i = 0; i < *n_ganhador - 1; i++) maoGanhador[i] = maoGanhador[i + 1];
    (*n_ganhador)--;
    for (int i = 0; i < *n_perdedor - 1; i++) maoPerdedor[i] = maoPerdedor[i + 1];
    (*n_perdedor)--;

    maoGanhador[*n_ganhador] = cartaGanhador;
    (*n_ganhador)++;
    maoGanhador[*n_ganhador] = cartaPerdedor;
    (*n_ganhador)++;

    for(int i = 0; i < *n_pote; i++){
        maoGanhador[*n_ganhador] = pote[i];
        (*n_ganhador)++;
    }
    *n_pote = 0;
}

void empatarCartas(Carta maoJogador[], int *n_jogador, Carta maoComputador[], int *n_computador, Carta pote[], int *n_pote){
    pote[*n_pote] = maoJogador[0];
    (*n_pote)++;
    pote[*n_pote] = maoComputador[0];
    (*n_pote)++;

    for (int i = 0; i < *n_jogador - 1; i++) maoJogador[i] = maoJogador[i + 1];
    (*n_jogador)--;
    for (int i = 0; i < *n_computador - 1; i++) maoComputador[i] = maoComputador[i + 1];
    (*n_computador)--;
}

int main() {
    srand(time(NULL));
    const int TAMANHO_BARALHO = 8;
    Carta baralho[TAMANHO_BARALHO];
    Carta maoJogador[TAMANHO_BARALHO];
    Carta maoComputador[TAMANHO_BARALHO];
    Carta pote[TAMANHO_BARALHO];
    int n_jogador = 0, n_computador = 0, n_pote = 0;

    inicializarBaralho(baralho, TAMANHO_BARALHO);
    embaralhar(baralho, TAMANHO_BARALHO);

    for (int i = 0; i < TAMANHO_BARALHO; i++) {
        if (i % 2 == 0) maoJogador[n_jogador++] = baralho[i];
        else maoComputador[n_computador++] = baralho[i];
    }

    int turnoJogador = 1;
    int rodada = 1;

    while (n_jogador > 0 && n_computador > 0) {
        limparTela();
        printf("--- RODADA %d ---\n", rodada++);
        printf("PLACAR: Jogador (%d cartas) vs Computador (%d cartas)\n\n", n_jogador, n_computador);

        Carta cartaAtualJogador = maoJogador[0];
        Carta cartaAtualComputador = maoComputador[0];

        if(n_pote > 0) {
            printf("Pote acumulado: %d cartas!\n\n", n_pote);
        }

        printf("VEZ DO %s\n", turnoJogador ? "JOGADOR" : "COMPUTADOR");

        if (turnoJogador) {
            printf("Sua carta:\n");
            exibirCarta(cartaAtualJogador, 1);
        } else {
            printf("Sua carta (aguardando o computador jogar):\n");
            exibirCarta(cartaAtualJogador, 0);
        }

        int escolha = 0;
        if (turnoJogador) {
            printf("\nEscolha um atributo (1-4): ");
            while(scanf("%d", &escolha) != 1 || escolha < 1 || escolha > 4) {
                while(getchar() != '\n');
                printf("Entrada invalida. Escolha um atributo (1-4): ");
            }
        } else {
            escolha = (rand() % 4) + 1;
            printf("\nComputador escolheu o atributo %d.\n", escolha);
            sleep(2);
        }

        printf("\n-- COMPARANDO CARTAS --\n\n");
        printf("Sua carta:\n");
        exibirCarta(cartaAtualJogador, 1);
        printf("\nCarta do Computador:\n");
        exibirCarta(cartaAtualComputador, 1);
        printf("\n-------------------------\n");

        int resultado = compararCartas(cartaAtualJogador, cartaAtualComputador, escolha);

        if (resultado == 1) {
            printf("\nVOCE GANHOU A RODADA!\n");
            moverCartas(maoJogador, &n_jogador, maoComputador, &n_computador, pote, &n_pote);
            turnoJogador = 1;
        } else if (resultado == -1) {
            printf("\nO COMPUTADOR GANHOU A RODADA!\n");
            moverCartas(maoComputador, &n_computador, maoJogador, &n_jogador, pote, &n_pote);
            turnoJogador = 0;
        } else {
            printf("\nEMPATE! As cartas vao para o pote.\n");
            empatarCartas(maoJogador, &n_jogador, maoComputador, &n_computador, pote, &n_pote);
        }
        sleep(4);
    }

    limparTela();
    printf("--- FIM DE JOGO ---\n\n");
    if (n_jogador > 0) {
        printf("PARABENS! VOCE VENCEU O JOGO!\n");
    } else {
        printf("QUE PENA! O COMPUTADOR VENCEU.\n");
    }
    printf("Placar final: Jogador (%d) vs Computador (%d)\n", n_jogador, n_computador);

    return 0;
}
