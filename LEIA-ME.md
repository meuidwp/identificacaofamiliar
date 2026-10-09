# Identificação Familiar automática (PWA)

Aplicativo em que o técnico digita os dados do atendimento e baixa a ficha de **Identificação Familiar** do CRAS em PDF (4 páginas, modelo da Prefeitura), preenchida em Helvetica azul, **tudo em caixa alta**, pronta para imprimir e assinar.
Tudo roda no aparelho: nenhum dado é enviado a servidores e nada fica salvo depois que a página é fechada.

## Arquivos
| Arquivo | Para que serve |
|---|---|
| `index.html` | O aplicativo inteiro (tela, regras e modelo do formulário embutidos) |
| `manifest.webmanifest` | Nome, cores e ícones do aplicativo instalado |
| `sw.js` | Service worker: faz o aplicativo abrir sem internet |
| `icons/` | Ícones 48 a 512 px, versão *maskable* (Android), `apple-touch-icon` (iPhone/iPad) e favicon |

## O que a ficha preenche
- **Página 1:** CRAS, data, demanda (espontânea ou encaminhada, com o texto) e todos os dados do responsável.
- **Página 2:** programas sociais (quadradinhos), renda, situação habitacional com valor de aluguel ou financiamento.
- **Página 3:** composição familiar, até 14 pessoas.
- **Página 4:** rede de apoio (2 linhas), 11 códigos de vulnerabilidade (2 caracteres cada), demanda identificada com detalhes e técnico(a).

**Enter passa para o próximo campo** de digitação (nome, nome social, telefone...). Em um nome de familiar deixado vazio, o Enter encerra a lista e vai para a rede de apoio; no último campo, o Enter leva ao botão de gerar. Quadradinhos e opções ficam para o clique ou o Tab.

Máscaras automáticas para telefone, RG (12.345.678-9, aceita X no final), CPF (com aviso se o dígito verificador não bater), NIS, datas e valores em reais.
Os códigos de vulnerabilidade seguem a "tabela no verso", que não veio no PDF enviado: o aplicativo só escreve o código digitado.

## Como publicar (precisa de HTTPS)
O navegador só permite instalar PWA e usar service worker em endereço `https://` (ou `localhost`).
Abrir o `index.html` direto do disco funciona como página comum, mas **não instala**.

Opções gratuitas, todas enviando a pasta inteira:
- **Netlify Drop**: app.netlify.com/drop e arraste esta pasta.
- **GitHub Pages**: suba os arquivos em um repositório e ative Settings > Pages.
- **Cloudflare Pages**: Create project > Upload assets.

Para testar no computador: dentro desta pasta, rode `python3 -m http.server 8000` e abra `http://localhost:8000`.

Este app e o da Justificativa de Ponto podem ficar no mesmo site, cada um na sua pasta (ex.: `/justificativa/` e `/identificacao-familiar/`); eles não se misturam.

## Instalar
- **Chrome/Edge (computador e Android):** botão "Instalar aplicativo" no topo da página, ou o ícone de instalar na barra de endereço.
- **iPhone/iPad (Safari):** Compartilhar > Adicionar à Tela de Início.

## Atualizar uma versão
Troque os arquivos no hospedeiro e aumente `VERSION` em `sw.js` (ex.: `v1` para `v2`). Os aparelhos atualizam na próxima abertura.

## Observações
- O formulário usa as medidas do modelo "Identificação Familiar" enviado. Se a Prefeitura mudar o modelo, as posições dos campos precisam ser refeitas.
- Tudo é digitado e impresso em caixa alta, inclusive as sugestões das listas (escolaridade, estado civil, raça/cor etc.). Textos longos demais para o espaço têm a letra reduzida automaticamente; se ainda assim não couberem, o texto é cortado com "…" na borda do campo e o aplicativo avisa quais campos.
