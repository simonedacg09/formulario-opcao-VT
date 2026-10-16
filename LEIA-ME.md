# Declaração de Vale-Transporte: instalação no GitHub Pages

Este pacote cria dois endereços:

- **Formulário do funcionário** (`index.html`): abre em qualquer celular, sem login.
- **Painel do RH** (`rh.html`): protegido por senha. Nele o RH gera os links personalizados e vê as respostas chegando sozinhas, com atualização a cada 30 segundos.

O GitHub Pages hospeda apenas as páginas. As respostas são gravadas numa **Planilha Google**, e as fotos (comprovante, cartão e carta) numa **pasta privada do Google Drive** da conta do RH, por meio de um pequeno script gratuito do Google (Apps Script). Nenhum dado fica no GitHub.

Tempo estimado: 20 a 30 minutos, uma única vez.

---

## Passo 1: criar a planilha e o script (Google)

1. Entre na conta Google do RH (de preferência uma conta da empresa, não pessoal).
2. Crie uma planilha nova em sheets.google.com e dê o nome **Vale-Transporte - Respostas**.
3. Na planilha, abra o menu **Extensões > Apps Script**.
4. Apague o conteúdo que aparecer e **cole todo o conteúdo do arquivo `Code.gs`**.
5. Na linha `const SENHA_RH = "TROQUE-ESTA-SENHA";`, troque o texto entre aspas por uma senha forte. Essa é a senha que o RH vai usar no painel.
6. Clique em **Salvar** (ícone de disquete).
7. No seletor de funções no topo, escolha **configurar** e clique em **Executar**.
   - O Google vai pedir autorização. Clique em *Revisar permissões*, escolha a conta, depois *Avançado > Acessar (não seguro)* e *Permitir*. Esse aviso aparece porque o script é seu e não foi publicado na loja do Google.
   - Ao terminar, a planilha terá as abas **Empresas** (já com os 6 CNPJs), **Convites** e **Respostas**, e o Drive terá a pasta **Vale-Transporte - Anexos**.

## Passo 2: publicar o script como App da Web

1. No Apps Script, clique em **Implantar > Nova implantação**.
2. Na engrenagem, escolha o tipo **App da Web**.
3. Preencha:
   - **Executar como:** *Eu* (a conta do RH)
   - **Quem pode acessar:** *Qualquer pessoa*
4. Clique em **Implantar** e **copie o URL do App da Web**, que termina em `/exec`.

> Se depois você alterar o `Code.gs`, use **Implantar > Gerenciar implantações > Editar (lápis) > Versão: Nova versão**. Assim o URL continua o mesmo.

## Passo 3: colar o endereço no `config.js`

Abra o arquivo `config.js` e troque `COLE_AQUI_O_ENDERECO_DO_APPS_SCRIPT` pelo URL copiado. Mantenha as aspas:

```js
window.API_URL = "https://script.google.com/macros/s/AKfy.../exec";
```

## Passo 4: publicar no GitHub Pages

1. Crie uma conta em github.com, se ainda não tiver.
2. Clique em **New repository**. Use o nome `vale-transporte` e deixe o repositório **Public**, porque o GitHub Pages gratuito exige isso. As páginas não contêm dados pessoais nem a senha.
3. Na página do repositório, clique em **Add file > Upload files** e envie os 5 arquivos: `index.html`, `rh.html`, `style.css`, `config.js` e `LEIA-ME.md`. **Não envie o `Code.gs`**. Clique em **Commit changes**.
4. Vá em **Settings > Pages**. Em *Branch*, escolha **main** e a pasta **/(root)**, e clique em **Save**.
5. Depois de 1 a 2 minutos, o site estará em:
   - Painel do RH: `https://SEU-USUARIO.github.io/vale-transporte/rh.html`
   - Formulário: `https://SEU-USUARIO.github.io/vale-transporte/index.html`

## Passo 5: usar

1. Abra o **Painel do RH** e entre com a senha.
2. Em **Gerar link**, escolha a empresa, digite o nome do funcionário (e o WhatsApp, se quiser) e clique em **Gerar link**.
3. Envie pelo botão **Enviar pelo WhatsApp** ou copie a mensagem pronta.
4. O funcionário abre o link no celular, já com a razão social, o CNPJ e o nome dele preenchidos, e envia a declaração.
5. A resposta aparece sozinha na aba **Respostas** do painel e também na planilha Google. A situação do link muda para *Respondeu*.

## Cadastro de empresas

Para incluir ou alterar uma razão social ou um CNPJ, edite a aba **Empresas** da planilha. O painel lê essa aba automaticamente.

## Segurança e LGPD

- As fotos ficam numa pasta **privada** do Drive. Só abrem para quem tem acesso à conta do RH ou para quem o RH compartilhar a pasta.
- O painel só mostra dados com a senha. Troque a senha de tempos em tempos (passo 1, item 5, e depois publique uma nova versão, como explicado no passo 2).
- O formulário só aceita envios com um código de convite válido gerado pelo RH.
- Use uma conta Google **da empresa** e restrinja o compartilhamento da planilha e da pasta à equipe de RH.
- Recomenda-se que o jurídico ou o encarregado de dados (DPO) revise o texto do termo.

## Problemas comuns

- **"Formulário ainda não configurado"**: o `config.js` não foi atualizado com o URL do passo 2.
- **O painel não entra / "Sem conexão"**: confira se a implantação está com *Quem pode acessar: Qualquer pessoa* e se o URL termina em `/exec`.
- **Alterei o Code.gs e nada mudou**: publique uma *Nova versão* da implantação (passo 2).
