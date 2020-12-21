# Docker sem sudo

Por utilizar o docker com grande frequência 0 criar comandos para coisas que não quero instalar no computador. Como assim?  Irá criar uma máquina virtual, aplicando um projeto, configurações diferenciadas. E a partir do shell script chamado entrypoint, coloco em um diretório /bin e não preciso mais me preocupar se esta instalado ou não! Veja o exemplo do script abaixo:

```
#!/bin/sh
echo "Back - Iniciando o Endpoint da Aplicação e setando o dono da pasta storage"
docker run --rm -ti -v $PWD:/app --link=mysql-server AysllaGomes/entrypoint entrypoint $*
```

Agora temos um problema, pois temos que rodar o sudo para executar o script no *Docker*. Então, resolvi buscar me documentação e fóruns para utilizar no meu usuário local para ter a utilização do Docker sem a necessidade de sudo. Segue a call abaixo.

## Segura a Call

1. (Se não existir) crie um grupo para o Docker
`sudo groupadd docker`
2. Adicione o seu usuário no grupo
`sudo usermod -aG docker $USER`
3. Realize o logoff do usuário e o logue novamente
4. Para ver se está dando certo (ah, vá!?) o comando
` docker run hello-world`

Caso apareça o erro...
`WARNING: Error loading config file: /home/user/.docker/config.json - stat /home/user/.docker/config.json: permission denied`

É possível que você já tenha utilizado alguma vez o Docker com este usuário, usando o sudo. Caso seja essa a situação, rode os comandos:
```
sudo chown "$USER":"$USER" /home/"$USER"/.docker -R
sudo chmod g+rwx "$HOME/.docker" -R
```

Esta call eu peguei na página da [documentação do Docker](https://docs.docker.com/engine/install/linux-postinstall/) e ela termina aqui dizendo que tudo funciona!
Porém, entretanto, todavia não foi meu caso. Pois continuou com erro de acesso, como se precisasse usar o sudo para rodar o comando do Docker.
Então achei a solução em um post do AskUbuntu que se resume em rodar mais um comando:
`sudo setfacl -m user:$USER:rw /var/run/docker.sock`

Agora sim, tudo funciona! Se não der certo, reze teu pc mano

#### Status do Projeto: Concluido :heavy_check_mark:
