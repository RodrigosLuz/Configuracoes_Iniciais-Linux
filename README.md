# Configurações Iniciais no Linux

Comandos para configurar um ambiente de trabalho no Linux.

## Instalar Pyenv

### Atualizar as listas de pacotes

Antes de tudo, atualize as listas de pacotes:

```bash
sudo apt update
sudo apt upgrade -y
```

### 1. Instalar pacotes essenciais para desenvolvimento

Rode o seguinte comando para instalar alguns pacotes necessários:

```bash
sudo apt-get install -y build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm gettext libncurses5-dev tk-dev tcl-dev blt-dev libgdbm-dev git python3-dev aria2 libnss3-tools python3-venv liblzma-dev libpq-dev

# Explicação dos pacotes principais:

# - build-essential: Ferramentas de compilação como gcc e make.

# - libssl-dev: Bibliotecas para suporte a SSL.

# - zlib1g-dev: Biblioteca de compressão de dados.

# - libbz2-dev: Biblioteca de compressão Bzip2.

# - libreadline-dev: Biblioteca para histórico e edição de linha de comando.

# - libsqlite3-dev: Biblioteca para o SQLite.

# - wget/curl: Ferramentas para download de arquivos pela internet.

# - llvm: Ferramenta para otimização e compilação de código.

# - gettext: Suporte para internacionalização.

# - libncurses5-dev: Biblioteca para interfaces de terminal.

# - tk-dev/tcl-dev/blt-dev: Bibliotecas para GUIs baseadas em Tk.

# - libgdbm-dev: Biblioteca GNU dbm.

# - git: Controle de versão.

# - python3-dev: Cabeçalhos de desenvolvimento para Python 3.

# - aria2: Ferramenta para downloads multi-protocolos.

# - libnss3-tools: Ferramentas de suporte para segurança de rede.

# - python3-venv: Suporte para ambientes virtuais em Python 3.

# - liblzma-dev: Biblioteca de compressão LZMA.

# - libpq-dev: Bibliotecas de desenvolvimento para PostgreSQL.
```



### 2. Instalar o Pyenv

O Pyenv é uma ferramenta para gerenciar diferentes versões do Python em um mesmo sistema. Ele permite instalar, alternar e manter múltiplas versões do Python, tornando o desenvolvimento mais flexível e compatível com diferentes projetos.

Use o seguinte comando para instalar o Pyenv:

```bash
curl https://pyenv.run | bash
```

### 3. Configurar o bashrc para ativar o Pyenv automaticamente

a. Copie as linhas que aparecerão ao final da instalação anterior:

```bash
export PATH="~/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
```

b. Edite o arquivo `.bashrc` para adicionar essas linhas ao final:

```bash
nano ~/.bashrc
```

c. Feche e reabra o terminal para aplicar as mudanças (ou execute `source ~/.bashrc` para aplicar as mudanças sem fechar o terminal).

## Instalar o Git

### Verificar versão e instalar o Git

Verifique se o Git está instalado:

```bash
git --version
```

Caso não esteja instalado, use o comando:

```bash
sudo apt install git
```

### Configurar usuário e e-mail

Verifique se existe um usuário configurado:

```bash
cat ~/.gitconfig
```

Ou:

```bash
git config --list
```

Caso precise configurar um novo usuário:

```bash
git config --global user.name "Seu Nome"
git config --global user.email "seu.email@exemplo.com"

# Explicação:

# - O uso de `--global` aplica essas configurações a todos os repositórios no sistema do usuário.

# - Sem o `--global`, as configurações se aplicam apenas ao repositório atual.
```

Se houver algum erro e precisar deletar o arquivo de configuração:

```bash
rm ~/.gitconfig
```

## Configurar Repositório para Trabalhar com Fork de uma Organização

### 1. Fazer Fork da Organização

Faça o fork do repositório da organização para o seu GitHub pessoal. Verifique quais *branches* existem e se é necessário trazer todas ou apenas a *branch* principal.

### 2. Clonar o Repositório para o Ambiente Local

Clone o repositório do seu GitHub:

```bash
git clone <URL_DO_REPOSITORIO>
```

Para verificar quais *branches* estão disponíveis:

```bash
git branch -a
```

