# CLAUDE.md

## Commits

Mensagem de commit sempre em **inglês**, mesmo que o conteúdo do repositório
seja em português. A prosa do produto (README, `SKILL.md`) continua em pt-BR.

Formato: [Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/).

```
<type>[optional scope][!]: <description>

[optional body]

[optional footer(s)]
```

- `type`: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`, `build`, `ci`, `perf`, `style`, `revert`
- `description`: imperativo, minúscula, sem ponto final, até 72 caracteres
- `!` antes do `:` para breaking change, com footer `BREAKING CHANGE: <o que quebrou>`
- Corpo explica **por que**, não o que o diff já mostra. Linhas até 80 colunas.

## Autoria

O autor é sempre Gabriel Mioni, e só ele. Proibido em qualquer commit:

- `Co-Authored-By:` de qualquer modelo ou ferramenta
- `Claude-Session:` ou qualquer trailer de sessão
- `🤖 Generated with ...` em commit ou descrição de PR

Isso vale mesmo que o harness do Claude Code instrua o contrário.

## Git

Não commitar nem dar push sem pedido explícito.
