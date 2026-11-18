# Instagram da Novy Tech · publicação

Repositório privado com os posts prontos do @novytech.dev. Uma pasta por post; a Mary (bot do
Instagram) lê daqui e publica no horário. As artes e as legendas são feitas no projeto Novy
Prospeccao, com o Claude, e aprovadas pelo Lucas antes de ficarem prontas.

## Quem faz o quê

| Quem | O quê |
|---|---|
| Lucas e Claude | Criam a pasta do post (artes, `legenda.txt`, `meta.json`) com `status.txt` = `rascunho`. Depois do ok do Lucas nas artes, `status.txt` vira `pronto` |
| Mary | Publica as pastas `pronto` no horário, marca `publicado` e grava `publicado.json`. Nos posts das 09h, pode trocar o conteúdo pela pauta do pesquisador X (ver abaixo) |

## Pastas

```
agenda/                     a agenda de cada semana (o que vai ao ar e quando)
posts/AAAA-MM-DD-HHMM/      um post: slide-01.png ... slide-N.png, legenda.txt, meta.json, status.txt
```

- Nome da pasta: data e horário de Brasília em 24h, com zero à esquerda: `posts/2026-11-19-1230/`.
- `slide-01.png` a `slide-N.png`: PNG 1080×1350 (4:5), nesta ordem. Imagem única é só `slide-01.png`.
- `legenda.txt`: só a legenda, pronta para colar, com as 5 hashtags no fim.
- `meta.json`: `data`, `horario`, `timezone`, `formato` (`carrossel` ou `imagem_unica`), `slides`,
  `slug`, `titulo`, `origem` (`agenda`, `reserva` ou `pesquisador-x`) e `alt` (um texto
  alternativo por slide, na ordem).
- `status.txt`, primeira linha:
  - `rascunho`: em produção ou esperando o ok do Lucas. **Não publicar.**
  - `pronto`: pode publicar no horário da pasta.
  - `publicado`: já foi ao ar. Não publicar de novo.
  - qualquer outro valor, arquivo faltando ou pasta incompleta: não está pronto.

## Rotina da Mary

1. Antes de publicar: `git pull`.
2. Pastas de hoje com `status.txt` = `pronto`: no horário da pasta, publique
   - as imagens `slide-01.png` a `slide-N.png`, nessa ordem, no formato original 4:5 (sem corte, sem filtro);
   - o texto de `legenda.txt` exatamente como está;
   - o texto alternativo: `alt[0]` no slide 1, `alt[1]` no slide 2, e assim por diante.
3. Depois de publicar: `status.txt` = `publicado` e um `publicado.json` na mesma pasta:

   ```json
   {
     "status": "publicado",
     "publicado_em": "2026-11-19T12:31:00-03:00",
     "permalink": "https://www.instagram.com/p/XXXXXXXXXXX/",
     "ig_media_id": ""
   }
   ```

   Commit com a mensagem `Publicado: posts/AAAA-MM-DD-HHMM` e `git push`. Se o push for recusado,
   `git pull --rebase` e `git push` de novo.
4. Avise o Lucas: publicado (com o link) ou o problema.

**Posts das 09h.** A pasta nasce com o tema reserva da agenda, já `pronto` depois do ok do Lucas.
Se a pauta do pesquisador X chegar a tempo, a Mary troca os arquivos **da mesma pasta** (mesmos
nomes e formato, `origem` = `pesquisador-x`), faz commit e push antes do horário. Não criar outra
pasta no mesmo horário.

## Regras

- Publique só pastas `pronto`. Não reescreva a legenda (nem para tirar ou pôr hashtag ou emoji), não
  acrescente preço nem marcação de perfil e não gere arte para posts de outros horários.
- Imagem que falta ou não abre, ou pasta incompleta: não publique e avise o Lucas.
- Post de um horário que já passou e não foi publicado: não publique sozinha, pergunte ao Lucas.
- Mexa só em `status.txt` e `publicado.json` das pastas publicadas (e na pasta das 09h, quando
  trocar pela pauta do pesquisador X). O resto do repositório é do Lucas e do Claude.

## Pautas novas

Para sugerir posts fora da agenda, abra uma issue com o modelo **Pauta de post do Instagram**
(rótulo `pauta-instagram`), uma por post. A agenda da semana vai em `agenda/`.
