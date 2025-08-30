# app-camva
app novo
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>Gerador de 10 Artes Automáticas</title>
  <script src="https://sdk.canva.com/designembed/v1/sdk.js"></script>
  <style>
    body { font-family: Arial, sans-serif; padding: 20px; }
    input, button { margin: 8px 0; padding: 8px; width: 100%; }
    label { font-weight: bold; margin-top: 10px; display: block; }
  </style>
</head>
<body>
  <h2>Gerar Packs Automáticos (10 Artes por Empresa)</h2>

  <label for="sheetId">ID da Planilha do Google:</label>
  <input type="text" id="sheetId" placeholder="Coloque aqui o ID da planilha">

  <label for="apiKey">API Key do Google:</label>
  <input type="text" id="apiKey" placeholder="Sua API Key do Google Cloud">

  <button id="gerar">Gerar Packs</button>

  <script>
    const app = Canva.App.init();

    async function carregarDados(sheetId, apiKey) {
      const url = https://sheets.googleapis.com/v4/spreadsheets/${sheetId}/values/A2:E?key=${apiKey};
      const response = await fetch(url);
      const data = await response.json();
      return data.values; // Retorna lista de empresas
    }

    document.getElementById("gerar").onclick = async () => {
      const sheetId = document.getElementById("sheetId").value;
      const apiKey = document.getElementById("apiKey").value;

      if (!sheetId || !apiKey) {
        alert("Por favor, preencha o ID da planilha e a API Key.");
        return;
      }

      const empresas = await carregarDados(sheetId, apiKey);

      empresas.forEach(([empresa, logoUrl, cor, slogan, promocao], index) => {
        const baseY = index * 1200; // desloca as artes por empresa

        // Função auxiliar para criar textos centralizados
        function addTexto(text, size, color, x, y) {
          app.createText({
            text: text,
            fontSize: size,
            color: color,
            x: x,
            y: y
          });
        }

        // Função auxiliar para adicionar logo
        function addLogo(url, x, y, w=200, h=200) {
          if (url) {
            app.createImage({ url: url, x: x, y: y, width: w, height: h });
          }
        }

        // Arte 1 - Logo + Slogan
        addLogo(logoUrl, 100, baseY + 50);
        addTexto(slogan || empresa, 48, cor, 350, baseY + 120);

        // Arte 2 - Promoção
        addTexto("Oferta da " + empresa, 52, cor, 100, baseY + 300);
        addTexto(promocao || "Desconto imperdível!", 64, "#ff9900", 100, baseY + 380);

        // Arte 3 - Motivacional
        addTexto("Sua marca, sua força!", 48, cor, 100, baseY + 500);

        // Arte 4 - Nome da empresa grande
        addTexto(empresa, 80, cor, 100, baseY + 650);

        // Arte 5 - Logo centralizado
        addLogo(logoUrl, 400, baseY + 700, 300, 300);

        // Arte 6 - Slogan grande
        addTexto(slogan || "Seu sucesso começa aqui!", 56, cor, 100, baseY + 1100);

        // Arte 7 - Promoção destacada
        addTexto(promocao || "Até 70% OFF", 72, "#ff0000", 100, baseY + 1250);

        // Arte 8 - Frase curta
        addTexto("Exclusivo " + empresa, 60, cor, 100, baseY + 1400);

        // Arte 9 - Combinação
        addLogo(logoUrl, 100, baseY + 1600, 150, 150);
        addTexto(slogan || "Sempre com você!", 48, cor, 300, baseY + 1650);

        // Arte 10 - Grande destaque
        addTexto(empresa + " - " + (promocao || "Ofertas únicas!"), 64, cor, 100, baseY + 1850);
      });
    };
  </script>
</body>
</html>
