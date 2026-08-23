# Diagramas UML — Sistema de Troca de Livros

Este arquivo reúne os códigos **PlantUML** dos diagramas UML desenvolvidos para o Sistema de Troca de Livros.

## Estrutura dos diagramas

### Casos de Uso
1. Visão Geral
2. Conta e Perfil
3. Livros e Descoberta
4. Processo de Troca
5. Segurança e Comunicação
6. Administração

### Sequência
7. Diagrama de Sequência Geral

### Atividades
8. Cadastro e Disponibilização de Livro
9. Processo Completo da Troca

### Classes
10. Modelo Geral do Sistema

---

# 1. Diagramas de Casos de Uso

## 1.1 — Visão Geral

```plantuml
@startuml
left to right direction

skinparam packageStyle rectangle
skinparam actorStyle awesome

actor "Usuário" as Usuario
actor "Administrador" as Admin
actor "Administrador Master" as Master

Admin --|> Usuario
Master --|> Admin

rectangle "Sistema de Troca de Livros" {

    package "Conta e Perfil" {
        usecase "Gerenciar conta\ne perfil" as Conta
    }

    package "Livros e Descoberta" {
        usecase "Gerenciar e descobrir\nlivros" as Livros
    }

    package "Troca" {
        usecase "Realizar troca\nde livros" as Troca
    }

    package "Segurança e Comunicação" {
        usecase "Comunicar e\nproteger usuários" as Seguranca
    }

    package "Administração" {
        usecase "Gerenciar usuários\ne privilégios" as AdminUC
    }
}

Usuario --> Conta
Usuario --> Livros
Usuario --> Troca
Usuario --> Seguranca

Master --> AdminUC

@enduml
```

---

## 1.2 — Conta e Perfil

```plantuml
@startuml
left to right direction

skinparam packageStyle rectangle
skinparam actorStyle awesome

skinparam usecase {
    BackgroundColor White
    BorderColor Black
    ArrowColor Black
}

actor "Usuário" as Usuario

rectangle "Conta e Perfil" {

    usecase "Criar conta" as UC01
    usecase "Autenticar usuário" as UC02
    usecase "Recuperar senha" as UC03
    usecase "Gerenciar perfil" as UC04
    usecase "Configurar interesses\nliterários" as UC05
    usecase "Gerenciar privacidade\ne direitos do titular" as UC06
}

Usuario --> UC01
Usuario --> UC02
Usuario --> UC03
Usuario --> UC04
Usuario --> UC05
Usuario --> UC06

@enduml
```

---

## 1.3 — Livros e Descoberta

```plantuml
@startuml
left to right direction

skinparam packageStyle rectangle
skinparam actorStyle awesome

skinparam usecase {
    BackgroundColor White
    BorderColor Black
    ArrowColor Black
}

actor "Usuário" as Usuario

rectangle "Livros e Descoberta" {

    usecase "Localizar usuário" as UC07
    usecase "Gerenciar livros" as UC08
    usecase "Visualizar livros\ndisponíveis na região" as UC09
    usecase "Visualizar detalhes\ndo livro" as UC10
    usecase "Gerenciar disponibilidade\ne visibilidade" as UC11
    usecase "Demonstrar interesse" as UC12
}

Usuario --> UC07
Usuario --> UC08
Usuario --> UC09
Usuario --> UC10
Usuario --> UC11
Usuario --> UC12

@enduml
```

---

## 1.4 — Processo de Troca

```plantuml
@startuml
left to right direction

skinparam packageStyle rectangle
skinparam actorStyle awesome

skinparam usecase {
    BackgroundColor White
    BorderColor Black
    ArrowColor Black
}

actor "Usuário" as Usuario

rectangle "Processo de Troca" {

    usecase "Demonstrar interesse" as UC12
    usecase "Formalizar match" as UC13
    usecase "Reservar livros\ndo match" as UC14
    usecase "Disponibilizar chat\nprivado" as UC15
    usecase "Combinar local e\nhorário da troca" as UC16
    usecase "Concluir ou\ncancelar troca" as UC17
    usecase "Avaliar participantes" as UC18
    usecase "Consultar histórico\nde trocas" as UC19
}

Usuario --> UC12
Usuario --> UC13
Usuario --> UC14
Usuario --> UC15
Usuario --> UC16
Usuario --> UC17
Usuario --> UC18
Usuario --> UC19

@enduml
```

