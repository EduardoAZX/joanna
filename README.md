# Dra. Joanna Loppes — Landing Page

Landing page de captação de leads para a **Dra. Joanna Loppes**, referência em **Harmonização Corporal Avançada** em Petrolina (Clínica Florence), com protocolos exclusivos de Criolipólise e Criomodelagem — sem cirurgia e sem pós-operatório.

---

## 📁 Estrutura de pastas

```
Joanna/
├── index.html          # Estrutura semântica das 5 dobras + modais de Termos/Privacidade
├── style.css            # Design system + componentes + responsividade
├── app.js                # Reveals, máscara de WhatsApp, validação e submit do form, modais
├── obrigado.html          # Página de obrigado (redirecionada após o envio do formulário)
├── README.md              # Este arquivo
└── assets/
    ├── hero-bg.webp                # Hero (foto da Dra. Joanna na clínica)
    ├── dra-joana.webp              # Foto editorial para a seção "Sobre"
    ├── antes-01..03.webp           # Antes/depois — lado A (por protocolo)
    ├── depois-01..03.webp          # Antes/depois — lado B (por protocolo)
    ├── favicon.jpg                 # Ícone da aba (logo lótus)
    └── antes e depois*.jpg         # Fotos originais enviadas (backup, não usadas diretamente na página)
```

> Todas as imagens referenciadas em `index.html` apontam para a pasta `assets/`.

---

## 🎨 Design system

Paleta mesclando o roxo escuro e o roxo claro (sem seletor de tema — um único visual):

| Token              | Valor       | Uso                                                   |
|--------------------|-------------|--------------------------------------------------------|
| `--c-primary`      | `#c785e9`   | Destaques, gradientes de CTA (ponta clara)             |
| `--c-primary-2`    | `#9b52d4`   | Gradientes de CTA (ponta média)                        |
| `--c-primary-deep` | `#7937a3`   | Títulos em destaque, ícones e textos sobre fundo claro  |
| `--c-secondary`    | `#f3e9fb`   | Fundos sutis, placeholders                             |
| `--c-accent`       | `#190733`   | Roxo bem escuro — seções de contraste (form/rodapé)    |
| `--c-bg`           | `#faf6fd`   | Fundo padrão (lavanda claro)                           |
| `--c-text`         | `#2e1245`   | Texto principal                                        |
| `--c-muted`        | `#7a6890`   | Texto secundário                                       |

- **Fonte:** Montserrat (única família, headings e corpo) — via Google Fonts.
- **Efeitos:** glassmorphism (`backdrop-filter: blur`), sombras suaves em camadas (`--shadow-sm/md/lg/glow`), molduras de foto com contorno roxo translúcido suave.
- **Favicon:** `assets/favicon.jpg` (logo em formato de lótus).

---

## 🧩 Seções da LP

### 🟣 Dobra 1 — Hero
- **Imagem:** `assets/hero-bg.webp`.
- Título com palavra de destaque em cor sólida (sem gradiente).
- CTA único: "Quero minha avaliação" (`#agendar`).
- Marquee com diferenciais (Sem cirurgia, Criolipólise, Criomodelagem, Harmonização Corporal...).

### 🟣 Dobra 2 — Prova Social (Antes & Depois)
- **Imagens:** `assets/antes-0[1-3].webp` e `assets/depois-0[1-3].webp`.
- 3 cards de protocolo, cada um com foto antes/depois (recortada da mesma imagem original: metade de cima = antes, metade de baixo = depois) + legenda:
  - **Combo Definição Premium** — hidrolipoclasias, radiofrequência, criolipólise e massagens modeladoras.
  - **Crio Modelagem** — criolipólise, ozonioterapia e enzima redutora.
  - **Redução Intensiva** — criolipólise, lipo enzimática e ultrassom de alta potência.
- Selo de autoridade ("+1.000 procedimentos realizados") com CTA.

