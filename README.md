# trm-social-artes 🎨

Repositório **público** que hospeda duas coisas do sistema de posts da TRM Sistemas:

1. **As artes finais aprovadas** (PNGs) — a API do Instagram exige que a
   imagem esteja numa URL pública para poder publicá-la. Como as artes vão
   para o feed público do Instagram de qualquer forma, elas não são segredo.
2. **O workflow do GitHub Actions** (`.github/workflows/pipeline.yml`) —
   o "despertador" que roda o pipeline 2x por dia. Ele roda AQUI (repo
   público = minutos ilimitados e gratuitos) mas o código dos agentes é
   baixado do repositório privado `trm-social` na hora da execução, usando
   um token guardado nos Secrets. **Nenhum código privado fica exposto.**

O cérebro do sistema (agentes de IA, prompts, configuração) vive no
repositório privado [`trm-social`](https://github.com/marcelojuste/trm-social).

## Como funciona uma execução

```
11h e 17h (horário de Brasília), todos os dias:
  1. Baixa o código privado (via PRIVATE_REPO_TOKEN)
  2. Gera um post novo — só se já passou o intervalo configurado
  3. Envia para aprovação no Telegram (aguarda até 2h; processa também
     cliques dados fora da janela, que ficam na fila do Telegram)
  4. Copia as artes aprovadas para este repositório (vira URL pública)
  5. Publica no Instagram via Graph API
  6. Salva o estado dos posts de volta no repositório privado
```

## Secrets necessários (Settings → Secrets and variables → Actions)

| Secret | O que é | Onde obter |
|---|---|---|
| `PRIVATE_REPO_TOKEN` | Token com acesso de leitura/escrita ao repo privado | docs/05-producao.md do repo privado |
| `GEMINI_API_KEY` | Chave da IA (agentes) | aistudio.google.com/apikey |
| `TELEGRAM_BOT_TOKEN` | Senha do bot de aprovação | @BotFather |
| `TELEGRAM_CHAT_ID` | Sua conversa com o bot | scripts/descobrir_chat_id.py |
| `IG_USER_ID` | ID da conta profissional do Instagram | docs/04-setup-meta-app.md do repo privado |
| `IG_ACCESS_TOKEN` | Token de longa duração da Meta | docs/04-setup-meta-app.md do repo privado |
