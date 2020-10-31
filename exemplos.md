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