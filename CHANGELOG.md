# Novidades do BOB BOT

## 02/11/2026 — Moderação à prova de pressa

### Novos recursos
- **Canal armadilha**: com `CANAL_ARMADILHA` configurado, quem escrever naquele canal leva softban na hora (ban + desban imediato, apagando as mensagens recentes). Vale para qualquer pessoa, inclusive staff, administradores e outros bots; só o próprio bot é ignorado. Cada caso é avisado em `CANAL_STAFF`.
- `?sync` — só o dono do bot re-sincroniza os comandos slash na mão.

### Melhorias
- `/clear` com muita mensagem não mostra mais "o aplicativo não respondeu": o bot avisa na hora que está pensando e apaga em seguida.
- Uma regra só de moderação: dono do servidor, administradores e cargo **ADM** — em ban, unban, kick, mute, unmute, lock, unlock, clear, role, automod e organizar_cargos.
- Os cargos **ADM** e os isentos da censura são reconhecidos pelo ID configurado em `CARGO_ADM_ID` e `AUTOMOD_ISENTOS`: renomear o cargo não muda mais quem manda. Sem `CARGO_ADM_ID`, o bot avisa e volta a comparar pelo nome.
- `/organizar_cargos` põe Dono, ADM, BobBOT e membros na ordem certa, de cima para baixo.
- Os comandos são sincronizados uma vez na inicialização (e o `?sync` refaz na mão), sem repetir a cada reconexão.
- As fontes do cartão de boas-vindas ficam em cache e o desenho + PNG rodam fora do loop, então o bot não trava com várias entradas seguidas.

### Corrigido
- Editar uma mensagem para incluir palavra bloqueada agora cai no automod — antes a edição passava direto pela censura.
- README e `.env.example` atualizados com as variáveis novas, e a linha do `/enquete` agora explica que a pergunta e cada opção são campos separados.

## 26/10/2026 — Cargo em dois toques

### Novos comandos
- `?role add @membro @cargo` e `?role remove @membro @cargo` dão e tiram cargos (também no `/role add`).
- Só o **dono** ou o cargo **ADM** usa; o cargo fica abaixo de quem usou e do BobBOT.

## 25/10/2026 — Automod não pune quem manda

### Melhorias
- O automod não apaga mais a mensagem de quem tem os cargos **dono** ou **adm**.
- Agora dá para usar `?automod remover` sem a própria mensagem ser censurada.
- Tentar bloquear um cargo em vez de palavra é recusado com aviso.

## 24/10/2026 — Limpeza de mensagens

### Novos comandos
- `/clear` (ou `?clear`) — apaga de 1 a 1000 mensagens em lotes: `/clear 50`.
- Só o dono do servidor, administradores e o cargo **ADM** usam.

### Como usar
- Confirmação discreta: some sozinha no `?clear` e só você vê no `/clear`.
- Mensagens com mais de 14 dias saem uma por uma; se o canal tiver menos que o pedido, o bot diz quantas apagou de verdade.

## 23/10/2026 — Moderação e automod

### Novos comandos
- `/lock` e `/unlock` fecham e abrem um canal; `/kick`, `/ban` e `/unban` (desbanir é pelo ID); `/mute` e `/unmute` silenciam por alguns minutos.
- `/automod add`, `/automod remover` e `/automod listar` escolhem as palavras censuradas.

### Como usar
- Só o cargo **ADM** usa esses comandos.
- A mensagem com palavra bloqueada é apagada e o bot avisa: "Ei! @fulano você não pode falar isto!".
- Quem tem o cargo automod não é censurado.

## 22/10/2026 — Comando de cargos e novidades no canal

### Novos comandos
- `/organizar_cargos` (ou `?organizar_cargos`) — reorganiza os cargos na ordem recomendada: Dono e ADM em cima, identidade no meio, bots embaixo. Só o cargo **ADM**.

### Melhorias
- O comando avisa o que não foi movido e pede para arrastar na mão os cargos criados por outros bots (Loritta, Jockie Music…).
- O bot posta as novidades **sozinho** a cada atualização. Não precisa mais pedir `/novidades`.

## 21/10/2026 — Novos comandos, prefixo `?` e boas-vindas renovadas

