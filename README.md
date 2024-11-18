<!DOCTYPE html>
<html lang="en">
  <head>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.10.0/p5.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.10.0/addons/p5.sound.min.js"></script>
    <link rel="stylesheet" type="text/css" href="style.css">
    <meta charset="utf-8" />

  </head>
  <body>
    <main>
    </main>
    <script src="sketch.js"></script>
    <script src="p5.collide2d.js"></script>
  </body>
</html>

//variáveis da bolinha
let xBolinha = 100; //deixar a variável xBolinha com valor 100
let yBolinha = 200; //deixar a variável yBolinha com valor 200
let diametro = 15;  //deixar o valor do diâmetro com valor 22
let raio = diametro / 2; //deixar seu raio como diâmetro dividido por 2

//variáveis do oponente
let xRaqueteOponente = 585; //deixar variável da raquete do oponente com valor 585
let yRaqueteOponente = 150; //deixar variável da raquete do oponente com valor 150

//velocidade da bolinha
let velocidadeXBolinha = 6; //deixar velocidade da variável com valor 6
let velocidadeYBolinha = 6; //deixar velocidade da variável com valor 6

//variáveis da raquete
let xRaquete = 5; //variável x da raquete com valor 5
let yRaquete = 150; //variável y da raquete com valor 150
let raqueteComprimento = 10; //variável do comprimento da raquete com valor 10
let raqueteAltura = 90; //variável da altura da raquete com valor 90

//placar do jogo
let meusPontos = 0; //zerar meus pontos no início da partida 
let pontosDoOponente = 0; //zerar pontos do oponente no início da partida

//sons do jogo
let raquetada; // deixar variável do som "raquetada"
let ponto; //deixar variável do som "ponto"
let trilha; //deixar variável do som "trilha"

let colidiu = false; //variável de colisão

function setup() { //preparar a tela 
  createCanvas(600, 400); //criar canvas
  trilha.loop(); //som da variável "trilha" em loop
  
}

function draw() { //desenhar cenário 
  background(0); //fundo de tela 
  mostraBolinha(); //variável para mostrar bolinha
  movimentaBolinha(); //variável para movimentar bolinha
  verificaColisaoBorda(); //variável para verificar colisão borda
  mostraRaquete(xRaquete, yRaquete); //variável para mostrar raquete, variáveis da raquete
  movimentaMinhaRaquete(); //variável para movimentar minha raquete 
  verificaColisaoRaquete (xRaquete, yRaquete); //variável para verificar colisão da raquete, variáveis da raquete
  verificaColisaoRaquete (xRaqueteOponente, yRaqueteOponente); //variável para verificar colisão da raquete, variáveis da raquete do oponente
  mostraRaquete (xRaqueteOponente, yRaqueteOponente); //variável para mostrar raquete, variáveis da raquete do oponente
  movimentaRaqueteOponente ();
  incluiPlacar() 
  marcaPonto();
}

function mostraBolinha() { //função mostrar a bolinha 
  circle(xBolinha, yBolinha, diametro); //posição e diâmetro do círculo (nossa bolinha)
}

function movimentaBolinha() { //função movimentar a bolinha
  xBolinha += velocidadeXBolinha; //X da bolinha será seu valor acrescido de sua velocidade
  yBolinha += velocidadeYBolinha; //Y  da bolinha será seu valor acrescido de sua velocidade
}

function verificaColisaoBorda() { //função verificar a colisão da borda 
   if (xBolinha + raio > width || xBolinha - raio < 0) { //se xBolinha adicionada ao seu raio for maior que largura/ se xBolinha diminuido ao seu raio for menor que 0...
    velocidadeXBolinha *= -1; //multiplique ou iguale a -1
  }
  if (yBolinha + raio > height || yBolinha - raio < 0) { //se yBolinha adicionada ao seu raio for maior que largura/ se yBolinha diminuido ao seu raio for menor que 0...
    velocidadeYBolinha *= -1; //multiplique ou iguale a -1
  }
}