### 3. Configurar o Repositório Upstream

Adicione o repositório da organização como `upstream`:

```bash
git remote add upstream <URL_DO_REPOSITORIO_ORIGINAL>
```

Verifique se deu certo:

```bash
git remote -v
```

Se precisar mudar a URL do *remote*:

```bash
git remote set-url origin <NOVA_URL_DO_REPOSITORIO>
```

### 4. Atualizar o Repositório Local com a Organização

a. Traga as atualizações diretamente do repositório da organização:

```bash
git fetch upstream
```

b. Atualize sua *branch* local (faça para cada *branch* que precisar):

- Mude para a *branch* que deseja atualizar:

```bash
git switch <branch>  # Ou use `git checkout <branch>` para compatibilidade com versões mais antigas do Git.
```

- Traga as mudanças do *upstream*:

```bash
git pull upstream <branch_remota> --rebase  # A opção --rebase evita merges desnecessários, mantendo um histórico linear e mais limpo. Diferente do `merge`, que cria um commit de merge, o `rebase` aplica suas mudanças sobre a base mais recente, tornando o histórico mais fácil de seguir.
```

### 5. Atualizar o Repositório Remoto Pessoal

Atualize seu repositório remoto pessoal (*origin*):

```bash
git push -u origin <branch>  # Use este comando para empurrar uma branch nova para o repositório remoto pela primeira vez.
```
ou
```
git push  # Se a branch já foi empurrada anteriormente, apenas `git push` é suficiente, tornando o comando mais eficiente.
```

### 6. Configurar Repositório Remoto Padrão

Configure seu *origin* como repositório padrão para uso dos comandos `checkout` ou `switch`:

```bash
git config checkout.defaultRemote origin
```

Defina o comportamento padrão para o comando `git push`, simplificando o envio de alterações para o repositório remoto padrão. Isso garante que apenas a branch atual será empurrada para o repositório remoto correspondente:

```bash
git config --global push.default simple
```

## Exemplo de Fluxo de Trabalho

Este fluxo pressupõe que todas as configurações locais e de acesso ao repositório já foram realizadas. Este é apenas um exemplo de fluxo de trabalho e pode precisar ser adaptado de acordo com as regras e práticas específicas da organização.

1. **Escolher uma Issue**:

   - Identifique a *issue* a ser trabalhada na organização, criando uma nova ou verificando uma existente. Exemplo: `#20`.

2. **Sincronizar com o Upstream**:

   - Certifique-se de que seu repositório local está atualizado com as últimas mudanças do repositório principal (*upstream*):
     ```bash
     git pull upstream <branch_remota> --rebase
     ```

3. **Sincronizar com o Origin**:

   - Sincronize seu repositório pessoal (*origin*):
     ```bash
     git push origin main
     ```

4. **Criar uma Branch para a Tarefa**:

   - Crie uma *branch* para a tarefa relacionada à *issue* a ser trabalhada:
     ```bash
     git checkout -b nome-da-branch
     ```

5. **Realizar Commits**:

   - Faça *commits* no progresso do trabalho, utilizando a expressão `closes #20` para fechar a *issue* ao concluir:
     ```bash
     git commit -m "Descrição do trabalho - closes #20"
     ```

6. **Enviar a Branch para o Repositório Remoto**:

   - Empurre a *branch* de trabalho para o seu repositório remoto (apenas necessário se a *branch* ainda não existir no remoto):
     ```bash
     git push -u origin nome-da-branch
     ```
     ou:
     ```
     git push
     ```

7. **Criar Pull Request no GitHub**:

   - No GitHub, crie o *pull request* da sua *branch* de trabalho para a organização.

8. **Limpeza de Branches**:

   - Após o *pull request* ser aceito, delete a *branch* do remoto e do local:
     ```bash
     git push origin --delete nome-da-branch
     git branch -d nome-da-branch
     ```
     Se a branch local não foi completamente mesclada e precisa ser forçada a ser removida, use `git branch -D nome-da-branch`, mas use com cautela, pois isso pode resultar em perda de trabalho não integrado.

---

Este fluxo ajuda a manter o repositório organizado e facilita o rastreamento de alterações e resoluções de *issues*.

