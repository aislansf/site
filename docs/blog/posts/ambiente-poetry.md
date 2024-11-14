---
date: 2024-11-13
authors:
  - aislansf
categories:
  - Ambiente
# description: >
#   We are transforming Material for MkDocs to ensure its community continues to thrive and doubling down on our commitment to Open Source.
# title: How we're transforming Material for MkDocs
---

# Preparação de um ambiente de desenvolvimento Python

!!! info "Objetivo"

    Nessa postagem iremos aprender como preparar um ambiente de desenvolvimento para a linguagem Python.


<!-- more -->

## Introdução
Ao aprender uma linguagem de programação, o foco é essencialmente entender a sintaxe, o estilo de código e os conceitos subjacentes. Com o tempo, você se torna suficientemente confortável com a linguagem e começa a escrever programas que resolvem problemas realmente interessantes. No entanto, para avançar para essa etapa, há um aspecto que pode ser subestimado que é como construir o ambiente certo. Um ambiente que impõe boas práticas de engenharia de software, melhora a produtividade e facilita a colaboração.

Outra característica importante é que este ambiente suporte o trabalho simultâneo em vários projetos, utilizando diferentes versões de uma linguagem de programação, além de bibliotecas e ferramentas específicas para cada um dos projetos. Para que um projeto não interfira em outro, eles devem ser isolados de alguma forma.

Quando se trata da linguagem Python, vários projetos de código aberto surgiram nos últimos anos e visam facilitar o gerenciamento de versões, dependências e ambientes virtuais.

