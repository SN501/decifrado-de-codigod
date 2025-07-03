<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="Cifra y descifra mensajes con código César, Morse, Binario, Polibio y más. Herramienta online gratuita.">
  <meta name="keywords" content="código César, Morse, cifrado, descifrado, encriptar mensajes, herramienta online">
  <meta name="author" content="Santiago Nigro">
  <title>Cifrador y Descifrador Universal</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #000;
      color: #006400;
      padding: 20px;
    }
    .container {
      max-width: 700px;
      margin: auto;
      background-color: #111;
      padding: 20px;
      border-radius: 10px;
    }
    textarea, select, input[type="number"], button {
      width: 100%;
      margin: 10px 0;
      padding: 10px;
      font-size: 16px;
    }
    button {
      background-color: #006400;
      color: #fff;
      border: none;
      cursor: pointer;
    }
    button:hover {
      background-color: #00a000;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Cifrador y Descifrador Universal</h1>
    
    <textarea id="mensaje" placeholder="Escribe tu mensaje..."></textarea>

    <select id="tipo">
      <option value="cesar">César</option>
      <option value="morse">Morse</option>
      <option value="binario">Binario</option>
      <option value="polibio">Polibio</option>
    </select>

    <input id="desplazamiento" type="number" placeholder="Desplazamiento (solo para César)" value="3">

    <button onclick="procesar('cifrar')">Cifrar</button>
    <button onclick="procesar('descifrar')">Descifrar</button>

    <h2>Resultado:</h2>
    <textarea id="resultado" readonly></textarea>
  </div>

  <script>
    const alfabeto = "ABCDEFGHIJKLMNÑOPQRSTUVWXYZ";

    function procesar(accion) {
      const tipo = document.getElementById("tipo").value;
      const mensaje = document.getElementById("mensaje").value.toUpperCase();
      const desplazamiento = parseInt(document.getElementById("desplazamiento").value) || 0;
      let resultado = "";

      if (tipo === "cesar") {
        resultado = mensaje.replace(/[A-ZÑ]/g, letra => {
          let i = alfabeto.indexOf(letra);
          if (i === -1) return letra;
          let nuevoIndex = accion === "cifrar"
            ? (i + desplazamiento) % alfabeto.length
            : (i - desplazamiento + alfabeto.length) % alfabeto.length;
          return alfabeto[nuevoIndex];
        });
      } else if (tipo === "binario") {
        if (accion === "cifrar") {
          resultado = mensaje.split('').map(c => c.charCodeAt(0).toString(2).padStart(8, '0')).join(' ');
        } else {
          resultado = mensaje.split(' ').map(b => String.fromCharCode(parseInt(b, 2))).join('');
        }
      } else if (tipo === "morse") {
        const morse = {
          A: ".-", B: "-...", C: "-.-.", D: "-..", E: ".", F: "..-.",
          G: "--.", H: "....", I: "..", J: ".---", K: "-.-", L: ".-..",
          M: "--", N: "-.", Ñ: "--.--", O: "---", P: ".--.", Q: "--.-",
          R: ".-.", S: "...", T: "-", U: "..-", V: "...-", W: ".--",
          X: "-..-", Y: "-.--", Z: "--..", " ": "/"
        };
        const inverso = Object.fromEntries(Object.entries(morse).map(([k,v])=>[v,k]));

        if (accion === "cifrar") {
          resultado = mensaje.split('').map(c => morse[c] || c).join(' ');
        } else {
          resultado = mensaje.split(' ').map(c => inverso[c] || c).join('');
        }
      } else if (tipo === "polibio") {
        const cuadricula = "ABCDEFGHIJKLMNÑOPQRSTUVWXYZ";
        if (accion === "cifrar") {
          resultado = mensaje.replace(/[A-ZÑ]/g, letra => {
            let i = cuadricula.indexOf(letra);
            if (i === -1) return letra;
            const fila = Math.floor(i / 5) + 1;
            const col = (i % 5) + 1;
            return `${fila}${col}`;
          });
        } else {
          let pares = mensaje.match(/.{1,2}/g) || [];
          resultado = pares.map(par => {
            let fila = parseInt(par[0]) - 1;
            let col = parseInt(par[1]) - 1;
            let i = fila * 5 + col;
            return cuadricula[i] || par;
          }).join('');
        }
      }

      document.getElementById("resultado").value = resultado;
    }
  </script>
</body>
</html>
# decifrado-de-codigod
