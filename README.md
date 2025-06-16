#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <string.h>

struct Carta {
    char nome[50];
    int forca;
    int velocidade;
    int inteligencia;
};

int main() {
    struct Carta baralho[5];
    strcpy(baralho[0].nome, "Guerreiro");
    baralho[0].forca = 90;
    baralho[0].velocidade = 60;
    baralho[0].inteligencia = 40;

    strcpy(baralho[1].nome, "Mago");
    baralho[1].forca = 50;
    baralho[1].velocidade = 70;
    baralho[1].inteligencia = 95;

    strcpy(baralho[2].nome, "Arqueiro");
    baralho[2].forca = 75;
    baralho[2].velocidade = 85;
    baralho[2].inteligencia = 65;

    strcpy(baralho[3].nome, "Bardo");
    baralho[3].forca = 40;
    baralho[3].velocidade = 75;
    baralho[3].inteligencia = 80;
    
    strcpy(baralho[4].nome, "Barbaro");
    baralho[4].forca = 100;
    baralho[4].velocidade = 50;
    baralho[4].inteligencia = 20;

    srand(time(NULL));

    int indiceJogador = rand() % 5;
    int indiceComputador;

    do {
        indiceComputador = rand() % 5;
    } while (indiceComputador == indiceJogador);

    struct Carta cartaJogador = baralho[indiceJogador];
    struct Carta cartaComputador = baralho[indiceComputador];

    printf("--- SUPER TRUNFO ---\n\n");
    printf("Sua carta: %s\n", cartaJogador.nome);
    printf("1. Forca: %d\n", cartaJogador.forca);
    printf("2. Velocidade: %d\n", cartaJogador.velocidade);
    printf("3. Inteligencia: %d\n", cartaJogador.inteligencia);
    
    int escolha;
    printf("\nEscolha um atributo para desafiar (1-3): ");
    scanf("%d", &escolha);

    printf("\n-- Resultado --\n");
    printf("Sua carta: %s (For: %d, Vel: %d, Int: %d)\n", cartaJogador.nome, cartaJogador.forca, cartaJogador.velocidade, cartaJogador.inteligencia);
    printf("Carta do Computador: %s (For: %d, Vel: %d, Int: %d)\n\n", cartaComputador.nome, cartaComputador.forca, cartaComputador.velocidade, cartaComputador.inteligencia);

    if (escolha == 1) {
        if (cartaJogador.forca > cartaComputador.forca) {
            printf("Voce venceu com Forca!\n");
        } else if (cartaJogador.forca < cartaComputador.forca) {
            printf("Voce perdeu! O computador tinha mais Forca.\n");
        } else {
            printf("Empate!\n");
        }
    } else if (escolha == 2) {
        if (cartaJogador.velocidade > cartaComputador.velocidade) {
            printf("Voce venceu com Velocidade!\n");
        } else if (cartaJogador.velocidade < cartaComputador.velocidade) {
            printf("Voce perdeu! O computador era mais veloz.\n");
        } else {
            printf("Empate!\n");
        }
    } else if (escolha == 3) {
        if (cartaJogador.inteligencia > cartaComputador.inteligencia) {
            printf("Voce venceu com Inteligencia!\n");
        } else if (cartaJogador.inteligencia < cartaComputador.inteligencia) {
            printf("Voce perdeu! O computador era mais inteligente.\n");
        } else {
            printf("Empate!\n");
        }
    } else {
        printf("Opcao invalida. Fim de jogo.\n");
    }

    return 0;
}
