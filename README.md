# Clínica Florence · Dra. Joanna Loppes — Landing Page

Landing page estática (HTML + CSS, sem framework) para captação de leads da
**Clínica Florence · Dra. Joanna Loppes — Harmonização Corporal Avançada** (Petrolina-PE).

---

## 📁 Estrutura de arquivos

```
Joana/
├── index.html              → página principal (hero, antes/depois, formulário, sobre, FAQ)
├── obrigado.html           → página de agradecimento (pós-envio do formulário)
├── styles.css              → todos os estilos (tokens de tema, layout, responsivo)
├── .htaccess               → regras de URL (remove .html, serve em subpasta)
├── assets/                 → imagens herdadas (NÃO usadas — substituídas por placeholders)
├── meta-capi/              → Pixel + CAPI em PHP puro (versão p/ a landing estática)
│   ├── capi.php
│   └── capi.js
├── meta-pixel-geovana/     → mesma integração, mas como plugin WordPress (herdado)
│   ├── meta-pixel-geovana.php
│   └── capi.js
└── meta-pixel-geovana.zip  → o plugin WordPress compactado, pronto p/ instalar
```

> ⚠️ Há **duas implementações** do Pixel/CAPI na pasta (`meta-capi` em PHP puro e
> `meta-pixel-geovana` como plugin WordPress). Use **uma** conforme onde a página
> roda — ver seção Rastreamento e Pendências.

---

## 🖼️ Imagens (placeholders)

As fotos do projeto anterior **não são exibidas**. Todas as imagens do site (3 cards
de antes/depois, foto da doutora e fundo do hero) usam **placeholders em gradiente
roxo** com o rótulo "Foto em breve".

- Os arquivos `.png` herdados continuam na pasta `assets/`, mas **não são referenciados**
  por nenhum HTML/CSS.
- Quando as fotos da Dra. Joanna chegarem, basta inserir as novas imagens e reconectar
  os trechos (cards em `index.html` e o `.hero-bg-img` em `styles.css`).

---

## 🚀 Como o site é publicado (IMPORTANTE)

O site **não fica na raiz do domínio**. Ele é enviado para uma **subpasta** dentro
de `public_html`, ao lado de um **WordPress que ocupa a raiz** (mesmo modelo dos
outros projetos).

```
public_html/
├── (WordPress: wp-admin, wp-content, wp-includes, ...)  ← raiz do domínio
└── subpasta/        ← AQUI vão os arquivos desta landing
    ├── index.html
    ├── obrigado.html
    ├── styles.css
    ├── .htaccess
    ├── assets/
    └── meta-capi/   (se usar o Pixel via PHP puro)
```

Portanto a landing abre em `dominio.com.br/subpasta/` e a página de obrigado em
`dominio.com.br/subpasta/obrigado`. **Testar `/obrigado` na raiz dá 404** — lá é o
WordPress, não esta landing.

> Ao subir arquivos, envie **todos** (incl. `.htaccess`, que é oculto — ligue
> "mostrar arquivos ocultos" no Gerenciador de Arquivos/FTP).

---

## 🔗 .htaccess · URLs limpas

O `.htaccess` faz, com caminhos **relativos** (funciona em qualquer subpasta e nunca
redireciona pra raiz / WordPress):

1. Remove a extensão: `/subpasta/obrigado.html` → `/subpasta/obrigado`
2. Serve internamente o arquivo: `/subpasta/obrigado` entrega `obrigado.html`

Há também um bloco de **forçar HTTPS comentado** — só descomente **depois** que o
SSL estiver instalado no painel da hospedagem (senão o site fica inacessível).

---

## 📨 Formulário → Make → Planilha

O formulário (`#contactForm` no `index.html`) envia por `fetch` (POST JSON) para um
**webhook do Make**:

```
https://hook.us2.make.com/cjxjnd5wr41srcaf3bjtu8smsohiwym8
```

Campos enviados no payload:

| Campo          | Origem                             |
|----------------|------------------------------------|
| `nome`         | input `#form-name`                 |
| `telefone`     | input `#form-whatsapp`             |
| `email`        | input `#form-email`                |
| `procedimento` | select `#form-procedure`           |
| `origem`       | URL da página                      |
| `data`         | data do envio (ex: `12 jun. 2026`) |
| `hora`         | hora do envio (ex: `09h30`)        |

Opções do select de protocolo: **Harmonização Corporal (Combo Completo) ·
Criolipólise / Criomodelagem · PEIM — Vasinhos · Redução de Medidas / Gordura
Localizada · Não sei ainda, quero avaliação**.

