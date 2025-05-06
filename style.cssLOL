<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulação de Erosão</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.9.0/p5.js"></script>
    <style>
        body {
            margin: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
        }
        #canvas-holder {
            border: 1px solid #aaa;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            border-radius: 8px;
            overflow: hidden; /* Garante que nada saia dos limites do canvas */
        }
        .button-container {
            margin-top: 20px;
            display: flex;
            gap: 10px;
        }
        .tipo-solo-button {
            padding: 10px 15px;
            border: none;
            border-radius: 5px;
            background-color: #ddd;
            cursor: pointer;
            font-size: 14px;
            transition: background-color 0.3s ease;
        }
        .tipo-solo-button:hover {
            background-color: #ccc;
        }
        .tipo-solo-button.selected {
            background-color: #aaa;
            color: white;
        }
    </style>
</head>
<body>
    <div id="canvas-holder"></div>
    <div class="button-container">
        <button class="tipo-solo-button" data-tipo="vegetacao">Vegetação</button>
        <button class="tipo-solo-button" data-tipo="exposto">Exposto</button>
        <button class="tipo-solo-button" data-tipo="urbanizado">Urbanizado</button>
    </div>
    <script>
        let gotas = [];
        let solo;
        let tipoSolo = "vegetacao"; // valor inicial
        let botoes;

        function setup() {
            let canvas = createCanvas(600, 400);
            canvas.parent("canvas-holder");
            solo = new Solo(tipoSolo);
            botoes = document.querySelectorAll('.tipo-solo-button');
            botoes.forEach(botao => {
                botao.addEventListener('click', function() {
                    tipoSolo = this.dataset.tipo;
                    solo = new Solo(tipoSolo);
                    atualizarBotoesSelecionados();
                });
            });
            atualizarBotoesSelecionados();
        }

        function draw() {
            background(200, 220, 255); // céu

            for (let i = gotas.length - 1; i >= 0; i--) {
                gotas[i].cair();
                gotas[i].mostrar();

                if (gotas[i].atingeSolo(solo.altura)) {
                    solo.aumentarErosao();
                    gotas.splice(i, 1);
                }
            }

            solo.mostrar();

            if (frameCount % 5 === 0) {
                gotas.push(new Gota());
            }
        }

        function atualizarBotoesSelecionados() {
            botoes.forEach(botao => {
                if (botao.dataset.tipo === tipoSolo) {
                    botao.classList.add('selected');
                } else {
                    botao.classList.remove('selected');
                }
            });
        }


        class Gota {
            constructor() {
                this.x = random(width);
                this.y = 0;
                this.vel = random(4, 6);
            }

            cair() {
                this.y += this.vel;
            }

            mostrar() {
                stroke(0, 0, 200);
                line(this.x, this.y, this.x, this.y + 10);
            }

            atingeSolo(ySolo) {
                return this.y > ySolo;
            }
        }

        class Solo {
            constructor(tipo) {
                this.tipo = tipo;
                this.altura = height - 80;
                this.erosao = 0;
                this.casas = []; // Array para armazenar as casas
                this.arvores = []; // Array para armazenar as árvores
                this.cavalos = []; // Array para armazenar os cavalos
                if (this.tipo === "urbanizado") {
                  this.gerarCasas();
                } else if (this.tipo === "vegetacao") {
                  this.gerarArvores();
                } else if (this.tipo === "exposto") {
                    this.gerarCavalos();
                }
            }

          gerarCasas() {
            // Determina o número de casas com base na largura do solo urbanizado
            let numCasas = Math.floor(width / 80); // Ajuste o divisor para controlar a densidade
            for (let i = 0; i < numCasas; i++) {
              let x = (width / numCasas) * i + width / (numCasas * 2);
              let larguraCasa = random(20, 40);
              let alturaCasa = random(30, 50);
              this.casas.push({
                x: x,
                y: this.altura - alturaCasa,
                largura: larguraCasa,
                altura: alturaCasa,
              });
            }
          }
          gerarArvores() {
              let numArvores = Math.floor(width / 60);  // Ajuste o divisor para controlar a densidade das árvores
              for (let i = 0; i < numArvores; i++) {
                let x = (width / numArvores) * i + width / (numArvores * 2);
                let alturaArvore = random(50, 120);
                let larguraTronco = random(5, 15);
                this.arvores.push({
                  x: x,
                  y: this.altura - alturaArvore,
                  altura: alturaArvore,
                  larguraTronco: larguraTronco,
                });
              }
            }

            gerarCavalos() {
                let numCavalos = Math.floor(width / 100);  // Ajuste o divisor para controlar a densidade dos cavalos
                for (let i = 0; i < numCavalos; i++) {
                  let x = (width / numCavalos) * i + width / (numCavalos * 2);
                  let alturaCavalo = random(30, 60);
                  let larguraCavalo = random(40, 70);
                  this.cavalos.push({
                    x: x,
                    y: this.altura - alturaCavalo,
                    largura: larguraCavalo,
                    altura: alturaCavalo,
                    cor: [random(100, 200), random(50, 100), random(0, 50)], // Cor marrom variada
                  });
                }
            }

            aumentarErosao() {
                let taxa;
                if (this.tipo === "vegetacao") taxa = 0.1;
                else if (this.tipo === "exposto") taxa = 0.5;
                else if (this.tipo === "urbanizado") taxa = 0.3;
                this.erosao += taxa;
                this.altura += taxa;
            }

            mostrar() {
                noStroke();
                if (this.tipo === "vegetacao") fill(60, 150, 60);
                else if (this.tipo === "exposto") fill(139, 69, 19);
                else if (this.tipo === "urbanizado") fill(120);

                rect(0, this.altura, width, height - this.altura);

                if (this.tipo === "urbanizado") {
                  this.mostrarCasas();
                } else if (this.tipo === "vegetacao") {
                  this.mostrarArvores();
                } else if (this.tipo === "exposto") {
                    this.mostrarCavalos();
                }

                fill(0);
                textSize(14);
                textAlign(LEFT);
                text(`Erosão: ${this.erosao.toFixed(1)}`, 10, 20);
                text(`Tipo de solo: ${this.tipo}`, 10, 40);
            }

          mostrarCasas() {
            for (let casa of this.casas) {
              fill(200); // Cor das casas
              rect(casa.x, casa.y, casa.largura, casa.altura);
              // Adiciona telhado
              fill(150, 50, 50);
              triangle(
                casa.x, casa.y,
                casa.x + casa.largura / 2, casa.y - 10,  // Ajuste a altura do telhado se necessário
                casa.x + casa.largura, casa.y
              );
            }
          }
          mostrarArvores() {
              for (let arvore of this.arvores) {
                // Desenha o tronco
                fill(100, 50, 0); // Cor marrom para o tronco
                rect(arvore.x - arvore.larguraTronco / 2, arvore.y, arvore.larguraTronco, arvore.altura);

                // Desenha a copa da árvore
                fill(0, 100, 0); // Cor verde para a copa
                ellipse(arvore.x, arvore.y - arvore.altura / 2, arvore.altura / 2, arvore.altura / 3); // Ajuste os valores para mudar a forma da copa
              }
            }

            mostrarCavalos() {
              for (let cavalo of this.cavalos) {
                fill(cavalo.cor); // Usa a cor definida para o cavalo
                // Desenha o corpo do cavalo (um retângulo alongado)
                rect(cavalo.x, cavalo.y, cavalo.largura, cavalo.altura / 2);
                // Desenha a cabeça do cavalo (um círculo menor)
                ellipse(cavalo.x + cavalo.largura, cavalo.y + cavalo.altura / 4, cavalo.altura / 2, cavalo.altura / 2);
                // Desenha as pernas do cavalo (linhas verticais)
                stroke(0); // Cor preta para as pernas
                line(cavalo.x + cavalo.largura / 4, cavalo.y + cavalo.altura / 2, cavalo.x + cavalo.largura / 4, cavalo.y + cavalo.altura);
                line(cavalo.x + cavalo.largura * 3/4, cavalo.y + cavalo.altura / 2, cavalo.x + cavalo.largura * 3/4, cavalo.y + cavalo.altura);
              }
            }
        }
    </script>
</body>
</html>