---

## 1.5 — Segurança e Comunicação

```plantuml
@startuml
left to right direction

skinparam packageStyle rectangle
skinparam actorStyle awesome

skinparam usecase {
    BackgroundColor White
    BorderColor Black
    ArrowColor Black
}

actor "Usuário" as Usuario

rectangle "Segurança e Comunicação" {

    usecase "Disponibilizar chat\nprivado" as UC15
    usecase "Enviar notificações" as UC20
    usecase "Denunciar ou\nbloquear" as UC21
}

Usuario --> UC15
Usuario --> UC20
Usuario --> UC21

@enduml
```

---

## 1.6 — Administração

```plantuml
@startuml
left to right direction

skinparam packageStyle rectangle
skinparam actorStyle awesome

skinparam usecase {
    BackgroundColor White
    BorderColor Black
    ArrowColor Black
}

actor "Usuário" as Usuario
actor "Administrador" as Admin
actor "Administrador Master" as Master

Admin --|> Usuario
Master --|> Admin

rectangle "Administração" {

    usecase "Gerenciar usuários\ne privilégios" as UC22
}

Master --> UC22

@enduml
```

---

# 2. Diagrama de Sequência

## 2.1 — Sequência Geral do Sistema

Este diagrama representa o fluxo principal do sistema, desde a autenticação e descoberta de livros até a realização da troca, avaliação e consulta do histórico.

```plantuml
@startuml
title Diagrama de Sequência Geral — Sistema de Troca de Livros

actor "Usuário" as Usuario

participant "Aplicativo Web" as App
participant "API / Sistema" as Sistema
participant "Serviço de Autenticação" as Auth
participant "Serviço de Livros" as Livros
participant "Serviço de Match" as Match
participant "Serviço de Troca" as Troca
participant "Serviço de Notificações" as Notif
database "Banco de Dados" as BD

== 1. Cadastro / Autenticação ==

Usuario -> App : Acessa o sistema
App -> Sistema : Solicita autenticação
Sistema -> Auth : Validar credenciais
Auth -> BD : Consultar usuário
BD --> Auth : Dados do usuário
Auth --> Sistema : Autenticação aprovada
Sistema --> App : Sessão iniciada
App --> Usuario : Exibe página inicial

== 2. Descoberta de livros ==

Usuario -> App : Busca livros disponíveis
App -> Sistema : Solicita livros da região
Sistema -> Livros : Buscar livros
Livros -> BD : Consultar livros disponíveis
BD --> Livros : Lista de livros
Livros --> Sistema : Livros encontrados
Sistema --> App : Retorna resultados
App --> Usuario : Exibe livros disponíveis

Usuario -> App : Visualiza detalhes do livro
App -> Sistema : Solicita detalhes
Sistema -> Livros : Consultar livro
Livros -> BD : Buscar informações
BD --> Livros : Dados do livro
Livros --> Sistema : Detalhes
Sistema --> App : Retorna detalhes
App --> Usuario : Exibe detalhes

== 3. Interesse e Match ==

Usuario -> App : Demonstra interesse
App -> Sistema : Registra interesse
Sistema -> Match : Analisar interesse
Match -> BD : Consultar compatibilidade
BD --> Match : Dados dos usuários/livros

alt Match não encontrado

    Match --> Sistema : Interesse registrado
    Sistema -> Notif : Gerar notificação
    Notif --> Usuario : Interesse registrado

else Match encontrado

    Match --> Sistema : Match confirmado

    Sistema -> Troca : Solicitar reserva
    Troca -> BD : Reservar livros

    alt Reserva realizada

        BD --> Troca : Reserva confirmada
        Troca --> Sistema : Match ativo

        Sistema -> Notif : Notificar participantes
        Notif --> Usuario : Match confirmado

    else Falha na reserva

        BD --> Troca : Reserva não realizada
        Troca --> Sistema : Operação cancelada
        Sistema --> App : Informa falha
        App --> Usuario : Reserva não realizada

    end

end

== 4. Chat e negociação ==

Usuario -> App : Abre chat do match
App -> Sistema : Solicita chat
Sistema -> BD : Validar match ativo
BD --> Sistema : Match válido
Sistema --> App : Libera chat

Usuario -> App : Envia mensagem
App -> Sistema : Envia mensagem
Sistema -> BD : Salva mensagem
Sistema -> Notif : Gerar notificação
Notif --> Usuario : Nova mensagem

== 5. Combinação da troca ==

Usuario -> App : Propõe local e horário
App -> Sistema : Envia proposta
Sistema -> Troca : Registrar proposta
Troca -> BD : Salvar local/data/horário
Sistema -> Notif : Notificar participante
Notif --> Usuario : Nova proposta recebida

Usuario -> App : Confirma proposta
App -> Sistema : Confirmar encontro
Sistema -> Troca : Atualizar troca
Troca -> BD : Salvar confirmação

== 6. Conclusão da troca ==

Usuario -> App : Confirma realização da troca
App -> Sistema : Solicita conclusão
Sistema -> Troca : Validar conclusão
Troca -> BD : Registrar conclusão

alt Troca concluída

    BD --> Troca : Operação concluída
    Troca --> Sistema : Troca finalizada

    Sistema -> BD : Registrar histórico
    Sistema -> Notif : Enviar confirmação
    Notif --> Usuario : Troca concluída

else Divergência

    BD --> Troca : Divergência identificada
    Troca --> Sistema : Solicitar tratamento
    Sistema --> App : Exibir divergência
    App --> Usuario : Orientação sobre divergência

end

== 7. Avaliação ==

Usuario -> App : Avalia participante
App -> Sistema : Envia avaliação
Sistema -> BD : Registrar avaliação
BD --> Sistema : Avaliação registrada
Sistema --> App : Confirma avaliação
App --> Usuario : Avaliação concluída

== 8. Histórico ==

Usuario -> App : Consulta histórico
App -> Sistema : Solicita histórico
Sistema -> BD : Buscar trocas concluídas
BD --> Sistema : Histórico
Sistema --> App : Retorna histórico
App --> Usuario : Exibe histórico

@enduml
```

