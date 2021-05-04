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

<script>
window.onbeforeunload = function(event) {
  return confirm("Tem certeza de que deseja atualizar a página? O código que você escreveu será perdido.");
};
</script>

<textarea id="editor">
#include &lt;inge9&gt;
int main() {
  double x = -50, y = 50;
  while (true) {
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
<canvas tabindex="1" id="gamecanvas" width="640" height="360" style="background: black;"></canvas>

<p></p>

<p><b>Saída (cout) e mensagens de erro:</b><p>
<textarea id="output" disabled style="width: 640px; height: 200px;"></textarea>

<p></p>
<p><b>Entrada (cin):</b><p>
<textarea id="input" style="width: 640px;" rows="5"></textarea>

<script>
// Logs to textarea
let oldLog = console.log;
let oldError = console.err;
var outputElem = document.getElementById('output');
outputElem.value = '';
const newLogger = function (message) {
    if (typeof message == 'object') {
        outputElem.value += (JSON && JSON.stringify ? JSON.stringify(message).replace(/\\n/g, '\n') : message);
    } else {
        outputElem.value += message.replace(/\\n/g, '\n');
    }
}
console.log = newLogger;
console.error = newLogger;
</script>

<script src="assets/JSCPP.es5.min.js"></script>
<script type="text/javascript">
  async function run(code, count, input, config) {
    config = config || {};
    if (!('stdio' in config)) {
      config.stdio = {
        write: function(s) {
          console.log(s);
        }
      }
    }
    try {
      config.debug = true;
      let mydebugger = JSCPP.run(code, input, config);
      let finished = false;
      window["forceQuit" + count] = false;
      do {
        window.debuggerPromise = undefined;
        finished = mydebugger.next();
        if (window.debuggerPromise) {
          await window.debuggerPromise;
        }
      } while (!finished && !window["forceQuit" + count]);
    } catch (e) {
      newLogger(e.message);
      document.getElementById("output").scrollIntoView();
    }
    delete window["forceQuit" + count];
  }
</script>

<script type="text/javascript">  
counter = 0;

var canvas = document.getElementById("gamecanvas");
var context = canvas.getContext('2d');
var inputElem = document.getElementById("input");

function runCode() {
    // stop previous running code
    window["forceQuit" + counter] = true;
    // run code
    counter++;
    context.clearRect(0, 0, canvas.width, canvas.height);
    canvas.focus();
    run(editor.getValue(), counter, inputElem.value);
}
document.getElementById("run").addEventListener("click", runCode);
</script>