---
title: "Exercício 3 - Testar seu recurso com o servidor do Model Context Protocol (MCP) do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar o recurso de filtragem com o modo de agente. Antes de abrir um pull request (PR), confirme se ele realmente funciona no navegador. Em vez de navegar pelo aplicativo manualmente, você conectará o servidor MCP do **Playwright** e permitirá que o Copilot controle um navegador real para testar o recurso — depois, publicará seu branch e abrirá o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam os recursos do Copilot com novas ferramentas;
- adicionar o servidor MCP do Playwright ao workspace;
- pedir ao Copilot que use o servidor para testar manualmente o recurso de filtragem em um navegador;
- publicar o branch e abrir um pull request para o trabalho de filtragem.

## O que é o MCP?

O modo de agente fica muito mais poderoso quando pode acessar recursos além do editor. O MCP permite que o Copilot faça isso: é uma maneira padrão para o agente se comunicar com ferramentas e serviços externos.

![Diagrama mostrando o funcionamento interno do modo de agente e como ele interage com contexto, modelo de linguagem grande (LLM) e ferramentas, incluindo ferramentas disponibilizadas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [MCP](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de inteligência artificial (IA) se comuniquem com ferramentas e serviços externos. Com o MCP, os agentes de IA podem se comunicar com ferramentas e serviços externos em tempo real. Assim, eles podem acessar informações atualizadas (usando recursos) e realizar ações em seu nome (usando ferramentas).

Essas ferramentas e esses recursos são acessados por meio de um servidor MCP, que funciona como uma ponte entre o agente de IA e as ferramentas e os serviços externos. O servidor MCP gerencia a comunicação entre o agente de IA e as ferramentas externas (como APIs existentes ou ferramentas locais, por exemplo, pacotes npm). Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente de IA pode acessar.

Alguns servidores MCP bastante usados:

- **[Servidor MCP do GitHub](https://github.com/github/github-mcp-server)**: fornece acesso a um conjunto de interfaces de programação de aplicações (APIs) para gerenciar repositórios do GitHub. Ele permite que o agente de IA execute ações como criar repositórios, atualizar os existentes e gerenciar problemas e pull requests.
- **[Servidor MCP do Playwright](https://github.com/microsoft/playwright-mcp)**: oferece recursos de automação de navegador usando o Playwright. Ele permite que o agente de IA execute ações como navegar por páginas Web, preencher formulários e selecionar botões.

Há muitos outros servidores MCP que dão acesso a diferentes ferramentas e recursos. O GitHub hospeda um [registro do MCP](https://github.com/mcp) para facilitar a descoberta de recursos e as contribuições ao ecossistema.

> [!CAUTION]
> Em relação à segurança, trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor MCP, examine cuidadosamente o código-fonte, verifique o publicador e considere as implicações de segurança. Use apenas servidores MCP em que você confia e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revise a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste workspace do VS Code.

1. Abra `.vscode/mcp.json` no codespace.
2. Você verá um servidor `github` já configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

Essa entrada `github` vem do modelo do projeto, por isso o Copilot conseguiu ler o backlog de problemas no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], então não há nada para instalar localmente — o VS Code se conecta a ele por HTTP e você se autentica com o GitHub na primeira vez que o Copilot usa uma de suas ferramentas.

## Adicione o servidor MCP do Playwright

Agora você adicionará um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador que ele pode controlar, exatamente o que você precisa para testar o recurso.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github` para que o arquivo fique assim:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Salve o arquivo.

Ao contrário do servidor remoto `github`, `playwright` é um servidor **local**: o VS Code o inicia na sua máquina executando `npx @playwright/mcp@latest`. O sinalizador `--headless` faz o Playwright executar o navegador sem uma janela visível, necessário dentro de um codespace, que não tem uma área de trabalho para exibi-la.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright para os testes de ponta a ponta, então o navegador necessário normalmente já está instalado. Se o Copilot informar mais tarde que o navegador está ausente, peça para executar `npx playwright install chromium` e tente novamente.

## Inicie e confie no servidor

O VS Code inicia os servidores MCP **sob demanda** — na primeira vez que o Copilot precisa das ferramentas de um servidor, o VS Code o inicia e pede que você confirme que confia nele.

1. Em `.vscode/mcp.json`, aparecerá uma ação **Start** acima da entrada `playwright`. Selecione-a para iniciar o servidor agora e confirmar que a configuração está correta.
2. Se o VS Code pedir que você confirme se confia no servidor, revise a configuração e escolha confiar nele para que possa iniciar.

Iniciá-lo manualmente é opcional — se você pular essa etapa, o VS Code iniciará o servidor (e solicitará sua confirmação) na primeira vez que o Copilot precisar das ferramentas, na próxima etapa.

## Teste o recurso de filtragem

O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador real para controlar. Em vez de navegar pelo aplicativo manualmente, o agente pode abrir uma página, navegar, aplicar filtros, ler o resultado e resumir o que observou. É a maneira mais rápida de confirmar se um recurso funciona como esperado sem sair da conversa.

Internamente, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página em vez de capturas de tela. Isso significa que o agente trabalha com elementos estruturados e rotulados (botões, links, itens de lista), como a tecnologia assistiva faz — assim, uma verificação funcional rápida também serve como uma verificação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot para testar o recurso de filtragem que você acabou de criar:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O Copilot abrirá um navegador pelo servidor MCP do Playwright, executará cada etapa e informará o que encontrou. Compare o resumo com os critérios de aceitação do problema. Se algo não estiver correto, faça perguntas complementares ou peça ao Copilot que corrija o código antes de abrir o pull request.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321` para esse teste. Se você tiver parado o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira vez que o Copilot usar o servidor MCP do Playwright, talvez seja necessário baixar um navegador — se ele informar que o navegador está ausente, peça para executar `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publique o branch e crie um pull request

Agora que confirmou que o recurso funciona, você pode abrir um pull request (PR) para que a equipe o revise. Primeiro, publique o branch `filtering-vscode`.

1. Acesse o painel **Source Control** no codespace e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Captura de tela do painel Source Control mostrando as alterações feitas](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar o branch ao repositório.

## Crie o pull request

Há várias maneiras de criar um pull request, incluindo o github.com e a interface de linha de comando do GitHub (CLI). Como você já está trabalhando com o GitHub Copilot, vamos pedir a ele que crie o PR. Ele pode localizar o problema relacionado e associá-lo ao PR.

1. Acesse o painel Copilot Chat e selecione **New Chat** para iniciar uma nova sessão.
2. Confirme que **Agent** está selecionado na lista suspensa de agentes para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot para criar um PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Se necessário, selecione **Continue** para permitir que o Copilot reúna informações e execute as operações. Na primeira vez que usar uma ferramenta do GitHub, talvez seja solicitado que você se autentique no GitHub — siga as instruções para permitir o acesso.
5. Observe como o Copilot pesquisa os problemas, encontra o correto e cria o PR.
6. Selecione o link gerado pelo Copilot para revisar seu pull request, mas **não faça merge ainda**.

## Resumo e próximas etapas

Parabéns! Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam os recursos do Copilot com novas ferramentas;
- adicionou o servidor MCP do Playwright ao workspace;
- usou o servidor para testar manualmente o recurso de filtragem em um navegador antes de publicá-lo;
- publicou o branch e abriu um pull request para o trabalho de filtragem.

Você usou um servidor MCP para *testar* um recurso — mas o MCP é apenas uma das maneiras de ampliar os recursos do Copilot. Em seguida, vamos [criar um agente personalizado][next-lesson] para agilizar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro de servidores MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
