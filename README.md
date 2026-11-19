# IA com Rota Reserva

[![IA com Rota Reserva](guia/assets/hero.jpg)](https://inematds.github.io/ia-com-rota-reserva/guia/)

**[Português](README.md) · [English](README.en.md) · [Español](README.es.md)**

[Abra o guia e teste a rota reserva](https://inematds.github.io/ia-com-rota-reserva/guia/)

## O que é

IA com Rota Reserva é um guia com uma demonstração para quem quer experimentar ferramentas de IA com limites de uso. Ele mostra como o FreeLLMAPI tenta outro provedor, o serviço que recebe seu pedido e fornece o modelo, e onde entram Claude Desktop, Claude Code, Arena e v0. Você pode começar pela demonstração, que roda no navegador e usa respostas fictícias.

**Pedido para seu assistente**

> Quero experimentar uma rota reserva para um configurador fictício de fones. O resultado precisa mudar a cor e girar a vista. Use como contexto https://inematds.github.io/ia-com-rota-reserva/guia/ . Sugira uma ferramenta e um teste que eu consiga conferir. Diga onde os dados seriam processados e quanto poderia custar. Aguarde meu aval antes de usar serviços externos ou alterar configurações.

## Instalação técnica

Para testar sem instalar nada, [baixe o ZIP do projeto](https://github.com/inematds/ia-com-rota-reserva/archive/refs/heads/main.zip), extraia a pasta e dê dois cliques em `guia/index.html`.

Para usar um servidor local, confirme que tem [Python 3](https://www.python.org/downloads/). No Windows, abra a pasta extraída no Explorador de Arquivos, digite `powershell` na barra de endereço e pressione Enter. No macOS ou Linux, abra o terminal dentro da pasta extraída. Confirme a instalação com `python3 --version` ou `py -3 --version` no Windows.

```sh
python3 -m http.server 18090 --bind 127.0.0.1
```

No Windows, execute `py -3 -m http.server 18090 --bind 127.0.0.1` no lugar do comando acima. O servidor fica aberto no terminal até você pressionar Ctrl+C.

Abra `http://127.0.0.1:18090/guia/`. Selecione **Limite de uso** no principal e **Disponível** na reserva. O registro deve mostrar duas tentativas e abrir um configurador em que você consegue mudar acabamento e rotação. Selecione **Acesso negado** para verificar a interrupção da rota. Com os dois serviços temporariamente indisponíveis, o configurador deve permanecer fechado.

O configurador local usa CSS e JavaScript e exemplifica o pedido de produto do vídeo. As respostas e os erros são simulados. A lógica do simulador é própria. Os códigos ilustrativos correspondentes seriam 429 para limite de uso, 503 para indisponibilidade temporária e 401 ou 403 para um problema de autenticação ou permissão. A política de fallback de um roteador real varia por provedor, tipo de erro e configuração.

## O processo, do início ao fim

A referência é [a demonstração no YouTube](https://www.youtube.com/watch?v=wbchejAAe5A). A documentação abaixo foi conferida em 19 de novembro de 2026. O roteiro do vídeo de referência contém capítulos que divergem do arquivo atual, por isso a tabela usa os tempos da reprodução observada.

| Na referência | Ferramenta | O que conferir |
| --- | --- | --- |
| 3:43 a 7:43 | [FreeLLMAPI](https://github.com/tashfeenahmed/freellmapi) | Desktop ou instalação local, provedores e ordem de fallback. A porta na demo era 31415. Confira a porta e a chave do seu gateway. |
| 7:43 | [Claude Desktop com inferência de terceiros](https://support.claude.com/en/articles/18426225-move-from-claude-enterprise-to-claude-desktop-on-a-cloud-provider) | Disponibilidade da configuração, URL do gateway e modelo efetivo nas rotas configuradas. |
| 10:20 | [Claude Code no aplicativo](https://support.claude.com/en/articles/18466061-move-claude-code-cli-users) | Selecione uma pasta de teste e autorize só os comandos necessários. No configurador de fones, teste cor, material e rotação após o build. |
| 11:39 | [Claude Code no terminal](https://code.claude.com/docs/en/llm-gateway) | Aponte a sessão para o gateway, selecione uma cadeia com opções habilitadas e envie primeiro um pedido curto. |
| 13:32 | FreeLLMAPI Playground | O protótipo do vídeo recebia um PDF. Em um teste real, abra o arquivo baixado e verifique se o conteúdo e o formato conferem. |
| 15:49 | [Arena Model API](https://platform.arena.ai/docs/api) | A documentação atual indica `https://api.arena.ai` para clientes Anthropic. O acesso direto a um modelo requer fundos pagos ou crédito promocional elegível. Consulte a conta antes de executar. |
| 17:23 | [v0](https://v0.app/pricing) | O plano Free lista sete mensagens por dia na consulta desta data. O anúncio de crédito da referência e a escolha de Fable 5.2 dependem da oferta e da conta. |

Na instalação demonstrada, o FreeLLMAPI usa Google AI Studio, Kilo Gateway, OpenRouter, NVIDIA, Z.ai e Cloudflare Workers AI. O [README oficial](https://github.com/tashfeenahmed/freellmapi) mantém a lista atual. Depois de baixar uma versão compatível pela página de [releases](https://github.com/tashfeenahmed/freellmapi/releases), habilite apenas provedores cujos termos e possíveis custos você tenha conferido. Configure pelo menos dois na cadeia de fallback e faça o primeiro teste com dados fictícios.

### Experimente com serviços reais

1. Instale [Claude Code](https://code.claude.com/docs/en/setup) e confira `claude --version`. Baixe o [FreeLLMAPI](https://github.com/tashfeenahmed/freellmapi/releases) para sua plataforma. Em instalações de servidor, use o [passo a passo oficial](https://github.com/tashfeenahmed/freellmapi#quick-start).
2. Abra o FreeLLMAPI. Em **Keys**, habilite dois provedores que você pode usar e cadastre as chaves quando exigidas. Confira seus limites antes de prosseguir. Em **Models → Fallback chains**, crie `free-claude-code` se ainda não existir, habilite os dois modelos na ordem desejada e salve. Teste no **Playground** com uma pergunta curta e confira qual opção respondeu.
3. No painel do FreeLLMAPI, copie a URL base Anthropic e a chave unificada. A [documentação do FreeLLMAPI para clientes](https://github.com/tashfeenahmed/freellmapi/blob/main/docs/en/clients/01-agent-clients.md#claude-desktop-third-party-gateway) indica `http://127.0.0.1:31415` no app e `http://localhost:3001` na instalação de servidor. Use a origem sem `/v1`.
4. No Claude Desktop, se a configuração de terceiros estiver disponível, abra **Setup → Gateway**. Em versões com o menu da referência, use **Help → Troubleshooting → Developer Mode** e depois **Developer → Configure third-party inference**. Se o menu não aparecer, verifique as exigências da versão e da sua organização. Use essa URL e a chave unificada com autenticação `x-api-key`, teste e salve. Confira o mapeamento de modelos no FreeLLMAPI. A aba **Code** herda a configuração de terceiros da aplicação.
5. No Claude Code instalado separadamente, abra uma nova sessão Bash em uma pasta de teste e cole o bloco abaixo com a URL da sua instalação. Ao executar, cole a chave unificada no prompt oculto. A subsessão mantém as variáveis fora do seu terminal normal.

Se o FreeLLMAPI estiver no Windows, execute o Claude Code no mesmo Windows e use a opção [PowerShell do manual do FreeLLMAPI](https://github.com/tashfeenahmed/freellmapi/blob/main/docs/en/clients/01-agent-clients.md#coding-agents). O bloco Bash abaixo serve a macOS e Linux com o roteador no mesmo ambiente.

```bash
(
  export ANTHROPIC_BASE_URL=http://127.0.0.1:31415
  read -rsp 'Chave unificada do FreeLLMAPI: ' ANTHROPIC_AUTH_TOKEN
  printf '\n'
  if [ -z "$ANTHROPIC_AUTH_TOKEN" ]; then
    echo 'Informe uma chave para continuar.' >&2
    exit 1
  fi
  export ANTHROPIC_AUTH_TOKEN
  export ANTHROPIC_MODEL=auto:free-claude-code
  claude
)
```

Peça uma resposta de uma frase e confira no registro do roteador quem respondeu. Se a mensagem disser que a cadeia está vazia, confira os modelos habilitados. Se faltar permissão, revise a chave antes de insistir. A Anthropic documenta a conexão por gateway, porém [não oferece suporte para Claude Code com modelos que não sejam Claude](https://code.claude.com/docs/en/llm-gateway). A integração com provedores externos exige as suas credenciais e não foi executada na demonstração deste projeto. Use dados fictícios até verificar termos, retenção, limites e permissões.

#### Exemplos de chaves

Duas opções da referência para encontrar as chaves

- **Google AI Studio**. Entre em [API keys](https://aistudio.google.com/apikey), crie uma chave e selecione Google AI Studio em **Keys → Add key** no FreeLLMAPI. Confira antes se seu projeto Google tem cobrança habilitada.
- **NVIDIA**. Escolha um modelo no [catálogo da NVIDIA](https://build.nvidia.com/explore/discover), entre na sua conta e use **Get API Key** conforme o [guia oficial](https://docs.api.nvidia.com/nim/re/docs/api-quickstart). Selecione NVIDIA em **Keys → Add key**.

Essas chaves permitem que o FreeLLMAPI fale com cada serviço. A chave unificada é outra: funciona como a senha do seu aplicativo para falar com o FreeLLMAPI. O gateway é o endereço do roteador que faz o encaminhamento. Use a URL base exibida no painel, sem acrescentar o trecho `/v1`.
