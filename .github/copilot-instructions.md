# BiblioConnect - Instruções para Agentes de IA

## Visão Geral do Projeto

BiblioConnect é uma aplicação React de rastreamento pessoal de leitura. O objetivo é organizar uma biblioteca de livros catalogando-os por trilhas temáticas, com recursos de busca, filtragem e estatísticas de progresso.

**Estrutura monolítica**: Toda a lógica está em `app.js` (335 linhas). Sem divisão em componentes reutilizáveis separados ainda.

## Arquitetura e Padrões de Dados

### Base de Dados
- **`ALL_BOOKS`**: Array consolidado com 12 livros hardcoded
- Cada livro tem: `id`, `titulo`, `autor`, `paginas`, `genero`, `tipo`, `trilha`, `capa`, `sinopse`, `destaque`
- As **trilhas** (categorias temáticas) são: "Mente Humana", "Leveza", "Propósito", "Sociedade", "Literatura", "Futuro", "Produtividade"

### Fluxo de Estado (React Hooks)
```javascript
activeTab     // qual aba está aberta: 'Home', 'Estante', 'Stats'
searchTerm    // texto de busca em tempo real
selectedBook  // livro clicado (abre modal com detalhes)
filter        // trilha selecionada para filtragem
```

### Computações Importantes
- **`filteredBooks` (useMemo)**: Filtra por busca + trilha. Sempre verificar lógica aqui antes de adicionar novos filtros
- **`trilhas`**: Lista dinâmica extraída de todos os livros

## Componentes e Seções da UI

### Navegação Estrutura (3 abas)
1. **Home**: Boas-vindas + Especial Dezembro + Frase do dia
2. **Estante**: Grid de livros (2 colunas) com busca e filtros
3. **Stats**: Círculo grande com total de páginas + cards de progresso + gráfico por trilha

### Padrão de Cards
- **BookCard**: Miniatura com capa colorida (usa classes `bg-{cor}-{intensidade}`) + título truncado + trilha + página
- **Capa (book.capa)**: Classes Tailwind pré-definidas. Exemplo: `bg-stone-800 text-white`

### Modal de Detalhes (Modal Bottom Sheet)
- Aberto quando `selectedBook` é setado
- Exibe: título + autor + grid 3-col (páginas/leitura/trilha) + sinopse + textarea "Diário de Insight" + botão de conversa

## Convenções de Código

### Styling
- **Tailwind CSS exclusivamente**: Sem CSS-in-JS externo (apenas `<style>` para fontes + reset)
- **Cores primárias**: Indigo (indigo-600, indigo-950, indigo-50)
- **Rounded-{32px}**: Padrão para cards principais
- **Breakpoint mobile**: max-w-md simulado + `@media (max-width: 640px)`

### Tipografia
- **Serif**: "Playfair Display" para h1, h2, h3, títulos de livros
- **Sans**: "Inter" para corpo do texto
- **Weights**: 900 para headlines, 600-700 para labels, 400-500 para corpo

### Animações
- `animate-in fade-in slide-in-from-bottom duration-500` para home
- `active:scale-95` e `active:scale-[0.98]` para feedback de clique
- `transition-all` para estados de botão (hover/focus)

### Ícones
- Usa **lucide-react**: `import { Icon1, Icon2 } from 'lucide-react'`
- Sempre verificar tamanho (`size={18}`), cor (`className="text-indigo-600"`) e strokeWidth

## Workflows de Desenvolvimento

### Adicionar um Novo Livro
1. Inserir objeto em `ALL_BOOKS` (copiar estrutura existente)
2. Usar `capa` com classe Tailwind escolhida
3. Não esquecer de definir `destaque` se é para "Especial Dezembro"

### Adicionar Novo Filtro
1. Modificar `filteredBooks` em `useMemo` (linha ~80)
2. Se for novo campo no livro, adicionar em `ALL_BOOKS`
3. Atualizar UI de filtros se necessário

### Modificar Estatísticas
- Stats são **mockadas** atualmente (valores hardcoded)
- Mudar valores em objeto `stats` (~80 linhas)
- Progresso por trilha tem hardcoded 45% para todas (linha ~280) - ver TODO

## Pontos de Extensão Futura

- **Persistência**: Trocar `ALL_BOOKS` por localStorage ou API backend
- **Modal Estendido**: O textarea "Diário de Insight" está pronto para integração com salvar notas
- **Stats Dinâmicas**: Calcular `lidosNoMes` e `meta2026` a partir de dados
- **Componentes**: Extrair `BookCard`, `BookModal`, `BottomNav` para arquivos separados
- **Chat Integration**: Botão "vamos conversar" pode integrar com chatbot (linha ~318)

## Dependências e Assets

- **React**: useState, useMemo
- **Lucide-react**: 18+ ícones importados
- **Tailwind CSS**: Config assume preset completo
- **Fontes Google**: Playfair Display + Inter (carregadas via `<style>`)

## Padrões NÃO Usar Neste Projeto

- ❌ CSS Modules ou styled-components (Tailwind only)
- ❌ Redux/Context (estado local é suficiente hoje)
- ❌ Componentes de classe (tudo é funcional)
- ❌ Type annotations TypeScript (ainda é JS vanilla)

---

**Última atualização**: Dezembro 2025  
**Autor do app**: Enzohenrique7