function mostraRaquete(x, y) { //função mostrar a raquete
  rect(x, y, raqueteComprimento, raqueteAltura); //retângulo, as variáveis de sua posição, de seu comprimento e altura
}

function mostraRaqueteOponente() { //função mostrar a raquete do oponente
  rect(xRaqueteoponente, yRaqueteOponente, raqueteComprimento, raqueteAltura); //retângulo, as variáveis de sua posição, de seu comprimento e altura
 }

function movimentaMinhaRaquete() { //função movimentar minha bolinha
  if(keyIsDown(UP_ARROW)) { //se tecla seta para cima for pressionada..
    yRaquete -= 10; //subtraia posição de Y
  }
  if(keyIsDown(DOWN_ARROW)) { //se tecla seta para baixo for pressionada...
    yRaquete += 10; //adicione posição de Y
  }
}

function verificaColisaoRaquete() { //função verificar colisão da raquete
if (xBolinha - raio < xRaquete + raqueteComprimento && yBolinha - raio < yRaquete + raqueteAltura && yBolinha + raio > yRaquete) { //se xBolinha subtraindo o raio for menor que xRaquete adicionado a comprimento da raquete e yBolinha menor que raio for menor que yRaquete adicionado à altura da  raquete e yBolinha adicionado ao raio maior que yRaquete
  velocidadeXBolinha *= -1; //velocidade da xBolinha 
  raquetada.play(); //tocar  som raquetada.play
  
 }
 }

function verificaColisaoRaquete (x, y) { //função verificar a colisão da raquete
    colidiu = collideRectCircle(x, y, raqueteComprimento, raqueteAltura, xBolinha, yBolinha, raio); //colidiu equivale a collideRectCircle, variáveis x e y, comprimento da raquete, altura da raquete, xBolinha, yBolinha e seus respectivos raios
    if (colidiu) { //se a bolinha colidir com a raquete
        velocidadeXBolinha *= -1; //velocidadeXBolinha *= -1
      raquetada.play(); //tocar a trilha sonora raquetada
        
    }
}

function movimentaRaqueteOponente(){ //função movimentar a raquete do oponente
    if (keyIsDown(87)){ //se a tecla W for pressionada...
        yRaqueteOponente -= 10; //yRaquete menos ou igual a 10
    }
    if (keyIsDown(83)){ //se a tecla S for pressionada...
        yRaqueteOponente += 10; //yRaquete mais ou igual a 10
    }
}

function incluiPlacar(){ //função para incluir o placar
    stroke (255); //contorno dos elementos, cor branca
    textAlign(CENTER); //determina a posição do texto
    textSize(18); //determina o tamanho do texto
    fill(color(255,140, 0)); //preencher cor do placar
    rect(150, 10, 40, 20); //tamanho e posição do retângulo 
    fill(255); //cor é determinada
    text(meusPontos, 170, 26); //tamanho e posição do texto do meu        placar
    fill(color(255,140, 0)); //preencher cor do placar
    rect(450, 10, 40, 20); //tamanho e posição do retângulo
    fill(255); //cor é determinada
    text(pontosDoOponente, 470, 26); //tamanho e posição do texto do placar do oponente 

}


 function marcaPonto() { //função marcar os pontos
  if (xBolinha > 595) { //se xBolinha for maior que 595...
    meusPontos += 1; //meus pontos adicionam ou se equivalem a 1
    ponto.play(); //tocar trilha sonora ponto 
  }
  if (xBolinha < 10) { //se xBolinha for menor que 11...
    pontosDoOponente += 1; //pontos do oponente adicionam ou se eqivalem a 1
    ponto.play(); //tocar trilha sonora ponto
    
  }
  }

function preload(){ //função dos sons
  trilha = loadSound("trilha.mp3"); //trilha é equivalente a "trilha.mp3"
  ponto = loadSound("ponto.mp3"); //ponto é equivalente a "ponto.mp3"
  raquetada = loadSound("raquetada.mp3"); //raquetada é equivalente a "raquetada.mp3"
 }
