# Dra. Raquel Fialeki — Landing Page

Landing page de captação de leads para a **Dra. Raquel Fialeki**, especialista em **Otomodelação** e responsável pelo método autoral **Otoslim** (correção de orelhas de abano sem cirurgia).

---

## 📁 Estrutura de pastas

```
Raquel/
├── index.html          # Estrutura semântica das 5 dobras + modais de Termos/Privacidade
├── style.css            # Design system + componentes + responsividade
├── app.js                # Reveals, máscara de WhatsApp, validação e submit do form, modais
├── README.md             # Este arquivo
├── obrigado/
│   └── index.html        # Página de obrigado (redirecionada após o envio do formulário)
└── assets/
    ├── img.webp                    # Hero (foto Dra. Raquel)
    ├── dra-raquel-editorial.webp   # Foto editorial para a seção "Sobre"
    ├── antes-01..04.webp           # Antes/depois — lado A
    └── depois-01..04.webp          # Antes/depois — lado B
```

> Todas as imagens referenciadas em `index.html` apontam para a pasta `assets/`.

---

## 🎨 Design system

| Token            | Valor       | Uso                                        |
|------------------|-------------|---------------------------------------------|
| `--c-primary`    | `#EDBC8E`   | Botões, badges, destaques, gradiente quente |
| `--c-secondary`  | `#EAE5D8`   | Fundos sutis, placeholders                  |
| `--c-accent`     | `#1C1712`   | Texto principal, CTA escuro                 |
| `--c-bg`         | `#FDFAF5`   | Fundo padrão                                |
| `--c-text`       | `#1C1712`   | Texto                                       |

- **Fontes:** Playfair Display (serif, headings) + Inter (sans, corpo) — via Google Fonts.
- **Efeitos:** glassmorphism (`backdrop-filter: blur`), sombras suaves em camadas (`--shadow-sm/md/lg/glow`).
- **Favicon:** SVG inline com a inicial **R** sobre o tom primário.

---

## 🧩 Seções da LP

### 🟠 Dobra 1 — Hero
- **Imagens:** `assets/img.webp`.
- CTA primário (`#agendar`) e CTA fantasma (`#resultados`).

### 🟠 Dobra 2 — Prova Social
- **Imagens:** `assets/antes-0[1-4].webp` e `assets/depois-0[1-4].webp`.
- Grid 2×2 de pares antes/depois com legenda.

### 🟠 Dobra 3 — Formulário (captação)
- **Campos:** Nome, WhatsApp (máscara `(00) 00000-0000`), "Quem realizará o procedimento" (select).
- Validação client-side em `app.js`. Ao validar com sucesso, redireciona para `obrigado/`.
- **Sem integração de envio configurada no momento** — o formulário hoje não envia os dados para nenhum webhook/CRM/planilha. A integração (Make.com, GTM, etc.) será feita posteriormente.

### 🟠 Dobra 4 — Sobre a Mentora
- **Imagens:** `assets/dra-raquel-editorial.webp`.
- Copy de autoridade + stats + CTA escuro.

### 🟠 Dobra 5 — Rodapé
- Endereço da clínica.
- Barra inferior: copyright + CRM, e links de Termos de Uso / Política de Privacidade (abrem em modal) / Desenvolvido por AZX Performance — dispostos lado a lado em telas desktop e empilhados em telas pequenas.

---

## 📜 Termos de Uso & Política de Privacidade

Acessíveis via modal a partir dos links no rodapé (`#openTerms` / `#openPrivacy`). A Política de Privacidade é redigida com base na **LGPD (Lei nº 13.709/2018)**, cobrindo: dados coletados, finalidade do tratamento, base legal, compartilhamento, armazenamento/segurança e direitos do titular.

---

## 🔌 Integrações

Google Tag Manager instalado (container `GTM-MJQJPGBC`), snippet no topo do `<head>` e o `<noscript>` logo após a abertura do `<body>` em `index.html`. Nenhum Pixel/Conversions API está instalado — caso necessário, o ponto de entrada é o submit handler em `app.js` (`form.addEventListener('submit', ...)`), logo após a validação client-side e antes do redirect para `obrigado/`.

---

## 📱 Responsividade

| Faixa                | Comportamento                                          |
|-----------------------|--------------------------------------------------------|
| **≥ 1024px**          | Layouts em 2 colunas (hero, form, about); grid 2×2 nas provas |
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
- Validação client-side do formulário em `app.js`; validação server-side deverá ser adicionada junto da integração de envio.

---

## 🤝 Como contribuir

- **Adicionar uma nova seção:** crie o markup dentro de `index.html` entre duas dobras existentes, atribua um `data-screen-label="NN Nome"` e adicione as classes ao final de `style.css` seguindo o padrão `.nome-secao__elemento`.
- **Mudar a paleta:** edite as custom properties no topo de `style.css` (`:root`).
- **Mudar copy:** todas as strings estão diretamente em `index.html` — sem CMS / template engine.

---

© 2026 Dra. Raquel — Todos os direitos reservados.
