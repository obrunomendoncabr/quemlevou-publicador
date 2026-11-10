# quemlevou-publicador

Publica os reels do **@quemlevou** direto no Instagram, no horário marcado. Separado do publicador do @obrunomendonca: token, segredos e fila próprios.

## Como funciona

Cada post é uma pasta, com o nome `AAAA-MM-DD-slug`:

```
posts/
  rascunhos/    preparando, nada aqui é publicado
  agendados/    aprovado, sai sozinho quando chegar a hora
  publicados/   já foi, com o link do post no post.json
```

### Reels (o formato principal)

- `video.mp4` o vídeo da final (1080×1920, H.264)
- `capa.jpg` opcional, a capa que aparece no perfil
- `legenda.txt` a legenda
- `post.json` com a data e o fuso: `{"titulo": "Final da Copa 2022", "publicar_em": "2026-12-18T19:00:00-03:00"}`

Uma pasta com `.mp4` vira reels sozinha. Também dá pra marcar `"tipo": "reels"`.

Campos opcionais no `post.json` do reels:

- `"marcar": ["flamengo", "corinthians"]` marca as contas no vídeo (até 20). Não depende de aceite.
- `"colaboradores": ["conta"]` convida pra collab (até 3). Só aparece no perfil da outra conta se ela aceitar; use só com quem combinou antes.

Se a Meta recusar algum @ (conta inexistente, por exemplo), o reels sai sem marcação e o aviso fica no log da execução.

### Feed e story

Igual ao publicador original: `01.jpg`, `02.jpg`… (1 a 10, 1080×1350) com `legenda.txt` viram post ou carrossel; `"tipo": "story"` publica cada imagem como story (1080×1920, sem legenda).

**Aprovar um post é mover a pasta de `rascunhos` pra `agendados`.** O GitHub roda a cada 30 minutos, publica o que estiver vencido e move a pasta pra `publicados`.

## Rodar na mão

Em **Actions › Publicar › Run workflow**:

- `verificar` confere token, conta e cota sem publicar nada
- `publicar` com uma pasta preenchida publica aquela pasta na hora, ignorando a data

## Segredos

Em **Settings › Secrets and variables › Actions**:

- `IG_ACCESS_TOKEN` token da Meta do @quemlevou. Hoje é um token de usuário do sistema (começa com `EAA`), gerado no Login do Facebook para Empresas, e não vence. O script também aceita token do login do Instagram (`IGAA`), que vence em uns 60 dias.
- `IG_USER_ID` ID da conta @quemlevou

## Atenção

O repositório é público porque a Meta busca os vídeos e imagens pela URL. Quem fuçar vê o que tá em `agendados` antes da hora.

## Token

O `token.json` guarda só a data e o tipo do token (nunca o token). O atual é de usuário do sistema e não vence; se um dia trocar por token do Instagram, atualiza `gerado_em` e cria um vigia de renovação.
