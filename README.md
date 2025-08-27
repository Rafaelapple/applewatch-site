<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Promoção Dia das Crianças</title>
  <style>
    body {
      font-family: 'Comic Sans MS', Arial, sans-serif;
      margin: 0;
      padding: 0;
      background: linear-gradient(to bottom, #fff8e1, #ffe0b2);
      color: #333;
      text-align: center;
    }

    header {
      background: #ff6f00;
      color: #fff;
      padding: 25px 15px;
      font-size: 1.8em;
      animation: pulse 2s infinite;
    }

    @keyframes pulse {
      0%, 100% { transform: scale(1); }
      50% { transform: scale(1.05); }
    }

    #temporizador {
      font-size: 1.2em;
      background: #d32f2f;
      color: #fff;
      padding: 10px;
      font-weight: bold;
      display: none;
    }

    .produtos {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
      gap: 20px;
      padding: 30px 10px;
      max-width: 1200px;
      margin: auto;
    }

    .produto {
      background: #fff;
      border: 3px dashed #ff6f00;
      border-radius: 15px;
      padding: 20px;
      transition: transform 0.3s, box-shadow 0.3s;
    }

    .produto:hover {
      transform: translateY(-5px);
      box-shadow: 0 6px 15px rgba(0,0,0,0.15);
    }

    .produto img {
      width: 100%;
      height: 180px;
      object-fit: cover;
      border-radius: 10px;
    }

    .preco-original {
      text-decoration: line-through;
      color: #888;
      font-size: 0.9em;
    }

    .preco-desconto {
      color: #d32f2f;
      font-size: 1.4em;
      font-weight: bold;
    }

    .produto button {
      background: #d32f2f;
      color: white;
      border: none;
      padding: 10px 20px;
      border-radius: 8px;
      cursor: pointer;
      font-size: 1em;
      margin-top: 10px;
      transition: background 0.3s;
    }

    .produto button:hover {
      background: #b71c1c;
    }

    #caixa-surpresa {
      cursor: pointer;
      display: inline-block;
      margin: 50px auto;
    }

    #caixa-surpresa .caixa {
      width: 160px;
      height: 160px;
      background: conic-gradient(#ffca28, #ff8f00, #f4511e, #ffca28);
      border-radius: 15px;
      display: flex;
      align-items: center;
      justify-content: center;
      color: #fff;
      font-size: 1.2em;
      font-weight: bold;
      box-shadow: 0 6px 15px rgba(0,0,0,0.2);
      transition: transform 0.5s, background 0.5s;
      margin: auto;
    }

    #mensagem-desconto {
      display: none;
      font-size: 1.2em;
      color: #388e3c;
      font-weight: bold;
      margin-top: 15px;
    }

    .carrinho {
      position: fixed;
      bottom: 20px;
      right: 20px;
      background: #388e3c;
      color: white;
      padding: 12px 18px;
      border-radius: 12px;
      font-weight: bold;
      font-size: 0.95em;
      z-index: 999;
      box-shadow: 0 4px 12px rgba(0,0,0,0.25);
    }

    .toast {
      position: fixed;
      bottom: 70px;
      right: 20px;
      background: #ff9800;
      color: #fff;
      padding: 12px 18px;
      border-radius: 10px;
      font-weight: bold;
      opacity: 0;
      transform: translateY(20px);
      transition: opacity 0.5s, transform 0.5s;
      z-index: 1000;
    }

    .toast.show {
      opacity: 1;
      transform: translateY(0);
    }
  </style>
</head>
<body>

<header>🎉 Promoção Dia das Crianças 🎉</header>
<div id="temporizador">⏰ Tempo restante com desconto: <span id="tempo">10:00</span></div>

<main>
  <!-- Caixa Surpresa -->
  <section id="caixa-surpresa" onclick="abrirCaixa()">
    <div class="caixa">🎁 Clique aqui!</div>
  </section>

  <div id="mensagem-desconto">🎈 Desconto de 45% ativado! Aproveite por tempo limitado!</div>

  <!-- Lista de Produtos -->
  <section class="produtos" id="lista-produtos" style="display: none;"></section>
</main>

<!-- Carrinho e Toast -->
<div class="carrinho" id="carrinho">Carrinho: 0 itens – R$ 0,00</div>
<div class="toast" id="toast"></div>

