---
layout: page
title: inge9 - exemplos
---

- [Exemplo exercitando o uso das funções do inge9](#exemplo-exercitando-o-uso-das-funções-do-inge9)

**Como usar os exemplos:**

Copie e cole o código no [playground](playground) para rodar o jogo.

## Exemplo exercitando o uso das funções do inge9

```c++
#include <iostream>
#include <cmath>
#include <inge9>
using namespace std;

int main() {
  fillRect(0, 0, 400, 90, "red");
  drawRect(100, 100, 260, 190, "blue");
  drawLine(0, 0, 100, 100, "yellow");
  drawText("Press any key to continue", 8, 30, 22, "white");
  readKey();
  clear("black");
  drawText(lastKey(), 8, 30, 22, "white");
  
  loadImage("panda", "https://upload.wikimedia.org/wikipedia/commons/b/bf/Farm-Fresh_emotion_face_panda.png");
  loadImage("hero", "https://upload.wikimedia.org/wikipedia/commons/thumb/1/11/Cc-by_new_white.svg/48px-Cc-by_new_white.svg.png");
  waitUntilResourcesLoad();

  double x = 180, y = 120, delta = 4;
  double heroy = 200;
  for (;;) {
    clear("black");
    if (isKeyDown("ArrowLeft")) x -= delta;
    if (isKeyDown("ArrowRight")) x += delta;
    if (isKeyDown("ArrowUp")) y -= delta;
    if (isKeyDown("ArrowDown")) y += delta;
    if (isKeyDown(" ")) {
      heroy += 5;
      clearKey(" ");
    }
  
    drawText(lastKey(), 8, 30, 22, "white");
    drawImage("hero", 400, heroy);
    drawImage("panda", x, y);
    delay(30);
  }
  
  return 0;
}
```

## Campo minado

```c++
#include <inge9>
#include <cstdlib>
#include <ctime>
#include <iostream>

using namespace std;

#define OCULTA 0
#define REVELADA 1
#define BANDEIRA 2

int qtdRevelados = 0;
int linhas = 8, colunas = 8;
int qtdMinas = 5;
// números de 1 a 8 = números de 1 a 8
// -1 representa bomba
// 0 representa célula vazia
int campo[linhas][colunas];
// estado:
//  0 se a célula está oculta
//  1 se a célula está revelada
//  2 se a célula possui uma bandeira
int estado[linhas][colunas];

void revelaPos(int x, int y){
  if(estado[x][y]!=REVELADA){
      estado[x][y]=REVELADA;    
      qtdRevelados++;
      if (campo[x][y]==0) {
          int v1[]={-1,0,1};
          int v2[]={-1,0,1};
          for(int i=0;i<3;i++){
              for(int j=0;j<3;j++){
                  if(x+v1[i]>=0&&x+v1[i]<linhas&&y+v2[j]>=0&&y+v2[j]<colunas&&estado[x+v1[i]][y+v2[j]]!=REVELADA){
                      revelaPos(x+v1[i],y+v2[j]);
                  }
              }
          }
      }
  }
}

/*
CONTROLES:

- Setas direcionais: move o cursor
- Enter: coloca/tira bandeira
- Espaço: revela célula
*/
int main() {
  int tamBloco = 20; // em pixels
  int cursorx = 0, cursory = 0;
  
  // inicializa campo minado com zeros
  for (int y = 0; y < linhas; y++) {
    for (int x = 0; x < colunas; x++) {
      campo[y][x] = 0;
      estado[y][x] = 0;
    }
  }

  // posiciona minas aleatoriamente
  srand(time(0));
  for (int i = 0; i < qtdMinas; i++) {
    int x, y;
    do {
      x = rand() % colunas;
      y = rand() % linhas;
    } while (campo[y][x] == -1);
    campo[y][x] = -1;
  }
  
  // calcula outras células (números)
  for (int y = 0; y < linhas; y++) {
    for (int x = 0; x < colunas; x++) {
      if (campo[y][x] == -1) {
        continue;
      }
      int numBombas = 0;
      if (y > 0 && campo[y - 1][x] == -1) numBombas++;
      if (y > 0 && x < colunas - 1 && campo[y - 1][x + 1] == -1) numBombas++;
      if (x < colunas - 1 && campo[y][x + 1] == -1) numBombas++;
      if (x < colunas - 1 && y < linhas - 1 && campo[y + 1][x + 1] == -1) numBombas++;
      if (y < linhas - 1 && campo[y + 1][x] == -1) numBombas++;
      if (x > 0 && y < linhas - 1 && campo[y + 1][x - 1] == -1) numBombas++;
      if (x > 0 && campo[y][x - 1] == -1) numBombas++;
      if (x > 0 && y > 0 && campo[y - 1][x - 1] == -1) numBombas++;
      
      campo[y][x] = numBombas;
    }
  }
  
  // carrega imagens
  loadImage("flag", "https://cdn.discordapp.com/attachments/752538025615163462/778379613977051206/flag.png");
  loadImage("bomb", "https://cdn.discordapp.com/attachments/752538025615163462/778379844328489001/bomb.png");  
  waitUntilResourcesLoad();

  bool gameOver = false;
  while (true) {
    // desenha campo minado  
    clear("black");
    for (int y = 0; y < linhas; y++) {
      for (int x = 0; x < colunas; x++) {

        // quadrado cinza
        fillRect(x * tamBloco, y * tamBloco, tamBloco, tamBloco, "lightgray");

        if (estado[y][x] == REVELADA) {
          fillRect(x * tamBloco, y * tamBloco, tamBloco, tamBloco, "darkgray");
          if (campo[y][x] == -1) {
            drawImage("bomb", x * tamBloco, y * tamBloco);
          } else if (campo[y][x] >= 1 && campo[y][x] <= 8) {
            drawText(campo[y][x], (x + 0.3) * tamBloco, (y + 0.7) * tamBloco, 12, "white");
          }
        } else if (estado[y][x] == BANDEIRA) {
          drawImage("flag", x * tamBloco, y * tamBloco);
        }

        drawRect(x * tamBloco, y * tamBloco, tamBloco, tamBloco, "white");
        
        // desenha o cursor
        if (x == cursorx && y == cursory) {
            drawRect(x * tamBloco, y * tamBloco, tamBloco, tamBloco, "cyan");
        }
      }
    }

    if (gameOver) {
      break;
    }

    // lê uma tecla
    readKey();
    if (isKeyDown("ArrowLeft") && cursorx > 0) {
      cursorx--;
    } else if (isKeyDown("ArrowRight") && cursorx < colunas - 1) {
      cursorx++;
    } else if (isKeyDown("ArrowUp") && cursory > 0) {
      cursory--;
    } else if (isKeyDown("ArrowDown") && cursory < linhas - 1) {
      cursory++;
    } else if (isKeyDown(" ")) {
      // estado[cursory][cursorx] = REVELADA;
      revelaPos(cursory, cursorx);
      if (campo[cursory][cursorx] == -1 || qtdRevelados == colunas * linhas - qtdMinas) {
        gameOver = true;
      }
    } else if (isKeyDown("Enter")) {
      if (estado[cursory][cursorx] == BANDEIRA) {
        estado[cursory][cursorx] = OCULTA;
      } else {
        estado[cursory][cursorx] = BANDEIRA;
      }
    }

    cout << "leu tecla";
  }

  if (campo[cursory][cursorx] == -1) {
    drawText("Você perdeu!", 0, 30, 28, "orange");
  } else {
    drawText("Você ganhou!", 0, 30, 28, "green");
  }

  
  return 0;
}
```