# Marlei no coletor: informações para a Fixee

Respostas aos 6 pontos pedidos no brief de integração do coletor.

## 1. Endereço da tela
- Tela de leitura: `https://marlei.com.br/coletor`
- Login: `https://marlei.com.br/coletor/entrar`
- Sair: `https://marlei.com.br/coletor/sair`

## 2. Domínios usados
Só **`marlei.com.br`**. A página, a API (`/backend/api/coletorApi.php`) e todo o
resto saem desse domínio. Sem CDN, sem fonte externa (usamos a fonte do sistema),
sem imagens externas e sem WebSocket. Tudo é servido pelo próprio Marlei.

## 3. Login
Form próprio na página (login + senha), uma conta compartilhada "coletor" do
cadastro do Marlei (não Google nem Microsoft). A sessão dura **30 dias** por um
cookie próprio (`coletor_token`, httponly/secure), reconstruída a cada abertura da
página, então sobrevive ao reinício do app e ao restart diário das 5h30. A tela de
leitura tem botão "Sair". A tela de login não registra o objeto de scanner.

## 4. Scanner e tipos de código
Usamos **`window.MarleiScanner`**: a função `onScan(codigo, tipo)` é registrada no
`<head>`, na janela principal, e dispara a busca sem bloquear. Ela existe na tela de
leitura e NÃO na tela de login (ali o app deve avisar "esta tela não recebe bipe",
como esperado).

Tipos de código dos produtos: **EAN-13 e UPC**. O **ITF-14** das caixas nós casamos
pelo EAN-13 da unidade que está nos nossos dados; se a caixa bipar um ITF-14 que não
corresponda ao EAN da unidade, a tela mostra "produto não encontrado". Se o uso de
caixa (ITF-14) for comum, nos avisem que adicionamos o mapeamento ITF-14 → unidade.

## 5. Observação sobre os dados
A tela mostra estatísticas de VENDA (vendido hoje, 7 e 30 dias, faturamento, ticket
médio, estoque, última venda). Um produto que nunca vendeu aparece como "não
encontrado", porque não há estatística dele.

## 6. Endereço de teste
Enviamos assim que publicarmos em staging ou na rede interna. Durante o teste local,
o endereço na rede é `http://<IP-da-maquina>:<porta>/coletor` (modo teste do app).
