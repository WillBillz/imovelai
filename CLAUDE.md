# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Visão Geral

**ImóvelAI** (marca no app: **HomeAI**) é uma plataforma imobiliária com busca assistida por IA, tour virtual 360°, painel de corretor e assistente de IA ("Aria"). É um dos produtos do portfólio **Lumina.IA** (ver `C:\Users\Scan\Lumina\CLAUDE.md`). O próprio README descreve o projeto como peça de portfólio internacional do autor, não necessariamente um SaaS com cliente pagante ativo.

**Não há build, bundler, framework nem `package.json`.** É um único arquivo `index.html` (~2600 linhas, HTML+CSS+JS inline) servido como estático, mais uma Netlify Function isolada que faz proxy para a API da Anthropic. Tudo roda no navegador; a única peça de "backend" é essa function.

## Comandos

Não há `npm install`/`build`/`lint`/`test` — é HTML estático.

- **Dev local**: abrir `index.html` direto no navegador funciona para UI, mas as chamadas a `/.netlify/functions/claude` falham (404) sem o Netlify Dev rodando. Use `netlify dev` (Netlify CLI) na raiz do repo para servir o site **e** a function localmente.
- **Deploy**: push na branch `main` → Netlify faz build/deploy automático (`netlify.toml`: `publish = "."`, `functions = "netlify/functions"`). Live: `imovelai.netlify.app`.

### Variáveis de ambiente (somente na Netlify Function)

```
ANTHROPIC_API_KEY=...
```

Configuradas no painel da Netlify, não em `.env` local (não há `.env.example` no repo).

## Arquitetura

### Tudo em `index.html`

O arquivo é dividido por comentários `// ============ NOME DA SEÇÃO ============`. Seções principais do script:
- **Config Supabase** — `SUPABASE_URL`/`SUPABASE_KEY` (anon/publishable key) **hardcoded direto no HTML**, não via env var. Isso é intencional/aceitável para uma anon key do Supabase (protegida por RLS no banco), mas significa que trocar de projeto Supabase exige editar o HTML, não um `.env`.
- **Auth Google** — login via Supabase + Google OAuth.
- **Tour Virtual** — integração com Kuula (360°), por iframe/embed.
- **Dados dos Imóveis** — listagens inline no script (não vêm do Supabase neste ponto do código).
- **Painel do Corretor** — CRUD de imóveis, com 4 sub-blocos:
  1. Agenda — agendamento de visitas
  2. Simulador de Financiamento (tabela de amortização)
  3. Score de Lead por IA
  4. Geração de Contrato por IA
- **Formulário — Anunciar Imóvel**
- **Painel — Proprietários + Mapa** (Leaflet + OpenStreetMap)

### IA: um único proxy compartilhado

Toda chamada de IA — chat da Aria, busca em linguagem natural, e as features do painel do corretor (score de lead, geração de contrato) — passa pela mesma função `chamarIA(mensagem, tipo)` (por volta da linha 1759), que faz `fetch('/.netlify/functions/claude', ...)`. O parâmetro `tipo` (`'chat'` | `'busca'` | `'corretor'`) seleciona o *system prompt* certo no próprio `index.html` antes de enviar à function — `netlify/functions/claude.js` é um proxy puro (não tem lógica de prompt), só repassa `body` para `api.anthropic.com/v1/messages` injetando a `ANTHROPIC_API_KEY` no header. Ao adicionar uma nova feature de IA, reaproveitar `chamarIA()` com um novo `tipo` em vez de criar outro caminho de fetch.

### Branches: duas versões de idioma

| Branch | Conteúdo |
|---|---|
| `main` | Versão em **inglês** — portfólio internacional |
| `pt-BR` | Versão em **português** — cliente brasileiro original |

As duas branches divergem em conteúdo de UI (textos, "Aria" etc.), não só em config. Ao corrigir bugs de lógica (não de texto), considerar se o fix precisa ser replicado manualmente na outra branch — não há merge automático entre elas.

## Convenções

- Sem framework: novas features seguem o padrão de funções JS puras + manipulação de DOM/template strings já presente no arquivo, não introduzir React/Vue etc. sem alinhar antes — mudaria a arquitetura inteira do projeto.
- Segredos (chave Anthropic) só existem como env var da Netlify Function; nunca colocar API keys de serviços pagos (Anthropic, etc.) diretamente no `index.html` como foi feito com a anon key do Supabase — aquela é segura por ser uma chave pública por design, a da Anthropic não é.
