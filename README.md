# BobBOT

BobBOT é um bot de Discord escrito em Python com **discord.py 2.x** e **Pillow**.
Todos os comandos principais são **híbridos**: funcionam com o prefixo `?` e
também como slash `/` no servidor.

## Funcionalidades

- **Boas-vindas com imagem** — gera um cartão com avatar e nome quando alguém entra.
- **Novidades automáticas** — lê o `CHANGELOG.md` e publica sozinho no canal de novidades.
- **Automod** — bloqueia palavras da sua lista (guardada em `logs/automod.json`), compara sem acento e por palavra inteira.
- **Moderação** — ban, kick, mute temporário, fechar/abrir canal e limpeza em lote de mensagens.
- **Cargos** — organiza a hierarquia do servidor e dá/tira cargos por comando.
- **Diversão/utilidade** — dado, enquete, avatar, ping e cartão "posso falar?".

## Comandos

### Geral (qualquer membro)

| Comando | O que faz |
| --- | --- |
| `/ola` · `/ping` · `/ajuda` | Teste, latência e lista de comandos |
| `/avatar @membro` | Avatar do membro |
| `/dado` | Rola um dado |
| `/enquete pergunta opção1 opção2 …` | Enquete com até 5 opções — a pergunta e cada opção são campos separados (no `?enquete`, use aspas em textos com espaço) |
| `/novidades` | Mostra as últimas mudanças do bot |
| `/oi_posso_falar` | Botões "Sim/Não" com imagem |

### Administração (dono do servidor, administradores ou cargo **ADM**)

| Comando | O que faz |
| --- | --- |
| `/ban` · `/unban` · `/kick` | Banir, desbanir (por ID) e expulsar |
| `/mute` · `/unmute` | Silenciar por N minutos / devolver a fala |
| `/lock` · `/unlock` | Fechar/abrir o canal para membros |
| `/clear 50` | Apaga até 1000 mensagens em lotes |
| `/organizar_cargos` | Reordena os cargos na hierarquia recomendada |
| `/role add @membro @cargo` · `/role remove …` | Dá ou tira um cargo |
| `/automod add\|remover\|listar` | Palavras censuradas |

> Permissões por **ID de cargo**: `CARGO_ADM_ID` libera a moderação e `AUTOMOD_ISENTOS` fica fora da censura. Sem `CARGO_ADM_ID`, o bot avisa no console e volta a comparar pelo nome ("adm" / "dono").

### Só com prefixo `?`

- `?teste` — teste rápido de conexão.
- `?enviar_embed` — envia um embed escrito na mão.
- `?sync` — re-sincroniza os comandos slash (só o dono do bot).

### Canal armadilha (softban)

Com `CANAL_ARMADILHA` configurado, qualquer pessoa que **escrever** nesse canal leva softban (ban seguido de unban imediato, apagando as mensagens recentes). Não há exceção de cargo: staff, administradores, o dono do servidor e outros bots também caem. Só o próprio bot é ignorado.

- O canal precisa de uma **mensagem fixa clara** avisando que escrever ali resulta em remoção — quem não leu o aviso não tem como saber.
- **A Loritta e qualquer outro bot não devem postar nesse canal**: os bots também são banidos pela armadilha.
- Cada caso é avisado em `CANAL_STAFF` (se configurado), com nome, ID e o resultado ("softban concluído", "NÃO consegui banir…", "banido, mas o desban falhou…").
- Sem `CANAL_ARMADILHA`, o recurso fica totalmente desligado.

## Requisitos

