CREATE SCHEMA IF NOT EXISTS Biblioteca DEFAULT CHARACTER SET utf8mb4 ;
USE Biblioteca ;

-- -----------------------------------------------------
-- Table Biblioteca . aluno
-- -----------------------------------------------------

CREATE TABLE IF NOT EXISTS Biblioteca.aluno (
ra INT AUTO_INCREMENT,
cpf_aluno CHAR(11) NOT NULL,
nome_aluno VARCHAR(45) NOT NULL,
email_aluno VARCHAR(100) NOT NULL,
telefone VARCHAR(45) NOT NULL,
PRIMARY KEY (ra),
UNIQUE INDEX cpf_aluno_UNIQUE (cpf_aluno),
UNIQUE INDEX email_aluno_UNIQUE (email_aluno) 
) ENGINE = InnoDB; -- oferece segurança, consistência e suporte a relacionamentos.--

CREATE TABLE IF NOT EXISTS Biblioteca.colaborador (
cpf_colaborador CHAR(11) NOT NULL,
nome_colaborador VARCHAR(45) NOT NULL,
email_colaborador VARCHAR(100) NOT NULL,
cargo_colaborador VARCHAR(45) NOT NULL,
PRIMARY KEY (cpf_colaborador),
UNIQUE INDEX cpf_colaborador_UNIQUE (cpf_colaborador),
UNIQUE INDEX email_colaborador_UNIQUE (email_colaborador)
) ENGINE = InnoDB; 

CREATE TABLE IF NOT EXISTS Biblioteca.emprestimo (
  id_emprestimo INT NOT NULL AUTO_INCREMENT,
  data_devolucao DATE NOT NULL,
  data_emprestimo DATE NOT NULL,
  isbn CHAR(13) NOT NULL,
  cpf_colaborador CHAR(11) NOT NULL,
  aluno_ra INT NOT NULL,
  PRIMARY KEY (id_emprestimo),
  INDEX fk_emprestimo_aluno1_idx (aluno_ra),
  INDEX fk_emprestimo_colaborador1_idx (cpf_colaborador),
  CONSTRAINT fk_emprestimo_aluno1
    FOREIGN KEY (aluno_ra)
    REFERENCES Biblioteca.aluno (ra)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT fk_emprestimo_colaborador1
    FOREIGN KEY (cpf_colaborador)
    REFERENCES Biblioteca.colaborador (cpf_colaborador)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


CREATE TABLE IF NOT EXISTS Biblioteca.livro (
  isbn CHAR(13) NOT NULL,
  nome_livro VARCHAR(45) NOT NULL,
  autor_livro VARCHAR(45) NOT NULL,
  paginas INT NOT NULL,
  emprestimo_id_emprestimo INT NOT NULL,
  PRIMARY KEY (isbn, emprestimo_id_emprestimo),
  INDEX fk_livro_emprestimo_idx (emprestimo_id_emprestimo), -- Cria um índice na coluna emprestimo_id_emprestimo.-- 
															-- Ajuda o banco a achar rapidamente os livros relacionados a um empréstimo.--
  CONSTRAINT fk_livro_emprestimo -- integridade e consistência
    FOREIGN KEY (emprestimo_id_emprestimo)
    REFERENCES Biblioteca.emprestimo (id_emprestimo)
    ON DELETE NO ACTION -- não permite deletar um empréstimo que tenha livros vinculados.
    ON UPDATE NO ACTION -- não permite alterar um empréstimo que tenha livros vinculados.
) ENGINE = InnoDB; -- Suporta transações, chaves estrangeiras e recuperação automática em caso de falhas.


