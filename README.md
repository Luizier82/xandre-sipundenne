# xandre-sipundenne
#include <stdio.h>
#include <string.h>
#include <stdbool.h>
#include <stdlib.h>

#define TAMANHO_TABULEIRO 8

// Tabuleiro do jogo: cada casa tem um ninja!
// Maiúsculas = Vilarejo da Folha (atacantes)
// Minúsculas = Vilarejo da Areia (defensores)
char tabuleiro[TAMANHO_TABULEIRO][TAMANHO_TABULEIRO] = {
    {'K', 'S', 'U', 'H', 'N', 'U', 'S', 'K'}, // Kakashi, Shikamaru, Sasuke, Hinata, Naruto...
    {'G', 'G', 'G', 'G', 'G', 'G', 'G', 'G'}, // Genins
    {' ', ' ', ' ', ' ', ' ', ' ', ' ', ' '},
    {' ', ' ', ' ', ' ', ' ', ' ', ' ', ' '},
    {' ', ' ', ' ', ' ', ' ', ' ', ' ', ' '},
    {' ', ' ', ' ', ' ', ' ', ' ', ' ', ' '},
    {'g', 'g', 'g', 'g', 'g', 'g', 'g', 'g'},
    {'k', 's', 'u', 'h', 'n', 'u', 's', 'k'}
};

// --- FUNÇÕES QUE "PENSAM" COMO UM NINJA ---

// 🧱 Kakashi (Torre) só anda em linha reta — como um relâmpago reto!
bool kakashi_pode_mover(int de_lin, int de_col, int para_lin, int para_col) {
    // Só pode ir pra frente, trás, esquerda ou direita — nunca na diagonal!
    if (de_lin != para_lin && de_col != para_col) {
        return false; // "Não, isso não é meu estilo."
    }

    // Vamos andar passo a passo e ver se tem alguém no caminho?
    if (de_lin == para_lin) {
        // Movendo na horizontal (mesma linha)
        int passo = (para_col > de_col) ? 1 : -1;
        for (int col = de_col + passo; col != para_col; col += passo) {
            if (tabuleiro[de_lin][col] != ' ') {
                return false; // Tem um inimigo ou aliado no caminho!
            }
        }
    } else {
        // Movendo na vertical (mesma coluna)
        int passo = (para_lin > de_lin) ? 1 : -1;
        int linha = de_lin + passo;
        while (linha != para_lin) { // Usando while, como um shinobi paciente
            if (tabuleiro[linha][de_col] != ' ') {
                return false;
            }
            linha += passo;
        }
    }
    return true; // "Missão cumprida!"
}

// 🔥 Sasuke (Bispo) corre pelas diagonais — como o Chidori!
bool sasuke_pode_mover(int de_lin, int de_col, int para_lin, int para_col) {
    int dif_lin = para_lin - de_lin;
    int dif_col = para_col - de_col;

    // Só pode ir em diagonal perfeita
    if (abs(dif_lin) != abs(dif_col)) {
        return false; // "Esse movimento é fraco."
    }

    int passos = abs(dif_lin);
    int dir_lin = (dif_lin > 0) ? 1 : -1;
    int dir_col = (dif_col > 0) ? 1 : -1;

    int l = de_lin + dir_lin;
    int c = de_col + dir_col;
    int contador = 1;

    // Usando do-while: pelo menos um passo ele dá!
    do {
        if (tabuleiro[l][c] != ' ') {
            return false; // Alguém está no caminho do meu poder!
        }
        l += dir_lin;
        c += dir_col;
        contador++;
    } while (contador < passos);

    return true; // "Meu caminho está livre."
}

// 💖 Hinata (Rainha) combina o poder de todos — ela é completa!
bool hinata_pode_mover(int de_lin, int de_col, int para_lin, int para_col) {
    // Ela pode se mover como Kakashi OU como Sasuke
    return kakashi_pode_mover(de_lin, de_col, para_lin, para_col) ||
           sasuke_pode_mover(de_lin, de_col, para_lin, para_col);
}

// 🦌 Shikamaru (Cavalo) pula como um veado — em "L", sem se importar com obstáculos!
bool shikamaru_pode_mover(int de_lin, int de_col, int para_lin, int para_col) {
    // Os 8 jeitos que um cavalo pode pular (movimento em "L")
    int possibilidades_linha[8] = { -2, -2, -1, -1,  1,  1,  2,  2 };
    int possibilidades_coluna[8] = { -1,  1, -2,  2, -2,  2, -1,  1 };

    // Testamos cada uma das 8 opções (loop simples, mas representa múltiplas direções)
    for (int i = 0; i < 8; i++) {
        int nova_linha = de_lin + possibilidades_linha[i];
        int nova_coluna = de_col + possibilidades_coluna[i];

        // Se alguma delas bate com o destino...
        if (nova_linha == para_lin && nova_coluna == para_col) {
            return true; // "Que preguiça... mas tá certo."
        }
    }
    return false; // "Isso não é movimento de cavalo."
}

// 🌸 Função que mostra o tabuleiro com carinho
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
    printf("S/s = Shikamaru (Cavalo)  G/g = Genin (Peão)\n\n");
}

// 🎌 Função principal — onde a história começa
int main() {
    printf("~-~-~-~-~-~-~-~-~-~-~-~-~-~-~-~-~-~-~\n");
    printf("   BEM-VINDO AO XADREZ NINJA! 🍥\n");
    printf("   Folha vs Areia — Quem será Hokage?\n");
    printf("~-~-~-~-~-~-~-~-~-~-~-~-~-~-~-~-~-~-~\n");

    mostrar_tabuleiro_com_amor();

    printf("Vamos testar se os ninjas podem se mover corretamente...\n\n");

    // Teste 1: Kakashi tenta ir de a8 para a5 (vertical livre)
    if (kakashi_pode_mover(0, 0, 3, 0)) {
        printf("✅ Kakashi: \"Posso ir de a8 para a5. Meu caminho está limpo.\"\n");
    } else {
        printf("❌ Kakashi: \"Alguém está no meu caminho.\"\n");
    }

    // Teste 2: Sasuke de c8 para f5 (diagonal livre)
    if (sasuke_pode_mover(0, 2, 3, 5)) {
        printf("✅ Sasuke: \"Minha diagonal está livre. Vou avançar.\"\n");
    } else {
        printf("❌ Sasuke: \"Há um obstáculo. Não posso passar.\"\n");
    }

    // Teste 3: Hinata de d8 para f6
    if (hinata_pode_mover(0, 3, 2, 5)) {
        printf("✅ Hinata: \"Meu Byakugan vê o caminho. Posso ir!\"\n");
    } else {
        printf("❌ Hinata: \"Não consigo avançar...\"\n");
    }

    // Teste 4: Shikamaru de b8 para c6 (movimento em L clássico)
    if (shikamaru_pode_mover(0, 1, 2, 2)) {
        printf("✅ Shikamaru: \"Que preguiça... mas sim, posso pular para c6.\"\n");
    } else {
        printf("❌ Shikamaru: \"Isso dá muito trabalho. Não é um L válido.\"\n");
    }

    printf("\n✨ Que a paz reine entre as vilas! ✨\n");
    return 0;
}