- Python **3.10 ou superior** (testado em 3.14.8)
- `discord.py` 2.7.1 e `Pillow` 13.0.0 (ver `requirements.txt`)
- Um bot criado no [Discord Developer Portal](https://discord.com/developers/applications)

## Instalação

```bash
git clone https://github.com/MoraesProgramando/BobBOT.git
cd BobBOT

python -m venv .venv
.venv/bin/pip install -r requirements.txt

cp .env.example .env   # depois edite o .env e cole seu token
```

## Configuração (`.env`)

| Variável | Obrigatória | Descrição |
| --- | --- | --- |
| `DISCORD_TOKEN` | **sim** | Token do bot (Developer Portal → Bot) |
| `CANAL_BOAS_VINDAS` | não | ID do canal de boas-vindas (padrão embutido no código) |
| `CANAL_NOVIDADES` | não | ID do canal de novidades (padrão embutido no código) |
| `CANAL_ARMADILHA` | não | Canal armadilha: quem escrever ali leva softban (sem ela, desligado) |
| `CANAL_STAFF` | não | Canal que recebe o aviso de cada armadilha |
| `CARGO_ADM_ID` | não | ID do cargo ADM (quem modera); aceita vários, separados por vírgula. Sem ela, o bot avisa e compara pelo nome "adm" |
| `AUTOMOD_ISENTOS` | não | IDs dos cargos livres da censura, separados por vírgula |

> O `.env` está no `.gitignore` e **nunca** deve ser enviado ao repositório.

## Execução

```bash
.venv/bin/python main.py                            # primeiro plano (log na tela)
nohup .venv/bin/python main.py >> bot.log 2>&1 &   # em segundo plano
```

## Estrutura

```
main.py           bot inteiro (ponto de entrada)
imagens/          imagens enviadas pelo bot
logs/             saída do bot, automod.json e estado (gitignored)
.env.example      modelo das variáveis de ambiente
CHANGELOG.md      novidades (o /novidades lê este arquivo)
```

## Changelog — BOB BOT

Histórico de alterações, novos comandos, melhorias e correções do **BOB BOT**, organizado do mais recente para o mais antigo. O arquivo [`CHANGELOG.md`](CHANGELOG.md) continua sendo a fonte lida pelo comando `/novidades` e pelo post automático de novidades.

---

### [02/11/2026] — Moderação à prova de pressa

#### ✨ Novos recursos

- **Canal armadilha**: quem escrever no canal configurado em `CANAL_ARMADILHA` leva softban (ban + desban imediato, apagando as mensagens recentes), inclusive staff, administradores e outros bots. O `CANAL_STAFF` recebe o aviso de cada caso.
- `?sync` re-sincroniza os comandos slash — só o dono do bot usa.

#### 🐛 Corrigido

- `/clear` com muita mensagem não mostra mais "o aplicativo não respondeu": o bot avisa que está pensando e apaga em seguida.
- Editar uma mensagem para incluir palavra bloqueada agora cai no automod; antes a edição passava direto pela censura.

#### 🔒 Permissões

- Regra única de moderação: dono do servidor, administradores e cargo **ADM** — em ban, unban, kick, mute, unmute, lock, unlock, clear, role, automod e organizar_cargos.
- Cargos **ADM** e isentos da censura são reconhecidos pelo **ID** configurado em `CARGO_ADM_ID` e `AUTOMOD_ISENTOS`; sem `CARGO_ADM_ID` o bot avisa e compara pelo nome.

---

### [26/10/2026] — Cargos em dois toques

#### ✨ Novos comandos

- `?role add @membro @cargo` — adiciona um cargo a um membro.
- `?role remove @membro @cargo` — remove um cargo de um membro.
- `/role add` — permite adicionar cargos usando comandos slash.

#### 🔒 Permissões e segurança

- Os comandos de gerenciamento de cargos são exclusivos do dono do servidor e de membros com o cargo **ADM**.
- O bot verifica a hierarquia dos cargos antes de permitir a operação.
- O cargo atribuído precisa estar abaixo do cargo de quem executa o comando e do cargo do próprio BOB BOT.

---

### [25/10/2026] — Automod não pune quem manda

#### ✨ Melhorias

- O automod não apaga mais mensagens de membros com os cargos **dono** ou **ADM**.
- O comando `?automod remover` pode ser utilizado sem que a própria mensagem do comando seja censurada.
- Tentativas de bloquear um cargo em vez de uma palavra são recusadas, acompanhadas de um aviso explicativo.

---

### [24/10/2026] — Limpeza de mensagens

#### ✨ Novos comandos

- `/clear` — apaga mensagens do canal usando um comando slash.
- `?clear` — permite realizar a mesma operação pelo prefixo tradicional.

#### ⚙️ Funcionamento

- Permite solicitar a exclusão de **1 a 1.000 mensagens**.
- As mensagens são apagadas em lotes, respeitando os limites do Discord.
- Mensagens com mais de 14 dias são excluídas individualmente.
- Se o canal tiver menos mensagens do que o solicitado, o bot informa a quantidade realmente apagada.

#### 🔒 Permissões

O comando pode ser utilizado por:

- Dono do servidor.
- Administradores do Discord.
- Membros com o cargo **ADM**.

#### 💬 Respostas

- Com `?clear`, a confirmação desaparece automaticamente.
- Com `/clear`, a confirmação é privada e visível somente para quem executou o comando.

**Exemplos:**

```text
?clear 50
/clear 50
```

---

### [23/10/2026] — Moderação e automod

#### ✨ Novos comandos

| Comando | Função |
|---|---|
| `/lock` | Bloqueia o canal para os membros. |
| `/unlock` | Libera novamente o canal para os membros. |
| `/kick` | Expulsa um membro do servidor. |
| `/ban` | Bane um membro do servidor. |
| `/unban` | Remove um banimento usando o ID do usuário. |
| `/mute` | Impede temporariamente um membro de falar. |
| `/unmute` | Remove a restrição de fala. |
| `/automod add` | Adiciona uma palavra à lista de palavras bloqueadas. |
| `/automod remover` | Remove uma palavra da lista de bloqueio. |
| `/automod listar` | Exibe as palavras bloqueadas. |

#### 🛡️ Proteção automática

- Mensagens que contêm palavras bloqueadas são apagadas automaticamente.
- O bot envia um aviso mencionando o membro que enviou a mensagem.
- Membros com o cargo **automod** ficam isentos da censura automática.

#### 🔒 Permissões

Os comandos de moderação e automod são restritos a membros com o cargo **ADM**.

---

### [22/10/2026] — Organização de cargos e novidades automáticas

#### ✨ Novo comando

- `/organizar_cargos`
- `?organizar_cargos`

Reorganiza os cargos do servidor seguindo a ordem recomendada pelo bot:

1. Dono e ADM.
2. Cargos de identidade.
3. Bots.

#### ⚙️ Melhorias

- O bot informa quais cargos não puderam ser movidos.
- Cargos gerenciados por outros bots, como Loritta e Jockie Music, podem precisar de ajuste manual.
- O BOB BOT passou a publicar automaticamente as novidades neste canal a cada atualização, sem precisar executar `/novidades`.

#### 🔒 Permissões

O comando de organização é exclusivo de membros com o cargo **ADM**.

---

### [21/10/2026] — Novos comandos, prefixo `?` e boas-vindas renovadas

#### ✨ Novos comandos

| Comando | Função |
|---|---|
| `/novidades` | Exibe o histórico de atualizações do bot. |
| `/ping` | Mostra a latência do bot. |
| `/avatar [membro]` | Exibe o avatar de um membro em tamanho grande. |
| `/dado [lados]` | Rola um dado de 2 a 1.000 lados. |
| `/enquete` | Cria uma enquete com 2 a 5 opções. |
| `?enviar_embed [texto]` | Envia uma mensagem dentro de um cartão formatado. |

#### ⚡ Suporte ao prefixo `?`

Os comandos compatíveis também podem ser executados como mensagens normais, incluindo:

- `?ping`
- `?avatar`
- `?dado`
- `?enquete`
- `?ola`
- `?oi_posso_falar`
- `?ajuda`

Os comandos `?teste` e `?enviar_embed` continuam disponíveis pelo prefixo.

**Exemplo de enquete:**

```text
?enquete "Qual jogo?" "Minecraft" "Valorant"
```

Textos com espaços precisam estar entre aspas quando utilizados como argumentos de comandos por prefixo.

#### 🎨 Melhorias nas boas-vindas

- Nova imagem de boas-vindas com o avatar do membro em destaque.
- Inclusão do nome de exibição, número de membro e mensagem de boas-vindas.
- Substituição do antigo retângulo preto por um cartão personalizado.

#### 🗳️ Melhorias nas enquetes

- As enquetes recebem automaticamente as reações numeradas de 1️⃣ a 5️⃣, conforme a quantidade de opções.

#### 🛠️ Correções

- Botões de `/oi_posso_falar` voltaram a funcionar corretamente.
- Erros de comandos passaram a gerar avisos no chat, em vez de falharem silenciosamente.
- As boas-vindas não interrompem mais o funcionamento do bot quando o canal configurado está indisponível.
- O comando `/ajuda` foi atualizado para listar os comandos disponíveis nas versões slash e prefixada.

---

### 📖 Como utilizar os comandos

#### Comandos slash (`/`)

Digite `/` no chat do Discord e selecione o comando desejado na lista exibida.

#### Comandos por prefixo (`?`)

Digite o comando como uma mensagem normal, por exemplo:

```text
?ping
?dado
?clear 50
```

#### Parâmetros opcionais

Os parâmetros entre colchetes, como `[membro]`, são opcionais. Quando não são informados, o bot utiliza o comportamento padrão definido para o comando.

#### Respostas privadas e públicas

- **Comandos slash (`/`):** podem utilizar respostas privadas, visíveis somente para quem executou o comando.
- **Comandos por prefixo (`?`):** as respostas são públicas, salvo quando houver um comportamento específico diferente.

---

*BOB BOT — histórico de desenvolvimento e atualizações.*

---

## Contribuindo

1. Abra uma **Issue** explicando o problema ou a ideia antes de codar.
2. Crie uma **branch** por tarefa (`git checkout -b feat/role-remove`).
3. Escreva mensagens de commit claras (`git commit -m "feat: ..."`) e envie com `git push`.
4. **Nunca** commite tokens, `.env`, logs ou dados de membros.