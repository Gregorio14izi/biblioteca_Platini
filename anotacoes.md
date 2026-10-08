-- 1-Criando as tabelas

CREATE TABLE alunos (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(100) NOT NULL
);

CREATE TABLE emprestimos (
    id SERIAL PRIMARY KEY,
    livro VARCHAR(150) NOT NULL,
    id_aluno INT REFERENCES alunos(id)
);

-- Inserção de 15 alunos
INSERT INTO alunos (nome) VALUES 
('Alice Mendes'),
('Bernardo Rocha'),
('Camila Alencar'),
('Davi Castro'),
('Eva Martins'),
('Felipe Nogueira'),
('Giovanna Prado'),
('Heitor Vasconcelos'),
('Isadora Duarte'),
('Jonas Freitas'),
('Kátia Xavier'),
('Luan Cardoso'),
('Mirella Farias'),
('Natan Aguiar'),
('Olga Moreira');

-- Inserção de 10 empréstimos (associados aos alunos de ID 1 a 10, deixando os alunos 11 a 15 sem livro)
INSERT INTO emprestimos (livro, id_aluno) VALUES 
('O Pequeno Príncipe', 1),
('Dom Casmurro', 2),
('Harry Potter e a Pedra Filosofal', 3),
('O Hobbit', 4),
('A Hora da Estrela', 5),
('Capitães da Areia', 6),
('O Menino do Pijama Listrado', 7),
('Percy Jackson e o Ladrão de Raios', 8),
('1984', 9),
('A Revolução dos Bichos', 10);

-- Consulta para ver todos os alunos
SELECT * FROM alunos;

-- Consulta para ver todos os empréstimos
SELECT * FROM emprestimos;

-- Mostrando o nome do aluno e o livro que ele pegou
SELECT alunos.nome, emprestimos.livro 
FROM emprestimos 
INNER JOIN alunos ON emprestimos.id_aluno = alunos.id;

-- Os alunos de ID 11 a 15 (Kátia Xavier, Luan Cardoso, Mirella Farias, Natan Aguiar e Olga Moreira).
-- Por quê? O INNER JOIN traz apenas as linhas que possuem correspondência em ambas as tabelas. Como esses 5 alunos não têm nenhum registro correspondente na tabela de emprestimos, eles são ignorados na consulta.

-- Mostrando todos os alunos e o que cada um pegou até os que não pegaram nem um
SELECT alunos.nome, emprestimos.livro 
FROM alunos 
LEFT JOIN emprestimos ON emprestimos.id_aluno = alunos.id;

-- Apareceu o valor NULL (nulo), indicando ausência de dados/informação para aquele campo específico.

-- Mostrando quem nunca pegou livro
SELECT alunos.nome, emprestimos.livro 
FROM alunos 
LEFT JOIN emprestimos ON emprestimos.id_aluno = alunos.id 
WHERE emprestimos.id_aluno IS NULL;

-- Tentando inserir um empréstimo para o aluno 50
INSERT INTO emprestimos (livro, id_aluno) VALUES ('Turma da Mônica', 50);

-- O que aconteceu? O banco de dados gerou um erro de restrição de chave estrangeira e a inserção foi bloqueada.
-- Por quê? Como usamos a tabela de alunos como referência para o (id_aluno), e na tabela alunos não tem um aluno com o id 50, o banco de dados impede que um empréstimo seja criado por um usuário inexistente.