# Projeto Robô envio de alertas Telegram

## Solicitação do cliente

O cliente recebe alertas de vários sistemas de monitoramento, e gostaria de centralizar todos esses alertas em um único local, o Telegram.

Um dos problemas principais é que os alertas são enviados por e-mail, e o cliente gostaria de receber esses alertas em tempo real, sem precisar ficar verificando a caixa de e-mail.

Uma coisa importante é que o cliente quer o controle de versionamento do código, para que ele possa fazer alterações futuras e trabalhar com outros desenvolvedores, então ele pediu para que o código seja versionado no GitHub.

## Solução

Desenvolver um robô que leia a caixa de entrada do e-mail, personalize a mensagem e envie para o Telegram.

Vamos usar o Apache Hop para fazer a extração dos e-mails, e o JavaScript para fazer a personalização da mensagem e envio para o Telegram.

Vamos usar o Airflow para agendar a execução do robô.

## Arquitetura do Projeto

```mermaid
graph TD
    subgraph Infraestrutura Docker
        subgraph Orquestração Airflow
            D[Agendamento e Execução]
        end
        subgraph Apache Hop
            A[Caixa de Entrada do E-mail] -->|Apache Hop| B[Robô]
            B -->|Processamento de Dados| C[Envio ao Telegram]
            B -->|JavaScript| C
        end
    end
    D -->|Orquestrando Apache Hop| B
```

## Configuração

- ### Configuração do Telegram

1. Acesse o Telegram e procure por `BotFather`
2. Crie um novo bot com o comando `/newbot`
3. Copie o token gerado

- ### Configuração do Gmail

1. Acesse o Gmail e ative o acesso a aplicativos menos seguros
2. Copie o e-mail e senha
3. Ative a autenticação em dois fatores
4. Pesquise app password e crie uma senha para o Apache Hop
5. Ative o pop3 nas configurações do Gmail

- ### Configuração do Apache Hop

1. Use o step input email para ler os e-mails
configuração:
    - Protocolo: pop3
    - Servidor: pop.gmail.com
    - Porta: 995
    - Usuário: seu e-mail
    - Senha: senha do app que você criou la no Gmail
    - SSL: true