### Novos comandos
- `/novidades` mostra o histórico de atualizações; `/ping` mostra a latência; `/avatar` exibe o avatar de um membro.
- `/dado` rola um dado de 2 a 1.000 lados; `/enquete` cria uma enquete com 2 a 5 opções.
- `?enviar_embed [texto]` envia uma mensagem dentro de um embed.

### Suporte ao prefixo `?`
- Os comandos híbridos também rodam como mensagem normal: `?ping`, `?avatar`, `?dado`, `?enquete`, `?ola`, `?oi_posso_falar`, `?ajuda`.
- `?teste` e `?enviar_embed` continuam só pelo prefixo.
- Exemplo: `?enquete "Qual jogo?" "Minecraft" "Valorant"` — textos com espaço precisam de aspas.

### Melhorias
- Nova imagem de boas-vindas com avatar em destaque, nome de exibição, número do membro e mensagem de boas-vindas (substituiu o antigo retângulo preto).
- As enquetes recebem as reações numeradas de 1️⃣ a 5️⃣ conforme a quantidade de opções.

### Correções
- Botões de `/oi_posso_falar` voltaram a funcionar corretamente.
- Erros de comandos agora geram aviso no chat em vez de falhar em silêncio.
- As boas-vindas não derrubam mais o bot quando o canal configurado está indisponível.
- `/ajuda` lista os comandos das versões slash e prefixada.
## 02/11/2026 — Nova central de ajuda com abas, cargo Moderador e checagem /automod

### Novos recursos
- **Central de ajuda com abas**: `/ajuda` agora abre uma interface com abas navegáveis (Início, Social, Informações, Prefixo, Moderação, Servidor, AutoMod). A aba atual fica azul e travada nos botões. Só quem chamou o comando consegue navegar, e os botões somem após 3 minutos.
- **Nova estrutura de abas:**
  - 🏠 Início, 🎉 Social, 📰 Bot, ⌨️ Prefixo — visíveis para todos
  - 🛡️ Moderação — visível para Moderador ou superior
  - ⚙️ Servidor — visível para ADM ou superior
  - 🤖 AutoMod — visível para cargo automod (e ADM/dono)
- **Moderador (novo cargo):** cargo ID 1566858218725183592. Moderador pode usar: /lock, /unlock, /kick, /mute, /unmute e /clear. Limites: /mute até 24 h e /clear até 100 mensagens. ADM e acima têm 28 dias e 1000.
- **Apenas ADM ou superior:** /ban, /unban, /role, /organizar_cargos aparecem na aba Moderação.
- **Checagem /automod:** hoje o bot compara o cargo automod pelo nome, não pelo ID. Mantido assim; se renomear o cargo a checagem quebra. Dono e ADM têm acesso. Para liberar só o cargo automod, mude `AUTOMOD_APENAS_CARGO = True` no topo.
- **Aba Servidor atualizada:** /organizar_cargos e /role já eram só de ADM, mas a aba agora fica oculta para quem não pode usar. Incluídos /role add e /role remove, que existiam no código mas não estavam no ?ajuda.
- **Cargo Moderador separado:** na ordem `/organizar_cargos`, Moderador tem grupo próprio logo abaixo do BobBOT, para o bot continuar conseguindo punir moderadores.
- **Permissões do Moderador:** /lock e /clear funcionam sem Gerenciar canais para o Moderador, porque quem executa é o bot.

### Melhorias
- `/role add` e `/role remove`: subcomandos novos que dão e tiram cargos de membros (dono, admin e ADM). Incluídos na central de ajuda.
- A ordem de cargos em `/organizar_cargos` agora separa Moderador em grupo próprio abaixo do BobBOT, evitando que o bot fique abaixo de moderadores ao punir.
- Nova interface de `/ajuda` com destaque visual da aba ativa e navegação bloqueada para outros usuários.
- Os botões da central de ajuda agora têm timeout de 3 minutos e somem após inatividade.

### Corrigido
- Editar uma mensagem para incluir palavra bloqueada agora cai no automod — antes a edição passava direto pela censura.
- README e `.env.example` atualizados com as variáveis novas (`CARGO_MODERADOR_ID`, `AUTOMOD_APENAS_CARGO`, `MODERADOR_MUTE_MAX_MINUTOS`, `MODERADOR_CLEAR_MAX`).
- Estrutura de cargos em `/organizar_cargos` agora coloca Moderador abaixo do BobBOT, garantindo que o bot pode punir moderadores.
