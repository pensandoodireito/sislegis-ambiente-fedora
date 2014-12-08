# Ambiente Fedora para execução do SISLEGIS

Você pode utilizar este ambiente de várias formas:

1. Através do Fedora instalado em tua própria máquina;
2. Através do Vagrant (numa máquina com qualquer sistema operacional suportado por ele);

## Clonando e configurando este projeto

```bash
git clone http://github.com/pensandoodireito/sislegis-ambiente-fedora
cd sislegis-ambiente-fedora
cp config.exemplo config
vim config
source config
```

Se estiver utilizando um proxy, configure-o também:
```bash
cp .proxy.exemplo .proxy
vim .proxy
```

## Montando o ambiente através do Vagrant

```bash
vagrant up
vagrant ssh -c /vagrant/instalar
```

## Montando o ambiente numa instalação Fedora

```bash
./instalar
```

## Acessando a aplicação

```
firefox http://localhost:$SISLEGIS_PORT/sislegis
```