### 🟣 Dobra 3 — Formulário (captação)
- **Campos:** Nome completo, WhatsApp (máscara `(00) 00000-0000`), E-mail, Protocolo de interesse (select: Harmonização Corporal, Criolipólise/Criomodelagem, PEIM, Redução de Medidas, Não sei ainda).
- Validação client-side em `app.js` (nome, whatsapp, e-mail e protocolo obrigatórios).
- **Envio:** ao validar com sucesso, os dados são enviados via `fetch` para o webhook do Make (`MAKE_WEBHOOK_URL` em `app.js`) e o usuário é redirecionado para `obrigado.html`.

### 🟣 Dobra 4 — Sobre a Dra. Joanna
- **Imagem:** `assets/dra-joana.webp`.
- Copy de autoridade + stats (+1.000 procedimentos, +5 anos, 100% foco em resultado) + CTA escuro.

### 🟣 Dobra 5 — Rodapé
- Endereço: Clínica Florence, R. Antônio Santana Filho, 755 - Centro, Petrolina - PE, 56302-300.
- Barra inferior: copyright + links de Termos de Uso / Política de Privacidade (abrem em modal) / Desenvolvido por AZX Performance.

---

## 📜 Termos de Uso & Política de Privacidade

Acessíveis via modal a partir dos links no rodapé (`#openTerms` / `#openPrivacy`). A Política de Privacidade é redigida com base na **LGPD (Lei nº 13.709/2018)**, cobrindo: dados coletados (nome, WhatsApp, e-mail e procedimento de interesse), finalidade do tratamento, base legal, compartilhamento, armazenamento/segurança e direitos do titular.

---

## 🔌 Integrações

- **Google Tag Manager:** container `GTM-MJQJPGBC` (snippet no `<head>` + `<noscript>` no `<body>` de `index.html` e `obrigado.html`). ⚠️ Este container ainda é o original do template — trocar pelo GTM da Dra. Joanna antes de publicar.
- **Envio de leads:** webhook do Make.com configurado em `app.js` (`MAKE_WEBHOOK_URL`). ⚠️ Aponta para o webhook original do template — trocar pelo webhook da Dra. Joanna.
- **WhatsApp (página de obrigado):** número configurado em `obrigado.html`. ⚠️ Ainda é o número original do template — trocar pelo WhatsApp da Clínica Florence.
- Nenhum Pixel/Conversions API está instalado.

---

## 📱 Responsividade

| Faixa                | Comportamento                                          |
|-----------------------|--------------------------------------------------------|
| **≥ 1024px**          | Layouts em 2 colunas (hero, form, about); grid 2 colunas nas provas |
| **768–1023px**        | Mesmas grids mantidas, com `clamp()` reduzindo gaps     |
| **≤ 880px**           | Hero, form e about colapsam para 1 coluna               |
| **≤ 760px**           | Grid de provas vira 1 coluna                            |
| **≤ 520px**           | Ajustes finos de tipografia e botões em largura total   |
| **≤ 480px**           | Barra inferior do rodapé empilha em coluna              |

---

## 🔒 Segurança

- Sem `innerHTML`/`eval`/`document.write` no client-side (sem vetor de XSS via DOM).
- Links externos (`target="_blank"`) sempre com `rel="noopener noreferrer"`.
- Sem conteúdo via `http://` (mixed content).
- Validação client-side do formulário em `app.js`; validação server-side deve ser garantida no webhook/CRM que recebe os dados.

---

## 🤝 Como contribuir

- **Adicionar uma nova seção:** crie o markup dentro de `index.html` entre duas dobras existentes, atribua um `data-screen-label="NN Nome"` e adicione as classes ao final de `style.css` seguindo o padrão `.nome-secao__elemento`.
- **Mudar a paleta:** edite as custom properties no topo de `style.css` (`:root`).
- **Mudar copy:** todas as strings estão diretamente em `index.html` — sem CMS / template engine.
- **Trocar fotos de antes/depois:** substitua os arquivos `assets/antes-0X.webp` / `depois-0X.webp` mantendo o mesmo nome, ou recorte uma imagem combinada (antes em cima, depois embaixo) ao meio.

---

© 2026 Clínica Florence — Todos os direitos reservados.