---

# 3. Diagramas de Atividade

Foram utilizados **dois diagramas de atividade** porque representam processos de negócio diferentes: o primeiro trata do ciclo de cadastro/disponibilização do livro e o segundo trata do processo de troca. A separação evita um fluxo excessivamente grande e melhora a legibilidade.

---

## 3.1 — Cadastro e Disponibilização de Livro

Relaciona-se principalmente ao gerenciamento de livros, disponibilidade e visibilidade.

```plantuml
@startuml
title Diagrama de Atividade — Cadastro e Disponibilização de Livro

start

:Usuário seleciona "Cadastrar livro";

:Informar ISBN, título ou autor;

:Consultar API de metadados;

if (Livro encontrado?) then (Sim)

    :Carregar metadados;

else (Não)

    :Realizar cadastro manual;

endif

:Informar estado de conservação;

:Adicionar fotografias;

:Definir disponibilidade;

:Definir visibilidade;

if (Publicar agora?) then (Sim)

    :Validar dados obrigatórios;

    if (Dados válidos?) then (Sim)

        :Publicar livro;
        :Livro aparece na descoberta;

    else (Não)

        :Exibir erros;

    endif

else (Não)

    if (Salvar como rascunho?) then (Sim)

        :Salvar rascunho;

    else (Ocultar)

        :Salvar livro como oculto;

    endif

endif

stop

@enduml
```

---

## 3.2 — Processo Completo da Troca

```plantuml
@startuml
title Diagrama de Atividade — Fluxo Completo da Troca

start

:Usuário acessa o sistema;

:Localizar livros disponíveis;

:Visualizar detalhes do livro;

if (Livro disponível?) then (Sim)

    :Demonstrar interesse;

    if (Match formado?) then (Sim)

        :Formalizar match;

        :Reservar os dois livros;

        if (Reserva realizada?) then (Sim)

            :Liberar chat privado;

            :Combinar local,\ndata e horário;

            if (Encontro confirmado?) then (Sim)

                :Realizar troca;

                :Confirmar conclusão;

                if (Ambos confirmaram?) then (Sim)

                    :Transferir livros;
                    :Registrar histórico;
                    :Oferecer cadastro do\nlivro recebido;

                    :Avaliar participantes;

                else (Não)

                    :Tratar divergência;

                endif

            else (Não)

                :Alterar ou cancelar proposta;

            endif

        else (Não)

            :Cancelar match;
            :Liberar livros;

        endif

    else (Não)

        :Registrar interesse;
        :Aguardar compatibilidade;

    endif

else (Não)

    :Informar indisponibilidade;

endif

stop

@enduml
```

---

