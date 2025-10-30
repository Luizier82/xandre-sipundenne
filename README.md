# xandre-sipundenne
#include <stdio.h>
#include <string.h>
#include <stdbool.h>
#include <stdlib.h>

#define TAMANHO_TABULEIRO 8

char tabuleiro[TAMANHO_TABULEIRO][TAMANHO_TABULEIRO] = {
    {'K', 'S', 'U', 'H', 'N', 'U', 'S', 'K'},
    {'G', 'G', 'G', 'G', 'G', 'G', 'G', 'G'},
    {' ', ' ', ' ', ' ', ' ', ' ', ' ', ' '},
    {' ', ' ', ' ', ' ', ' ', ' ', ' ', ' '},
    {' ', ' ', ' ', ' ', ' ', ' ', ' ', ' '},
    {' ', ' ', ' ', ' ', ' ', ' ', ' ', ' '},
    {'g', 'g', 'g', 'g', 'g', 'g', 'g', 'g'},
    {'k', 's', 'u', 'h', 'n', 'u', 's', 'k'}
};

bool kakashi_pode_mover(int de_lin, int de_col, int para_lin, int para_col) {
    if (de_lin != para_lin && de_col != para_col) {
        return false;
    }

    if (de_lin == para_lin) {
        int passo = (para_col > de_col) ? 1 : -1;
        for (int col = de_col + passo; col != para_col; col += passo) {
            if (tabuleiro[de_lin][col] != ' ') {
                return false;
            }
        }
    } else {
        int passo = (para_lin > de_lin) ? 1 : -1;
        int linha = de_lin + passo;
        while (linha != para_lin) {
            if (tabuleiro[linha][de_col] != ' ') {
                return false;
            }
            linha += passo;
        }
    }
    return true;
}

bool sasuke_pode_mover(int de_lin, int de_col, int para_lin, int para_col) {
    int dif_lin = para_lin - de_lin;
    int dif_col = para_col - de_col;

    if (abs(dif_lin) != abs(dif_col)) {
        return false;
    }

    int passos = abs(dif_lin);
    int dir_lin = (dif_lin > 0) ? 1 : -1;
    int dir_col = (dif_col > 0) ? 1 : -1;

    int l = de_lin + dir_lin;
    int c = de_col + dir_col;
    int contador = 1;

    do {
        if (tabuleiro[l][c] != ' ') {
            return false;
        }
        l += dir_lin;
        c += dir_col;
        contador++;
    } while (contador < passos);

    return true;
}

bool hinata_pode_mover(int de_lin, int de_col, int para_lin, int para_col) {
    return kakashi_pode_mover(de_lin, de_col, para_lin, para_col) ||
           sasuke_pode_mover(de_lin, de_col, para_lin, para_col);
}

bool shikamaru_pode_mover(int de_lin, int de_col, int para_lin, int para_col) {
    int possibilidades_linha[8] = { -2, -2, -1, -1,  1,  1,  2,  2 };
    int possibilidades_coluna[8] = { -1,  1, -2,  2, -2,  2, -1,  1 };

    for (int i = 0; i < 8; i++) {
        int nova_linha = de_lin + possibilidades_linha[i];
        int nova_coluna = de_col + possibilidades_coluna[i];

        if (nova_linha == para_lin && nova_coluna == para_col) {
            return true;
        }
    }
    return false;
}

void mostrar_tabuleiro_com_amor() {
    printf("\n   a   b   c   d   e   f   g   h\n");
    for (int linha = 0; linha < TAMANHO_TABULEIRO; linha++) {
        printf("%d ", 8 - linha);
        for (int coluna = 0; coluna < TAMANHO_TABULEIRO; coluna++) {
            char ninja = tabuleiro[linha][coluna];
            if (ninja == ' ') ninja = '.';
            printf(" %c ", ninja);
        }
        printf(" %d\n", 8 - linha);
    }
    printf("   a   b   c   d   e   f   g   h\n\n");
    
    printf("Legenda:\n");
    printf("N/n = Naruto (Rei)     H/h = Hinata (Rainha)\n");
    printf("K/k = Kakashi (Torre)  U/u = Sasuke (Bispo)\n");
    printf("S/s = Shikamaru (Cavalo)  G/g = Genin (Peao)\n\n");
}

