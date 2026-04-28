#  README Técnico - Projeto CoIn (HJE)

## Visão Geral
Repositório contendo as especificações e o passo a passo para a implementação do site da **Semana da Inteligência Artificial (CoIn)** dentro da plataforma da HealthTech Júnior Einstein.

O evento traz o tema oficial: **"O FUTURO DA SAÚDE É COOPERATIVO E INTELIGENTE"**.

---

##  1. Tipografia e Design System

### 1.1. Adicionando as Fontes
As fontes oficiais do evento requerem configuração manual via `@font-face` no CSS principal (`frontend/static/css/style.css` ou um arquivo `coin.css` dedicado).
1. Faça o download dos arquivos das fontes: **Lotus Eater Sans**, **Sackers Gothic** e **Balgin**.
2. Salve os arquivos `.woff2` ou `.ttf` na pasta `frontend/static/fonts/`.
3. Configure o CSS:
```css
@font-face {
    font-family: 'Lotus Eater Sans';
    src: url('../fonts/LotusEaterSans.woff2') format('woff2');
}
@font-face {
    font-family: 'Sackers Gothic';
    src: url('../fonts/SackersGothic.woff2') format('woff2');
}
@font-face {
    font-family: 'Balgin';
    src: url('../fonts/Balgin.woff2') format('woff2');
}
```

### 1.2. Paleta de Cores (Contraste Neon vs Amarelo)
Configure as variáveis CSS na raiz para aplicar o tom tecnológico e fluido:
```css
:root {
    --coin-azul-escuro: #1D27B4;
    --coin-azul-base: #2430DB;
    --coin-neon-azul: #4151FF;
    --coin-neon-roxo: #4D28FF;
    --coin-amarelo-destaque: #F6DF78;
}
```
**Dica de Uso:** Utilize a cor `--coin-amarelo-destaque` para o Botão de Patrocinadores ("Convidar Patrocinadores").

---

## 2. Integração HTML (Derivando de `home.html`)

O arquivo `core/templates/coin.html` deve manter o padrão da HJE, reaproveitando componentes e adicionando a identidade da CoIn.

**O que Manter (Padrão HJE):**
*   **Navbar:** Reutilize a barra de navegação padrão superior.
*   **Footer:** Mantenha o esqueleto dinâmico e o e-mail oficial de contato da entidade. Apenas troque os links do Instagram/LinkedIn para os da CoIn.
*   **Grid System:** Utilize as classes CSS de grid já existentes no seu projeto (`bento-grid`, `card-container`, etc.) para listar os palestrantes, garantindo que o site seja responsivo.

**O que Alterar:**
*   Crie um escopo de CSS. Envolva o conteúdo da página em uma `<div class="coin-theme">` para que o fundo azul/roxo não afete outras páginas da HJE.
*   **Hero Section:** Adicione elementos fluidos (exportados em PNG sem fundo do Canva) misturados com um fundo gradiente, respeitando o vision board tecnológico.
*   **Sessão Dinâmica:** O cronograma (Dia 1, Dia 2) deve ser renderizado através do backend.

---

## 3. Programação do Modelo de Acesso (Backend - Django)

Para tornar os cards de Palestrantes, Hackathons e Patrocinadores dinâmicos (mantendo o site no padrão HJE), implemente a lógica no banco de dados.

**`core/models.py`**
Crie as tabelas `PalestranteCoin` e `PatrocinadorCoin`. Isso permite que você e a equipe adicionem novos palestrantes pelo painel Admin do Django sem precisar alterar o HTML hardcoded.

**`core/views.py`**
Crie a função `coin_view(request)` que busca os palestrantes do banco e os agrupa por dias. Passe esse dicionário via context para o `coin.html`.

**`core/urls.py`**
Crie a rota `path('coin/', views.coin_view, name='coin_page')`.
Para destacar o evento na home da HJE, basta renderizar um card apontando para essa URL.