# 4. Diagrama de Classes

O diagrama de classes apresenta o modelo conceitual das principais entidades envolvidas no sistema e seus relacionamentos.

```plantuml
@startuml
title Diagrama de Classes — Sistema de Troca de Livros

class Usuario {
    - id: Long
    - nome: String
    - email: String
    - senhaHash: String
    - telefone: String
    - status: StatusUsuario
    + criarConta()
    + autenticar()
    + atualizarPerfil()
    + bloquear()
}

class Perfil {
    - id: Long
    - foto: String
    - biografia: String
    - cidade: String
    - raioBusca: Double
}

class InteresseLiterario {
    - id: Long
    - genero: String
    - autor: String
    - tema: String
}

class Livro {
    - id: Long
    - titulo: String
    - autor: String
    - isbn: String
    - edicao: String
    - capa: String
}

class CopiaLivro {
    - id: Long
    - estadoConservacao: String
    - disponibilidade: StatusLivro
    - visibilidade: Visibilidade
    - dataCadastro: Date
}

class Interesse {
    - id: Long
    - dataHora: DateTime
    - status: StatusInteresse
}

class Match {
    - id: Long
    - dataCriacao: DateTime
    - status: StatusMatch
}

class Reserva {
    - id: Long
    - dataHora: DateTime
    - status: StatusReserva
}

class Chat {
    - id: Long
    - dataCriacao: DateTime
}

class Mensagem {
    - id: Long
    - conteudo: String
    - dataHora: DateTime
    - lida: Boolean
}

class Troca {
    - id: Long
    - local: String
    - data: Date
    - horario: Time
    - status: StatusTroca
}

class Avaliacao {
    - id: Long
    - nota: Integer
    - comentario: String
    - data: DateTime
}

class Notificacao {
    - id: Long
    - titulo: String
    - mensagem: String
    - dataHora: DateTime
    - lida: Boolean
}

class Denuncia {
    - id: Long
    - motivo: String
    - dataHora: DateTime
    - status: StatusDenuncia
}

class Bloqueio {
    - id: Long
    - dataHora: DateTime
}

class HistoricoTroca {
    - id: Long
    - dataConclusao: DateTime
}

class Administrador {
    - nivel: String
    + gerenciarUsuarios()
    + gerenciarPrivilegios()
}

class AdministradorMaster {
    + criarAdministrador()
    + promoverAdministrador()
    + rebaixarAdministrador()
    + revogarAdministrador()
}

Usuario "1" -- "1" Perfil
Usuario "1" -- "*" InteresseLiterario

Usuario "1" -- "*" CopiaLivro : possui >
Livro "1" -- "*" CopiaLivro

Usuario "1" -- "*" Interesse : demonstra >
CopiaLivro "1" -- "*" Interesse

Interesse "1" -- "0..1" Match

Match "1" -- "2..*" Reserva
Reserva "*" -- "1" CopiaLivro

Match "1" -- "1" Chat
Chat "1" -- "*" Mensagem
Usuario "1" -- "*" Mensagem : envia >

Match "1" -- "0..1" Troca
Troca "1" -- "*" Avaliacao

Usuario "1" -- "*" Avaliacao : realiza >

Usuario "1" -- "*" Notificacao

Usuario "1" -- "*" Denuncia : realiza >
Usuario "1" -- "*" Bloqueio : realiza >

Troca "1" -- "1" HistoricoTroca

Administrador --|> Usuario
AdministradorMaster --|> Administrador

@enduml
```

---

# 5. Organização sugerida dos arquivos

```text
diagramas/
│
├── casos-de-uso/
│   ├── 01-visao-geral.puml
│   ├── 02-conta-perfil.puml
│   ├── 03-livros-descoberta.puml
│   ├── 04-processo-troca.puml
│   ├── 05-seguranca-comunicacao.puml
│   └── 06-administracao.puml
│
├── sequencia/
│   └── 01-sequencia-geral.puml
│
├── atividades/
│   ├── 01-cadastro-livro.puml
│   └── 02-processo-troca.puml
│
└── classes/
    └── 01-modelo-geral.puml
```

## Observação

Os diagramas de casos de uso apresentam a visão funcional do sistema. O diagrama de sequência detalha o fluxo temporal principal da aplicação. Os dois diagramas de atividade representam processos de negócio distintos, e o diagrama de classes apresenta a estrutura conceitual das entidades e seus relacionamentos.
