# CONTROL PRO — C3 — Guia de Deploy (GitHub + Vercel + Supabase)

> Esta é uma cópia independente do Control Pro da Stevens, pra uso exclusivo da C3.
> Ela usa seu próprio banco Supabase, seu próprio repositório e seu próprio link da Vercel —
> nada aqui se mistura com os dados da Stevens.

## 1. Criar a tabela no Supabase
1. Crie um projeto **novo** em https://supabase.com/dashboard (não use o projeto da Stevens)
2. Vá em **SQL Editor** (menu lateral) → **New query**
3. Cole todo o conteúdo do arquivo `schema.sql` (junto com este guia) e clique em **RUN**
4. Confirme que a tabela `app_state` apareceu em **Table Editor**
5. Em **Project Settings → API**, copie a **Project URL** e a **anon public key**

## 2. Configurar o `index.html`
Abra o arquivo `index.html` e troque os 4 valores marcados com `>>> TROCAR AQUI <<<`:

```js
const SUPABASE_URL = 'COLE_AQUI_A_PROJECT_URL_DO_SUPABASE_DA_C3';
const SUPABASE_ANON_KEY = 'COLE_AQUI_A_ANON_PUBLIC_KEY_DO_SUPABASE_DA_C3';
```
→ Cole a Project URL e a anon key que você copiou no passo 1.

```js
const TEAM_PASSWORD = 'MUDE_ESTA_SENHA_C3';
```
→ Troque pela senha combinada com a equipe da C3.

```js
const COMPANY_HEADER_TEXT = "NOME DA C3 AQUI\nENDEREÇO DA C3 AQUI\nPHONE: +1 (000) 000-0000\nEMAIL: CONTATO@C3.COM";
```
→ Troque pelos dados reais da C3 (aparece no cabeçalho das faturas exportadas).

> ⚠️ A senha de equipe só protege a tela de entrada do app — fica visível pra quem abrir o
> código-fonte da página (normal em apps client-side simples, não é criptografia). É uma trava
> de acesso básica pro time interno, igual já é no site da Stevens.

Salve o arquivo depois de trocar os 4 valores.

## 3. Subir pro GitHub
Crie um repositório **novo e separado** do repositório da Stevens (ex: `controle-c3`):
```bash
git init
git add .
git commit -m "Control Pro C3 - versão com Supabase"
git branch -M main
git remote add origin https://github.com/SEU_USUARIO/controle-c3.git
git push -u origin main
```
(Ou arraste os 3 arquivos direto pela interface web do GitHub, criando o repositório novo.)

## 4. Deploy no Vercel
1. Entre em https://vercel.com/new
2. Selecione o repositório `controle-c3` que você acabou de criar
3. HTML puro — o Vercel detecta sozinho, não precisa configurar Build Command
4. Clique em **Deploy**
5. Você recebe um link novo, tipo `https://controle-c3.vercel.app`

Esse é o link que a equipe da C3 vai usar — separado do link da Stevens.

## 5. Testar a sincronização
1. Abra o link em duas abas/dispositivos diferentes
2. Digite a senha da equipe da C3 nas duas
3. Edite algo (ex: adicione um item no Dashboard) em uma aba
4. Em até ~1 segundo, a outra aba deve atualizar sozinha, sem precisar de F5

## O que já está sincronizado entre a equipe (tempo real):
- ✅ Dashboard de Controle (itens, status, OTs)
- ✅ Semana aberta / ciclo ativo
- ✅ Histórico de meses arquivados
- ✅ Lista de Produção & Códigos AS-BUILT
- ✅ Calculadora de Fibra (todos os "Cálculos" salvos)
- ✅ Scanner Drive (varreduras e configuração)

## O que ainda fica só no navegador de cada pessoa (não sincronizado):
- ⚠️ Aba **Automação (Mapa)** — o PDF/imagem do mapa carregado, as caixas desenhadas e a
  chave de API de IA. Imagens de mapa são pesadas e encheriam rápido o limite gratuito do
  Supabase; se quiser isso também compartilhado, dá pra migrar pro Supabase Storage depois.

## Backup
O plano gratuito do Supabase não faz backup automático. Recomenda-se exportar os dados de
vez em quando pela função de Exportar XLSX (aba Produção).
