# Documentação Técnica e Diagramas Architecture — Sistema de Troca de Livros

Este documento reúne a especificação visual e estrutural do **Sistema de Troca de Livros**, englobando diagramas de Casos de Uso, Classes, Atividades e Sequência. Todos os diagramas foram disponibilizados em **PlantUML** e **Mermaid** para maior compatibilidade.

---

## 📑 Sumário

1. [Diagramas de Casos de Uso (UML)](#1-diagramas-de-casos-de-uso-uml)
   - [1.1 Pacote: Conta e Perfil](#11-pacote-conta-e-perfil)
   - [1.2 Pacote: Livros e Descoberta](#12-pacote-livros-e-descoberta)
   - [1.3 Pacote: Processo de Troca](#13-pacote-processo-de-troca)
   - [1.4 Pacote: Segurança e Comunicação](#14-pacote-segurança-e-comunicação)
   - [1.5 Pacote: Administração](#15-pacote-administração)
   - [1.6 Visão Geral do Sistema](#16-casos-de-uso--visão-geral-do-sistema)
2. [Diagrama de Classes](#2-diagrama-de-classes)
3. [Diagramas de Atividades](#3-diagramas-de-atividades)
   - [3.1 Fluxo Completo da Troca](#31-fluxo-completo-da-troca)
   - [3.2 Cadastro e Disponibilização de Livro](#32-cadastro-e-disponibilização-de-livro)
4. [Diagramas de Sequência](#4-diagramas-de-sequência)
   - [4.1 Cadastro e Autenticação](#41-cadastro-e-autenticação)
   - [4.2 Sequência Geral do Sistema](#42-sequência-geral-do-sistema)

---

## 1. Diagramas de Casos de Uso (UML)

### 1.1 Pacote: Conta e Perfil

#### PlantUML
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

flowchart LR
    classDef actor fill:#e1f5fe,stroke:#0288d1,stroke-width:2px;
    classDef uc fill:#ffffff,stroke:#000000,stroke-width:1px,rx:20,ry:20;

    Usuario(["👤 Usuário"]):::actor

    subgraph Pacote_Conta_Perfil ["Conta e Perfil"]
        UC01(["UC01 - Criar conta"]):::uc
        UC02(["UC02 - Autenticar usuário"]):::uc
        UC03(["UC03 - Recuperar senha"]):::uc
        UC04(["UC04 - Gerenciar perfil"]):::uc
        UC05(["UC05 - Configurar interesses literários"]):::uc
        UC06(["UC06 - Gerenciar privacidade e direitos do titular"]):::uc
    end

    Usuario --> UC01
    Usuario --> UC02
    Usuario --> UC03
    Usuario --> UC04
    Usuario --> UC05
    Usuario --> UC06

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
actor "API Externa de Metadados" as API << Service >>

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

UC09 .> UC07 : <<include>>
UC08 .right.> API : <<include>>

@enduml

flowchart LR
    classDef actor fill:#e1f5fe,stroke:#0288d1,stroke-width:2px;
    classDef uc fill:#ffffff,stroke:#000000,stroke-width:1px,rx:20,ry:20;

    Usuario(["👤 Usuário"]):::actor
    API(["🔌 API Externa de Metadados"]):::actor

    subgraph Pacote_Livros_Descoberta ["Livros e Descoberta"]
        UC07(["UC07 - Localizar usuário"]):::uc
        UC08(["UC08 - Gerenciar livros"]):::uc
        UC09(["UC09 - Visualizar livros disponíveis na região"]):::uc
        UC10(["UC10 - Visualizar detalhes do livro"]):::uc
        UC11(["UC11 - Gerenciar disponibilidade e visibilidade"]):::uc
        UC12(["UC12 - Demonstrar interesse"]):::uc
    end

    Usuario --> UC07
    Usuario --> UC08
    Usuario --> UC09
    Usuario --> UC10
    Usuario --> UC11
    Usuario --> UC12

    UC08 -. "<<include>>" .-> API
    UC09 -. "<<include>>" .-> UC07

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
Usuario --> UC16
Usuario --> UC17
Usuario --> UC18
Usuario --> UC19

UC13 .right.> UC14 : <<include>>
UC13 .down.> UC15 : <<include>>

@enduml

flowchart LR
    classDef actor fill:#e1f5fe,stroke:#0288d1,stroke-width:2px;
    classDef uc fill:#ffffff,stroke:#000000,stroke-width:1px,rx:20,ry:20;

    Usuario(["👤 Usuário"]):::actor

    subgraph Pacote_Troca ["Processo de Troca"]
        UC12(["UC12 - Demonstrar interesse"]):::uc
        UC13(["UC13 - Formalizar match"]):::uc
        UC14(["UC14 - Reservar livros do match"]):::uc
        UC15(["UC15 - Disponibilizar chat privado"]):::uc
        UC16(["UC16 - Combinar local e horário da troca"]):::uc
        UC17(["UC17 - Concluir ou cancelar troca"]):::uc
        UC18(["UC18 - Avaliar participantes"]):::uc
        UC19(["UC19 - Consultar histórico de trocas"]):::uc
    end

    Usuario --> UC12
    Usuario --> UC13
    Usuario --> UC16
    Usuario --> UC17
    Usuario --> UC18
    Usuario --> UC19

    UC13 -. "<<include>>" .-> UC14
    UC13 -. "<<include>>" .-> UC15

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

Usuario --> UC21

@enduml

flowchart LR
    classDef actor fill:#e1f5fe,stroke:#0288d1,stroke-width:2px;
    classDef uc fill:#ffffff,stroke:#000000,stroke-width:1px,rx:20,ry:20;

    Usuario(["👤 Usuário"]):::actor

    subgraph Pacote_Seguranca ["Segurança e Comunicação"]
        UC15(["UC15 - Disponibilizar chat privado"]):::uc
        UC20(["UC20 - Enviar notificações"]):::uc
        UC21(["UC21 - Denunciar ou bloquear"]):::uc
    end

    Usuario --> UC21

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

flowchart LR
    classDef actor fill:#e1f5fe,stroke:#0288d1,stroke-width:2px;
    classDef uc fill:#ffffff,stroke:#000000,stroke-width:1px,rx:20,ry:20;

    Usuario(["👤 Usuário"]):::actor
    Admin(["👮 Administrador"]):::actor
    Master(["👑 Administrador Master"]):::actor

    subgraph Pacote_Administracao ["Administração"]
        UC22(["UC22 - Gerenciar usuários e privilégios"]):::uc
    end

    Admin -- "herda de" --> Usuario
    Master -- "herda de" --> Admin

    Master --> UC22

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
actor "API Externa de Metadados" as API << Service >>

Admin --|> Usuario
Master --|> Admin

rectangle "Sistema de Troca de Livros" {

    package "Conta e Perfil" {
        usecase "UC01 - Criar conta" as UC01
        usecase "UC02 - Autenticar usuário" as UC02
        usecase "UC03 - Recuperar senha" as UC03
        usecase "UC04 - Gerenciar perfil" as UC04
        usecase "UC05 - Configurar interesses literários" as UC05
        usecase "UC06 - Gerenciar privacidade e direitos do titular" as UC06
    }

    package "Livros e Descoberta" {
        usecase "UC07 - Localizar usuário" as UC07
        usecase "UC08 - Gerenciar livros" as UC08
        usecase "UC09 - Visualizar livros disponíveis na região" as UC09
        usecase "UC10 - Visualizar detalhes do livro" as UC10
        usecase "UC11 - Gerenciar disponibilidade e visibilidade" as UC11
        usecase "UC12 - Demonstrar interesse" as UC12
    }

    package "Troca" {
        usecase "UC13 - Formalizar match" as UC13
        usecase "UC14 - Reservar livros do match" as UC14
        usecase "UC15 - Disponibilizar chat privado" as UC15
        usecase "UC16 - Combinar local e horário da troca" as UC16
        usecase "UC17 - Concluir ou cancelar troca" as UC17
        usecase "UC18 - Avaliar participantes" as UC18
        usecase "UC19 - Consultar histórico de trocas" as UC19
    }

    package "Segurança e Comunicação" {
        usecase "UC20 - Enviar notificações" as UC20
        usecase "UC21 - Denunciar ou bloquear" as UC21
    }

    package "Administração" {
        usecase "UC22 - Gerenciar usuários e privilégios" as UC22
    }
}

Usuario --> UC01
Usuario --> UC02
Usuario --> UC03
Usuario --> UC04
Usuario --> UC05
Usuario --> UC06

Usuario --> UC07
Usuario --> UC08
Usuario --> UC09
Usuario --> UC10
Usuario --> UC11
Usuario --> UC12

Usuario --> UC13
Usuario --> UC16
Usuario --> UC17
Usuario --> UC18
Usuario --> UC19

Usuario --> UC21

UC08 .> API : <<include>>
UC09 .> UC07 : <<include>>
UC13 .> UC14 : <<include>>
UC13 .> UC15 : <<include>>

Master --> UC22

@enduml

flowchart LR
    classDef actor fill:#e1f5fe,stroke:#0288d1,stroke-width:2px;
    classDef uc fill:#ffffff,stroke:#000000,stroke-width:1px,rx:20,ry:20;

    Usuario(["👤 Usuário"]):::actor
    Admin(["👮 Administrador"]):::actor
    Master(["👑 Administrador Master"]):::actor
    API(["🔌 API Externa de Metadados"]):::actor

    Admin -- "herda" --> Usuario
    Master -- "herda" --> Admin

    subgraph Sistema ["Sistema de Troca de Livros"]
        subgraph P1 ["Conta e Perfil"]
            UC01(["UC01 - Criar conta"]):::uc
            UC02(["UC02 - Autenticar usuário"]):::uc
            UC03(["UC03 - Recuperar senha"]):::uc
            UC04(["UC04 - Gerenciar perfil"]):::uc
            UC05(["UC05 - Configurar interesses literários"]):::uc
            UC06(["UC06 - Gerenciar privacidade e direitos"]):::uc
        end

        subgraph P2 ["Livros e Descoberta"]
            UC07(["UC07 - Localizar usuário"]):::uc
            UC08(["UC08 - Gerenciar livros"]):::uc
            UC09(["UC09 - Visualizar livros na região"]):::uc
            UC10(["UC10 - Visualizar detalhes do livro"]):::uc
            UC11(["UC11 - Gerenciar disponibilidade"]):::uc
            UC12(["UC12 - Demonstrar interesse"]):::uc
        end

        subgraph P3 ["Troca"]
            UC13(["UC13 - Formalizar match"]):::uc
            UC14(["UC14 - Reservar livros do match"]):::uc
            UC15(["UC15 - Disponibilizar chat privado"]):::uc
            UC16(["UC16 - Combinar local e horário"]):::uc
            UC17(["UC17 - Concluir ou cancelar troca"]):::uc
            UC18(["UC18 - Avaliar participantes"]):::uc
            UC19(["UC19 - Consultar histórico de trocas"]):::uc
        end

        subgraph P4 ["Segurança e Comunicação"]
            UC20(["UC20 - Enviar notificações"]):::uc
            UC21(["UC21 - Denunciar ou bloquear"]):::uc
        end

        subgraph P5 ["Administração"]
            UC22(["UC22 - Gerenciar usuários e privilégios"]):::uc
        end
    end

    Usuario --> UC01 & UC02 & UC03 & UC04 & UC05 & UC06
    Usuario --> UC07 & UC08 & UC09 & UC10 & UC11 & UC12
    Usuario --> UC13 & UC16 & UC17 & UC18 & UC19 & UC21

    Master --> UC22

    UC08 -. "<<include>>" .-> API
    UC09 -. "<<include>>" .-> UC07
    UC13 -. "<<include>>" .-> UC14 & UC15

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
Livro "1" -- "*" CopiaLivro : instância >

Usuario "1" -- "*" Interesse : demonstra >
CopiaLivro "1" -- "*" Interesse : alvo >

Interesse "1" -- "0..1" Match

Match "1" -- "2" Reserva : gera >
Reserva "1" -- "1" CopiaLivro : bloqueia >

Match "1" -- "1" Chat
Chat "1" -- "*" Mensagem
Usuario "1" -- "*" Mensagem : envia >

Match "1" -- "0..1" Troca
Troca "1" -- "*" Avaliacao

Usuario "1" -- "*" Avaliacao : realiza (Avaliador) >
Usuario "1" -- "*" Avaliacao : recebe (Avaliado) >

Usuario "1" -- "*" Notificacao

Usuario "1" -- "*" Denuncia : realiza >
Usuario "1" -- "*" Bloqueio : realiza >

Troca "1" -- "0..1" HistoricoTroca

Administrador --|> Usuario
AdministradorMaster --|> Administrador

@enduml

classDiagram
    class Usuario {
        -Long id
        -String nome
        -String email
        -String senhaHash
        -String telefone
        -StatusUsuario status
        +criarConta()
        +autenticar()
        +atualizarPerfil()
        +bloquear()
    }

    class Perfil {
        -Long id
        -String foto
        -String biografia
        -String cidade
        -Double raioBusca
    }

    class InteresseLiterario {
        -Long id
        -String genero
        -String autor
        -String tema
    }

    class Livro {
        -Long id
        -String titulo
        -String autor
        -String isbn
        -String edicao
        -String capa
    }

    class CopiaLivro {
        -Long id
        -String estadoConservacao
        -StatusLivro disponibilidade
        -Visibilidade visibilidade
        -Date dataCadastro
    }

    class Interesse {
        -Long id
        -DateTime dataHora
        -StatusInteresse status
    }

    class Match {
        -Long id
        -DateTime dataCriacao
        -StatusMatch status
    }

    class Reserva {
        -Long id
        -DateTime dataHora
        -StatusReserva status
    }

    class Chat {
        -Long id
        -DateTime dataCriacao
    }

    class Mensagem {
        -Long id
        -String conteudo
        -DateTime dataHora
        -Boolean lida
    }

    class Troca {
        -Long id
        -String local
        -Date data
        -Time horario
        -StatusTroca status
    }

    class Avaliacao {
        -Long id
        -Integer nota
        -String comentario
        -DateTime data
    }

    class Notificacao {
        -Long id
        -String titulo
        -String mensagem
        -DateTime dataHora
        -Boolean lida
    }

    class Denuncia {
        -Long id
        -String motivo
        -DateTime dataHora
        -StatusDenuncia status
    }

    class Bloqueio {
        -Long id
        -DateTime dataHora
    }

    class HistoricoTroca {
        -Long id
        -DateTime dataConclusao
    }

    class Administrador {
        -String nivel
        +gerenciarUsuarios()
        +gerenciarPrivilegios()
    }

    class AdministradorMaster {
        +criarAdministrador()
        +promoverAdministrador()
        +rebaixarAdministrador()
        +revogarAdministrador()
    }

    Usuario "1" -- "1" Perfil
    Usuario "1" -- "*" InteresseLiterario
    Usuario "1" -- "*" CopiaLivro : possui
    Livro "1" -- "*" CopiaLivro
    Usuario "1" -- "*" Interesse : demonstra
    CopiaLivro "1" -- "*" Interesse : alvo
    Interesse "1" -- "0..1" Match
    Match "1" -- "2" Reserva : gera
    Reserva "1" -- "1" CopiaLivro : bloqueia
    Match "1" -- "1" Chat
    Chat "1" -- "*" Mensagem
    Usuario "1" -- "*" Mensagem : envia
    Match "1" -- "0..1" Troca
    Troca "1" -- "*" Avaliacao
    Usuario "1" -- "*" Avaliacao : avalia
    Usuario "1" -- "*" Notificacao
    Usuario "1" -- "*" Denuncia : realiza
    Usuario "1" -- "*" Bloqueio : realiza
    Troca "1" -- "0..1" HistoricoTroca

    Administrador --|> Usuario
    AdministradorMaster --|> Administrador

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

        if (Reserva realizada com sucesso?) then (Sim)
            :Liberar chat privado;
            
            repeat
                :Combinar local, data e horário;
            backward :Alterar proposta;
            repeat while (Encontro confirmado?) is (Não)

            :Realizar troca presencial;
            :Confirmar conclusão no sistema;

            if (Ambos confirmaram?) then (Sim)
                :Transferir livros (operação transacional);
                :Registrar histórico da troca;
                :Oferecer cadastro do livro recebido;
                :Avaliar participante;
            else (Não / Divergência)
                :Tratar divergência na moderação;
            endif

        else (Não)
            :Cancelar match;
            :Liberar livros;
        endif

    else (Não)
        :Registrar interesse no sistema;
        :Aguardar compatibilidade (Match futuro);
    endif

else (Não)
    :Informar indisponibilidade;
endif

stop

@enduml

flowchart TD
    Start([Início]) --> A[Usuário acessa o sistema]
    A --> B[Localizar livros disponíveis]
    B --> C[Visualizar detalhes do livro]
    
    C --> D{Livro disponível?}
    D -- Não --> E[Informar indisponibilidade] --> End([Fim])
    
    D -- Sim --> F[Demonstrar interesse]
    F --> G{Match formado?}
    
    G -- Não --> H[Registrar interesse] --> I[Aguardar compatibilidade] --> End
    
    G -- Sim --> J[Formalizar match]
    J --> K[Reservar os dois livros]
    K --> L{Reserva realizada?}
    
    L -- Não --> M[Cancelar match] --> N[Liberar livros] --> End
    
    L -- Sim --> O[Liberar chat privado]
    O --> P[Combinar local, data e horário]
    P --> Q{Encontro confirmado?}
    
    Q -- Não --> R[Alterar proposta] --> P
    
    Q -- Sim --> S[Realizar troca presencial]
    S --> T[Confirmar conclusão no sistema]
    T --> U{Ambos confirmaram?}
    
    U -- Não --> V[Tratar divergência na moderação] --> End
    
    U -- Sim --> W[Transferir livros]
    W --> X[Registrar histórico da troca]
    X --> Y[Oferecer cadastro do livro recebido]
    Y --> Z[Avaliar participante] --> End

    @startuml
title Diagrama de Atividade — Cadastro e Disponibilização de Livro

start

:Usuário seleciona "Cadastrar livro";
:Informar ISBN, título ou autor;
:Consultar API de metadados;

if (Livro encontrado?) then (Sim)
    :Carregar metadados da API;
else (Não)
    :Realizar preenchimento manual;
endif

:Informar estado de conservação;
:Adicionar fotografias;
:Definir disponibilidade;
:Definir visibilidade;

if (Publicar agora?) then (Sim)
    repeat
        :Validar dados obrigatórios;
        if (Dados válidos?) then (Sim)
            :Publicar livro;
            :Livro fica disponível para descoberta;
            stop
        else (Não)
            :Exibir mensagens de erro nos campos;
            :Corrigir informações;
        endif
    repeat while (Deseja tentar novamente?) is (Sim)

else (Não)
    if (Salvar como rascunho?) then (Sim)
        :Salvar livro como rascunho;
    else (Ocultar)
        :Salvar livro na lista privada (Oculto);
    endif
endif

stop

@enduml

flowchart TD
    Start([Início]) --> A[Usuário seleciona 'Cadastrar livro']
    A --> B[Informar ISBN, título ou autor]
    B --> C[Consultar API de metadados]
    
    C --> D{Livro encontrado?}
    D -- Sim --> E[Carregar metadados]
    D -- Não --> F[Realizar preenchimento manual]
    
    E --> G[Informar estado de conservação]
    F --> G
    
    G --> H[Adicionar fotografias]
    H --> I[Definir disponibilidade]
    I --> J[Definir visibilidade]
    
    J --> K{Publicar agora?}
    
    K -- Não --> L{Salvar como rascunho?}
    L -- Sim --> M[Salvar como rascunho] --> End([Fim])
    L -- Ocultar --> N[Salvar livro como oculto] --> End
    
    K -- Sim --> O[Validar dados obrigatórios]
    O --> P{Dados válidos?}
    
    P -- Não --> Q[Exibir mensagens de erro] --> R[Corrigir informações] --> O
    P -- Sim --> S[Publicar livro] --> T[Livro aparece na descoberta] --> End

    @startuml
title Diagrama de Sequência Geral — Sistema de Troca de Livros

actor "Usuário A\n(Iniciador)" as UsuarioA
actor "Usuário B\n(Receptor)" as UsuarioB

participant "Aplicativo Web/Mobile" as App
participant "API / Gateway" as Sistema
participant "Serviço de Autenticação" as Auth
participant "Serviço de Livros" as Livros
participant "Serviço de Match" as Match
participant "Serviço de Troca" as Troca
participant "Serviço de Notificações" as Notif
database "Banco de Dados" as BD

== 1. Cadastro / Autenticação (RF-01 / RF-06) ==

UsuarioA -> App : Acessa o sistema
App -> Sistema : Solicita autenticação
Sistema -> Auth : Validar credenciais
Auth -> BD : Consultar usuário
BD --> Auth : Dados do usuário
Auth --> Sistema : Autenticação aprovada
Sistema --> App : Sessão iniciada (Token JWT)
App --> UsuarioA : Exibe página inicial

== 2. Descoberta de Livros (RF-02 / RF-04 / RF-10) ==

UsuarioA -> App : Busca livros disponíveis
App -> Sistema : Solicita livros da região
Sistema -> Livros : Buscar livros no raio definido
Livros -> BD : Consultar livros disponíveis
BD --> Livros : Lista de livros
Livros --> Sistema : Livros encontrados
Sistema --> App : Retorna resultados
App --> UsuarioA : Exibe livros disponíveis

UsuarioA -> App : Visualiza detalhes do livro
App -> Sistema : Solicita detalhes do livro e obra
Sistema -> Livros : Consultar livro
Livros -> BD : Buscar informações
BD --> Livros : Dados do livro
Livros --> Sistema : Detalhes
Sistema --> App : Retorna detalhes
App --> UsuarioA : Exibe detalhes

== 3. Interesse e Match (RF-05 / RF-20) ==

UsuarioA -> App : Demonstra interesse no livro do Usuário B
App -> Sistema : Registra interesse
Sistema -> Match : Analisar interesse e reciprocidade
Match -> BD : Consultar compatibilidade
BD --> Match : Dados dos usuários/livros

alt Match não encontrado
    Match --> Sistema : Interesse registrado unilateralmente
    Sistema -> Notif : Gerar notificação para o proprietário
    Notif --> UsuarioB : Notifica interesse no seu livro

else Match encontrado (Reciprocidade)
    Match --> Sistema : Match confirmado
    Sistema -> Troca : Solicitar reserva dos dois livros
    Troca -> BD : Reservar livros atomicamente (RNFR-20.2)

    alt Reserva realizada com sucesso
        BD --> Troca : Reserva confirmada
        Troca --> Sistema : Match ativo
        Sistema -> Notif : Notificar ambos participantes
        Notif --> UsuarioA : Match confirmed!
        Notif --> UsuarioB : Match confirmed!

    else Falha na reserva
        BD --> Troca : Erro na reserva
        Troca --> Sistema : Operação cancelada
        Sistema --> App : Informa falha de disponibilidade
        App --> UsuarioA : Não foi possível reservar os livros
    end
end

== 4. Chat e Negociação (RF-11) ==

UsuarioA -> App : Abre chat do match
App -> Sistema : Solicita histórico de mensagens
Sistema -> BD : Validar match ativo
BD --> Sistema : Match válido
Sistema --> App : Libera interface do chat

UsuarioA -> App : Envia mensagem
App -> Sistema : Envia mensagem criptografada
Sistema -> BD : Salva mensagem
Sistema -> Notif : Notificar participante
Notif --> UsuarioB : Recebeu nova mensagem

== 5. Combinação da Troca (RF-12) ==

UsuarioA -> App : Propõe local, data e horário
App -> Sistema : Envia proposta de encontro
Sistema -> Troca : Registrar proposta
Troca -> BD : Salvar dados do encontro
Sistema -> Notif : Notificar Usuário B
Notif --> UsuarioB : Recebeu proposta de encontro

UsuarioB -> App : Confirma proposta de encontro
App -> Sistema : Confirmar encontro
Sistema -> Troca : Atualizar status da troca
Troca -> BD : Salvar confirmação do encontro
Sistema -> Notif : Notificar Usuário A
Notif --> UsuarioA : Encontro confirmado por Usuário B

== 6. Conclusão e Transferência (RF-13) ==

UsuarioA -> App : Confirma realização da troca
UsuarioB -> App : Confirma realização da troca
App -> Sistema : Solicita conclusão
Sistema -> Troca : Validar confirmação dupla

alt Ambas confirmações recebidas
    Troca -> BD : Transação: Atualizar posse dos livros e liberar histórico
    BD --> Troca : Operação concluída com sucesso
    Troca --> Sistema : Troca finalizada

    Sistema -> Notif : Enviar confirmação e convite para avaliação
    Notif --> UsuarioA : Troca concluída! Opção de republicar e avaliar
    Notif --> UsuarioB : Troca concluída! Opção de republicar e avaliar

else Divergência registrada
    Troca -> BD : Registrar status de divergência
    BD --> Troca : Divergência cadastrada
    Troca --> Sistema : Encaminhar para moderação
    Sistema --> App : Exibir orientação de divergência
    App --> UsuarioA : Informações enviadas para análise
end

== 7. Avaliação (RF-14) ==

UsuarioA -> App : Avalia Usuário B
App -> Sistema : Envia nota e comentário
Sistema -> BD : Registrar avaliação e recalcular média
BD --> Sistema : Avaliação registrada
Sistema --> App : Confirma avaliação
App --> UsuarioA : Avaliação enviada

== 8. Histórico (RF-15) ==

UsuarioA -> App : Consulta histórico
App -> Sistema : Solicita histórico de trocas
Sistema -> BD : Buscar registros com paginação
BD --> Sistema : Histórico paginado
Sistema --> App : Retorna histórico
App --> UsuarioA : Exibe histórico de trocas

@enduml

sequenceDiagram
    autonumber
    actor A as 👤 Usuário A (Iniciador)
    actor B as 👤 Usuário B (Receptor)
    participant App as 📱 Aplicativo
    participant API as ⚙️ API / Gateway
    participant Auth as 🔐 Autenticação
    participant Livros as 📚 Serviço de Livros
    participant Match as 🤝 Serviço de Match
    participant Troca as 🔄 Serviço de Troca
    participant Notif as 🔔 Notificações
    participant BD as 🗄️ Banco de Dados

    rect rgb(240, 248, 255)
    note over A, BD: 1. Autenticação
    A->>App: Acessa o sistema
    App->>API: Solicita autenticação
    API->>Auth: Validar credenciais
    Auth->>BD: Consultar usuário
    BD-->>Auth: Dados do usuário
    Auth-->>API: Autenticação aprovada
    API-->>App: Sessão iniciada
    App-->>A: Exibe tela inicial
    end

    rect rgb(255, 250, 240)
    note over A, BD: 2. Descoberta de Livros
    A->>App: Busca livros disponíveis
    App->>API: Solicita livros da região
    API->>Livros: Buscar no raio definido
    Livros->>BD: Consultar disponíveis
    BD-->>Livros: Lista de livros
    Livros-->>API: Livros encontrados
    API-->>App: Exibe catálogo
    App-->>A: Apresenta livros
    end

    rect rgb(245, 255, 250)
    note over A, BD: 3. Interesse e Match
    A->>App: Demonstra interesse
    App->>API: Registra interesse
    API->>Match: Analisar reciprocidade
    Match->>BD: Consultar interesse recíproco

    alt Match formado
        BD-->>Match: Reciprocidade confirmada
        Match-->>API: Confirmar Match
        API->>Troca: Reservar livros atomicamente
        Troca->>BD: Bloquear os dois livros
        BD-->>Troca: Reserva confirmada
        Troca-->>API: Match Ativo
        API->>Notif: Notificar participantes
        Notif-->>A: Notifica Match
        Notif-->>B: Notifica Match
    else Interesse isolado
        BD-->>Match: Sem reciprocidade
        Match-->>API: Interesse registrado
        API->>Notif: Notificar proprietário
        Notif-->>B: Novo interesse no seu livro
    end
    end

    rect rgb(255, 245, 245)
    note over A, BD: 4. Chat e Negociação
    A->>App: Envia mensagem no chat
    App->>API: Envia mensagem
    API->>BD: Salva mensagem
    API->>Notif: Notificar destinatário
    Notif-->>B: Nova mensagem recebida
    end

    rect rgb(248, 248, 255)
    note over A, BD: 5. Conclusão e Transferência
    A->>App: Confirma troca presencial
    B->>App: Confirma troca presencial
    App->>API: Processa conclusão dupla
    API->>Troca: Executa transferência de posse
    Troca->>BD: Transação: altera proprietários
    BD-->>Troca: Registro atualizado
    Troca-->>API: Finalizado
    API->>Notif: Envia confirmação
    Notif-->>A: Troca concluída!
    Notif-->>B: Troca concluída!
    end