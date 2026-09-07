# Introdução à T.I. — Curso Gamificado

App React + TypeScript + Tailwind (Vite) com todas as melhorias pedidas:

- **Quiz após cada tópico**, com gate de progresso (só libera a próxima lição depois do quiz).
- **Simulador de peças do computador**: clique nos pontos sobre a imagem e veja o que é cada peça.
- **Drag-and-drop Hardware x Software**: arraste os itens para a categoria correta.
- **Simulador de montagem de PC**: monte o computador escolhendo as peças na ordem certa.
- **Sistema de XP e níveis** (10 níveis temáticos, barra de progresso, streak diário).
- **Conquistas** (10 conquistas com critérios diferentes, tela dedicada com desbloqueados/bloqueados).
- **Modo Laboratório Virtual**: hub para praticar as simulações livremente, fora da ordem das lições.
- **Ranking da turma**: pronto para Supabase (com fallback local caso ainda não configurado).

## Rodando localmente

```bash
npm install
npm run dev
```

Abra o endereço mostrado no terminal (geralmente `http://localhost:5173`).

## Build de produção

```bash
npm run build
npm run preview   # para testar o build antes de publicar
```

Os arquivos finais ficam em `dist/` — você pode subir essa pasta em qualquer hospedagem estática
(Vercel, Netlify, GitHub Pages, ou até substituir os arquivos do projeto Lovable).

## Ativando o Ranking da Turma (Supabase)

Sem configuração nenhuma, o app funciona 100% localmente (localStorage) e a tela de Ranking mostra
dados de exemplo. Para ativar o ranking real, compartilhado entre todos os alunos:

1. Crie um projeto gratuito em [supabase.com](https://supabase.com).
2. No **SQL Editor** do painel, rode o script `supabase/schema.sql` (cria a tabela `leaderboard`
   com as permissões básicas de leitura/escrita pública, pensado para uso em sala de aula).
3. Em **Project Settings > API**, copie a **Project URL** e a **anon public key**.
4. Copie `.env.example` para `.env` e preencha:
   ```
   VITE_SUPABASE_URL=https://SEU-PROJETO.supabase.co
   VITE_SUPABASE_ANON_KEY=SUA_CHAVE_ANON_PUBLICA
   ```
5. Reinicie `npm run dev` (ou refaça o build). O aviso amarelo na tela de Ranking desaparece e os
   dados passam a ser salvos/lidos do Supabase de verdade.

> Observação de segurança: as políticas do `schema.sql` são simplificadas (qualquer pessoa pode
> escrever), pensadas para um ambiente de sala de aula controlado. Para um uso mais robusto, vale
> adicionar autenticação de alunos (Supabase Auth) e restringir updates ao próprio registro.

## Estrutura do projeto

```
src/
  types.ts                     tipos compartilhados
  data/                        conteúdo: lições, quizzes, peças, itens, conquistas
  lib/
    xp.ts                      regras de XP e níveis
    supabaseClient.ts          cliente Supabase + funções de leaderboard
  context/
    StudentProgressContext.tsx estado global do aluno (XP, nível, conquistas, streak)
  components/
    Quiz/                      quiz ao final de cada lição
    PartsSimulator/            clique-e-identifique peças do PC
    DragDropSort/               jogo hardware x software
    PCAssembly/                 simulador de montagem
    VirtualLab/                 hub do laboratório virtual
    Achievements/                grid de conquistas
    Ranking/                     leaderboard da turma
    Layout/, Celebration/, ui/  navbar, toasts de celebração, componentes base
  pages/
    Home.tsx, LessonPage.tsx    trilha de lições e página de cada lição
```

## Editando o conteúdo

- **Lições**: edite `src/data/lessons.ts`.
- **Perguntas dos quizzes**: edite `src/data/quizzes.ts` (uma lista por `lessonId`).
- **Peças do simulador**: edite `src/data/parts.ts` (posição `x`/`y` em % sobre o diagrama).
- **Itens do jogo hardware x software**: `src/data/dragItems.ts` (`dragDropItems`).
- **Passos de montagem do PC**: `src/data/dragItems.ts` (`assemblySteps`).
- **Conquistas**: `src/data/achievements.ts` — cada uma tem uma função `criterio` que decide quando
  desbloquear, com base no progresso do aluno.
- **Regras de XP e nomes dos níveis**: `src/lib/xp.ts`.

## Próximos passos sugeridos

- Trocar o diagrama simplificado do simulador de peças (formas geométricas) por uma imagem real de
  computador com as mesmas coordenadas de clique.
- Adicionar autenticação de alunos (Supabase Auth) para o ranking não depender de digitar o nome.
- Code-splitting (o bundle atual é único, ~240 kB gzip — funciona bem, mas dá para otimizar com
  `React.lazy` por seção se o projeto crescer muito).
