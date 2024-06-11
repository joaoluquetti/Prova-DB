# Avaliação Final -Modelagem de Banco de Dados Completa


## 1- Cenário
  Banco de dados em SQL para uma Unidade Básica de Saúde (UBS) ou posto de saúde.

  Entidades:
   
   	- Pacientes
    
    - Médicos
    
    - Consultas
    
    - Medicamentos
    
    - Exames
    
    - Prontuários

  Atributos de cada entidade:
  
    - Pacientes:
      ID (chave primária)
      Nome
      Data de Nascimento
      Gênero
      Endereço
      Número de Telefone
      Histórico Médico

    - Médicos:
      ID (chave primária)
      Nome
      Especialidade
      Número de Telefone
      Horário de Trabalho

    - Consultas:
      ID (chave primária)
      ID do Paciente (Chave estrangeira referenciando a tabela de pacientes)
      ID do Médico (Chave estrangeira referenciando a tabela de médicos)
      ID do Medicamento (Chave estrangeira referenciando a tabela de medicamentos)
      Data e Hora da Consulta
      Diagnóstico
      Prescrição Médica

    - Medicamentos:
      ID (chave primária)
      Nome
      Dosagem
      Instruções de Uso

    - Exames:
      ID (chave primária)
      Nome do Exame
      Resultados
      ID do Paciente (chave estrangeira referenciando a tabela de pacientes)
      Data do Exame

    - Prontuários:
      ID (chave primária)
      ID do Paciente (chave estrangeira referenciando a tabela de pacientes)
      Histórico de Consultas
      Histórico de Exames
      Outras Observações

  Relacionamentos:
  
    Um paciente pode ter várias consultas, exames e um prontuário.
    Um médico pode realizar várias consultas.
    Um exame pode estar associado a um paciente específico.
    Um medicamento pode ser prescrito em uma consulta.
    
## 2 - Modelagem Conceitual
<img src="https://raw.githubusercontent.com/joaoluquetti/Prova-DB/main/imagens/Prova DB - Modelo Conceitual.jpeg"/>

## 3 - Modelagem Lógico
<img src="https://raw.githubusercontent.com/joaoluquetti/Prova-DB/main/imagens/Prova DB - Modelo Lógico.jpeg"/>

## 4 - Modelagem Física - Criação das tabelas no SQL Server
	CREATE TABLE Telefone_Paciente(
    		ID_Telefone     INT PRIMARY KEY IDENTITY,
    		Telefone        VARCHAR(20)
	);

	CREATE TABLE Paciente(
    		ID_Paciente     INT PRIMARY KEY IDENTITY,
    		Nome_Paciente   VARCHAR(100),
    		Data_Nasc       DATE,
    		Genero          VARCHAR(10),
    		Telefone_ID     INT,
    		Rua             VARCHAR(60),
    		Numero          INT,
    		Cidade          VARCHAR(30),
    		CEP             INT,
    		UF              VARCHAR(2),
    		Hist_Medico     TEXT,
    		FOREIGN KEY (Telefone_ID) REFERENCES Telefone_Paciente(ID_Telefone)
	);

	CREATE TABLE Telefone_Medico(
    		ID_Telefone INT PRIMARY KEY,
    		Telefone    VARCHAR(20)
	);

	CREATE TABLE Medico(
    		ID_Medico       INT PRIMARY KEY IDENTITY,
    		Nome_Medico     VARCHAR(100),
    		Especialidade   VARCHAR(100),
    		Horario_Trab    VARCHAR(100),
    		Telefone_ID     INT,
    		FOREIGN KEY (Telefone_ID) REFERENCES Telefone_Medico(ID_Telefone)
	);

	CREATE TABLE Medicamento(
		ID_Medicamento      INT PRIMARY KEY IDENTITY,
    		Nome_Medicamento    VARCHAR(100),
    		Dosagem             VARCHAR(50),
    		Instrucao           TEXT,
	);

	CREATE TABLE Consulta(
    		ID_Consulta         INT PRIMARY KEY IDENTITY,
    		Paciente_ID         INT,
    		Medico_ID           INT,
    		Medicamento_ID      INT,
    		Data_Hora_Consulta  DATETIME,
    		Prescricao          TEXT,
    		Diagnostico         TEXT,
    		FOREIGN KEY (Paciente_ID) REFERENCES Paciente(ID_Paciente),
    		FOREIGN KEY (Medico_ID) REFERENCES Medico(ID_Medico),
    		FOREIGN KEY (Medicamento_ID) REFERENCES Medicamento(ID_Medicamento)
	);

	CREATE TABLE Prontuario(
    		ID_Prontuario   INT PRIMARY KEY IDENTITY,
    		Paciente_ID     INT,
    		Hist_Consulta   TEXT,
    		Hist_Exames     TEXT,
    		FOREIGN KEY (Paciente_ID) REFERENCES Paciente(ID_Paciente)
	);

	CREATE TABLE Exame(
    		ID_Exame        INT PRIMARY KEY IDENTITY,
    		Paciente_ID     INT,
    		Nome_Exame      VARCHAR(100),
    		Resultado       TEXT,
    		Data_Exame      DATE,
    		FOREIGN KEY (Paciente_ID) REFERENCES Paciente(ID_Paciente)
	);


