# asimov-ihm — telas da IHM da bancada

Projeto do **EasyBuilder Pro** da IHM **WEG cMT1106X** (fabricada pela Weintek, tela touch de 10,1") que fica
ao lado do CLP PLC500 da bancada. É nela que o operador vê o estado da máquina e digita a **OP** (ordem de produção).

| Repositório | O que tem |
|---|---|
| [asimov-clp](https://github.com/aquario-team/asimov-clp) | programa do CLP PLC500 (CODESYS) |
| **asimov-ihm** (este) | telas da IHM cMT1106X (EasyBuilder Pro) |
| [asimov](https://github.com/aquario-team/asimov) | coletor que lê o CLP pela rede (privado) |
| [asimov-central](https://github.com/aquario-team/asimov-central) | painel web e toda a documentação (privado) |

## O que tem aqui

| Arquivo | O que é |
|---|---|
| `asimov-ihm.cmtp` | o projeto da IHM: telas, tags e a conexão com o CLP. **É o único arquivo que importa.** |
| `README.md` | este guia |
| `.gitignore` / `.gitattributes` | o que o git ignora / como trata cada tipo de arquivo |

Feito no **EasyBuilder Pro 6.10.02** (build 470).

## Como a IHM conversa com o CLP

```
 operador ── [ IHM cMT1106X ] ── rede Ethernet ── [ CLP PLC500 ] ── rede ── [ coletor asimov ]
              192.168.1.20        (switch)          192.168.1.10
```

- A IHM **lê** do CLP o estado da máquina e as peças (variáveis da `GVLColeta`).
- O operador digita a OP na tela e a IHM **grava** em `GVLColeta.opNumero` no CLP.
- O coletor lê tudo **do CLP**, nunca da IHM: o CLP é a fonte da verdade e funciona mesmo com a tela desligada.

As variáveis estão explicadas no [README do asimov-clp](https://github.com/aquario-team/asimov-clp).
O passo a passo da ligação (rede, IP, importar as tags) está em
[docs/PLC500.md](https://github.com/aquario-team/asimov-central/blob/main/docs/PLC500.md) no asimov-central.

## Mandar para a IHM

1. Instale o **EasyBuilder Pro** ([weintek.com/Download/Software](https://www.weintek.com/Download/Software), gratuito, pede login).
2. Abra `asimov-ihm.cmtp`.
3. **Project → Compile**: tem que terminar sem erros. Isso gera o `asimov-ihm.cxob`, que é o arquivo que vai para a tela.
4. Ligue o notebook na mesma rede da IHM (faixa `192.168.1.x`).
5. **Project → Download (PC → HMI)** → escolha a IHM pelo IP `192.168.1.20` → **Download**.

> O CLP precisa estar com o programa do asimov-clp (com a Symbol Configuration). Sem ele a IHM não acha as tags.

## Manter este repositório

- Editou no EasyBuilder Pro: salve, feche e faça commit do `asimov-ihm.cmtp`. Escreva na mensagem do commit
  **o que mudou na tela** (ex: "tela de parada: campo de motivo"), porque o `.cmtp` é binário e o diff não mostra nada.
- Os arquivos que o EasyBuilder Pro gera sozinho ficam fora do git (`.gitignore`):

| Arquivo gerado | O que é |
|---|---|
| `*.cmtp.bak1`, `*.cmtp.bak2`… | backups automáticos que o editor cria a cada vez que salva |
| `*.cxob` | projeto compilado para mandar à IHM (~47 MB). Gere de novo com **Project → Compile** |

> **Não use "Add files via upload" no site do GitHub.** O upload pelo site ignora o `.gitignore` e manda
> os arquivos gerados junto. Use `git add` + `git commit` + `git push` (ou o GitHub Desktop), que respeitam o `.gitignore`.