<figure markdown="span">
  ![Fonte: xkcd/1987.](https://imgs.xkcd.com/comics/python_environment.png){ width="500" }
  <figcaption>Fonte: xkcd/1987</figcaption>
</figure>

Afim de criar um ambiente que facilite a colaboração, imponha boas práticas, melhore a produtividade, utilizaremos as seguintes ferramentas:

- `ubuntu`: ambiente mais próximo de ambientes de produção;
- `pyenv`: gerencia diferentes versões do Python na mesma máquina;
- `poetry`: gerencia ambientes virtuais e as dependências de tais ambientes;
- `git`: sistema colaborativo e descentralizado de controle de versões.

Vamos então seguir os seguintes passos:

1. Instalar o `WSL` e o `Ubuntu`;
2. Instalar o `pyenv`;
3. Instalar o `poetry`;
4. Configurar o `git`.

!!! note "Nota"
    A instalação do ambiente dependerá do seu sistema operacional. Caso o seu sistema operacional seja o Windows, siga todos os passos. Caso use o sistema operacional Linux ou MacOS, inicie da [seção Instalar o pyenv](#2-instalar-o-pyenv).

## 1. Instalar o WSL
O *Windows subsystem for Linux (WSL)* é uma ferramenta poderosa que permite executar um ambiente Linux diretamente do Windows, sem a necessidade de uma máquina virtual separada. Para instalar o WSL no Windows você pode verificar a [documentação oficial](https://learn.microsoft.com/pt-br/windows/wsl/install), ver o [seguinte vídeo](https://www.youtube.com/watch?v=o1_E4PBl30s), ou simplesmente seguir os passos abaixo:

### Verifique os requisitos do sistema
Certifique-se de que você está utilizando o Windows 10 (versão 19041 ou superior) ou o Windows 11.

### Ative o WSL
Abra o PowerShell como administrador. Para fazer isso, clique com o botão direito no menu iniciar e selecione **Windows PowerShell (Admin)**. No PowerShell, execute o seguinte comando para abilitar o powershell:

```
wsl --install
```
Este comando instala o WSL e a distribuição padrão do Ubuntu. Após a instalação você será solicitado a reiniciar o computador. Salve o seu trabalho e reinicie.

### Iniciar o Ubuntu
Na primeira vez que você iniciar uma distribuição recem instalada, uma janela de console será aberta e não mostrará nada, aguarde algum tempo, pois os arquivos estão sendo descompactados. Após algum tempo você será solicitado a criar um novo nome de usuário *(username)* e, depois disso uma senha.

!!! note "Nota"
    Ao digitar a senha, nada aparecerá. Não se preocupe, pois este é um recurso de segurança que não exibe os caracteres digitados.

Pronto! Agora você já pode utilizar o Ubuntu instalado na sua máquina. Para garantir que o WSL e o Ubuntu estão instalados corretamente, abra o `PowerShell` e execute:

```
wsl --list --verbose
```

Este comando lista as distribuições instaladas e seus stadus. Você deve ver o Ubuntu listado como a distribuição padrão.

!!! tip "Dica"
    Uma outra ferramenta interessante de se instalar e que se integra perfeitamente com o WSL é o `Windows Terminal`, isso facilitará a utilização futura. Para instalar basta procurar por "Windows Terminal" na *Microsoft Store* e clicar em **Instalar**.

## 2. Instalar o `pyenv`
Antes de instalar o `pyenv`, iremos configurar o comando de chamada do Python. Por padrão, o **Ubuntu** vem com o comando `python3`, mas para facilitar, iremos instalar uma aplicação que cria um link simbólico entre o comando `python` e o comando `python3`, através do seguinte comando:

```
sudo apt install python-is-python3
```

Agora podemos utilizar o comando python normalmente. Tente usar

```
python -V
```
Vamos iniciar clonando o repositório do pyenv:

```
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
```

Depois disso, execute os seguintes comandos para configuração do bash:

```
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(pyenv init -)"' >> ~/.bashrc
```

Por fim, iremos reiniciar o SHELL:

```
exec "$SHELL"
```

Depois desse comando feche o Ubuntu e abra novamente.

### Instalar dependências de build do `pyenv`
Para que o `pyenv` funcione perfeitamente, você precisa instalar as dependências de build, necessárias para compilar as versões do python. Utilize o seguinte comando:

```
sudo apt-get update; sudo apt-get install make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
```

### Instalar o `python` 3.10.11
Agora podemos instalar versões do python. Antes de tudo, pode verificar quais versões tem instalada com o comando pyenv versions. Você verá somente a versão do sistema. No entanto, precisamos instalar a versão 3.10.11. Para tal, utilize o seguinte comando:

```
pyenv install 3.10.11
```

Prontinho, agora precisamos mudar para versão instalada utilizando o comando:

```
pyenv global 3.10.11
```

Para verificar se a seleção ocorreu de maneira correta, utilize o comando:

```
pyenv versions
```

A saída deste comando deverá mostrar qual versão do python está selecionada como global.

## 3. Instalar o `poetry`
Para realizar a gestão de dependências e ambientes virtuais precisamos instalar o Poetry. Fazemos isso através do seguinte comando:

```
curl -sSL https://install.python-poetry.org | python -
```

Depois disso, precisamos colocar o `poetry` no path usando o seguinte comando:

```
echo 'export PATH="$HOME/.poetry/bin:$PATH"' >> ~/.bashrc
```

Você precisará fechar e abrir o Ubuntu para seguir para o próximo passo. Antes de continuar, verifique se o `poetry` foi instalado corretamente, digitando o comando

```
poetry
```

A saída deste comando é o manual de uso da ferramenta. Caso tudo ocorra bem, prossiga para o próximo passo.

## 4. Configurar o git
### Identidade
Por padrão, o Ubuntu 20.04 vem com o git instalado. Desse modo, precisamos apenas realizar a configuração de identidade. Isto é necessário para que cada commit seja identificado de maneira correta. Para realizar a configuração utilize o seguinte comando, substituindo Fulano de Tal pelo seu nome e sobrenome:

```
git config --global user.name "Fulano de Tal"
```

Depois disso, configure o seu email através do seguinte comando, substituindo fulanodetal@exemplo.br pelo seu email:

```
git config --global user.email fulanodetal@exemplo.br
```
!!! tip "Dica"
    Caso erre a configuração, não se preocupe, pois você poderá inseri-la novamente apertando a tecla de movimentação para cima e corrigindo as informações.

### SSH
#### Gerar chave SSH
Além da configuração de identidade, é necessário efetuar geração de chave SSH e adicioná-la ao github. Para isto, abra o terminal e digite o seguinte comando, substituindo o endereço de e-mail pelo seu Github.

```
ssh-keygen -t ed25519 -C "fulanodetal@exemplo.br"
```

Isto cria uma nova chave SSH, usando o nome de e-mail fornecido como uma etiqueta.

Quando aparecer a solicitação "Enter file in which to save the key", pressione ++enter++. O local padrão do arquivo será aceito.

Também será requisitado uma senha para uso da credencial SSH. Caso não queira inserir, apenas aperte ++enter++.

#### Adicionar chave SSH ao Github
Após gerar um par de chaves SSH (uma pública e uma privada), você deve adicionar a chave pública no github para habilitar o acesso SSH para a sua conta.

Vá até o Ubuntu e utilize o seguinte comando para mostrar o conteúdo da chave SSH pública criada anteriormente:

```
cat ~/.ssh/id_ed25519.pub
```

Selecione o texto de saída do comando e copie para área de transferência.

Agora iremos inserir a chave copiada no github. Para isso, você pode dar uma olhada na documentação oficial ou seguir os passos abaixo:

1. Abra o github.com, clique na sua foto de perfil e, em seguida, clique em Settings (ou Configurações):
2. Na seção Access, da barra lateral, clique em SSH and GPG keys (ou Chaves SSH e GPG);
3. Clique em New SSH key (Nova chave SSH) ou Add SSH key (Adicionar chave SSH);
4. No campo Title (Título), adicione um título para sua nova chave;
5. Selecione o tipo de chave "autenticação" no campo Key type (Tipo de chave);
6. Cole sua chave no campo Key;
7. Clique em Add SSH key (adicionar chave SSH). Se solicitado, confirme acesso a sua conta do github.

## Conclusão
Se tudo ocorreu bem, finalizamos a nossa jornada de preparação do ambiente. Agora você está preparado para construir e colaborar com projetos Python de maneira produtiva e eficiente.

!!! tip "Dica"
    Se você quer preparar este ambiente para trabalhar com ciência de dados, o próximo passo é criar um repositório a partir do [seguinte template](https://github.com/aislansf/project-template), e efetuar o clone na sua máquina. Se você tem dúvidas de como criar repositórios a partir de templates, acesse a [documentação oficial](https://docs.github.com/pt/repositories/creating-and-managing-repositories/creating-a-repository-from-a-template) do github.