## 5 - Inserção de Dados

	INSERT INTO Telefone_Paciente (Telefone) VALUES
	('21987654321'), ('21987654322'), ('21987654323'), ('21987654324'), ('21987654325'),
	('21987654326'), ('21987654327'), ('21987654328'), ('21987654329'), ('21987654330'),
	('21987654331'), ('21987654332'), ('21987654333'), ('21987654334'), ('21987654335'),
	('21987654336'), ('21987654337'), ('21987654338'), ('21987654339'), ('21987654340');

	INSERT INTO Paciente (Nome_Paciente, Data_Nasc, Genero, Telefone_ID, Rua, Numero, Cidade, CEP, UF, Hist_Medico) VALUES
	('João Silva', '1980-01-01', 'M', 1, 'Rua A', 100, 'Rio de Janeiro', 20000000, 'RJ', 'Diabetes'),
	('Maria Souza', '1985-02-02', 'F', 2, 'Rua B', 200, 'São Paulo', 30000000, 'SP', 'Hipertensão'),
	('Carlos Oliveira', '1990-03-03', 'M', 3, 'Rua C', 300, 'Belo Horizonte', 40000000, 'MG', 'Asma'),
	('Ana Pereira', '1995-04-04', 'F', 4, 'Rua D', 400, 'Curitiba', 50000000, 'PR', 'Hipotireoidismo'),
	('Lucas Fernandes', '2000-05-05', 'M', 5, 'Rua E', 500, 'Porto Alegre', 60000000, 'RS', 'Obesidade'),
	('Fernanda Lima', '1975-06-06', 'F', 6, 'Rua F', 600, 'Salvador', 70000000, 'BA', 'Depressão'),
	('Rafael Almeida', '1982-07-07', 'M', 7, 'Rua G', 700, 'Fortaleza', 80000000, 'CE', 'Anemia'),
	('Bruna Santos', '1988-08-08', 'F', 8, 'Rua H', 800, 'Brasília', 90000000, 'DF', 'Enxaqueca'),
	('Marcelo Gomes', '1993-09-09', 'M', 9, 'Rua I', 900, 'Manaus', 10000000, 'AM', 'Gastrite'),
	('Juliana Costa', '1997-10-10', 'F', 10, 'Rua J', 1000, 'Recife', 11000000, 'PE', 'Bronquite'),
	('André Nunes', '1981-11-11', 'M', 11, 'Rua K', 1100, 'Belém', 12000000, 'PA', 'Sinusite'),
	('Carla Ribeiro', '1987-12-12', 'F', 12, 'Rua L', 1200, 'Goiânia', 13000000, 'GO', 'Artrite'),
	('Leonardo Rocha', '1992-01-13', 'M', 13, 'Rua M', 1300, 'Florianópolis', 14000000, 'SC', 'Rinite'),
	('Renata Mendes', '1996-02-14', 'F', 14, 'Rua N', 1400, 'Campo Grande', 15000000, 'MS', 'Psoríase'),
	('Rodrigo Barbosa', '1983-03-15', 'M', 15, 'Rua O', 1500, 'Maceió', 16000000, 'AL', 'Lúpus'),
	('Patrícia Dias', '1979-04-16', 'F', 16, 'Rua P', 1600, 'Natal', 17000000, 'RN', 'Dermatite'),
	('Gustavo Araujo', '1991-05-17', 'M', 17, 'Rua Q', 1700, 'João Pessoa', 18000000, 'PB', 'Eczema'),
	('Sabrina Freitas', '1994-06-18', 'F', 18, 'Rua R', 1800, 'Aracaju', 19000000, 'SE', 'Síndrome do Pânico'),
	('Henrique Martins', '1986-07-19', 'M', 19, 'Rua S', 1900, 'Palmas', 20000000, 'TO', 'Câncer de pele'),
	('Letícia Carvalho', '1999-08-20', 'F', 20, 'Rua T', 2000, 'Teresina', 21000000, 'PI', 'Fibromialgia');

	INSERT INTO Telefone_Medico (ID_Telefone, Telefone) VALUES
	(1, '21987654341'), (2, '21987654342'), (3, '21987654343'), (4, '21987654344'), (5, '21987654345'),
	(6, '21987654346'), (7, '21987654347'), (8, '21987654348'), (9, '21987654349'), (10, '21987654350'),
	(11, '21987654351'), (12, '21987654352'), (13, '21987654353'), (14, '21987654354'), (15, '21987654355'),
	(16, '21987654356'), (17, '21987654357'), (18, '21987654358'), (19, '21987654359'), (20, '21987654360');

	INSERT INTO Medico (Nome_Medico, Especialidade, Horario_Trab, Telefone_ID) VALUES
	('Dr. Antonio Lima', 'Cardiologista', '08:00-12:00', 1),
	('Dra. Beatriz Silva', 'Dermatologista', '13:00-17:00', 2),
	('Dr. Carlos Mendes', 'Pediatra', '08:00-12:00', 3),
	('Dra. Daniela Souza', 'Neurologista', '13:00-17:00', 4),
	('Dr. Eduardo Santos', 'Ortopedista', '08:00-12:00', 5),
	('Dra. Fernanda Costa', 'Oftalmologista', '13:00-17:00', 6),
	('Dr. Gustavo Rocha', 'Endocrinologista', '08:00-12:00', 7),
	('Dra. Helena Araujo', 'Ginecologista', '13:00-17:00', 8),
	('Dr. Igor Fernandes', 'Psiquiatra', '08:00-12:00', 9),
	('Dra. Julia Pereira', 'Nutricionista', '13:00-17:00', 10),
	('Dr. Kleber Nunes', 'Urologista', '08:00-12:00', 11),
	('Dra. Luana Dias', 'Hematologista', '13:00-17:00', 12),
	('Dr. Marcelo Freitas', 'Reumatologista', '08:00-12:00', 13),
	('Dra. Natalia Ribeiro', 'Infectologista', '13:00-17:00', 14),
	('Dr. Otávio Martins', 'Nefrologista', '08:00-12:00', 15),
	('Dra. Priscila Gomes', 'Pneumologista', '13:00-17:00', 16),
	('Dr. Ricardo Alves', 'Gastroenterologista', '08:00-12:00', 17),
	('Dra. Simone Carvalho', 'Fisioterapeuta', '13:00-17:00', 18),
	('Dr. Thiago Barbosa', 'Cirurgião', '08:00-12:00', 19),
	('Dra. Vanessa Lima', 'Oncologista', '13:00-17:00', 20);

	INSERT INTO Medicamento (Nome_Medicamento, Dosagem, Instrucao) VALUES
	('Paracetamol', '500mg', 'Tomar 1 comprimido a cada 8 horas'),
	('Ibuprofeno', '400mg', 'Tomar 1 comprimido a cada 6 horas'),
	('Amoxicilina', '500mg', 'Tomar 1 cápsula a cada 8 horas por 7 dias'),
	('Lorazepam', '2mg', 'Tomar 1 comprimido antes de dormir'),
	('Omeprazol', '20mg', 'Tomar 1 cápsula em jejum'),
	('Cetirizina', '10mg', 'Tomar 1 comprimido ao dia'),
	('Losartana', '50mg', 'Tomar 1 comprimido ao dia'),
	('Atorvastatina', '20mg', 'Tomar 1 comprimido ao dia'),
	('Metformina', '850mg', 'Tomar 1 comprimido duas vezes ao dia'),
	('Levotiroxina', '50mcg', 'Tomar 1 comprimido ao dia em jejum'),
	('Simeticona', '125mg', 'Tomar 1 cápsula após as refeições'),
	('Aspirina', '100mg', 'Tomar 1 comprimido ao dia'),
	('Azitromicina', '500mg', 'Tomar 1 comprimido ao dia por 3 dias'),
	('Dipirona', '500mg', 'Tomar 1 comprimido a cada 6 horas'),
	('Prednisona', '20mg', 'Tomar 1 comprimido ao dia'),
	('Hidroxicloroquina', '400mg', 'Tomar 1 comprimido ao dia'),
	('Vitamina D', '1000UI', 'Tomar 1 cápsula ao dia'),
	('Captopril', '25mg', 'Tomar 1 comprimido ao dia'),
	('Fluoxetina', '20mg', 'Tomar 1 cápsula ao dia'),
	('Clonazepam', '2mg', 'Tomar 1 comprimido antes de dormir');

	INSERT INTO Consulta (Paciente_ID, Medico_ID, Medicamento_ID, Data_Hora_Consulta, Prescricao, Diagnostico) VALUES
	(2, 1, 1, '2023-01-01 08:00:00', 'Paracetamol', 'Gripe'),
	(3, 2, 2, '2023-01-02 09:00:00', 'Ibuprofeno', 'Dor de cabeça'),
	(4, 3, 3, '2023-01-03 10:00:00', 'Amoxicilina', 'Infecção de garganta'),
	(5, 4, 4, '2023-01-04 11:00:00', 'Lorazepam', 'Ansiedade'),
	(6, 5, 5, '2023-01-05 08:00:00', 'Omeprazol', 'Refluxo'),
	(7, 6, 6, '2023-01-06 09:00:00', 'Cetirizina', 'Alergia'),
	(8, 7, 7, '2023-01-07 10:00:00', 'Losartana', 'Hipertensão'),
	(9, 8, 8, '2023-01-08 11:00:00', 'Atorvastatina', 'Colesterol alto'),
	(10, 9, 9, '2023-01-09 08:00:00', 'Metformina', 'Diabetes'),
	(11, 10, 10, '2023-01-10 09:00:00', 'Levotiroxina', 'Hipotireoidismo'),
	(12, 11, 11, '2023-01-11 10:00:00', 'Simeticona', 'Gases'),
	(13, 12, 12, '2023-01-12 11:00:00', 'Aspirina', 'Prevenção de AVC'),
	(14, 13, 13, '2023-01-13 08:00:00', 'Azitromicina', 'Infecção respiratória'),
	(15, 14, 14, '2023-01-14 09:00:00', 'Dipirona', 'Dor muscular'),
	(16, 15, 15, '2023-01-15 10:00:00', 'Prednisona', 'Inflamação'),
	(17, 16, 16, '2023-01-16 11:00:00', 'Hidroxicloroquina', 'Artrite reumatoide'),
	(18, 17, 17, '2023-01-17 08:00:00', 'Vitamina D', 'Deficiência de Vitamina D'),
	(19, 18, 18, '2023-01-18 09:00:00', 'Captopril', 'Pressão alta'),
	(20, 19, 19, '2023-01-19 10:00:00', 'Fluoxetina', 'Depressão'),
	(21, 20, 20, '2023-01-20 11:00:00', 'Clonazepam', 'Insônia');

	INSERT INTO Prontuario (Paciente_ID, Hist_Consulta, Hist_Exames) VALUES
	(2, 'Consulta 01/01/2023: Gripe', 'Exame de Sangue'),
	(3, 'Consulta 02/01/2023: Dor de cabeça', 'Exame de Urina'),
	(4, 'Consulta 03/01/2023: Infecção de garganta', 'Exame de Garganta'),
	(5, 'Consulta 04/01/2023: Ansiedade', 'Exame de Sangue'),
	(6, 'Consulta 05/01/2023: Refluxo', 'Endoscopia'),
	(7, 'Consulta 06/01/2023: Alergia', 'Exame de Pele'),
	(8, 'Consulta 07/01/2023: Hipertensão', 'Exame de Sangue'),
	(9, 'Consulta 08/01/2023: Colesterol alto', 'Exame de Sangue'),
	(10, 'Consulta 09/01/2023: Diabetes', 'Exame de Sangue'),
	(11, 'Consulta 10/01/2023: Hipotireoidismo', 'Exame de Sangue'),
	(12, 'Consulta 11/01/2023: Gases', 'Exame de Fezes'),
	(13, 'Consulta 12/01/2023: Prevenção de AVC', 'Exame de Sangue'),
	(14, 'Consulta 13/01/2023: Infecção respiratória', 'Exame de Sangue'),
	(15, 'Consulta 14/01/2023: Dor muscular', 'Exame de Sangue'),
	(16, 'Consulta 15/01/2023: Inflamação', 'Exame de Sangue'),
	(17, 'Consulta 16/01/2023: Artrite reumatoide', 'Exame de Sangue'),
	(18, 'Consulta 17/01/2023: Deficiência de Vitamina D', 'Exame de Sangue'),
	(19, 'Consulta 18/01/2023: Pressão alta', 'Exame de Sangue'),
	(20, 'Consulta 19/01/2023: Depressão', 'Exame de Sangue'),
	(21, 'Consulta 20/01/2023: Insônia', 'Exame de Sangue');

	INSERT INTO Exame (Paciente_ID, Nome_Exame, Resultado, Data_Exame) VALUES
	(2, 'Exame de Sangue', 'Normal', '2023-01-01'),
	(3, 'Exame de Urina', 'Normal', '2023-01-02'),
	(4, 'Exame de Garganta', 'Infecção detectada', '2023-01-03'),
	(5, 'Exame de Sangue', 'Normal', '2023-01-04'),
	(6, 'Endoscopia', 'Refluxo detectado', '2023-01-05'),
	(7, 'Exame de Pele', 'Alergia detectada', '2023-01-06'),
	(8, 'Exame de Sangue', 'Pressão alta detectada', '2023-01-07'),
	(9, 'Exame de Sangue', 'Colesterol alto detectado', '2023-01-08'),
	(10, 'Exame de Sangue', 'Diabetes detectado', '2023-01-09'),
	(11, 'Exame de Sangue', 'Hipotireoidismo detectado', '2023-01-10'),
	(12, 'Exame de Fezes', 'Normal', '2023-01-11'),
	(13, 'Exame de Sangue', 'Normal', '2023-01-12'),
	(14, 'Exame de Sangue', 'Infecção detectada', '2023-01-13'),
	(15, 'Exame de Sangue', 'Normal', '2023-01-14'),
	(16, 'Exame de Sangue', 'Inflamação detectada', '2023-01-15'),
	(17, 'Exame de Sangue', 'Artrite detectada', '2023-01-16'),
	(18, 'Exame de Sangue', 'Deficiência de Vitamina D detectada', '2023-01-17'),
	(19, 'Exame de Sangue', 'Pressão alta detectada', '2023-01-18'),
	(20, 'Exame de Sangue', 'Normal', '2023-01-19'),
	(21, 'Exame de Sangue', 'Normal', '2023-01-20');

## 6 - CRUD