O mapeamento desses campos para as colunas da planilha (Google Sheets) é feito
**dentro do cenário do Make**, não no código.

Após o envio bem-sucedido, o usuário é redirecionado para **`obrigado`** (URL limpa).

---

## 🙏 Página de obrigado

`obrigado.html` mostra a confirmação e um botão **"Falar pelo WhatsApp agora"**
(`wa.me/...`) com mensagem pré-preenchida, além de um botão "Voltar para o site".

---

## 🎨 Tema e identidade visual

- Tema **claro por padrão** (`<html data-theme="light">`).
- A preferência do usuário é salva em `localStorage` na chave `joanna-theme`.
- Botão de alternância (sol/lua) no menu — o tema claro continua disponível.
- **Paleta roxa/violeta** em degradê (token `--gold-grad`, herdado da fonte Clínica Florence):
  - Roxo primário `#7937a3` · secundário `#c785e9` · accent `#d4a8f0`, sobre fundo escuro `#100220`.
- Fonte única: **Montserrat** (títulos de destaque e texto), como na fonte original.

---

## 📊 Rastreamento

- **Google Tag Manager:** container `GTM-KVJVVKHN` no `<head>`.
- **Meta Pixel + Conversions API:** instalado. Pixel ID `935011722874678`.
  - Dispara **PageView** e **Lead** (após o envio bem-sucedido do formulário) com
    **deduplicação por `event_id`** (navegador + CAPI), faz **hash SHA-256** do
    e-mail/telefone no navegador e tem **rate limiting** por IP.
  - O **token da CAPI fica só no servidor**, nunca no navegador.

Há **duas formas** prontas na pasta — escolha conforme onde a página é servida:

| Cenário                                  | Use                          | Como                                                                 |
|------------------------------------------|------------------------------|----------------------------------------------------------------------|
| Landing estática em subpasta              | `meta-capi/` (PHP puro)       | Subir a pasta junto da landing; o `index.html` já chama `capi.js` e o `<head>` tem o Pixel. |
| Página rodando no WordPress da raiz       | `meta-pixel-geovana.zip` (WP) | WP Admin → Plugins → Adicionar novo → Enviar plugin → Ativar.        |

> Não use as duas ao mesmo tempo na mesma página (dispararia o evento em duplicidade
> sem o mesmo `event_id`).

---

## ✅ Pendências / a confirmar

- [ ] **Marca** — confirmar se o site deve liderar por "Dra. Joanna Loppes" (logo/título
      atuais) ou "Clínica Florence" (rodapé/legal atuais). Hoje usa ambos.
- [ ] **Fotos da Dra. Joanna** — hoje o site usa placeholders. Inserir as imagens
      reais (antes/depois, foto da doutora, fundo do hero).
- [ ] **Números da seção "Sobre"** — estão como placeholders `[X]+ anos` e
      `[X mil]+ procedimentos`. Preencher com os valores reais.
- [x] **Número de WhatsApp** — `obrigado.html` usa o link de chat oficial dela
      (`api.whatsapp.com/message/TY7FPVLQATYBH1`).
- [ ] **Webhook do Make** — confirmar se o webhook é mesmo desta clínica (é o mesmo de
      outro projeto; pode ser cópia a revisar).
- [ ] **Domínio + subpasta de produção** — confirmar a URL de publicação.
- [ ] **SSL** — confirmar certificado ativo antes de descomentar o "forçar HTTPS".
- [ ] **GTM** — confirmar se o container `GTM-KVJVVKHN` é desta clínica (mesmo ID
      usado em outros projetos; pode ser cópia a revisar).
- [ ] **Token da CAPI (CRÍTICO)** — `meta-pixel-geovana/meta-pixel-geovana.php` contém
      um token em **texto puro** herdado de outro projeto. Gerar token novo no
      Gerenciador de Eventos e substituir; tratar o antigo como comprometido.
- [ ] **Escolher UMA implementação do Pixel** — `meta-capi` (PHP puro) p/ a landing
      estática, OU o plugin WordPress; remover a que não for usada para evitar confusão.
- [ ] **Renomear o plugin WordPress** — a pasta `meta-pixel-geovana/`, o `.zip` e os
      prefixos internos (`geo_*`, `GEO_*`) ainda têm o nome de outro projeto; renomear
      se for usado.

---

## 🛠️ Rodar localmente

Sirva a pasta com um servidor estático (ex.: `python -m http.server`).
Obs.: o servidor estático local **ignora o `.htaccess`**, então as URLs limpas
(`/obrigado` sem `.html`) só funcionam no servidor de produção (Apache). O endpoint
PHP (`meta-capi/capi.php`) também só roda num servidor com PHP.
