# claudish-to-brazilian-portuguese

Um plugin de Claude Code que faz o Claude **escrever em português do Brasil
direto e curto**, sem os tiques do "claudish": travessão como muleta, frase de
quarenta palavras, hedge, preâmbulo, jargão traduzido do inglês, adjetivo no
lugar de número.

Não é tradução. É estilo. O Claude já escreve português quando você pede; o que
ele não faz sozinho é escrever **como um dev brasileiro escreve**.

Sem LLM local, sem ollama, sem API extra, sem dependência. O plugin é um arquivo
de instrução e um hook que injeta ele no início de cada sessão.

> Fork de [gvzdv/claudish-to-english](https://github.com/gvzdv/claudish-to-english),
> de Mike Gvozdev. A ideia é dele. O original reescreve a mensagem depois de
> pronta, com um LLM local; aqui a instrução vem antes, e quem escreve é o
> próprio Claude.

---

## Instalação

```shell
/plugin marketplace add gab3mioni/claudish-to-brazilian-portuguese
/plugin install claudish-to-brazilian-portuguese@gab3mioni-plugins
```

Se o resumo disser `Run /reload-plugins to activate.`, rode isso. Depois
**reinicie a sessão**: a instrução entra no `SessionStart`.

Testar antes de instalar, por uma sessão só:

```bash
claude --plugin-dir /caminho/para/claudish-to-brazilian-portuguese
```

Funcionou se a sessão abrir com `CLAUDISH → PT-BR ATIVO`.

---

## Como funciona

```
sessão abre (startup, resume, clear ou compact)
   └─► hook roda `cat ESTILO.md`
         └─► o texto entra no contexto do Claude
               └─► ele escreve assim até o fim da sessão
```

`hooks/hooks.json` registra um hook `SessionStart` que faz `cat` no `ESTILO.md`.
O stdout de um hook `SessionStart` vira contexto. É o plugin inteiro.

O matcher cobre `startup|resume|clear|compact`, então a instrução volta depois de
uma compactação de contexto — que é justamente quando o estilo escorregaria de
volta.

## O que a instrução manda cortar

Travessão como conector, antítese "não é X — é Y", tricolon, preâmbulo ("Ótima
pergunta!"), fecho recapitulando, hedge ("vale notar que"), adjetivo de esforço
("robusto", "abrangente"), nominalização, gerundismo, decalque do inglês ("no
final do dia", "em termos de"), negrito espalhado, lista onde cabia uma frase.

E o que manda fazer: uma ideia por frase, voz ativa, número e `arquivo:linha` no
lugar de adjetivo, resposta primeiro, termo técnico que ninguém traduz mantido em
inglês (commit, build, deploy, branch, log).

Vale para chat, `.md`, mensagem de commit, descrição de PR e comentário em
português. Não vale para código, identificador, nome de API, chave de config nem
saída de log.

Leia o [`ESTILO.md`](./ESTILO.md) inteiro — é curto, e é o produto. Edite ele se
seu gosto for outro; é para isso que ele é um arquivo separado.

---

## Desligar

Diga `para claudish` no chat para pausar na sessão, ou desinstale o plugin. Não
tem variável de ambiente: sem hook rodando, não sobra nada para desligar.

## Limite conhecido

A instrução entra uma vez por sessão. Em sessão muito longa o estilo pode
escorregar, como escorrega com qualquer instrução de sistema. O `ESTILO.md` tem
uma cláusula de persistência para segurar isso, e o matcher de `compact` recarrega
no ponto de maior risco. Se ainda assim escorregar no seu uso, o próximo passo é
registrar o mesmo `cat` também em `UserPromptSubmit` — recarrega a cada turno, ao
custo de repetir ~700 tokens por mensagem.

## Estrutura

```
claudish-to-brazilian-portuguese/
├── .claude-plugin/
│   ├── plugin.json         # manifesto do plugin
│   └── marketplace.json    # para o repo ser adicionado como marketplace
├── hooks/
│   └── hooks.json          # SessionStart -> cat ESTILO.md
├── ESTILO.md               # a instrução (o produto)
├── LICENSE
└── README.md
```

## Licença

MIT — veja [LICENSE](./LICENSE). Trabalho derivado de
[gvzdv/claudish-to-english](https://github.com/gvzdv/claudish-to-english).
