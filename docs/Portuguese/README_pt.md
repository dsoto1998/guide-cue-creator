# Guide Cue Creator

Lê uma sessão do Ableton Live (`.als`), associa os marcadores aos arquivos de cue de áudio e gera um único arquivo WAV estéreo (`guide_cues.wav`) pronto para ser colocado em uma faixa de cue guia dedicada no tempo 0.

---

## O que está incluído

```
cue_creator.html    ← o aplicativo (abrir este arquivo)
GUIDE CUES/         ← biblioteca de cues de áudio (pré-instalada)
```

---

## Requisitos

- **Chrome ou Edge** (recomendado) — acesso à pasta sem cliques adicionais após o primeiro uso
- **Firefox** — funciona, selecionar novamente a pasta uma vez por sessão do navegador se necessário
- Conexão com a Internet (carrega pako, Fuse.js, Dexie do CDN)

---

## Início rápido

1. Clique duas vezes em `cue_creator.html` — abre no seu navegador padrão
2. Clique em **Selecionar pasta** e escolha a pasta que contém `GUIDE CUES/`
3. Selecione um **idioma** no menu suspenso
4. Arraste seu arquivo `.als` para a área de upload (ou clique para procurar)
5. Revise a tabela de cues — verde = boa correspondência, amarelo = baixa confiança, vermelho = sem correspondência
6. Clique em **Pré-visualizar** (ou pressione **Espaço**) para ouvir
7. Clique em **Renderizar WAV** — baixa `guide_cues.wav`
8. Arraste `guide_cues.wav` para sua sessão do Ableton no tempo 0 em uma faixa dedicada

No Chrome/Edge, a escolha da pasta é lembrada. O passo 2 é uma configuração única.

---

## O que o aplicativo faz

- Coloca cada cue de seção **1 compasso antes** do seu marcador
- Gera uma **contagem de entrada** para os primeiros 2 compassos (tempos 0–7 em 4/4)
- Suprime cues de seção que caem na região da contagem
- Ignora marcadores chamados `count off` ou `next song`
- Suporta automação de tempo (sessões com BPM variável)

---

## Compassos de tempo suportados

3/4 · 4/4 · 6/8 · 12/8

---

## Idiomas suportados

Inglês · Espanhol · Francês · Indonésio · Coreano · Filipino · Português

---

## Controles da tabela de cues

| Coluna | O que fazer |
|--------|-------------|
| Marcador | Somente leitura — do seu .als |
| Cue associado | Editar para substituir a associação automática |
| Confiança | Verde ≥ 80% · Amarelo ≥ 50% · Vermelho < 50% |

---

## Controles da linha do tempo

| Ação | Resultado |
|------|-----------|
| Clique | Ir para a posição |
| Espaço | Reproduzir / Pausar |
| Ctrl+rolar ou pinçar | Zoom em direção ao cursor |
| Botões +/− | Aproximar/afastar |
| Redefinir | Ajustar sessão completa à largura |
| Seguir reprodução | Ativar/desativar rolagem automática |

---

## Especificações de saída

| Configuração | Valor |
|--------------|-------|
| Taxa de amostragem | 48.000 Hz |
| Canais | Estéreo |
| Profundidade de bits | PCM de 16 bits |
| Arquivo | `guide_cues.wav` |
