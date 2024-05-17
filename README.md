# gRPC Server
*Antes de mais nada, segue links úteis para o entendimento do gRPC e do ProtoBuffer:*
- [Documentação gRPC para Go](https://grpc.io/docs/languages/go/quickstart/)
- [Documentação ProtoBuff](https://protobuf.dev)

Devemos sempre nos lembrar que gRPC é apenas uma das várias formas de expor 
nossa aplicação. Nesse projeto teremos um servidor web responsável por expor
os contratos de nossas chamadas gRPC a ser consumidas por outros serviços (Nesse
caso, usaremos o Evans como client para consumir nossa aplicação).

## Dev
A nível de desenvolvimento, precisaremos de um compilador do protobuf, o protoc3
para rodar o projeto, e de dois plugins para gerar código boilerplate, sendo um deles o 
responsável por gerar todas as suas entidades a partir do seu protobuf, enquanto
o outro fica responsável por gerar todas as interfaces de comunicação usando
o gRPC.

Para instalar esses plugins, basta rodar os comandos:
>  go install google.golang.org/protobuf/cmd/protoc-gen-go@v1.28

>  go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@v1.2

Para cada chamada criada no arquivo protobuf, você deve rodar os plugins e 
implementar na camada de serviço de sua aplicação métodos que implementem as
interfaces gRPC geradas pelos plugins.

Para rodar os plugins e gerar nosso código boilerplate, basta rodar o comando:
> protoc --go_out=. --go-grpc_out=. proto/<sua_entidade>.proto

## Banco de dados
Para esse projeto, escolhemos trabalhar com um banco em memória como o SQLite3.
Para rodar queries nesse banco, você vai precisar do sqlite3 instalado em sua
máquina, e para isso basta instala-lo com o comando `sudo apt install sqlite3`.
Para acessar os dados desse banco e manipula-lo via terminal, basta rodar `sqlite3
nome_banco.db` e rodar suas queries.

### Migrations (a fins de registro apenas)
`create table categories (id string, name string, description string)`

## Consumindo a aplicação
Para consumir a aplicação, será necessário instalar o [Evans](https://github.com/ktr0731/evans),
um client gRPC de terminal, e escrever o consumo das services desejadas. Para acessar
o evans, basta rodar o comando:
> evans -r repl

> package pb
 
> service <nome_da_service>
 
> call <nome_da_chamada>