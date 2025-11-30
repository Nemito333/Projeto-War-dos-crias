#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

#define MAX_TERRITORIOS 5
#define TAM_STRING 50

typedef struct {
    char nome[TAM_STRING];
    char cor[TAM_STRING];
    int tropas;
} Cafofo;

void cadastrarCafofosFixos(Cafofo t[]) {
    int i;

    printf("=== Cadastro da sua Base ===\n\n");

    for (i = 0; i < MAX_TERRITORIOS; i++) {
        printf("Base %d:\n", i + 1);

        printf("Cafofo: ");
        fgets(t[i].nome, TAM_STRING, stdin);
        t[i].nome[strcspn(t[i].nome, "\n")] = '\0';

        printf("Cor: ");
        fgets(t[i].cor, TAM_STRING, stdin);
        t[i].cor[strcspn(t[i].cor, "\n")] = '\0';

        printf("Quantidade dos crias: ");
        scanf("%d", &t[i].tropas);
        getchar();

        printf("\n");
    }

    printf("\n=== Cafofos presentes ===\n\n");
    for (i = 0; i < MAX_TERRITORIOS; i++) {
        printf("Cafofo %d:\n", i + 1);
        printf("Vulgo: %s\n", t[i].nome);
        printf("Cor: %s\n", t[i].cor);
        printf("Crias: %d\n\n", t[i].tropas);
    }
}

Cafofo* criarCafofo(int qtd) {
    Cafofo* t = (Cafofo*) malloc(qtd * sizeof(Cafofo));
    if (!t) {
        printf("Erro ao alocar Cafofos!\n");
        exit(1);
    }
    return t;
}

void cadastrarCafofos(Cafofo* t, int qtd) {
    for (int i = 0; i < qtd; i++) {
        printf("\n=== Lança o Cafofo %d ===\n", i + 1);

        printf("Vulgo: ");
        fgets(t[i].nome, TAM_STRING, stdin);
        t[i].nome[strcspn(t[i].nome, "\n")] = '\0';

        printf("Cor: ");
        fgets(t[i].cor, TAM_STRING, stdin);
        t[i].cor[strcspn(t[i].cor, "\n")] = '\0';

        printf("Tropinhas: ");
        scanf("%d", &t[i].tropas);
        getchar();
    }
}

void exibirCafofos(Cafofo* t, int qtd) {
    printf("\n\n=== Cafofos ===\n");
    for (int i = 0; i < qtd; i++) {
        printf("\nCafofo %d:\n", i + 1);
        printf("Vulgo: %s\n", t[i].nome);
        printf("Cor: %s\n", t[i].cor);
        printf("Tropipinhas: %d\n", t[i].tropas);
    }
}

void atacar(Cafofo* atq, Cafofo* def) {

    printf("\n\n=== Ataque Lançado ===\n");
    printf("%s Porradaria comendo %s!\n", atq->nome, def->nome);

    if (atq->tropas < 1) {
        printf("\nO Cafofo não tem Tropinhas suficientes pra esse corre!\n");
        return;
    }

    while (atq->tropas > 0 && def->tropas > 0) {
        int dadoAtq = rand() % 6 + 1;
        int dadoDef = rand() % 6 + 1;

        printf("\nAtaque: %d | Defesa: %d\n", dadoAtq, dadoDef);

        if (dadoAtq > dadoDef) {
            def->tropas--;
            printf("Defesa mamou! Perdeu 1 cria! Restam: %d\n", def->tropas);
        } else {
            atq->tropas--;
            printf("Ataque foi paioso e perdeu 1 cria! Restam: %d\n", atq->tropas);
        }
    }

    printf("\n=== Resultado do Corre ===\n");
    if (def->tropas <= 0) {
        printf("%s tomou o cafofo de %s!\n", atq->nome, def->nome);
    } else {
        printf("%s segurou a bronca e NÃO VAI SUBIR NINGUÉM!\n", def->nome);
    }
}

int menu() {
    int opc;
    printf("\n\n=== Menu ===\n");
    printf("1 - Exibir Cafofos\n");
    printf("2 - Bote pra arrepentar (Atacar)\n");
    printf("3 - Fica frio ai e pensa mais um pouco (Sair)\n");
    printf("Lança a braba: ");
    scanf("%d", &opc);
    getchar();
    return opc;
}

int main() {
    srand(time(NULL));

    Cafofo territoriosFixos[MAX_TERRITORIOS];
    cadastrarCafofosFixos(territoriosFixos);

    int qtd;
    printf("Quantos Cafofos dinâmicos deseja criar? ");
    scanf("%d", &qtd);
    getchar();

    Cafofo* cafofos = criarCafofo(qtd);
    cadastrarCafofos(cafofos, qtd);

    int opc;
    do {
        opc = menu();

        if (opc == 1) {
            exibirCafofos(cafofos, qtd);
        }
        else if (opc == 2) {
            int a, d;

            printf("Índice do atacante (1-%d): ", qtd);
            scanf("%d", &a);

            printf("Índice do defensor (1-%d): ", qtd);
            scanf("%d", &d);
            getchar();

            if (a >= 1 && a <= qtd && d >= 1 && d <= qtd && a != d) {
                atacar(&cafofos[a - 1], &cafofos[d - 1]);
            } else {
                printf("Entrada inválida!\n");
            }
        }

    } while (opc != 3);

    free(cafofos);
    printf("\nMemória liberada. Programa finalizado.\n");

    return 0;
}
