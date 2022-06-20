# aula-do-careca-20.06
aula do careca 20.06


drop database dbDistribuidora;
create database dbDistribuidoraLuizeMauricio;
use dbDistribuidoraLuizeMauricio;

create table tbCliente (
	Id int primary key auto_increment,
	Nome varchar(50) not null,
    CEP varchar(8) not null,
    CompEnd varchar(50),
    foreign key (CEP) references tbEndereco(CEP)
);

create table tbClientePF (
	IdCliente int auto_increment,
    foreign key (IdCliente) references tbCliente(Id),
	Cpf varchar(11) not null primary key,
	Rg varchar(8),
	RgDig varchar(1),
    Nasc date
);
create table tbClientePJ (
	IdCliente int auto_increment,
    foreign key (IdCliente) references tbCliente(Id),
	Cnpj varchar(14) not null primary key,
    Ie varchar(9)
);

create table tbUF (
	IdUF int auto_increment primary key,
    UF char(2) unique
);
create table tbBairro (
	IdBairro int auto_increment primary key,
    Bairro varchar(200)
);
create table tbCidade (
	IdCidade int auto_increment primary key,
    Cidade varchar(200)
);

create table tbEndereco (
	CEP varchar(8) primary key,
    Logradouro varchar(200),
    IdBairro int,
    foreign key (IdBairro) references tbBairro(IdBairro),
    IdCidade int,
    foreign key (IdCidade) references tbCidade(IdCidade),
    IdUF int,
    foreign key (IdUF) references tbUF(IdUF)
);

create table tbNotaFiscal (
	NF int primary key,
    TotalNota decimal(7, 2) not null,
    DataEmissao date not null
);

create table tbFornecedor (
	Codigo int primary key auto_increment,
    Cnpj varchar(14) not null,
    Nome varchar(200),
    Telefone varchar(11)
);
create table tbCompra (
	NotaFiscal int primary key,
    DataCompra date not null,
    ValorTotal decimal(8, 2) not null,
    QtdTotal int not null,
    Cod_Fornecedor int,
    foreign key (Cod_Fornecedor) references tbFornecedor(Codigo) 
);

create table tbProduto (
	CodBarras varchar(14) primary key,
	Qtd int,
	Nome varchar(50),
    ValorUnitario decimal(6, 2) not null
);

create table tbItemCompra (
	Qtd int not null,
    ValorItem decimal(6, 2) not null,
    NotaFiscal int,
    CodBarras int,
    primary key (NotaFiscal, CodBarras),
    foreign key (NotaFiscal) references tbCompra(NotaFiscal),
    foreign key (CodBarras) references tbProduto(CodBarras)
);

create table tbVenda (
	IdCliente int,
    foreign key (IdCliente) references tbCliente(Id),
	NumeroVenda int auto_increment primary key,
    DataVenda date not null,
	TotalVenda decimal(7, 2) not null,
    NotaFiscal int,
    foreign key (NotaFiscal) references tbNotaFiscal(NF)
);

create table tbItemVenda (
	NumeroVenda int,
    CodBarras int,
	primary key (NumeroVenda, CodBarras),
    foreign key (NumeroVenda) references tbVenda(NumeroVenda),
    foreign key (CodBarras) references tbProduto(CodBarras),
	Qtd int,
    ValorItem decimal(6, 2)
);

insert into tbFornecedor (CNPJ, Nome, Telefone) values ("1245678937123", "Revenda Chico Loco", "11934567897"),
													   ("1345678937123", "Jose faz tudo s/a", "11934567898"),
                                                       ("1445678937123", "Vadalto entregas", "11934567899"),
                                                       ("1545678937123", "Astrogildo estrelas", "11934567800"),
                                                       ("1645678937123", "Amoroso e doce", "11934567801"),
                                                       ("1745678937123", "Marcelo dedal", "11934567802"),
                                                       ("1845678937123", "franciscano cachaça", "11934567803"),
                                                       ("1945678937123", "Joãozinho chupeta", "11934567804");
                                                       select * from tbFornecedor;
                                                       
-- Exercico 2 -- 

delimiter $$
create procedure spInsert(vCidade varchar(200))
begin
insert into tbCidade(Cidade)
values(vCidade);
end $$

  
call spInsert('São Carlos');
call spInsert('Campinas');
call spInsert('Franco da Rocha');
call spInsert('Osasco');
call spInsert('Pirituba'); 
call spInsert('Lapa');
call spInsert('Ponta Grossa');
 select * from tbCidade;
 
 -- 3 --
 delimiter $$
create procedure spEstadao(vUf char(2))
begin
insert into tbUF(Uf)
values(vUf);
end $$

call spEstadao('SP');
call spEstadao('RJ');
call spEstadao('RS');
select * from tbUF;
 
 -- 4 --
 
delimiter $$
create procedure spbairrao(vbairro varchar(200))
begin
insert into tbBairro(Bairro)
values(vbairro);
end $$

call spbairrao('Aclimação');
call spbairrao('Capão Redondo');
call spbairrao('Pirituba');
call spbairrao('Liberdade');
select * from tbBairro;
 
-- 5 -- 
delimiter $$
create procedure spProd(vCod varchar(14), vNome varchar(50), vValor decimal(6,2), vQtd int)
begin
insert into tbProduto(CodBarras, Nome, ValorUnitario, Qtd )
values(vProduto);
end $$

call spProd("12345678910111", "Rei de Papel Mache", "54.61", "120");

select * from tbProduto;