<script>
  const produtos = [
    { nome: "Boneco Labubu", preco: 199.00, imagem: "https://down-br.img.susercontent.com/file/br-11134207-7r98o-m9zzn8o7e1q1aa" },
    { nome: "Tablet Samsung", preco: 1499.00, imagem: "https://gazin-images.gazin.com.br/cNBvTxoXDdo6uT5Wqw3FkJ4nzl8=/1920x/filters:format(webp):quality(75)/https://gazin-marketplace.s3.amazonaws.com/midias/imagens/2024/07/tablet-samsung-galaxy-tab-s6-lite-104-64gb-4gb-android-14-sm-p620nzadzto-112407032345.jpg" },
    { nome: "Bola da Copa 2022", preco: 120.00, imagem: "https://img.odcdn.com.br/wp-content/uploads/2022/07/bola-copa-22.jpg" },
    { nome: "Pista de Skate de Dedo", preco: 75.00, imagem: "https://a-static.mlcdn.com.br/800x600/pista-skate-dedo-profissional-rampa-e-corrimao-com-skate-dm-toys/fastvarejoloja/dmt6686-dellboard/53052402e90354dba997df19b78e5515.jpeg" },
    { nome: "Quadro do Neymar", preco: 89.00, imagem: "https://m.media-amazon.com/images/I/71YgRiaji1L._UF350,350_QL80_.jpg" },
    { nome: "Carrinho Controle Remoto", preco: 299.00, imagem: "https://i.zst.com.br/thumbs/12/c/3b/-1347048930.jpg" },
    { nome: "Hoverboard Infantil", preco: 999.00, imagem: "https://m.media-amazon.com/images/I/51zpqiSLytL._UF894,1000_QL80_.jpg" },
    { nome: "Celular Samsung", preco: 2199.00, imagem: "https://planoscelular.claro.com.br/medias/18599-0-zero-300Wx300H-productCard?context=bWFzdGVyfGltYWdlc3w2ODEzN3xpbWFnZS9wbmd8YUdKaUwyaGlZUzg1TlRrME1qUTBORGszTkRNNEx6RTROVGs1WHpCZmVtVnliMTh6TURCWGVETXdNRWhmY0hKdlpIVmpkRU5oY21RfDcyZjliNjg1OTk0NzNiOTI5ZTFkZDJkN2I3NWQzMTU2NDk3MTY5ZTk4NjM4OWExZTkwNWM1Y2Q3YTQ0MzI2OTY" },
    { nome: "Patinete Eletrônico", preco: 899.00, imagem: "https://www.mymax.ind.br/wp-content/uploads/2020/02/009239_1.jpg" },
    { nome: "Boobie Goods", preco: 150.00, imagem: "https://a-static.mlcdn.com.br/1500x1500/kit-120-canetinhas-livro-de-colorir-bobbie-goods-ponta-dupla-estojo-50-paginas-estojo-de-colorir-120-canetinhas/mbcomericoatacado/kitcnt120rosa/70d56208f977f12dc898ce1c86d36b81.jpeg" },
    { nome: "Patins", preco: 299.00, imagem: "https://freitasvarejo.vteximg.com.br/arquivos/ids/175986-440-500/16092876001_1.jpg?v=637993807985830000" },
    { nome: "Máscara do Batman", preco: 129.00, imagem: "https://superlegalbrinquedos.vtexassets.com/arquivos/ids/228056/Mascara-Eletronica---Batman-Armor-Up---15-Sons-e-Luzes---DC-Comics---Sunny-1.jpg?v=638422414673770000" },
    { nome: "Boneco do Coringa", preco: 249.00, imagem: "https://http2.mlstatic.com/D_NQ_NP_949891-MLB89275608717_082025-O-boneco-coringa-estatua-de-resina-joker-23cm.webp" },
    { nome: "Caixa de Som JBL", preco: 1099.00, imagem: "https://d3alv7ekdacjys.cloudfront.net/Custom/Content/Products/10/65/1065174_caixa-de-som-jbl-boombox-portatil-com-bluetooth-a-prova-dagua-camuflado_m1_636911497357161076.webp" },
  ];

  let totalItens = 0;
  let totalValor = 0;
  let descontoAtivo = false;
  let tempoRestante = 600;

  function abrirCaixa() {
    if (descontoAtivo) return;
    descontoAtivo = true;

    const caixa = document.querySelector("#caixa-surpresa .caixa");
    const mensagem = document.getElementById("mensagem-desconto");
    const lista = document.getElementById("lista-produtos");
    const timer = document.getElementById("temporizador");

    caixa.innerText = "Desbloqueando...";
    setTimeout(() => {
      caixa.style.display = "none";
      mensagem.style.display = "block";
      timer.style.display = "block";
      lista.style.display = "grid";
      carregarProdutos();
      iniciarTemporizador();
    }, 1000);
  }

  function carregarProdutos() {
    const lista = document.getElementById("lista-produtos");
    produtos.forEach(p => {
      const precoComDesconto = (p.preco * 0.55).toFixed(2);
      const produtoEl = document.createElement("article");
      produtoEl.className = "produto";
      produtoEl.innerHTML = `
        <img src="${p.imagem}" alt="${p.nome}">
        <h2>${p.nome}</h2>
        <p class="preco-original">R$ ${p.preco.toFixed(2).replace('.', ',')}</p>
        <p class="preco-desconto">R$ ${precoComDesconto.replace('.', ',')}</p>
        <button onclick="adicionarAoCarrinho('${p.nome}', ${precoComDesconto})">Comprar</button>
      `;
      lista.appendChild(produtoEl);
    });
  }

  function iniciarTemporizador() {
    const timerEl = document.getElementById("tempo");
    const interval = setInterval(() => {
      if (tempoRestante <= 0) {
        clearInterval(interval);
        timerEl.innerText = "00:00";
        encerrarDesconto();
        return;
      }
      const min = Math.floor(tempoRestante / 60);
      const seg = tempoRestante % 60;
      timerEl.innerText = `${String(min).padStart(2, '0')}:${String(seg).padStart(2, '0')}`;
      tempoRestante--;
    }, 1000);
  }

  function encerrarDesconto() {
    document.querySelectorAll(".produto button").forEach(btn => {
      btn.disabled = true;
      btn.innerText = "Tempo Esgotado";
      btn.style.background = "#aaa";
      btn.style.cursor = "not-allowed";
    });
    mostrarToast("⛔ Tempo esgotado! O desconto foi encerrado.");
  }

  function adicionarAoCarrinho(produto, preco) {
    totalItens++;
    totalValor += preco;
    atualizarCarrinho();
    mostrarToast(`✅ ${produto} adicionado ao carrinho!`);
  }

  function atualizarCarrinho() {
    document.getElementById("carrinho").innerText =
      `Carrinho: ${totalItens} item(s) – R$ ${totalValor.toFixed(2).replace('.', ',')}`;
  }

  function mostrarToast(mensagem) {
    const toast = document.getElementById("toast");
    toast.textContent = mensagem;
    toast.classList.add("show");
    setTimeout(() => toast.classList.remove("show"), 2500);
  }
</script>

</body>
</html>
