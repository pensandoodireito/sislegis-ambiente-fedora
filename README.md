# Ambiente Fedora para execução do SISLEGIS

## Resumo
Este projeto prepara um ambiente Fedora para a execução do SISLEGIS. Esse ambiente pode ser usado de duas formas:

1. Através do Vagrant (numa máquina com qualquer sistema operacional suportado por ele);
2. Através do Fedora instalado em tua própria máquina.

Neste momento, a versão suportada para o Fedora é a 21.

## Preparação

Para utilizar este ambiente, edite o arquivo hosts da tua máquina e adicione o nome ``sislegis.local`` ao ip 127.0.0.1.

Clonando este projeto:

```bash
git clone http://github.com/pensandoodireito/sislegis-ambiente-fedora
cd sislegis-ambiente-fedora
```

Crie o arquivo config a partir do exemplo existente e, se desejar (ou precisar), configure-o. Nesse arquivo de configuração você pode informar, por exemplo, que utilizará um proxy no ambiente. Também poderá configurar as variáveis que referenciam os projetos do sislegis para os teus próprios forks no GitHub, se estiver trabalhando neles ao invés de diretamente nos projetos da organização ``pensandoodireito``. Execute:
```bash
cp config.exemplo config
vim config
```

Se desejar (para carregar algumas variáveis de ambiente), solicite o carregamento do arquivo ``config`` através da inclusão da seguinte linha em teu arquivo ``~/.bashrc``
```
source <CAMINHO ATÉ ESTE PROJETO>/config
```

Se estiver utilizando um proxy, configure-o também:
```bash
cp .proxy.exemplo .proxy
vim .proxy
```

## Montando o ambiente através do Vagrant (alternativa 1)

Instale o plugin vbguest no Vagrant:
```
vagrant plugin install vagrant-vbguest
```

Inicie e provisione a máquina com os seguintes comandos:
```bash
vagrant up
vagrant ssh -c /vagrant/instalar
```

Ao término do provisionamento, faça um reload da máquina com o comando abaixo. Dessa forma, após a reinicialização, se houver alguma atualização de kernel, o plugin ``vagrant-vbguest`` ficará encarregado de instalar o [VirtualBox Guest Additions](https://www.virtualbox.org/manual/ch04.html) nesse novo kernel.
```bash
vagrant reload
```

Para eliminar o(s) kernel(s) antigo(s) e recuperar o espaço em disco ocupado(s) por ele(s), execute:
```bash
vagrant ssh -c 'sudo yum -y install yum-utils; sudo package-cleanup -y --oldkernels --count=1'
```

Após a montagem do ambiente, você pode acessar as URLs relatias a aplicação.

Você pode ter acesso ao ambiente montado executando o seguinte comando:
```bash
vagrant ssh
```

## Montando o ambiente numa instalação do Fedora (alternativa 2)

Execute o script instalar:
```bash
./instalar
```

Após a montagem do ambiente, você pode acessar as URLs relativas a aplicação.

Você pode ter acesso ao ambiente montado executando o seguinte comando:
```
sudo su - sislegis
```

## URLs relativas a aplicação

* Acesso a aplicação: http://sislegis.local:8080
* Acesso a administração do Wildfly: http://sislegis.local:9990
* Acesso a administração dos usuários da aplicação: http://localhost:8180/auth

## Salvando o ambiente

O salvamento do ambiente é interessante de ser realizado para que ele possa ser reconstruído de forma mais ágil numa próxima montagem. Para realizar essa operação, execute o seguinte script dentro do ambiente que estiver senod executado (real ou o virtual executado pelo usuário ``vagrant``):

```bash
./salvar
```

Se o ambiente estiver sendo executado pelo ``vagrant``, também é possível executar o script de salvamente a partir do ``HOST`` da seguinte forma:

```bash
vagrant ssh -c /vagrant/salvar
```

## Problemas conhecidos (e soluções)

### Instalação do plugin vagrant-vbguest no ambiente Windows.

No Windows 8.1, a execução do comando que instala o plugin ``vagrant-vbguest`` apresenta o seguinte problema:

```bash
$ vagrant plugin install vagrant-vbguest
Installing the 'vagrant-vbguest' plugin. This can take a few minutes...
C:/HashiCorp/Vagrant/embedded/lib/ruby/2.0.0/rubygems.rb:517:in `inflate': incorrect header check (Zlib::DataError)
        from C:/HashiCorp/Vagrant/embedded/lib/ruby/2.0.0/rubygems.rb:517:in `inflate'
        from C:/HashiCorp/Vagrant/embedded/gems/gems/bundler-1.7.11/lib/bundler/fetcher.rb:147:in `fetch_spec'
```

A solução definitiva para esse problema ainda não foi depurada.

A solução de contorno é ignorar essa instalação. Para isso, você deve alterar o script ``instalar``, na linha em que é feito o update dos pacotes, para que a atualização do kernel seja ignorada. Essa alteração, após realizada, deverá deixar a linha que contém esse comando escrita da seguinte forma:

```bash
sudo yum -y update --exclude='kernel*'
```