void menu_principal() {
    printf("\n");
    printf("╔══════════════════════════════════════════╗\n");
    printf("║                                          ║\n");
    printf("║     🍥  XADREZ NINJA - NARUTO  🍥        ║\n");
    printf("║                                          ║\n");
    printf("║   Folha vs Areia - Quem sera Hokage?    ║\n");
    printf("║                                          ║\n");
    printf("╚══════════════════════════════════════════╝\n");
    printf("\n");
    printf("   [1] Começar o Jogo\n");
    printf("   [2] Ver Regras\n");
    printf("   [3] Sair\n");
    printf("\n");
    printf("Escolha uma opção: ");
}

void mostrar_regras() {
    printf("\n╔══════════════════════════════════════════╗\n");
    printf("║           REGRAS DO JOGO                 ║\n");
    printf("╚══════════════════════════════════════════╝\n\n");
    printf("Peças e Movimentos:\n\n");
    printf("🔸 KAKASHI (K/k) - Torre\n");
    printf("   Move-se em linha reta (horizontal/vertical)\n\n");
    printf("🔸 SASUKE (U/u) - Bispo\n");
    printf("   Move-se em diagonais\n\n");
    printf("🔸 HINATA (H/h) - Rainha\n");
    printf("   Combina movimentos de Torre e Bispo\n\n");
    printf("🔸 SHIKAMARU (S/s) - Cavalo\n");
    printf("   Move-se em 'L' (pode pular peças)\n\n");
    printf("🔸 NARUTO (N/n) - Rei\n");
    printf("   Move-se uma casa em qualquer direção\n\n");
    printf("🔸 GENIN (G/g) - Peão\n");
    printf("   Move-se uma casa para frente\n\n");
    printf("Maiúsculas = Vilarejo da Folha\n");
    printf("Minúsculas = Vilarejo da Areia\n\n");
    printf("Pressione Enter para voltar...");
    getchar();
}

void executar_demonstracao() {
    printf("\n~-~-~-~-~-~-~-~-~-~-~-~-~-~-~-~-~-~-~\n");
    printf("   DEMONSTRAÇÃO DOS MOVIMENTOS\n");
    printf("~-~-~-~-~-~-~-~-~-~-~-~-~-~-~-~-~-~-~\n");

    mostrar_tabuleiro_com_amor();

    printf("Vamos testar se os ninjas podem se mover corretamente...\n\n");

    if (kakashi_pode_mover(0, 0, 3, 0)) {
        printf("✅ Kakashi: \"Posso ir de a8 para a5. Meu caminho esta limpo.\"\n");
    } else {
        printf("❌ Kakashi: \"Alguem esta no meu caminho.\"\n");
    }

    if (sasuke_pode_mover(0, 2, 3, 5)) {
        printf("✅ Sasuke: \"Minha diagonal esta livre. Vou avancar.\"\n");
    } else {
        printf("❌ Sasuke: \"Ha um obstaculo. Nao posso passar.\"\n");
    }

    if (hinata_pode_mover(0, 3, 2, 5)) {
        printf("✅ Hinata: \"Meu Byakugan ve o caminho. Posso ir!\"\n");
    } else {
        printf("❌ Hinata: \"Nao consigo avancar...\"\n");
    }

    if (shikamaru_pode_mover(0, 1, 2, 2)) {
        printf("✅ Shikamaru: \"Que preguica... mas sim, posso pular para c6.\"\n");
    } else {
        printf("❌ Shikamaru: \"Isso da muito trabalho. Nao e um L valido.\"\n");
    }

    printf("\n✨ Demonstração concluída! ✨\n");
    printf("\nPressione Enter para continuar...");
    getchar();
}

int main() {
    int opcao;
    char buffer[100];

    while (1) {
        menu_principal();
        
        if (fgets(buffer, sizeof(buffer), stdin) != NULL) {
            opcao = atoi(buffer);
            
            if (opcao == 1) {
                executar_demonstracao();
            } else if (opcao == 2) {
                mostrar_regras();
            } else if (opcao == 3) {
                printf("\n✨ Que a paz reine entre as vilas! ✨\n");
                printf("Ate logo, ninja! 🍥\n\n");
                break;
            } else {
                printf("\nOpcao invalida! Tente novamente.\n");
                printf("Pressione Enter para continuar...");
                getchar();
            }
        }
    }

    return 0;
}
