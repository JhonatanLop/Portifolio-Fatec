# Portfólio APIs

Trabalho de Aprendizagem a partir de Projeto Integrador (APIs), apresentado à Faculdade de Tecnologia de São José dos Campos do curso de Banco de Dados 3º Semestre/2022.

## Sobre mim

*Sou um desenvolvedor Back-End com foco em Banco de Dados e APIs, atualmente cursando Banco de Dados na FATEC São José dos Campos. Minha trajetória na área começou com minha formação técnica em Informática pela Etec de Jaú, onde tive meu primeiro contato com programação e bancos de dados.

Minha experiência acadêmica tem sido marcada por projetos práticos e colaborativos, especialmente no desenvolvimento de sistemas completos por meio da metodologia de Aprendizagem por Projeto Integrador (API). Tenho interesse em soluções eficientes para armazenamento e manipulação de dados, sempre buscando aprimorar minhas habilidades técnicas e contribuir para projetos de alto impacto.*

<div align="center">•
    <a href="https://github.com/JhonatanLop/JhonatanLop"> Github </a> •
    <a href="https://linkedin.com/in/jhonatan-o-lopes"> Linkedin </a>•
</div>

---
## Ferramenta de Avaliação 360º

**1º semestre • 2022**  
[Repositório GitHub](https://github.com/JhonatanLop/api1/blob/main/README.md)  
**Parceiro Acadêmico:** [PBLTeX](https://2rpnet.com.br/)

### Prévia da Solução

Desenvolvemos uma solução computacional que possibilita a aplicação da técnica de Avaliação 360° e a análise dos dados coletados de alunos e instrutores da instituição de ensino PBLTeX, especializada em metodologias de ensino baseadas em problemas (PBL — *Problem Based Learning*).

Utilizamos a linguagem Python como base do projeto, devido à sua simplicidade e eficiência, o que facilitou a implementação das funcionalidades necessárias para coleta e análise de dados. A interface foi desenvolvida utilizando a biblioteca Tkinter.

Este foi meu primeiro projeto acadêmico, portanto, possui um caráter mais experimental e iniciante. Enfrentei muitos desafios técnicos e de organização, mas cada obstáculo superado contribuiu significativamente para meu aprendizado e desenvolvimento profissional. Essa experiência foi essencial para consolidar minhas bases na programação.

### Tecnologias Utilizadas

- [Python 3](https://www.python.org/downloads/)
- [Tkinter](https://docs.python.org/pt-br/3/library/tkinter.html)

### Contribuições Pessoais

- Implementei parte do controle de permissões de acesso, garantindo que apenas usuários autorizados pudessem realizar determinadas ações no sistema.
- Atuei também no desenvolvimento de módulos da interface gráfica, contribuindo para a construção dos dashboards do sistema.

### Lições Aprendidas

Como foi minha primeira vez atuando como *Scrum Master* e trabalhando com Python, tive bastante dificuldade em compreender e definir as tarefas, bem como em entender a lógica por trás das soluções. Também fazia muito tempo desde meu último contato com programação, então precisei reaprender tanto os fundamentos de lógica quanto as particularidades da linguagem.

#### Hard Skills

- Metodologias Ágeis<br>
  Foi meu primeiro contato com metodologias ágeis e entendo os passos do scrum, aplicar isso pela primeira vez foi muito bom, mesmo que não perfeito.

#### Soft Skills

- Trabalho em Equipe<br>
  Pela primeira vez tive que aprender como trabalhar em uma equipe de desenvolvimento. Foi um desafio coordenar as entregas com os outros membros e manter um padrão de código.

- Liderança e Gestão de Equipe<br>
  Atuei como *Scrum Master*, aprendendo na prática como guiar e organizar o time. Como uma pessoa tímida foi muito difícil sair da minha zona de conforto para assumir esse tipo de papél no time.

---

## Projeto 2 — Ferramenta para Controle de Horas Extras e Sobreavisos

**2º semestre • 2022**  
[Repositório GitHub](https://github.com/projetoKhali/API2Semestre/blob/main/README.md)  
**Parceiro Acadêmico:** [2RP Net](https://2rpnet.com.br/)

### Prévia da Solução

O objetivo do projeto foi desenvolver um sistema para controle de jornada de trabalho dos colaboradores da 2RP Net, com foco na classificação automatizada de horas extras e sobreavisos.

A solução permite que colaboradores registrem seus horários de entrada e saída e classifiquem essas horas, com base em regras de negócio configuradas previamente por administradores. A aplicação também oferece a geração de relatórios e estatísticas sobre o tempo trabalhado.

No back-end, criamos uma lógica que processa os apontamentos enviados pelos usuários, realiza a classificação de acordo com regras específicas, e persiste os dados já tratados no banco de dados PostgreSQL. O sistema também permite exportar relatórios em `.csv`, com a opção de filtrar colunas de acordo com o interesse do usuário.

### Tecnologias Utilizadas

#### Back-End

- [Java 17](https://www.oracle.com/br/java/technologies/downloads/#jdk17-windows)
- [Docker](https://www.docker.com/) com [Docker Compose](https://docs.docker.com/compose/)
- [PostgreSQL 15](https://www.postgresql.org/)

#### Front-End

- [JavaFX](https://www.oracle.com/br/java/technologies/javase/javafx-overview.html)
- [SceneBuilder](https://www.oracle.com/java/technologies/javase/javafxscenebuilder-info.html)

---

### Contribuições Pessoais

#### Criptografia de Senhas

Implementei a criptografia das senhas dos usuários utilizando o algoritmo MD5. O objetivo era garantir segurança, evitando que senhas em texto puro fossem salvas no banco.

Além da função de criptografia, adaptei o fluxo de autenticação para validar senhas criptografadas.

<details>
<summary>Código - Criptografia</summary>

```java
public static String encode(String input) {
    try {
        MessageDigest md = MessageDigest.getInstance("MD5");

        byte[] bytes = input.getBytes();
        byte[] digest = md.digest(bytes);

        StringBuilder sb = new StringBuilder();
        for (byte b : digest) {
                sb.append(String.format("%02x", b));
        }

        return sb.toString();
    } catch (NoSuchAlgorithmException e) {
        e.printStackTrace();
        return null;
    }
}
```

```java
@PostMapping
public User createUser(@RequestBody User user) {    
    user.setPassword(Cryptography.encode(user.getRegistration()));
    return userRepository.save(user);
}

@PostMapping("/login")
public User login(@RequestBody LoginRequest loginRequest) {
    return userService.getValidatedUser(loginRequest.getEmail(), Cryptography.encode(loginRequest.getPassword()));
}
```
</details>

<br>

#### Modelagem e Implementação do Banco de Dados
Fui responsável pela modelagem e implementação do banco de dados. Utilizamos PostgreSQL em contêiner Docker com Docker Compose, o que garantiu portabilidade e agilidade no desenvolvimento.

<details>
<summary>Código - Estrutura das Tabelas</summary>

```sql
-- apontamento
CREATE TABLE IF NOT EXISTS public.apontamento(
    id serial NOT NULL,
    hora_inicio TIMESTAMP null,
    hora_fim TIMESTAMP null,
    usr_id INT NULL,
    projeto VARCHAR NULL,
    clt_id INT NULL,
    tipo BOOLEAN NULL,
    justificativa VARCHAR NULL,
    cr_id INT NULL,
    aprovacao INT DEFAULT 0,
    feedback VARCHAR NULL,
    CONSTRAINT apontamento_pkey PRIMARY KEY (id)
);
-- usuário
CREATE TABLE IF NOT EXISTS public.usuario(
    id serial NOT NULL,
    nome VARCHAR NULL,
    email VARCHAR NOT NULL UNIQUE,
    senha TEXT NULL,
    perfil INT NOT NULL,
    matricula VARCHAR NULL,
    CONSTRAINT usuario_pkey PRIMARY KEY (id)
);
```
</details>

<br>

#### Extração de relatório
Implementei a geração de relatórios `.csv` com apontamentos realizados no sistema. Adicionalmente, desenvolvi um filtro para seleção de colunas, permitindo personalização dos dados exportados.

<details> 
<summary>Código - Exportação de Relatório CSV</summary>

```java
try (PrintWriter writer = response.getWriter()) {
    CSVWriter csvWriter = new CSVWriter(writer);

    List<String> header = new ArrayList<>();

    for (int i = 0; i < headers.length; i++) { if (camposBoolean[i]) { header.add(headers[i]); }}

    csvWriter.writeNext(header.toArray(String[]::new));

    List<String> data = new ArrayList<>();
    for (Appointment apt : allAppointments) {
        Timestamp total = new Timestamp(apt.getEndDate().getTime() - apt.getStartDate().getTime());
        if (camposBoolean[1]) data.add(apt.getUser().getRegistration());
        if (camposBoolean[2]) data.add(apt.getUser().getName());
        if (camposBoolean[3]) data.add(apt.getStartDate().toString());
        if (camposBoolean[4]) data.add(apt.getEndDate().toString());
        if (camposBoolean[5]) data.add(total.toString());
        if (camposBoolean[6]) data.add(apt.getType().toString());
        if (camposBoolean[7]) data.add(apt.getResultCenter().getName());
        if (camposBoolean[8]) data.add(apt.getClient().getName());
        if (camposBoolean[9]) data.add(apt.getProject().getName());
        if (camposBoolean[10]) data.add(apt.getJustification());

        csvWriter.writeNext(data.toArray(new String[0]));
    }

    csvWriter.close();
}
```
</details>

<br>

#### CRUD de Projetos
Implementei os endpoints REST do módulo de Projetos, que permite cadastrar, editar, excluir e listar os projetos disponíveis para apontamentos.

<details> 
<summary>Código - CRUD de Projetos</summary>

```java
@RestController
@RequestMapping("/projects")
public class ProjectController {

    @Autowired private ProjectRepository projectRepository;

    public ProjectController(ProjectRepository projectRepository) {
        this.projectRepository = projectRepository;
    }

    @PostMapping
    public Project createProject(@RequestBody Project project) {
        return projectRepository.save(project);
    }

    @GetMapping()
    public List<Project> getAllProjects(){
        return projectRepository.findAllActiveProjects();
    }
}
```
</details>

### Lições Aprendidas

Este foi o primeiro projeto em que atuei exclusivamente como desenvolvedor, o que me permitiu focar 100% na codificação das funcionalidades e no aprendizado técnico mais profundo.

#### Hard Skills
- Java & OOP:<br>
Primeiro projeto utilizando Java e programação orientada a objetos. Aprendi desde a sintaxe da linguagem até padrões básicos de projeto e boas práticas. Também tive contato com o framework JavaFX para desenvolvimento desktop.

- Modelagem de Banco de Dados:<br>
Fui responsável por toda a estruturação do banco relacional. Aprofundei conhecimentos em normalização, integridade referencial e design de tabelas.

- Docker & PostgreSQL:<br>
Aprendi a utilizar docker, nesse projetos utilizamos o banco de dados postgres em conteiner usando docker-compose.

### Soft Skills
- Trabalho em Equipe e Comunicação:<br>
Sainda de um papel de liderança para apenas ser um desenvolvidor melhorei minha comunicação com os colegas, comecei a reportar status, esclarecer dúvidas e contribuir com sugestões, já que meu foco nesse projeto foi o desenvolvimento.

---

## Projeto 3 — Ferramenta para Controle de Horas Extras e Sobreavisos (Reformulação)

**3º semestre • 2022**  
[Repositório GitHub](https://github.com/projetoKhali/api3/blob/main/README.md)  
**Parceiro Acadêmico:** [2RP Net](https://2rpnet.com.br/)

### Prévia da Solução

Este projeto foi uma evolução da solução desenvolvida no semestre anterior, com o objetivo de migrar o software para o ambiente web e refinar o controle da jornada de trabalho de colaboradores da 2RP Net. O sistema continuou oferecendo funcionalidades de registro de horários, classificação automática de horas extras e sobreavisos, além da geração de relatórios e estatísticas.

As APIs de registro de horas se comunicam com uma central de processamento que aplica regras de negócio configuráveis para classificar os registros. Os dados são então persistidos em um banco PostgreSQL.

Desta vez, o projeto contou com uma stack mais moderna no front-end, com React e TypeScript, e com uso do Spring Boot no back-end.

###  Tecnologias Utilizadas

#### Back-End

- [Java 17](https://www.oracle.com/br/java/technologies/downloads/#jdk17-windows)
- [Spring Boot](https://spring.io/)
- [Docker](https://www.docker.com/) + [Docker Compose](https://docs.docker.com/compose/)
- [PostgreSQL 15](https://www.postgresql.org/)

#### Front-End

- [React](https://react.dev/)
- [TypeScript](https://www.typescriptlang.org)

### Contribuições Pessoais

Neste projeto, atuei como **Product Owner (PO)**, papel no qual fui responsável por organizar as demandas junto ao cliente, definir prioridades, escrever histórias de usuário e acompanhar a entrega técnica do time.

Mesmo como PO, contribuí diretamente com o desenvolvimento back-end em pontos específicos, como segurança, API de projetos e relatórios.

#### Extração de Relatórios

A exportação de relatórios foi um requisito no projeto, na aplicação fizemos um relatório customizado de acordo com as necessidades. Nesse relatório é possível escolher quais colunas exportar.

<details>
<summary>Código - Exportação de Relatório CSV</summary>

```java
try (PrintWriter writer = response.getWriter()) {
    CSVWriter csvWriter = new CSVWriter(writer);

    List<String> header = new ArrayList<>();

    for (int i = 0; i < headers.length; i++) { 
        if (camposBoolean[i]) { 
            header.add(headers[i]); 
        }
    }

    csvWriter.writeNext(header.toArray(new String[0]));

    List<String> data = new ArrayList<>();
    for (Appointment apt : allAppointments) {
        Timestamp total = new Timestamp(apt.getEndDate().getTime() - apt.getStartDate().getTime());
        if (camposBoolean[1]) data.add(apt.getUser().getRegistration());
        if (camposBoolean[2]) data.add(apt.getUser().getName());
        if (camposBoolean[3]) data.add(apt.getStartDate().toString());
        if (camposBoolean[4]) data.add(apt.getEndDate().toString());
        if (camposBoolean[5]) data.add(total.toString());
        if (camposBoolean[6]) data.add(apt.getType().toString());
        if (camposBoolean[7]) data.add(apt.getResultCenter().getName());
        if (camposBoolean[8]) data.add(apt.getClient().getName());
        if (camposBoolean[9]) data.add(apt.getProject().getName());
        if (camposBoolean[10]) data.add(apt.getJustification());

        csvWriter.writeNext(data.toArray(new String[0]));
    }

    csvWriter.close();
}
```
</details>

#### CRUD de Projetos

Fiquei responsável pela criação de alguns controlers dentro do projeto, este foi um deles. Esse controler especificamente lida com o CRUD de Projetos.

<details> <summary>Código - Controller de Projeto</summary>

```java
@RestController
@RequestMapping("/projects")
public class ProjectController {

    @Autowired 
    private ProjectRepository projectRepository;

    public ProjectController(ProjectRepository projectRepository) {
        this.projectRepository = projectRepository;
    }

    @PostMapping
    public Project createProject(@RequestBody Project project) {
        return projectRepository.save(project);
    }

    @GetMapping()
    public List<Project> getAllProjects(){
        return projectRepository.findAllActiveProjects();
    }
}
```
</details>

### Lições Aprendidas
Este projeto foi uma grande virada no meu desenvolvimento profissional. Pela primeira vez, atuei como `Product Owner (PO)`, papel que exige uma visão mais ampla do projeto, equilíbrio entre as necessidades do cliente e as limitações técnicas da equipe, além de uma forte habilidade de comunicação.

#### Hard Skills
- Spring Boot<br>
Aprofundei meu conhecimento no ecossistema Spring ao acompanhar a criação de APIs RESTful com autenticação, segurança e integração com banco de dados.

#### Soft Skills
- Gerenciamento de Pessoas e Tarefas<br>
Como minha primeira vez como PO tive que aprender a criar e organizei um backlog, defini prioridades com base no feedback do cliente e garantir o foco do time nos itens de maior valor.

---

## Projeto 4 — Dashboard de Parceiros

**4º semestre • 2022**  
[Repositório GitHub](https://github.com/projetoKhali/api4/blob/main/README.md)  
**Parceiro Acadêmico:** [Oracle](https://www.oracle.com/partnernetwork/program/)

### Prévia da Solução

O objetivo do projeto era criar um sistema com **dashboards analíticos interativos** para visualizar o progresso dos parceiros da Oracle em trilhas de capacitação. Os dados apresentados envolviam:

- Progresso individual dos parceiros por track, expertise e qualificador.
- Tempo médio de conclusão.
- Número de parceiros por conteúdo.
- Comparativos e métricas agregadas de desempenho.

Essa visualização era importante para os gestores acompanharem a evolução dos parceiros e planejarem ações de capacitação com base em dados concretos.

### Tecnologias Utilizadas

#### Back-End

- [Java 17](https://www.oracle.com/br/java/technologies/downloads/#jdk17-windows)
- [Spring Boot](https://spring.io/)
- [Docker](https://www.docker.com/) + [Docker Compose](https://docs.docker.com/compose/)
- [PostgreSQL 15](https://www.postgresql.org/)

#### Front-End

- [Vue.js](https://vuejs.org/)
- [TypeScript](https://www.typescriptlang.org/)
- [ESLint + Prettier](https://eslint.org/)

### Contribuições Pessoais

Nesse projeto atuei como **desenvolvedor front-end** e **dba**, focando no desenvolvimento de novos componentes mantendo um padrão de estilização. Como DBA, além de modelar as tabelas que iriamos precisar, também fiz uso de views para facilitar a utilização dos dados.

#### Componentes no front-end

Fiquei responsável pela criação de alguns componentes em vue no nosso front-end, um deles é o card simples de informações

<details>
<summary>Código - Card component</summary>

```html
<template>
  <div class="conteiner">
    <div class="chart">
      <div
        v-for="(bar, i) in data"
        :key="i"
        class="bar-container"
        @mouseover="showTooltip(i, $event)"
        @mouseleave="hideTooltip"
      >
        <div class="bar-segment total-bar" :style="getTotalBarStyle(i)"></div>
        <div
          v-for="(segment, j) in bar.slice(1)"
          :key="j"
          class="bar-segment"
          :style="getBarStyle(i, j)"
        ></div>
      </div>
    </div>
    <div class="title">
      <h2>{{ title }}</h2>
    </div>
    <div
      v-if="tooltip.visible"
      class="tooltip"
      :style="{ top: tooltip.y + 'px', left: tooltip.x + 'px' }"
    >
      <div><strong>Parceiro:</strong> {{ itens[tooltip.index] }}</div>
      <div><strong>Total:</strong> {{ data[tooltip.index][0] }}</div>
      <div><strong>Associado:</strong> {{ data[tooltip.index][1] }}</div>
      <div><strong>Finalizado:</strong> {{ data[tooltip.index][2] }}</div>
    </div>
  </div>
</template>
```

</details>

#### View de métricas da track

Implementei uma view que já realiza a sumarização dos dados da track necessários para o componente `table` no front-end. Optei por utilizar uma view para praticar e aprimorar meus conhecimentos em SQL.

<details>
<summary>Código - View track_metrics</summary>

```sql
CREATE OR REPLACE VIEW track_metrics
    AS select
    tk.tk_id,
    tk.tk_name,
    (expertise_count(tk.tk_id)) AS expertise_count,

    (SELECT qualifier_count(tk.tk_id)) AS qualifier_count,

    (SELECT COUNT(pt.pt_id) 
        FROM Partner_Track AS pt 
        WHERE pt.tk_id = tk.tk_id
    ) AS partner_count,

    -- tempo médio para completar uma expertise
    (
        SELECT ROUND(AVG((pt_ex.pt_ex_complete_date - pt_ex.pt_ex_insert_date)::numeric), 2)
        FROM Partner_Expertise AS pt_ex
        JOIN Expertise AS ex ON pt_ex.ex_id = ex.ex_id
        WHERE ex.tk_id = tk.tk_id
    ) AS avg_expertise_completion_time,

    (
        SELECT ROUND(AVG((pt_ql.pt_ql_complete_date - pt_ql.pt_ql_insert_date)::numeric), 2)
        FROM Partner_Qualifier AS pt_ql 
        JOIN Expertise_Qualifier AS ex_ql ON pt_ql.ql_id = ex_ql.ql_id
        WHERE ex_ql.ex_id IN (
            SELECT ex_id 
            FROM Expertise ex 
            WHERE ex.tk_id = tk.tk_id
        )
    ) AS avg_qualifier_completion_time,
    ...
```
</details>

#### Componente de gráfico de barra

Um dos componentes essenciais para a visualização das métricas foi o componente de gráfico de barras amontuadas. Por ser um componente simples não precisei fazer o uso de nenhuma biblioteca com um componente pronto, além disso, quis me desenvolver mais como desenvolvedor front-end visto que é a minha primeira vez fora do back-end.

<details>
<summary>Código - Stacked Bar Chart</summary>

Este foi o código de exemplo 

```tsx
const title = 'Track Shell'
const itens = ['Marcos', 'Tania', 'Paulo']
const data = [[22, 10, 5], [22, 13, 8], [22, 9, 8]]
const height = (() => {
    const heightList = [];
    for (let i = 0; i < data.length; i++) {
        const listAtIndex = [];
        const dataAtIndex = data[i]!;
        for (let j = 0; j < dataAtIndex.length; j++) {
            if (!dataAtIndex || !dataAtIndex[j]) {
                continue;
            }
            const total = dataAtIndex.reduce((a, b) => a + b, 0);
            const dataAtSegment = dataAtIndex[j];
            
            if (!total || !dataAtSegment) {
                continue;
            }
            const segmentHeight = (dataAtSegment / total) * 100;
            listAtIndex.push(segmentHeight);
        }
        heightList.push(listAtIndex);
    }
    return heightList;
})();
```
![alt text](image.png)

</details>

### Lições Aprendidas
Esse foi meu primeiro projeto trabalhando fora da minha zona de conforto que é o backend, nesse projeto me dediquei exclusivamente ao fronto de end e ao banco de dados. Como primeira vez trabalhando no front end foi um desafio coordenar o comportamento e a reatividades dos componentes fazendo com que toda a interface ficasse uma coisa única e não vários componentes com formatos e estilos distantes.

#### Hard Skills
- Swagger<br>
Integração de documentação automática dos endpoints REST.

- React<br>
Usei React com TS para o desenvolvimento das páginas e dos componentes.

#### Soft Skills
- Responsabilidade Individual<br>
Assumi partes críticas do sistema por conta, mantendo padrão de código e boas práticas. Com isso tive um crescimento e melhor confiança para trabalhar em grandes partes do projeto eu mesmo.

---

## Projeto 5 — Dashboard de Processos Seletivos

**5º semestre • 2022**  
[Repositório GitHub](https://github.com/projetoKhali/api5/blob/main/README.md)  
**Parceiro Acadêmico:** [PRO4TEC](https://www.pro4tech.com.br/)

### Prévia da Solução

A proposta apresentada pela Pro4Tec consistia em **otimizar o processo de recrutamento e seleção**, centralizando e analisando dados fragmentados sobre as etapas dos processos seletivos.  

A solução desenvolvida foi um **dashboard interativo** capaz de:
- Visualizar a quantidade de processos em andamento, encerrados e expirados;
- Analisar o tempo médio de duração dos processos;
- Identificar prazos que estão se aproximando do fim;
- Fornecer insights estratégicos para as lideranças.

### Tecnologias Utilizadas

#### Back-End
- [Golang](https://go.dev/)
- [Ent](https://entgo.io/) (ORM)
- [Docker](https://www.docker.com/) + [Docker Compose](https://docs.docker.com/compose/)
- [PostgreSQL 15](https://www.postgresql.org/)

#### Front-End
- [React](https://react.dev/)
- [TypeScript](https://www.typescriptlang.org)

### Contribuições Pessoais

#### Banco de Dados

- Fui responsável por toda a **documentação e estruturação do banco de dados**, desde o **DER (Diagrama Entidade Relacionamento)** até a publicação.
- Utilizei a ferramenta **Vertabelo** para projetar e documentar a base, garantindo clareza para a equipe de desenvolvimento e alinhamento com as necessidades da empresa parceira.

#### Lógica de Back-End

Implementei a lógica para alimentar os cards do dashboard com indicadores como:  
- Total de processos por status (aberto, expirado, encerrado);
- Processos próximos do prazo final;
- Tempo médio dos processos seletivos.

<details>
<summary>Função de Análise de Processos</summary>


```go
for _, hiring := range hiringData {
    process, err := hiring.Edges.DimProcessOrErr()
    if err != nil {
        return cardInfos, fmt.Errorf("error getting process data: %v", err)
    }

    switch process.Status {
    case 1:
        cardInfos.openProcess++
    case 2:
        cardInfos.expirededProcess++
    case 3:
        cardInfos.closeProcess++
    }
    totalDuration := process.FinishDate.Sub(process.InitialDate)
    twentyPercentDuration := totalDuration * 20 / 100
    if time.Until(process.FinishDate) < twentyPercentDuration && process.Status == 1 {
        cardInfos.approachingDeadlineProcess++
    }
    totalHiringTime += hiring.MetSumDurationHiringProces
}
```
</details>

#### Testes Unitários

Implementei testes unitários para garantir a precisão das métricas apresentadas no dashboard, garantindo que mesmo depois de atualizações no código, a saída deveria continuar sendo a mesma.

<details>
<summary>Validação dos Cálculos com Testes</summary>

```go
func TestComputingCardInfo(t *testing.T) {
	process1 := &ent.DimProcess{
		Status:      1,
		InitialDate: time.Now().Add(-10 * 24 * time.Hour),
		FinishDate:  time.Now().Add(2 * 24 * time.Hour),
	}
	process2 := &ent.DimProcess{
		Status:      2,
		InitialDate: time.Now().Add(-15 * 24 * time.Hour),
		FinishDate:  time.Now().Add(-5 * 24 * time.Hour),
	}
	process3 := &ent.DimProcess{
		Status:      3,
		InitialDate: time.Now().Add(-30 * 24 * time.Hour),
		FinishDate:  time.Now().Add(-20 * 24 * time.Hour),
	}

	hiringData := []*ent.FactHiringProcess{
		{ Edges: ent.FactHiringProcessEdges{ DimProcess: process1 }, MetSumDurationHiringProces: 10 },
		{ Edges: ent.FactHiringProcessEdges{ DimProcess: process2 }, MetSumDurationHiringProces: 15 },
		{ Edges: ent.FactHiringProcessEdges{ DimProcess: process3 }, MetSumDurationHiringProces: 20 },
	}

	cardInfos, err := ComputingCardInfo(hiringData)

	assert.NoError(t, err)
	assert.Equal(t, 1, cardInfos.openProcess)
	assert.Equal(t, 1, cardInfos.expirededProcess)
	assert.Equal(t, 1, cardInfos.closeProcess)
	assert.Equal(t, 1, cardInfos.approachingDeadlineProcess)
	assert.Equal(t, 15, cardInfos.averageHiringTime)
}
```
</details>

### Lições Aprendidas
Este projeto foi marcante por me apresentar a um novo ecossistema de desenvolvimento, com a linguagem Go (Golang) e a biblioteca Ent ORM.

Também foi minha introdução ao mundo DevOps, especialmente em relação à organização e versionamento de documentação técnica e ambientes com Docker.

Além disso, retornei à função de Scrum Master com maior maturidade, ajudando a equipe a manter ritmo, foco e transparência nas entregas.

#### Hard Skills
- Golang + Ent<br>
Aprendi as particularidades da linguagem Go e o framework Ent no back-end com tipagem forte e geração automática de modelos.

- Testes Unitários<br>
Nesse projeto comecei a fazer uso de testes unitários para validação de lógica com cobertura de testes, além de ser uma boa prática.

#### Soft Skills
- Liderança Técnica como Scrum Master<br>
Retornei à função de SM depois de muito tempo sem a certeza de que conseguiria assumir o papel e dessa vez, com uma qualidade técnica maior. Consegui cumprir o papel de SM deixando um pouco de lado minha função como desenvolvedor. Tomei conta da organização do time, entrega e monitoramento das tarefas e repasse dessas informações com os professores responsáveis.

---

## Projeto 6 - Sistema Inteligente de Planejamento e Monitoramento de Reflorestamento

**6º semestre • 2022**
[Repositório GitHub](https://github.com/projetoKhali/api6/blob/main/README.md)  
**Parceiro Acadêmico:** [Kersys](https://www.kersys.com.br/)

### Prévia da Solução

#### Prévia da solução

Análise e previsão de plantio

Nossa proposta é a criação de uma aplicação web inteligente para monitoramento e gestão de plantios, permitindo que administradores e consultores acompanhem métricas essenciais, realizem análises de produtividade e gerenciem eventos do plantio de forma eficiente.

A plataforma visa integrar dados de diferentes fontes, aplicar inteligência artificial para previsões de produtividade e fornecer insights estratégicos para otimização do cultivo.


### Tecnologias utilizadas

#### Back-End
>* [Python](https://www.python.org/).
>* [Docker](https://www.docker.com/) com [Docker Compose](https://docs.docker.com/compose/).
>* [PostgreSQL 15](https://www.postgresql.org/).
>* [MongoDB](https://www.mongodb.com/).

#### Front-End
> * [React](https://react.dev/).
> * [Typescript](https://www.typescriptlang.org)

### Contribuições Pessoais

#### Criptografia de Senhas
- Criação e Documentação do banco de dados

Fui responsável pela publicação, documentação e esquematização do banco de dados.

Para isso utilziamos a ferramenta Vertabelo para auxiliar o processo de doucmentação. Na mesma ferramenta fizemos o DER que guiou o desenvolvimento durante o projeto em partes envolvendo banco de dados

#### Notificação

Fui responsável pela criação o app de notificações, um requisito importante para o projeto que cujo papel é enviar notificação para os usuários do sistema atravéz de alguns filtros.

<details> <summary>Notificação de usuários</summary>

Arquivo de configuração:

```yaml
filters:
  permission_id: 2                    # opcional: Ex: enviar só para usuários com permissão específica
  disabled: false                     # opcional: true = desabilitados, false = ativos
  email_domain: '@gmail.com'          # opcional: filtra domínio do email (ex: só gmail)
  user_id:                            # opcional: filtra por ID de usuário específico
  - 1
  login_conteins: 'a'                 # opcional: filtra todos os logins que contem o valor
  name_conteins: 'alice'              # opcional: filtra todos os nomes que contem o valor

email:
  from: 'host@email.com'
  subject: 'Atualização Importante'
  body: |
    Olá {{name}},
    Temos uma mensagem importante para você.
    Atenciosamente,
    Equipe Empresa
smtp:
  host: smtp.gmail.com
  port: 587
  user: 'host@email.com'
  password: 'app_password'
```
</details>

#### Backup

A LGPD foi outro requisito a ser cumprido, com isso vem a responsabilidade de garantir que os dados dos usuários fiquem seguros enquanto estão sob o domínio da empresa e que os mesmos nunca retornem em caso de exclusão do usuário.

<details> <summary>Backup conforme LGPD</summary>

```python

def create_backup(bkp_file_name="backup_users_permissions_"):
    os.makedirs(BACKUP_DIR, exist_ok=True)

    file_name = generate_file_name(bkp_file_name)
    container_path = f"/var/lib/postgresql/data/{file_name}"
    host_path = os.path.join(BACKUP_DIR, file_name)

    comando = [
        "docker", "exec",
        "-e", f"PGPASSWORD={DB_PASSWORD}",
        DOCKER_CONTAINER,
        "pg_dump",
        "-U", DB_USER,
        "-p", DB_PORT,
        "-d", DB_NAME,
        "-F", "c",
        *sum([["-t", tabela] for tabela in TABELAS], []),
        "-f", container_path
    ]

    try:
        subprocess.run(comando, check=True)

        subprocess.run(["docker", "cp", f"{DOCKER_CONTAINER}:{container_path}", host_path], check=True)
        subprocess.run(["docker", "exec", DOCKER_CONTAINER, "rm", container_path], check=True)

    except subprocess.CalledProcessError as e:
        print(f"❌ Erro durante processo de backup: {e}")
```

### Lições Aprendidas
Este projeto foi essencial para meu aprofundamento com Python no desenvolvimento back-end, especialmente em aplicações que lidam com manipulação de dados e integração entre múltiplas fontes (PostgreSQL e MongoDB).

Foi também uma das primeiras vezes em que lidei diretamente com temas relacionados à LGPD, como backup seguro e notificações segmentadas. Isso me ajudou a entender melhor as responsabilidades envolvidas no armazenamento e tratamento de dados sensíveis.

Aprendi também a construir aplicações auxiliares (como o sistema de notificações), separadas da aplicação principal, seguindo o modelo de microsserviços.

#### Hard Skills
- MongoDB
Aprendi a trabalhar com banco de dados não relacional, na criação de coleções (com validador) e manipulação de documentos. Além disso, integrar com minhas tasks em python.

- Arquitetura de Microsserviços
Separação de responsabilidades e criação de serviços independentes, como o de notificação, backup e restore.

#### Soft Skills
- Atenção a Requisitos Legais
Tive que entender e compreender as regras práticas da LGPD e como isso afeta no desenvolvimento de software. A partir disso, todas as soluções desenvolvidas precisariam estar de acordo e foi oque fizemos.
