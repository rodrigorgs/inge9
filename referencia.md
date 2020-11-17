---
layout: page
title: inge9 - referência
---

- [Desenho básico](#desenho-básico)
- [Imagens](#imagens)
- [Tela](#tela)
- [Tempo](#tempo)
- [Teclado](#teclado)
- [Representação de teclas](#representação-de-teclas)
- [Representação de cores](#representação-de-cores)

## Desenho básico

```c++
clear(string cor)
```
Pinta toda a tela com a cor `cor`.

Consulte a seção [Cores](#cores) para saber como especificar uma cor.

> Exemplos: `clear("black")`, `clear("#ff0000")`

```c++
fillRect(double x, double y, double w, double h, string cor)
```

Desenha um retângulo com `w` de largura e `h` de altura, se estendendo para a direita e para baixo a partir da posição (`x`, `y`). O retângulo é preenchido com a cor `cor`.

> Exemplo: `fillRect(0, 0, 200, 100, "red")`

```c++
drawRect(double x, double y, double w, double h, string cor)
```

Similar a `fillRect`, exceto que desenha apenas o contorno do retângulo, usando a cor `cor`.

```c++
drawLine(double x1, double y1, double x2, double y2, string cor)
```

Desenha uma linha da posição (`x1`, `y1`) até a posição (`x2`, `y2`).

```c++
drawText(string texto, double x, double y, double tam, string cor)
```

Escreve o texto `texto` na tela, na posição (`x`, `y`), com tamanho de `tam` pixels, na cor `cor`.

> Exemplo: `drawText("Hello World", 8, 30, 22, "white");`


## Imagens

```c++
loadImage(string id, string url)
```

Carrega a imagem disponível no endereço `url`, e atribui a ela o identificador `id`. Não pode haver duas imagens com o mesmo `id`, e não pode haver outros elementos na página com o mesmo `id`.

> Exemplo: `loadImage("panda", "https://upload.wikimedia.org/wikipedia/commons/b/bf/Farm-Fresh_emotion_face_panda.png");`

```c++
waitUntilResourcesLoad()
```

Aguarda até que todas as imagens sejam carregadas antes de continuar a execução do programa. Normalmente é usado após uma sequência de chamadas a `loadImage(id, url)`.

```c++
drawImage(string id, double x, double y)
```

Desenha a imagem identificada por `id` na posição (`x`, `y`). A posição se refere ao ponto onde será posicionado o canto superior esquerdo da imagem.

Exemplo:

```c++
loadImage("panda", "https://upload.wikimedia.org/wikipedia/commons/b/bf/Farm-Fresh_emotion_face_panda.png");
loadImage("hero", "https://upload.wikimedia.org/wikipedia/commons/thumb/1/11/Cc-by_new_white.svg/48px-Cc-by_new_white.svg.png");

waitUntilResourcesLoad();

drawImage("panda", 0, 0);
drawImage("panda", 50, 0);
drawImage("hero", 100, 50);
```

```c++
drawImageTile(string id, double x, double y, int tileWidth, int tileHeight, int tileId)
```

Desenha uma região da imagem identificada por `id` na posição (`x`, `y`). A imagem é dividida em regiões retangulares de tamanho `tileWidth`x`tileHeight`, numeradas sequencialmente a partir de 0. A numeração cresce da esquerda para a direita e de cima para baixo.

## Tela

```c++
canvasWidth()
```

Retorna a largura da tela de desenho, do tipo *double*.

> Exemplo: `double w = canvasWidth();`

```c++
canvasHeight()
```

Retorna a altura da tela de desenho, do tipo *double*.

> Exemplo: `double h = canvasHeight();`

## Tempo

```c++
delay(int ms)
```

Aguarda `ms` milisegundos antes de continuar a execução do programa.

## Teclado

```c++
readKey()
```

Aguarda até o usuário pressionar uma tecla antes de continuar a execução do programa. Você pode descobrir a tecla pressionada com a função `lastKey()`.

```c++
lastKey()
```

Retorna a tecla pressionada pelo usuário na ocasião da última execução de `readKey()`, como uma string. Consulte a seção sobre [representação de teclas](#representação-de-teclas) para mais informações.

```c++
isKeyDown(string key)
```

Retorna um booleano (`true` ou `false`) indicando se a tecla `key` está pressionada. Consulte a seção sobre [representação de teclas](#representação-de-teclas) para mais informações.

```c++
clearKey(string key)
```

Altera o estado da tecla `key` para não-pressionada (mesmo que esteja sendo pressionada), de forma que as próximas chamadas a `isKeyDown(key)` retornarão `false` até o usuário pressionar a tecla novamente.


## Representação de teclas

As teclas são representadas no inge9 através de um código tipo string.

Se a tecla produz um caractere ao ser pressionada em um campo de edição de texto, o código é esse caractere. Exemplos: `"A"`, `"a"`, `"!"`, `" "`, `"a"`. Note que uma mesma tecla pode ser representada com códigos diferentes, a depender do estado das teclas Shift, Caps Lock, entre outras.

Algumas teclas comuns:

- Teclas de navegação: `ArrowLeft`, `ArrowRight`, `ArrowUp`, `ArrowDown`, `PageUp`, `PageDown`, `Home`, `End`
- Teclas modificadoras: `Alt`, `Control`, `Shift`, `CapsLock`
- Teclas de espaço em branco: `Enter`, `Tab`, ` ` (espaço)
- Teclas de edição: `Backspace`, `Delete` 

Para outros códigos de teclas, consulte a [documentação da Mozilla](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key/Key_Values).

## Representação de cores

As funções de desenho do inge9 aceitam qualquer especificação de cor que possa ser entendida pelo navegador. Há varias formas de representar cores:

- Nome: algumas cores podem ser representadas pelo seu nome, como `black`, `red`, `darkgreen`, `deepskyblue`, dentre outras. Consulte a [lista completa](https://www.rapidtables.com/web/css/css-color.html).
- Notação hexadecimal, `#RRGGBB`: `#` seguido de 6 dígitos hexadecimais (isto é, entre `0` e `f`), onde os 2 primeiros dígitos representam o componente vermelho, os 2 dígitos do meio representam o componente verde e os dois últimos representam o componente azul. Exemplos: `#ff0000` representa vermelho, `#000000` representa preto.
  - É possível fornecer dois dígitos hexadecimais adicionais, que representam a opacidade, onde `00` representa totalmente transparente e `ff` representa totalmente opaco.
- Notação hexadecimal, `#RGB`: `#` seguido de 3 dígitos hexadecimais (isto é, entre `0` e `f`), onde primeiro dígito representa o componente vermelho, o segundo dígito representa o componente verde e o último dígito representa o componente azul. Exemplos: `#f00` representa vermelho, `#000` representa preto.
  - É possível fornecer um dígito hexadecimal adicional, que representa a opacidade, onde `0` representa totalmente transparente e `f` representa totalmente opaco.
- Notação funcional, `rgb(R, G, B, A)`: onde `R`, `G`, `B` representam os componentes vermelho, verde e azul, respectivamente. Os componentes podem ser especificados com o um inteiro entre `0` e `255` ou como uma porcentagem entre `0%` e `100%`. O componente `A` é opcional, e representa a opacidade da cor. O componente `A` pode ser especificado como um número entre `0.0` e `1.0` ou como uma porcentagem entre `0%` e `100%`.
- Notação funcional, `hsl(H, S, L, A)`: especifica uma cor através de seus componentes de matiz (*hue*, `H`), saturação (`S`) e luminosidade (`L`), opcionalmente com um valor de opacidade (`A`). `H` é um número inteiro entre `0` e `359`, onde `0` representa a cor vermelha, `120` o verde e `240` o azul. Os valores de saturação podem ser especificados como um número entre `0.0` e `1.0` ou como uma porcentagem entre `0%` e `100%`.

Para mais informações, consulte a [documentação da Mozilla sobre cores](https://developer.mozilla.org/pt-BR/docs/Web/CSS/color_value).



