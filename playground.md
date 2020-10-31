---
layout: page
title: inge9 - playground
---

<!-- 
TODO:

* parar código
 -->

<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.58.2/codemirror.min.css">
<script src="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.58.2/codemirror.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.58.2/addon/edit/matchbrackets.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.58.2/mode/clike/clike.min.js"></script>

<textarea id="editor">
#include &lt;inge9&gt;
int main() {
  double x = -50, y = 50;
  for (;;) {
    clear("black");
    drawText("Hello", x, y, 22, "white");
    x += 4;
    if (x > canvasWidth()) x = -50;
    delay(20);
  }
  return 0;
}</textarea>

<script>
  editor = CodeMirror.fromTextArea(document.getElementById("editor"), {
    lineNumbers: true,
    matchBrackets: true,
    mode: "text/x-c++src",
    extraKeys: {
      "Ctrl-Space": "autocomplete",
      "Ctrl-Enter": function () { runCode(); },
      "Cmd-Enter": function () { runCode(); }
    }
  });
</script>

<button id="run">Rodar</button>
<canvas id="gamecanvas" width="640" height="360" style="background: black;"></canvas>
<p/>

<script src="assets/JSCPP.es5.min.js"></script>
<script type="text/javascript">
  async function run(code, count, config) {
    config = config || {};
    if (!('stdio' in config)) {
      config.stdio = {
        write: function(s) {
          console.log(s);
        }
      }
    }
    config.debug = true;
    let mydebugger = JSCPP.run(code, '', config);
    let finished = false;
    window["forceQuit" + count] = false;
    console.log("begin");
    do {
      window.debuggerPromise = undefined;
      finished = mydebugger.next();
      if (window.debuggerPromise) {
        await window.debuggerPromise;
      }
      
    } while (!finished && !window["forceQuit" + count]);
    console.log("end");
    delete window["forceQuit" + count];
  }
</script>

<script type="text/javascript">  
counter = 0;

var canvas = document.getElementById("gamecanvas");
var context = canvas.getContext('2d');

function runCode() {
    // stop previous running code
    window["forceQuit" + counter] = true;
    // run code
    counter++;
    context.clearRect(0, 0, canvas.width, canvas.height);
    run(editor.getValue(), counter);
}
document.getElementById("run").addEventListener("click", runCode);
</script>