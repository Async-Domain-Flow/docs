![Async Domain Flow](https://i.imgur.com/C4Fe9p7.png)

# Async Domain Flow



No mundo do desenvolvimento de software, arquiteturas como a Hexagonal, a Limpa (Clean Architecture) e o MVC tornaram-se padrões pela promessa de organização e desacoplamento. No entanto, à medida que os sistemas escalam, uma ironia emerge: a mesma estrutura que deveria simplificar o código passa a gerar fadiga. Desenvolvedores gastam horas navegando em pastas como application, domain, infrastructure, adapters, e controllers apenas para alterar uma única regra de negócio. Esse cenário expõe uma lacuna nas arquiteturas tradicionais: a separação rígida por camadas técnicas, não por domínio. É nesse contexto que abordagens como o Configurable-Orchestration Domain Flow (CODF) ganham relevância, reorganizando o código em torno do domínio e substituindo a complexidade técnica pela agilidade empresarial.

O Problema das Arquiteturas Tradicionais: Camadas vs. Domínio
Arquiteturas como a Hexagonal e a Clean Architecture priorizam a separação entre regras de negócio (domínio) e detalhes técnicos (infraestrutura). Embora isso promova testabilidade e desacoplamento, introduz três problemas críticos:

Navegação Fragmentada: Para modificar um fluxo de pagamento, por exemplo, é necessário alterar entidades no diretório domain, casos de uso em application, adaptadores em infrastructure, e controllers em presentation. Cada camada exige saltos entre pastas, aumentando o risco de erros e a carga cognitiva.

Rigidez na Orquestração: A lógica de fluxo (ex: validar pagamento → debitar estoque → notificar cliente) é codificada em serviços ou casos de uso. Alterar a ordem das etapas ou adicionar comportamentos (como retentativas) demanda recompilação e reimplantações.

Cross-Cutting Concerns Espalhados: Funcionalidades transversais (logging, autenticação) são implementadas em camadas distintas, muitas vezes duplicadas ou acopladas a frameworks específicos (ex: middlewares no Spring).

O resultado é um sistema onde a estrutura técnica sobrepõe-se à lógica de negócio, dificultando a evolução ágil.

Em um mundo onde a tecnologia evolui a um ritmo acelerado e as demandas de negócios são cada vez mais dinâmicas, a capacidade de escalar sistemas de forma eficiente e sem interrupções torna-se um superpoder. A abordagem Configurable-Orchestration Domain Flow (CODF) com o uso de decorators oferece uma via poderosa para transformar e escalar sistemas legados, garantindo resiliência, performance e gerenciabilidade, tudo isso sem alterar uma linha de código existente.



## Cross-Cutting Concerns: O que São e Exemplos no Mundo Real

Cross-Cutting Concerns (Preocupações Transversais) são funcionalidades que afetam múltiplos componentes de um sistema, transcendendo limites de módulos ou camadas. Elas não pertencem a um domínio específico, mas são necessárias em vários contextos. Exemplos clássicos incluem logging, autenticação, cache, e gerenciamento de transações. Se mal implementadas, essas preocupações podem levar a código duplicado, alto acoplamento e dificuldades de manutenção.

A abordagem Configurable-Orchestration Domain Flow (CODF) resolve problemas de cross-cutting concerns ao:

Desacoplar comportamentos transversais via decoradores configuráveis (ex: @log, @retry).

Centralizar a definição em arquivos externos (YAML/JSON), evitando poluição do código de domínio.

Permitir reutilização de preocupações transversais em múltiplos fluxos (ex: logging aplicado a pagamentos e pedidos).

## VALIDAR ESSE TEXTo
Esse é um projeto que tende a aplicar uma arquitetura um pouco diferente das existentes, que nada mais é que uma mescla das melhores partes de cada uma, essa arquitetura é voltada

Essa arquitetura pode sewr enquadrada em algumas baseadas em Orquestração, Domínio, Eventos e @Decorators, elas são:

Interceptor Pattern – Quando os decorators são utilizados para interceptar chamadas e adicionar lógica antes ou depois da execução de um método. Isso é muito comum em frameworks que implementam middlewares.

Pipeline Pattern – Se os decorators forem aplicados em cadeia para transformar ou processar os dados, pode ser considerado um padrão de pipeline.

Aspect-Oriented Programming (AOP) – Quando o uso de decorators serve para injetar lógica transversal, como logging, métricas e validações, sem modificar a lógica principal.

Domain Event Interceptor – Em uma arquitetura orientada a eventos, pode-se usar decorators para capturar e modificar eventos antes que eles cheguem aos handlers de domínio.

Cross-Cutting Concerns Handler – Em DDD (Domain-Driven Design), se os decorators estiverem gerenciando aspectos como segurança, validação e auditoria, eles podem ser considerados handlers de cross-cutting concerns.

Essa arquitetura é mais um sistema de execução de fluxos escaláveis, podendo ser síncronos ou assíncronos, baseados em eventos ou em funções. Quam vai definir isso é o usuário, mas ele deverá configurar manualmente as modifiucações das configurações default.

Esse projeto é fruto de um muito estudo em arquiteturas descentralizas e assíncronas, porém eu náo quis deixar essa solução presa apenas para sistemas que querem der descentralziados.


Agregação de Cross-Cutting Concerns
Você encapsulou múltiplas preocupações técnicas em um único decorator @AutoScaleOrchestrator, incluindo:

Escalabilidade Automática (cpuThreshold, ramThreshold).

Segurança (AuthGuardJWT, CSRFGuard).

Validação (Validate(PedidoSchema)).

Resiliência (CircuitBreakerProtect, Bulkhead, Timeout).

Mensageria (MessageEnqueue, QueueConsumer).

Sincronização/Async (SyncMessage).

## Por que do nome "Async Domain Flow"?

O nome Async Domain Flow reflete os três pilares fundamentais desta arquitetura:

Async (Assíncrono)
O primeiro elemento do nome, Async, enfatiza o uso nativo de eventos e processamento assíncrono no fluxo de execução do sistema. Diferente de arquiteturas tradicionais que operam predominantemente de forma síncrona, o Agentic Domain Flow prioriza a comunicação baseada em eventos para garantir escalabilidade, desacoplamento e eficiência.

Por que o foco em assíncrono?

Permite alta escalabilidade sem bloqueios, aproveitando filas de mensagens e eventos distribuídos.
Melhora a resiliência, pois falhas pontuais em um serviço não afetam todo o sistema.
Reduz latência, pois partes do sistema podem processar eventos em paralelo.

## Configurable-Orchestration Domain Flow with Decorators 

É uma abordagem arquitetônica que combina orquestração configurável, fluxos de domínio específicos e padrões de decoradores para criar sistemas flexíveis e modulares. Vamos explorar cada componente e sua integração:

### 1. Conceitos Fundamentais
a. Configurable-Orchestration
Orquestração: Coordenação de serviços, componentes ou tarefas para executar um fluxo de trabalho complexo.

Configurável: A definição do fluxo é externa ao código (ex: YAML, JSON), permitindo ajustes sem reimplantações.

Exemplo: Um pipeline de processamento de dados onde a ordem das etapas é definida em um arquivo de configuração.

b. Domain Flow
Domínio: Contexto de negócio específico (ex: finanças, saúde).

Fluxo de Domínio: Sequência de etapas que resolvem um problema no domínio, seguindo princípios de Domain-Driven Design (DDD).

Exemplo: Em um sistema de pagamentos, o fluxo pode ser: validação → processamento → notificação.

c. Decorators
Padrão de Projeto: Adiciona responsabilidades a objetos/funções dinamicamente.

Uso Típico: Cross-cutting concerns (ex: logging, autenticação, cache).

### 2. Integração dos Componentes
A abordagem une os três conceitos:

Orquestração Configurável: Define a sequência de etapas do fluxo de domínio via configuração.

Decoradores Dinâmicos: Comportamentos adicionais (ex: retentativas) são injetados nos passos do fluxo com base na configuração.

Separação de Responsabilidades: Lógica de negócio (domínio) é isolada de preocupações técnicas (ex: logging).


### 3. Implementação Prática
Passo a Passo:
Definir o Fluxo de Domínio:

Criar funções que implementem suas regras de negócio, implementar Decorators reutilizáveis ou deixar que o sistema os implemente por você, cada etapa do fluxo utilizará alguns Decorators por padrão podendo ser desligados via configuração.

Configuração (YAML/JSON):

### 4. Casos de Uso

Microserviços: Adicionar autenticação ou rate-limiting a endpoints via configuração.

Pipelines de Dados: Injetar etapas de validação ou transformação dinamicamente.

Sistemas de Pagamento: Retentativas em falhas de conexão com gateways externos.

5. Vantagens
Flexibilidade: Comportamentos são alterados via configuração.

Manutenção Simplificada: Separação clara entre lógica de negócio e técnicalidades.

Reuso: Decoradores são compartilhados entre múltiplos fluxos.



6. Desafios
Ordem dos Decoradores: A sequência pode afetar o resultado (ex: autenticação antes de logging).

Gerenciamento de Configurações: Validação de configurações inválidas.

Performance: Muitos decoradores podem introduzir overhead.
Minha ideal foi desenvolver uma arquitetura descentralizada e assíncrona que fosse simples para migrar para microserviços e que funcionasse nos 2 mundos.

Escolhi Unir DDD, Hexagonal, Event-Driven e Agents em uma Arquitetura Assíncrona Domain-Based?

Eu estou estudando bastante sobre sistemas distribuídos e assíncronos e como já utilizava DDD e IA, pensei em unir elas, com um a pitada de Hexagonal para criar uma arquitetura Domain-Event-Async-based, tentei unir os melhores princípios arquiteturais para garantir baixo acoplamento, reatividade, flexibilidade e um domínio forte e independente. A combinação de DDD (Domain-Driven Design), Arquitetura Hexagonal, Event-Driven Architecture (EDA) e Agents oferece uma abordagem poderosa para lidar com sistemas complexos e altamente distribuídos.

## O que Cada Pilar Contribui para a Arquitetura?

### DDD (Domain-Driven Design) – Fortalecendo o Domínio

#### Por que?

Separa a lógica de negócio da infraestrutura, evitando acoplamento excessivo.
Modela as regras de negócio de forma clara, organizando entidades, agregados e eventos de domínio.
Garante que a aplicação seja construída em torno do linguajar ubíquo, tornando-a mais compreensível e alinhada ao negócio.

#### Como é usado?

Toda mudança de estado de uma entidade gera um evento de domínio (user.created, order.completed).
Agents interagem diretamente com entidades e agregados para garantir que a lógica de negócio seja mantida dentro do Domain Layer.
Cada serviço tem seu bounded context, garantindo que domínios distintos não se sobreponham.

### Arquitetura Hexagonal – Adaptabilidade e Baixo Acoplamento

#### Por que?

Separa interface (portas) e infraestrutura (adaptadores), permitindo que a aplicação mude de banco de dados, mensageria ou API sem impactar o domínio.
Define entradas e saídas claras, facilitando integração e testes.
Garante que a lógica de negócio não dependa de detalhes técnicos, como um banco SQL ou NoSQL.

#### Como é usado?

O Domain Layer não tem conhecimento de tecnologias externas (bancos, APIs, mensageria).
A Agent Layer atua como intermediária, garantindo que infraestrutura (Tools Layer) possa ser trocada dinamicamente via configuração (.env).
Orquestradores e adaptadores são criados na Tools Layer, conectando a infraestrutura ao domínio.


### Event-Driven Architecture (EDA) – Reatividade e Escalabilidade

#### Por que?

Em sistemas distribuídos, eventos desacoplam serviços, permitindo escalabilidade e resiliência.
Mensagens assíncronas garantem que nenhum serviço bloqueie outro, melhorando a eficiência.
Facilita event sourcing, garantindo rastreabilidade e auditoria.

#### Como é usado?

Toda alteração em um agente gera eventos, como user.created.queue, order.completed.queue.
Filas e tópicos de eventos (RabbitMQ, Kafka, BullMQ) garantem que cada agente reaja dinamicamente.
Saga Patterns pode ser implementados para orquestrar transações distribuídas.

Essa arquitetura serve tanto para sistemas uni-servidor que desejam maior reatividade e escalabilidade


### Configurações - Padrão

O sistema já vem pré-configurado com as seguintes configurações:

```
APP_NAME=EventForge

SERVER_IP=localhost
SERVER_CORS=localhost
SERVER_CLUSTER=kubernets
SERVER_CPUS=1 # por intancia de replica
SERVER_RAM="512"
SERVER_CLUSTER_INSTANCESS=3
SERVER_NETWORK=EventForge
SERVER_SESSION=memory # cache

STORAGE_SERVICE=true # roda a camada de Storage em nós/servidores separados
STORAGE_CQRS=true # separa banco de escrita do banco de leitura
STORAGE_AUTO_SCALING=false # implementado apenas quando se usao MongoDb na WRITE e READ
STORAGE_READ=mongodb # postgres
STORAGE_WRITE=postgres # mongodb
STORAGE_CACHE=redis # elasticsearch
STORAGE_LOG=elasticsearch
STORAGE_REPLICA_SET=true #false
STORAGE_SHARDING=false # implementado apenas quando se usao MongoDb na WRITE e READ
STORAGE_SOFT_DELETE=true
STORAGE_AUDIT=true
STORAGE_RETRY=true
STORAGE_CIRCUITBREAKER=true
STORAGE_FAILOVER=true
STORAGE_ACID=true


FLOW_COMMUNICATION=event # direct | websocket | gRPC



MESSAGE_BROKERS_EVENTS=rabbitmq # kafka | NATS | redpanda
MESSAGE_BROKERS_JOB=bullmq
MESSAGE_BROKERS_REALTIME=nats # kafka
MESSAGE_BROKERS_DLQ=true
MESSAGE_BROKERS_CLUSTER=true
MESSAGE_BROKERS_MESSAGING_DURABLE=true
MESSAGE_BROKERS_MESSAGING_ROLLBACK=true

SECURITY_JWT_SECRET=mnudsfjSUIHNSI
SECURITY_CSRF_HEADER=event-forge-csrf

METRICS_API=true
METRICS_REALTIME=true
METRICS_LOG=true
METRICS_TRACING=true
METRICS_AUDIT=true
METRICS_ERRORS=true
METRICS_SENTRY_DSN=#obrigatório adicionar a chave ou colocar METRICS_ERRORS=false

EVENT_SOURCING=true


GATEWAY_API_SYNC=true
GATEWAY_CRUD=true
GATEWAY_SMARTCACHE=true
GATEWAY_ROUTE_LIMITER_WINDOW=30000
GATEWAY_BULLHEAD=10
GATEWAY_RATELIMIT_MAX_REQUEST=10
GATEWAY_RATELIMIT_WINDOW_MS=1000
GATEWAY_CIRCUITBREAKER_THRESHOLD=10
GATEWAY_CIRCUITBREAKER_THRESHOLD=30
GATEWAY_FAILOVER=true # o sistema sobe um cluster do sistema
GATEWAY_TIMEOUT=30000
GATEWAY_IDEMPOTENT=true
GATEWAY_THROTTLE=100
GATEWAY_CSRF=true
GATEWAY_VALIDATE_SCHEMA=true
GATEWAY_AUTOSCALESENTINEL_CPU_THRESHOLD=75
GATEWAY_AUTOSCALESENTINEL_RAM_THRESHOLD=80
GATEWAY_AUTOSCALESENTINEL_MAX_WOKERS=80
GATEWAY_AUTOSCALESENTINEL_RAM_THRESHOLD=2000
GATEWAY_AUTOSCALESENTINEL_RAM_THRESHOLD=[]


ENTITY_DATA_PROCESSIG_SERVICE=true
ENTITY_ORCHESTRATOR_SERVICE=false
ENTITY_USECASE_SERVICE=false
ENTITY_SERVICE_SERVICE=false
ENTITY_OBSERVER_SERVICE=false
ENTITY_SIGNAL_AGENT=true
ENTITY_OBSERVER_AGENT=true
ENTITY_EVENTS_NAMES_DEFAULT=true

ORCHESTRATOR_SERVICE=false
ORCHESTRATOR_DOORMAN=true
ORCHESTRATOR_AUTOSCALESENTINEL=true
ORCHESTRATOR_CIRCUITBREAKER=true
ORCHESTRATOR_TIMEOUT=true
ORCHESTRATOR_FAILOVER=true
ORCHESTRATOR_RETRY=true
ORCHESTRATOR_METRICS_TRACING=true
ORCHESTRATOR_METRICS_ERRORS=true
ORCHESTRATOR_METRICS_LOG=true
ORCHESTRATOR_METRICS_AUDIT=false
ORCHESTRATOR_AUTOSCALESENTINEL=true
ORCHESTRATOR_CIRCUITBREAKER=true
ORCHESTRATOR_TIMEOUT=true
ORCHESTRATOR_FAILOVER=true
ORCHESTRATOR_LOG=true
ORCHESTRATOR_RETRY=true

USECASE_SERVICE=false
USECASE_AUTOSCALESENTINEL=true
USECASE_CIRCUITBREAKER=true
USECASE_TIMEOUT=true
USECASE_FAILOVER=true
USECASE_RETRY=true
USECASE_METRICS_TRACING=true
USECASE_METRICS_ERRORS=true
USECASE_METRICS_LOG=true
USECASE_METRICS_AUDIT=false
USECASE_AUTOSCALESENTINEL=true
USECASE_CIRCUITBREAKER=true
USECASE_TIMEOUT=true
USECASE_FAILOVER=true
USECASE_LOG=true
USECASE_RETRY=true

SERVICE_SERVICE=false
SERVICE_AUTOSCALESENTINEL=true
SERVICE_CIRCUITBREAKER=true
SERVICE_TIMEOUT=true
SERVICE_FAILOVER=true
SERVICE_RETRY=true
SERVICE_METRICS_TRACING=true
SERVICE_METRICS_ERRORS=true
SERVICE_METRICS_LOG=true
SERVICE_METRICS_AUDIT=false
SERVICE_AUTOSCALESENTINEL=true
SERVICE_CIRCUITBREAKER=true
SERVICE_TIMEOUT=true
SERVICE_FAILOVER=true
SERVICE_LOG=true
SERVICE_RETRY=true


```


O 

## 🔑 **5. Benefícios da Arquitetura**

| **Recurso**          | **Descrição** |
|----------------------|------------------------------------------------|
| **Suporte WebSocket** | Eventos processados em tempo real |
| **Suporte gRPC** | Comunicação eficiente entre serviços |
| **Execução Paralela** | Agentes podem processar eventos simultaneamente |
| **Orquestração Modular** | Fluxos configuráveis para diferentes tipos de eventos |
| **Alta Escalabilidade** | Integração simplificada para múltiplos serviços |



O primeiro tipo de configuração que você poderá

Tudo começa com a configuração do Domínio das suas Entidades, você deverá definir todos os campos e suas validações e é possível definir quais campos serão retornados em uma pesquisa, o sistema irá geral os DTOs e o Model de cada Entidade.

Para cada Entidade você poderá definir se ela irá se comunicar via


### Agents Layer – Orquestração Inteligente e Independência

#### Por que?

Garante que a lógica de domínio seja processada de forma isolada e (a)ssíncrona.
Cada Agent tem um papel específico, evitando sobrecarga de responsabilidade.
Orquestra a comunicação entre diferentes partes da aplicação, garantindo atomicidade e consistência eventual.

#### Como é usado?

Execution Agents executam ações do domínio (ex.: OrderAgent processa pedidos).
Signal Agents emitem eventos (UserSignalAgent publica eventos de criação e atualização).
Orchestrator Agents gerenciam fluxos de longa duração, como transações distribuídas.
Cognitive Agents aplicam inteligência de negócios/artificial para otimizar decisões.
Observer Agents fazem o serviço de logger no sistema persistindo os logs em qualquer tipo de storage.

Alguns Agents padrão da minha arquitetura:

- ${Entity}SchemaValidatorAgent
- ${Entity}DataProcessorAgent
- ${Entity}DataMapperAgent
- ${Entity}OrchestratorAgent
- ${Entity}SignalAgent
- ${Entity}CognitiveAgent
- ${Entity}ObserverAgent
- ${Entity}${Case}UseCaseAgent
- ${Entity}${Action}ServiceAgent
- StorageAgent
- InfrastrutureObserverAgent
- InfrastructureMetricsAgent
- InfrastructureCognitiveAgent
- InfrastructureSignalAgent

O O InfrastructureCognitiveAgent será responsável por analisar o uso da infraestrutura (CPU, RAM, Workers, Pods, etc.), prever demandas futuras e escalar automaticamente os recursos no Kubernetes antes que ocorra sobrecarga. Tentarser mais proativo do que reativo, para a atividade existe o InfrastructureOrchestratorAgent






### Domínio: Processamento de Pedidos
Fluxo: validar_pedido →  → criar pedido → espera pagamento → reservar_estoque → enviar

```ts
import { PedidoSchema } = "domains/Pedido/PedidoSchema";

class PedidoController {
  @AutoScaleOrchestrator(
    { cpuThreshold: 70, ramThreshold: 80, maxWorkers: 8 },
    AuthGuardJWT(JWT_SECRET),
    CSRFGuard,
    SchemaValidator(PedidoSchema),
    CircuitBreakerProtect(3, 60000),
    RateLimit(3, 1000),
    Bulkhead(10),
    Timeout(10000),
    MessageEnqueue("order.create.queue"),
    QueueConsumer({
      "success.order.validate.queue": MessageEnqueue("order.create.queue"),
      "success.order.create.queue": MessageEnqueue("waiting.order.payment.queue"),
      "success.order.payment.queue": MessageEnqueue("order.stock.allocated.queue"),
      "success.order.stock.allocated.queue": MessageEnqueue("waiting.order.delivery.queue"),
      "success.order.delivery.queue": MessageEnqueue("success.order.flux.queue"),
      "success.order.flux.queue": MessageEnqueue("success.create.queue"),
    }),
    SyncMessage("success.order.create") // Aguarda resposta síncrona
  )
  async createPedido(payload: any) {
    return payload; // O decorator pega o payload e publica na fila
  }
}
```

Mas que também pode ser reduzido a:

```ts
import { PedidoSchema } = "domains/Pedido/PedidoSchema";

class PedidoController {
  @AutoScaleOrchestrator(
    { cpuThreshold: 70, ramThreshold: 80, maxWorkers: 8 },
    SECURITY,
    VALIDATOR(PedidoSchema),
    RESILIENCE,
    AUTOSCALLING,
    MessageEnqueue("order.create.queue"),
    QueueExecutor({
      ["success.order.validate.queue", "order.create.queue"],
      ["success.order.create.queue", "waiting.order.payment.queue"],
      ["success.order.payment.queue", "order.stock.allocated.queue"],
      ["success.order.stock.allocated.queue", "waiting.order.delivery.queue"],
      ["success.order.delivery.queue", "success.order.flux.queue"],
      ["success.order.flux.queue": "success.order.create.queue"],
    }),
    SyncMessage("success.order.create") // Aguarda resposta síncrona
  )
  async createPedido(payload: any) {
    return payload; // O decorator pega o payload e publica na fila
  }
}
```

Ou apenas:

```ts
import { PedidoSchema } = "domains/Pedido/PedidoSchema";

class PedidoController {
  @AutoScaleOrchestrator(,
    API,
    MessageEnqueue("order.create.queue"),
    QueueExecutor({
      ["success.order.validate.queue", "order.create.queue"],
      ["success.order.create.queue", "waiting.order.payment.queue"],
      ["success.order.payment.queue", "order.stock.allocated.queue"],
      ["success.order.stock.allocated.queue", "waiting.order.delivery.queue"],
      ["success.order.delivery.queue", "success.order.flux.queue"],
      ["success.order.flux.queue": "success.order.create.queue"],
    }),
    SyncMessage("success.order.create") // Aguarda resposta síncrona
  )
  async createPedido(payload: any) {
    return payload; // O decorator pega o payload e publica na fila
  }
}
```

Essa é apenas uma das formas de definir um fluxo nessa arquitetura, caso você deseje de uma gerenciamento mais específico de cada fluxo você pode definir cada etapa do fluxo.
Além de poder definir se o fluxo será síncrono ou Assíncrono e se serão via Eventos ou Chama de função, caso você deseje usar uma configuração mais simples poderá definir via JSON e minha arquitetura irá executar o fluxo da maneira mais segura e escalável possível, você tem a possibilidade de desligar os Guardrails que desejar. O mesmo exemplo anterior ficaris assim:

```ts
{
  "gateways": {
    "routes": {
      "POST /orders": [
        "OrderCreate",
        "ValidatePayment",
        "AllocateStock",
        "DeliveryOrder"
      ],
    }
  }
}
```

Uma das coisas que você deve ter notado é que não definimos o Validator no fluxo, isso por que ele é automático em qualquer rota que receba dados


```ts
{
  "gateways": {
    "routes": {
      "POST /orders": {
        "async":[
          "DefaultResponse",
          "OrderCreate",
          "ValidatePayment",
          "AllocateStock",
          "DeliveryOrder"
          ]
        },
      }
    }
}
```

Agora podemos definir que a rota é assíncrona e definimos uma função de resposta padrão.


E também temos a possibilidade de definir um fluxo bem mais complexo com várias características

```js
export const AgentConfig = {
  syncMode: true,
  gateway: {
    "type": "api", //websocket | filas | gRPC
    "method": "post",
    "/messages",
  },
  "OrchestratorAgent": {
    "execute": (payload) => {
        switch (messageType):{
          case "text": {[
            "ResponseWithAutoMessageAgent",
            "TranslateMessage?",
            "SentimentAnalyze",
            "StoreAgent"
          ]},
          case "audio": {[
            "TranscribeAudio",
            "TranslateMessage?",
            "SentimentAnalyze",
            "StoreAgent"
          ]},
          break;
          default: {
            "StoreAgent"
          }
        }
    };
  }
}
```


```js
// src/config/AgentConfig.ts
export const AgentConfig = {
  syncMode: true, // true = síncrono, false = assíncrono
  // Configuração do Fluxo por Tipo de Mensagem
    OrchestratorAgent: {
      type: "api",
      // Definição do Fluxo baseado no tipo de mensagem
      entryPoint: "/messages",
      method: "POST",
      execute: (payload: any) => {
        const { messageType } = payload;

        switch (messageType) {
          case "text":
            return {
              sequence: ["SchemaValidator", "UseCaseAgent"], // Executa em sequência
              parallel: ["ResponseWithAutoMessageAgent", "StorageAgent"], // Executa em paralelo
            };

          case "audio":
            return {
              sequence: ["DownloadAudioAgent", "TranscribeServiceAgent"],
              parallel: ["TranslateServiceAgent", "StorageAgent"],
            };

          case "image":
            return {
              sequence: ["DownloadImageAgent", "VectorizeDocumentAgent"],
              parallel: ["StorageAgent"],
            };

          default:
            return {
              sequence: [],
              parallel: [],
            };
        }
      },
    },
  },

```

Como você pode configurar uma sequência que no meio tenha um fluxo paralelo:

```js
// src/config/AgentConfig.ts
export const AgentConfig = {
  syncMode: true, // true = síncrono, false = assíncrono
  // Configuração do Fluxo por Tipo de Mensagem
    OrchestratorAgent: {
      type: "api",
      // Definição do Fluxo baseado no tipo de mensagem
      execute: (payload: any) => {
        const { messageType } = payload;

        switch (messageType) {
          case "text":
            return {
              sequence: ["SchemaValidator", "UseCaseAgent"], // Executa em sequência
              parallel: ["ResponseWithAutoMessageAgent", "StorageAgent"], // Executa em paralelo
            };

          case "audio":
            return {
              sequence: [
                "DownloadAudioAgent", [
                  "TranscribeServiceAgent", "TranslateServiceAgent"
                ], 
                "StorageAgent"
              ],
            };

          case "image":
            return {
              sequence: ["DownloadImageAgent", "VectorizeDocumentAgent"],
              parallel: ["StorageAgent"],
            };

          default:
            return {
              sequence: [],
              parallel: [],
            };
        }
      },
    },
  },

```

Assim como você pode definir na Entidade o CRUD ativado que ele sempre executará no final do fluxo o StrogeAgent fazendo create no POST, update no PUT/PATCH, findAll no GET /, findBy /propriedade/:propriedade e remove DELETE


## Domain - Entities

Nessa arquitetura você define seu domínio via JSON com diversas possibilidades, como padrão todas as instâncias possuem uma replicaimplememtei as ténicas de CQRS para a separação automática do Banco de leitura do Banco de escrita. Eu escolhi como padrão para a escrita o Postgres pois ele bem versátil e e tem transações ACID, mas ele também possui a funcionalidade de Replicação Read-Only, então você tem a possibilidade de escolher o Postrges para ReadDB e WriteDB, assim como o MongoDb, porém com o MongoDB você também pode ativar a funcionalidade de Sharding. E para o Cache temos o Redis e o ElasticSearch.

- Storage:
  - ReadDB: padrâo MongoDB
  - WriteDB: padrão Postgres
  - Cache: Redis

A camada da Storage é um serviço em separado que só pode ser acessado via Eventos


## Agents
Nossos Agentes desempenham papéis diferentes em diferentes camada, a seguir elenco todas seus possíveis nomes, não é necessário usar todos, a lista é apenas para você ter uma ideia do que um Agente é capaz. Eles são definidos em:

1. Comando e Fluxo de Caso de Uso (UseCase)
- Executor: Executa a ação principal do domínio.
- Orchestrator: Coordena múltiplas operações em um fluxo lógico.
- Invoker: Inicia a execução de uma ação específica.
- Operator: Realiza a operação de domínio esperada.
- Handler: Trata a lógica de entrada para uma funcionalidade.
- Trigger: Dispara um processo ou evento com base em uma ação.

Exemplo: CreateUserExecutor, SendMessageInvoker

2. Processamento de Dados (Transformação e Validação)
Processor: Processa dados de entrada e saída.
Sanitizer: Garante que os dados estejam formatados e seguros.
Validator: Valida as regras de negócio antes da execução.
Transformer: Converte dados entre camadas.
Enricher: Adiciona dados complementares ao processo.
Exemplo: UserDataProcessor, MessageSanitizer

3. Gerenciamento de Estado e Fluxo (Estado de Domínio)
Coordinator: Gerencia o estado entre múltiplos serviços.
Supervisor: Garante a consistência de uma transação complexa.
Controller: Controla o fluxo de ações.
Dispatcher: Direciona as ações para os agentes corretos.
Mediator: Atua como intermediário entre diferentes processos.
Exemplo: OrderFlowCoordinator, PaymentDispatcher

Execução Assíncrona e Eventos
Publisher: Publica eventos para outros agentes.
Subscriber: Escuta eventos e reage a eles.
Scheduler: Agenda tarefas para execução futura.
Listener: Observa mudanças e dispara ações.
Watcher: Monitora recursos em tempo real.
Exemplo: OrderEventPublisher, UserCreationListener

5. Segurança e Acesso
Guard: Protege recursos sensíveis.
Authenticator: Verifica identidade do usuário.
Authorizer: Garante permissões corretas.
Protector: Impede ações não autorizadas.
Sentinel: Monitora tentativas suspeitas.
Exemplo: JWTAuthenticator, UserAccessGuard

6. Integração com Infraestrutura (Externa e Interna)
Connector: Estabelece conexão com serviços externos.
Adapter: Adapta diferentes interfaces de APIs.
Bridge: Conecta diferentes tecnologias.
Proxy: Age como intermediário controlado.
Integrator: Junta múltiplos serviços em um só fluxo.
💡 Exemplo: WhatsAppConnector, PaymentAdapter

7. Regras de Negócio e Decisões (Domínio)
RuleEngine: Avalia regras e toma decisões.
Decider: Escolhe entre diferentes caminhos.
Resolver: Resolve conflitos ou ambiguidades.
Advisor: Sugere o melhor curso de ação.
Evaluator: Calcula resultados baseados nas regras.
xemplo: PaymentRuleEngine, DiscountEvaluator


1. Orchestrator para a Plataforma (Infraestrutura)
O que faria:

Gerenciar os serviços de infraestrutura, como banco de dados, mensageria, cache, e armazenamento.
Monitorar a saúde das conexões, provisionar e liberar recursos conforme a demanda.
Aplicar automações como auto-healing, escalonamento e reinicialização em caso de falhas.
Vantagens:
Facilidade de gerenciar ambientes (local, staging, produção).
Reduz o acoplamento entre serviços e infraestrutura.
Permite integrações dinâmicas, como troca de banco de dados sem alterar código.

 Um Platform Orchestrator pode ser responsável por gerenciar os serviços de infra, garantindo provisionamento dinâmico, monitoramento e automação.

2. Cognitive Agent para Infraestrutura
O que faria:

Aplicar inteligência nas decisões de infraestrutura.
Analisar métricas de desempenho e sugerir ajustes automáticos.
Detectar anomalias e gerar alertas proativos.
Exemplo:

Se a mensageria estiver sobrecarregada, sugerir aumentar as filas.
Se um banco estiver lento, recomendar redistribuir a carga ou ajustar índices.
Conclusão: O Cognitive Agent não substitui o Orchestrator, mas complementa.
O Orchestrator gerencia recursos, enquanto o Cognitive analisa, sugere e automatiza decisões baseadas em dados.

Orchestrator para Autenticação
O que faria:

Centralizar o controle de autenticação e autorização.
Gerenciar tokens, sessões, e integração com IAM (ex.: Keycloak, Auth0).
Aplicar políticas de segurança (MFA, rate limiting, bloqueio após falha).
Vantagens:
 Unifica a lógica de segurança em um só lugar.
Melhora a escalabilidade ao delegar autenticação para um serviço dedicado.
Facilita a auditoria e o rastreamento de acessos.

Desvantagens:
Introduz latência adicional em cada requisição se mal configurado.
Dependência centralizada: se o orquestrador cair, a autenticação para.

Conclusão: Sim, um Auth Orchestrator é uma excelente prática para controlar autenticação em ambientes distribuídos. Se bem configurado, melhora a segurança sem comprometer a performance.



Vou te dar um exemplo simples de uma implementação em Ecommerce:

```ts
// agents/orchestrationAgent.ts
import { EventBus } from "../communications/eventBus";

export class OrchestrationAgent {
  private domain: string;

  constructor(domain: string) {
    this.domain = domain;
  }

  public handleEvent(eventType: string, payload: any): void {
    console.log(`[${this.domain}] Handling event: ${eventType}`);

    switch (eventType) {
      case "order.event.create":
        EventBus.publish("OrderCreated", payload);
        break;
      case "prodcut.event.add":
        EventBus.publish("ProductAdded", payload);
        break;
      case "payment.task.process":
        EventBus.publish("PaymentProcessed", payload);
        break;
      default:
        console.warn(`No handler defined for event type: ${eventType}`);
    }
  }
}
```

```ts
// src/agents/OrderExecutor.ts
import { Cart } from "../domain/entities/Cart";
import { PaymentService } from "../services/PaymentService";
import { OrderRepository } from "../repositories/OrderRepository";
import { PublisherAgent } from "@/shared/agents/publishers/KafkaPublisher";
import { OrderConfig } from "../domain/entities/configs/OrderConfig";

const ORDER_EVENTS = {
  start: "order.queue.execute.begin",
  fail: "fail.order.queue.execute",
  saved: "order.queue.saved"
}

export class OrderExecutor {
  constructor(
    private readonly paymentService: PaymentService,
    private readonly orderRepo: OrderRepository;
    private readonly publisherAgent: PublisherAgent;
  ) {
    this.publisherAgent = new PublisherAgent(OrderConfig)
  }

  async execute(cart: Cart, paymentDetails: any): Promise<void> {
    console.log("🛒 Iniciando execução do pedido...");

    publisherAgent.publish(ORDER_EVENTS.start, {entity: cart, details: paymentDetails})
    const paymentResult = await this.paymentService.processPayment(paymentDetails);
    if (!paymentResult.success) {

    publisherAgent.publish(ORDER_EVENTS.fail, {entity: cart, details: paymentDetails})
      throw new Error("Pagamento falhou!");
    }

    const order = {
      id: `order_${Date.now()}`,
      items: cart.items,
      total: cart.total,
      status: "Pago",
      createdAt: new Date(),
    };

    this.orderRepo.insertOne(order);
    console.log("✅ Pedido executado com sucesso:", order);
    
    publisherAgent.publish(ORDER_EVENTS.saved, {entity: cart, details: paymentDetails})
  }
}
```


📌 
## 🔑 Como Cada Técnica é Aplicada em Cada Agente

| **Técnica**            | **Execution Agent** 🟢 | **Signal Agent** 📣 | **Coordinator Agent** 🔄 | **Observer Agent** 👀 | **Cognitive Agent** 🤖 |
|------------------------|--------------------------|----------------------|---------------------------|-------------------------|------------------------|
| **Event Sourcing**     | Garante histórico das ações. | Emite eventos após execução. | Orquestra eventos em sequência. | Registra cada evento processado. | Usa eventos para treinar modelos. |
| **CQRS**               | Processa comandos (Write). | Publica eventos (Read). | Separa comandos e eventos. | Monitora projeções de leitura. | Usa dados para análise preditiva. |
| **Saga Pattern**       | Executa cada etapa da transação. | Emite sinais entre as etapas. | Garante consistência com compensação. | Observa falhas e reprocessamentos. | Toma decisão de rollback. |
| **Circuit Breaker**    | Evita sobrecarga nas APIs. | Impede notificações em cascata. | Isola falhas entre etapas. | Monitora estado do circuito. | Decide continuar ou abortar. |
| **Retry Policy**       | Tenta reexecutar após falha. | Reenvia sinal se falhar. | Reexecuta transação com backoff. | Registra tentativas e falhas. | Decide se o retry é viável. |
| **DLQ (Dead Letter)**  | Envia tarefa falha para a fila. | Encaminha eventos não entregues. | Registra falhas em longa duração. | Monitora mensagens enviadas para DLQ. | Decide reprocessar ou arquivar. |
| **Idempotência**       | Garante execução única. | Emite sinal único por evento. | Evita repetição nas etapas da Saga. | Registra eventos duplicados. | Detecta duplicidade nos dados. |
| **Observabilidade**    | Registra logs locais. | Emite eventos rastreáveis. | Garante rastreamento entre serviços. | Logging, Tracing e Métricas. | Registra métricas de inferência. |
| **Consistência**       | Confirma ou aborta transação. | Emite eventos síncronos ou assíncronos. | Define fluxo como atômico ou eventual. | Monitora status das transações. | Decide melhor modo de consistência. |

Essa é a estrutura 

domain-agents/
├─ agent-core/               # Base genérica para qualquer agente
│  ├─ state/                 # Gerenciamento de estado
│  ├─ cqrs/                  # Comandos (execute) e consultas (query)
│  ├─ events/                # Event Sourcing e publicação de eventos
│  ├─ policies/              # Circuit Breaker, Retry, DLQ
│  ├─ observability/         # Logs, métricas e tracing
│  └─ config/                # Configurações de ambiente (atômico/eventual)
│  └─ tools/                 # Funcionalidades externas

│
├─ order-agent/              # Exemplo: Agente do domínio de pedidos
│  ├─ state/                 # Estado do pedido
│  ├─ cqrs/                  # Casos de uso e consultas
│  ├─ events/                # Eventos do ciclo de vida do pedido
│  ├─ infrastructure/        # Banco de dados e APIs externas
│  ├─ policies/              # Resiliência e consistência
│  └─ observability/         # Monitoramento do agente
│  └─ tools/                 # Funcionalidades externas
│
├─ stock-agent/              # Agente do domínio de estoque (mesma estrutura)
│
└─ payment-agent/            # Agente do domínio de pagamento (mesma estrutura)


📌 Exemplo real:

> No e-commerce, ao processar um pagamento, não é necessário aguardar a resposta do sistema bancário para continuar com a experiência do usuário. Em vez disso, um evento assíncrono como "PagamentoRecebido" pode ser disparado, e os serviços de notificação, estoque e logística podem consumi-lo independentemente.

2️⃣ Domain (Domínio)
O segundo elemento, Domain, vem da filosofia do Domain-Driven Design (DDD). Ele reforça que o núcleo da aplicação é totalmente independente da infraestrutura e das interfaces externas.

🔹 Por que o foco no domínio?

O código de negócio é organizado em agregados e entidades bem definidos.
O domínio é rico e independente, permitindo mudanças sem impactar a infraestrutura.
Aplicamos eventos de domínio, garantindo que as regras de negócio se comuniquem de forma clara e autônoma.
📌 Exemplo real:
Em um sistema de gestão financeira, a lógica para validar um pagamento recorrente deve estar no Domínio, e não espalhada por diferentes camadas. Isso garante reusabilidade, testabilidade e flexibilidade.

3️⃣ Flow (Fluxo)
O último elemento, Flow, representa a fluidez e a orquestração dos eventos e interações dentro do sistema. O Agentic Domain Flow não é uma arquitetura rígida e linear, mas sim dinâmica e adaptável, onde eventos, comandos e queries fluem entre as camadas sem bloqueios desnecessários.

🔹 Por que o foco no fluxo?

O sistema não é monolítico ou preso a uma única abordagem; ele se adapta dinamicamente às interações.
Eventos fluem naturalmente entre componentes, reduzindo acoplamento e melhorando a observabilidade.
O design promove interoperabilidade entre diferentes partes da aplicação, permitindo evolução contínua.
📌 Exemplo real:
Em um CRM baseado nessa arquitetura, quando um cliente muda seu plano de assinatura, um evento "PlanoAtualizado" pode ser propagado automaticamente para serviços de cobrança, suporte e analytics, sem que nenhum deles precise chamar diretamente os outros.


🎯 Objetivo do Agentic Domain Flow
A proposta dessa arquitetura é garantir que sistemas sejam modularizados, escaláveis e preparados para eventos assíncronos, mantendo uma organização clara entre camadas de domínio, infraestrutura e orquestração de eventos.

O que queremos alcançar? ✅ Código modular e bem definido → Separação clara entre domínio e infraestrutura.
✅ Facilidade para evoluir → Suporte a novos casos de uso sem refatorações gigantes.
✅ Escalabilidade nativa → Uso de eventos para distribuir carga e processar tarefas assíncronas.
✅ Independência da tecnologia → Capacidade de trocar banco de dados, API ou mensageria sem afetar o domínio.
✅ Integração com múltiplos canais → API REST, WebSockets, filas, etc., sem reescrever regras de negócio.


🔀 A Fusão das Três Arquiteturas
O Agentic Domain Flow nasce da combinação dos melhores conceitos de DDD, Arquitetura Hexagonal e Event-Driven Architecture (EDA).



Essa fusão resolve problemas comuns de arquiteturas tradicionais:

Evita acoplamento entre domínio e infraestrutura

O domínio é puro e independente.
A infraestrutura implementa apenas adapters para persistência e comunicação externa.
Facilita a escalabilidade e o processamento assíncrono

Eventos permitem distribuir tarefas sem bloqueios.
Filas e streamings podem ser usados para desacoplar serviços.
Permite múltiplas interfaces sem modificar o domínio

REST, WebSockets, mensageria e GraphQL podem coexistir sem impactar as regras de negócio.

| Arquitetura                          | O que traz para o Agentic Domain Flow?                                      |
|--------------------------------------|---------------------------------------------------------------------------|
| **Domain-Driven Design (DDD)**       | Define um **modelo rico** com entidades, agregados e eventos de domínio.  |
| **Arquitetura Hexagonal (Ports & Adapters)** | Garante que o domínio seja **independente de tecnologia** e **infraestrutura**. |
| **Event-Driven Architecture (EDA)**  | Introduz **eventos como primeira classe**, garantindo **reatividade e escalabilidade**. |


## 📐 Estrutura do Agentic Domain Flow
A arquitetura é dividida em cinco camadas, cada uma com uma responsabilidade bem definida:

> Antes de começar a organizar seu código você deve definir suas Entities pois é necessário configurar algumas carasterísticas delas:

```ts

export interface EntityConfig = {
  database: DATABASES_ENUM,
  cache: CACHE_ENUM,
  queues: QUEUES_ENUM,
  jobs: JOBS_ENUM,
  events: EVENTS_ENUM,
  name: typeof getEntityName()
}
```
Se quiser aplicar uma configuração para todas as entidades basta você preencher a AppConfig.

A configuração deixa em aberto para que você possa utilizar diferentes sistemas de mensageria, podemos separa elas pelas suas responsabilidades mais específicas, você poder usar uma para as 3 funcionalidades, mas internamente no sistema suas funções são diferentes, confira a seguir suas funionalidades.


#### Message Queue (Fila de Mensagens)

- Propósito: Transmissão de mensagens entre serviços.
- Exemplos: RabbitMQ, Kafka.
- Característica: Mensagem descartada após o consumo.
- Uso: Comunicação entre microsserviços, eventos em tempo real.

📜 1️⃣ Interface Genérica para Message Queue

A Message Queue é focada em comunicação assíncrona entre serviços, enviando e recebendo mensagens.

```ts
// src/shared/agents/queues/interfaces/IMessageQueue.ts
export interface IMessageQueue {
  publish<T>(event: string, message: T): Promise<void>;
  subscribe<T>(event: string, handler: (message: T) => Promise<void>): void;
  ack(messageId: string): Promise<void>;
  nack(messageId: string): Promise<void>;
}
```

Exemplo de uso:

```ts
const messageQueue: IMessageQueue = new KafkaQueue();
await messageQueue.publish("order.state.created", { orderId: "123", userId: "456" });
messageQueue.subscribe("order.state.created", async (message) => {
  console.log("Pedido recebido:", message);
});
```

Message Queue com RabbitMQ:
```ts
import { IMessageQueue } from "./IMessageQueue";

export class RabbitMQ implements IMessageQueue {
  async publish<T>(event: string, message: T): Promise<void> {
    console.log(`📤 [MQ] Publicando: ${event}`, message);
  }

  subscribe<T>(event: string, handler: (message: T) => Promise<void>): void {
    console.log(`📥 [MQ] Inscrito: ${event}`);
    // Simula recebimento de mensagem
    setTimeout(() => handler({ id: "123", content: "Olá do RabbitMQ!" } as T), 2000);
  }
}
```

#### Task Queue / Job Queue (Fila de Tarefas / Trabalhos)

Propósito: Agendamento e processamento de tarefas assíncronas.
Exemplos: BullMQ, Celery.
Característica: A mensagem representa um "job" a ser processado, com estados como waiting, active, completed, failed e delayed.
Uso: Processamento em background, trabalhos agendados, retry automático.

⚙️ 2️⃣ Interface Genérica para Task Queue (Job Queue)

A Task Queue gerencia tarefas assíncronas e agendadas, como processamento de pagamentos ou envios de e-mails.

```ts
// src/shared/agents/queues/interfaces/ITaskQueue.ts
export interface ITaskQueue {
  addJob<T>(queueName: string, jobData: T, options?: Record<string, unknown>): Promise<void>;
  processJob<T>(queueName: string, handler: (jobData: T) => Promise<void>): void;
  removeJob(jobId: string): Promise<void>;
}
```

Exemplo de uso:

```ts
const taskQueue: ITaskQueue = new BullMQQueue();
await taskQueue.addJob("order.task.payment", { orderId: "123", amount: 100.0 });
taskQueue.processJob("order.task.payment", async (job) => {
  console.log("Processando pagamento do pedido:", job);
});
```

Task Queue com BullMQ:
```ts
import { ITaskQueue } from "./ITaskQueue";
import { Queue, Worker } from "bullmq";

export class BullTaskQueue implements ITaskQueue {
  private queue: Queue;

  constructor(queueName: string) {
    this.queue = new Queue(queueName);
  }

  async addJob<T>(queueName: string, jobData: T): Promise<void> {
    await this.queue.add(queueName, jobData);
    console.log(`🚀 [TaskQueue] Tarefa adicionada: ${queueName}`);
  }

  processJob<T>(queueName: string, handler: (jobData: T) => Promise<void>): void {
    new Worker(queueName, async (job) => {
      console.log(`⚙️ [TaskQueue] Processando: ${queueName}`);
      await handler(job.data);
    });
  }
}
```

#### Event Queue (Fila de Eventos)

Propósito: Propagar eventos entre sistemas.
Exemplos: Kafka (quando usado como Event Log), Redis Streams.
Característica: Foco na persistência do evento e histórico de mudanças.
Uso: Event Sourcing, rastreamento de mudanças.

🔔 3️⃣ Interface Genérica para Event Queue (Event Bus)
A Event Queue gerencia eventos do domínio para comunicação entre serviços, geralmente sem resposta esperada

```ts
// src/shared/agents/queues/interfaces/IEventQueue.ts
export interface IEventQueue {
  emit<T>(event: string, payload: T): void;
  on<T>(event: string, handler: (payload: T) => void): void;
  off(event: string, handler: Function): void;
}
```

```ts
const eventQueue: IEventQueue = new EventEmitterQueue();
eventQueue.emit("order.event.paid", { orderId: "123", userId: "456" });

eventQueue.on("order.event.paid", (payload) => {
  console.log("Evento de pedido pago recebido:", payload);
});
```

```ts
import { EventEmitter } from "events";
import { IEventQueue } from "./IEventQueue";

export class EventBus implements IEventQueue {
  private eventEmitter = new EventEmitter();

  emit<T>(event: string, payload: T): void {
    console.log(`🔔 [EventBus] Emitindo evento: ${event}`, payload);
    this.eventEmitter.emit(event, payload);
  }

  on<T>(event: string, handler: (payload: T) => void): void {
    console.log(`👂 [EventBus] Ouvindo evento: ${event}`);
    this.eventEmitter.on(event, handler);
  }

  off(event: string, handler: Function): void {
    this.eventEmitter.off(event, handler);
  }
}
```


### Convenções

#### CamelCase vs. entity.action
Entre camelCase e entity.action, a abordagem entity.action (exemplo: order.create) é mais recomendada, especialmente em sistemas distribuídos, mensageria e eventos.

Clareza Semântica: order.create indica claramente que a ação "create" está sendo realizada na entidade "order". Com camelCase, como orderCreate, a separação entre entidade e ação não é tão explícita.

Padrão em Mensageria: Em filas como RabbitMQ, Kafka e sistemas de eventos, o padrão entity.action é amplamente adotado. Ele organiza melhor os tópicos e roteamentos.

Escalabilidade: Em sistemas com muitas entidades e ações, usar entity.action facilita a filtragem, logs e busca.

Legibilidade: order.create é mais fácil de ler e entender do que orderCreate ou createOrder.


1️⃣ Gateway Layer (Portas de Entrada e Saída)
Define múltiplas interfaces para interagir com o sistema (API, CLI, WebSockets, mensageria).
Converte formatos de entrada e saída.
Aplica autenticação e autorização.

Exemplo:

```ts
// src/gateway/controllers/OrderController.ts
import { Request, Response } from "express";
import { CheckoutAgent } from "@/agents/CheckoutAgent";
import { CreateOrderDTO } from "@/presentation/dto/CreateOrderDTO";

export class OrderController {
  constructor(private readonly checkoutAgent: CheckoutAgent) {}

  async createOrder(req: Request, res: Response): Promise<Response> {
    const dto = new CreateOrderDTO(req.body);

    const order = await this.checkoutAgent.execute(dto);
    return res.status(201).json(order);
  }
}
```


2️⃣ Agents  Layer (Casos de Uso e Orquestração)
Orquestra as chamadas ao Domínio e gerencia fluxos de aplicação.
Manipula eventos assíncronos e chamadas síncronas.
Aplica validações e fluxos de trabalho.

Exemplo: Agente que processa o pedido

```ts
// src/agents/CheckoutAgent.ts
import { CreateOrderDTO } from "@/presentation/dto/CreateOrderDTO";
import { Order } from "@/domain/entities/Order";
import { OrderRepository } from "@/data-access/repositories/OrderRepository";
import { PaymentTool } from "@/tools/PaymentTool";

export class CheckoutAgent {
  constructor(
    private readonly orderRepo: OrderRepository,
    private readonly paymentTool: PaymentTool
  ) {}

  async execute(dto: CreateOrderDTO): Promise<Order> {
    console.log("🛒 Iniciando o checkout...");

    // 1. Processar pagamento
    const paymentResult = await this.paymentTool.processPayment(dto.payment);
    if (!paymentResult.success) throw new Error("Falha no pagamento!");

    // 2. Criar pedido
    const order = new Order(dto.customerId, dto.items, paymentResult);
    await this.orderRepo.insertOne(order);

    console.log("✅ Pedido finalizado:", order);
    return order;
  }
}
```


3️⃣ Domain  Layer (Núcleo de Negócio)
Define entidades, agregados, serviços de domínio e eventos.
Independente de infraestrutura e frameworks externos.
Modela regras de negócio puras.


Exemplo: Entidade `Order`
```ts
// src/domain/entities/Order.ts
export class Order {
  constructor(
    public readonly customerId: string,
    public readonly items: { productId: string; quantity: number }[],
    public readonly paymentResult: { success: boolean; transactionId: string }
  ) {
    if (items.length === 0) throw new Error("O pedido deve ter ao menos um item.");
  }
}
```


4️⃣ Tools Layer (Implementações de Ferramentas)
Implementa APIs externas e mensageria. Qual quer ação que saia do sistema pertence a uma Tool, também deve implementar a mensageria usada por todos.

Exemplo: Ferramenta de Pagamento

```ts
// src/domain/entities/Order.ts
// src/tools/PaymentTool.ts
export class PaymentTool {
  async processPayment(paymentInfo: any): Promise<{ success: boolean; transactionId: string }> {
    console.log("💳 Processando pagamento...");
    // Simulação de sucesso
    return { success: true, transactionId: `tx_${Date.now()}` };
  }
}

```

5️⃣ Presentation Layer (Exposição das Informações Internas do Sistema)
Expões as informações internas do sistema, como: DTOs, Schemas, Estrutura as respostas e entradas, valida dados recebidos antes de repassar ao Agent, controla os contratos de entrada/saída.

Exemplo: DTO para criação do Pedido

```ts
// src/presentation/dto/CreateOrderDTO.ts
export class CreateOrderDTO {
  customerId: string;
  items: { productId: string; quantity: number }[];
  payment: { method: string; cardNumber: string };

  constructor(data: any) {
    this.customerId = data.customerId;
    this.items = data.items;
    this.payment = data.payment;

    if (!this.customerId || this.items.length === 0) {
      throw new Error("Dados inválidos para criar pedido.");
    }
  }
}
```

Rota: ``



6️⃣ Data Access Layer (Armazenamento e Acesso aos dados Internos do Sistema)
Implementa o padão Adapter, Factory e Provier. Responsável por interagir com bancos de dados, localStorage ou Caches. Implementa o CRUD das entidades e isola o domínio do tipo de armazenamento.

Só pode ser utilizado via Eventos e acessados pelo Agentes.

Exemplo: Repositório de `Order´

```ts
// src/data-access/repositories/OrderRepository.ts
import { Order } from "@/domain/entities/Order";
import { RecordAgent } from "@/agents/repositories/RecordAgent";
export class OrderRepository {
  private orders: Order[] = [];

  RecordAgent.subscribe("order.record.inserOne", order:Order)
  async insertOne(order: Order): Promise<void> {
    this.orders.push(order);
    console.log("💾 Pedido salvo:", order);
  }

  async findAll(): Promise<Order[]> {
    return this.orders;
  }
}
```

Aqui vai a implementação da Interface genérica para os Repositories.

RecordAgent com EventEmitter:

```ts
import { EventEmitter } from "events";

export class RecordAgent {
  private static eventEmitter = new EventEmitter();

  static subscribe<T>(event: string, listener: (data: T) => void) {
    this.eventEmitter.on(event, listener);
  }

  static publish<T>(event: string, entity: T) {
    this.eventEmitter.emit(event, entity);
  }
}
```

```ts
// getEventName
export enum CrudEvent {
  CREATE = "create",
  UPDATE = "update",
  SOFT_DELETE = "softDeleted",
  HARD_DELETE = "hardDeleted",
  FIND_ALL = "findAll",
  FIND_ONE = "findOne",
}

export const getEventName = (entity: string, action: CrudEvent): string => {
  return `${entity}.record.${action}`;
};
```

Para utilizar nosso CRUD você deve implementar o `GenericAgentRepository`:

```ts
// GenericAgentsRepository
import { EventBus } from "../infrastructure/queue/services/EventBus";
import { getEventName } from "../utils/getEventName";
import { CrudEvent } from "../utils/getEventName";
export class GenericRepository<T extends { id: string }> {
  private entityName: string;

  constructor(entityName: string) {
    this.entityName = entityName;
  }

  async create(data: T): Promise<T> {
    const event = getEventName(this.entityName, CrudEvent.CREATE);
    await RecordAgent.publish(event, data);
    return data;
  }

  async update(id: string, data: Partial<T>): Promise<T | null> {
    const event = getEventName(this.entityName, CrudEvent.UPDATE);
    await RecordAgent.publish(event, { id, ...data });
    return { id, ...data } as T;
  }

  async softDelete(id: string): Promise<void> {
    const event = getEventName(this.entityName, CrudEvent.SOFT_DELETE);
    await RecordAgent.publish(event, { id });
  }

  async hardDelete(id: string): Promise<void> {
    const event = getEventName(this.entityName, CrudEvent.HARD_DELETE);
    await RecordAgent.publish(event, { id });
  }

  async findAll(): Promise<T[]> {
    const event = getEventName(this.entityName, CrudEvent.FIND_ALL);
    return RecordAgent.request(event, {});
  }

  async findOne(id: string): Promise<T | null> {
    const event = getEventName(this.entityName, CrudEvent.FIND_ONE);
    return RecordAgent.request(event, { id });
  }
}
```

Resgistrar Produto:
```ts
// src/modules/product/domain/repositories/ProductRepository.ts
import { RecordAgent } from "@/shared/agents/RecordAgent";
import { getEventName, CrudEvent } from "@/shared/utils/getEventName";

export interface Product {
  id: string;
  name: string;
  price: number;
  stock: number;
}

// Simula um banco de dados em memória
const productsDb: Product[] = [
  { id: "1", name: "Produto A", price: 100, stock: 10 },
  { id: "2", name: "Produto B", price: 200, stock: 5 },
];

// Escutar requisições de busca
RecordAgent.subscribe<{ id: string }>(getEventName("Product", CrudEvent.FIND_ONE), ({ id }) => {
  const product = productsDb.find((p) => p.id === id);
  if (product) {
    console.log(`📦 Produto encontrado:`, product);
    RecordAgent.publish(getEventName("Product", CrudEvent.FIND_ONE), product);
  } else {
    console.log(`❌ Produto com ID ${id} não encontrado.`);
  }
});

```

Criar Pedido (OrderRepository):

```ts
// src/domain/repositories/OrderRepository.ts
import { RecordAgent } from "@/shared/agents/RecordAgent";
import { getEventName, CrudEvent } from "@/shared/utils/getEventName";
import { Record}

export interface Order {
  id: string;
  productId: string;
  quantity: number;
  total: number;
}

// Banco em memória para pedidos
const ordersDb: Order[] = [];

// Escutar eventos para criar pedidos
RecordAgent.subscribe<Order>(getEventName("Order", CrudEvent.CREATE), (order) => {
  ordersDb.push(order);
  console.log(`✅ Pedido criado com sucesso:`, order);
});
```


## 🚀 Como o Agentic Domain Flow Melhora o Desenvolvimento?
1. Escalabilidade Natural
Com eventos nativos, podemos distribuir o processamento facilmente, melhorando performance e resiliência.
Por exemplo, um serviço de pagamento pode processar transações em workers assíncronos, sem bloquear a aplicação principal.
2. Facilidade de Manutenção e Evolução
A separação de camadas permite que mudanças sejam feitas sem afetar todo o sistema.
Exemplo: Se um novo canal (GraphQL) for adicionado, basta criar um novo adapter na Interface Layer, sem alterar regras de negócio.
3. Testabilidade
O Domain Layer é 100% testável sem dependências externas, pois não acessa banco de dados ou APIs diretamente.
Isso melhora a qualidade do código e reduz tempo de desenvolvimento.
4. Facilidade de Integração
Como a arquitetura já é event-driven, integrações com sistemas externos (ERP, CRM, gateways de pagamento) ocorrem naturalmente através de eventos.
Exemplo: Um sistema de e-commerce pode publicar um evento "PedidoCriado", que é consumido por múltiplos serviços (pagamento, estoque, logística).


📊 Comparação com Arquiteturas Tradicionais
Aqui está a comparação das arquiteturas e o que o Agentic Domain Flow melhora:

| Característica           | DDD                         | Hexagonal                     | Event-Driven                 | Agentic Domain Flow (Novo)     |
|-------------------------|----------------------------|------------------------------|------------------------------|------------------------------|
| 📦 **Separação de Domínio** | ✅ Sim                      | ✅ Sim                        | ⚠️ Parcial (foco em eventos)  | ✅ Melhorada, com domínio reativo |
| 🔄 **Independência de Infra** | ⚠️ Depende de aplicação    | ✅ Sim                        | ✅ Sim                        | ✅ Sim (infra apenas na camada correta) |
| 🔀 **Eventos Assíncronos**  | ⚠️ Manual                  | ⚠️ Pouco usado                | ✅ Sim                        | ✅ Nativo e centralizado |
| 🚀 **Escalabilidade**      | ⚠️ Média                    | ⚠️ Média                      | ✅ Alta                        | ✅ Alta e otimizada |


## Layers

### Gateway Layer (Portas de Entrada)
A Interface Layer é a camada responsável por receber dados do mundo externo. Ela representa as "Portas" na Arquitetura Hexagonal, permitindo que múltiplos canais se comuniquem com a aplicação de forma desacoplada.

Essa camada não contém regras de negócio — apenas transforma, valida e chama o Agente Orquestrador, garantindo que a aplicação possa ser exposta em diversos formatos sem modificar o domínio, O Agente Orquestrador entrega o seu Schema e o Gateway é responsável pela transformação e mapeamento dos dados para se transformarem no Schema para que aí sim os dados possam ser aceitos.

📌 Características Principais
✅ Multicanal → Suporte para APIs, WebSockets, CLI, Mensageria, etc.
✅ Independente da Regra de Negócio → Apenas recebe e encaminha dados.
✅ Conversão de Formato → Adapta a entrada/saída para o formato necessário.
✅ Validação e Autenticação → Verifica permissões antes de processar a requisição.


#### Técnicas utilizadas

🟢 Circuit Breaker
Evita falhas em cascata, interrompendo chamadas para serviços instáveis.
Ferramentas: opossum (Node.js), resilience4j (Java).

🔁 Retry Pattern (Exponential Backoff)
Reenvia requisições falhadas, aumentando progressivamente o tempo entre tentativas.
Exemplo: Requisições Axios com axios-retry.

⏱️ Timeout Pattern
Define um tempo máximo de espera por resposta, evitando bloqueios.

⚖️ Rate Limiting & Throttling
Limita o número de requisições por cliente/IP para evitar sobrecarga.
Ferramentas: express-rate-limit, nginx limit_req.

🎛️ Bulkhead Pattern
Segrega recursos em compartimentos isolados, evitando que uma falha impacte todo o sistema.

📊 Health Checks e Heartbeats
Monitora a integridade de serviços e remove instâncias falhas do balanceamento.


📍 Exemplos do Mundo Real
Aqui estão diferentes implementações da Interface Layer e como elas se aplicam em sistemas reais:

1️⃣ API REST para E-commerce
📌 Cenário: Uma loja virtual possui uma API REST para permitir que clientes façam pedidos.

📜 Exemplo de Endpoints (Express.js / NestJS):

```ts
import { EventBus } from "../../communications/eventBus";

export const createOrder = async (req, res) => {
  const { customerId, items } = req.body;
  await EventBus.publish("OrderCreated", { customerId, items });
  res.status(201).json({ message: "Order placed successfully." });
};
```


✅ Porta de Entrada: Endpoint /orders recebe uma requisição POST.
✅ Conversão de Formato: JSON recebido é convertido em CreateOrderDto.
✅ Encaminhamento: O DTO é passado para a camada de aplicação.

2️⃣ WebSockets para Aplicativos de Mensagens
📌 Cenário: Um sistema de chat precisa de comunicação em tempo real via WebSockets.

📜 Exemplo de Implementação (Socket.IO + NestJS):

```ts
@WebSocketGateway()
export class ChatGateway {
  @SubscribeMessage("message")
  handleMessage(@MessageBody() data: ChatMessageDto): void {
    this.chatService.processMessage(data);
  }
}
```

✅ Porta de Entrada: Evento "message" escutado via WebSockets.
✅ Encaminhamento: Mensagem validada e enviada para a camada de aplicação.
✅ Resposta Assíncrona: Mensagem pode ser salva e retransmitida para clientes.

3️⃣ CLI (Command Line Interface) para DevOps
📌 Cenário: Um desenvolvedor precisa rodar um comando para gerar relatórios diretamente no terminal.

📜 Exemplo de Comando CLI (Node.js / Commander.js):


```ts
program
  .command("generate-report <month>")
  .description("Gera um relatório de vendas para o mês especificado")
  .action((month) => {
    reportService.generate(month);
  });

program.parse(process.argv);
```

✅ Porta de Entrada: Usuário executa node cli.js generate-report 01.
✅ Conversão de Formato: O argumento "01" é interpretado como month.
✅ Encaminhamento: Passa o comando para a Application Layer para gerar o relatório.

4️⃣ Mensageria com RabbitMQ (Fila de Processamento)
📌 Cenário: Um sistema de pagamentos processa transações assíncronas usando filas.

📜 Exemplo de Consumer RabbitMQ (amqplib - Node.js):


```ts
channel.consume("payments", (msg) => {
  const paymentData = JSON.parse(msg.content.toString());
  paymentService.processPayment(paymentData);
});
```

✅ Porta de Entrada: Uma mensagem é recebida na fila payments.
✅ Conversão de Formato: O JSON é convertido para um objeto paymentData.
✅ Encaminhamento: A mensagem é enviada para o serviço de pagamentos.

5️⃣ GraphQL para APIs mais Flexíveis
📌 Cenário: Uma API GraphQL permite que um cliente solicite apenas os dados necessários.

📜 Exemplo de Resolver GraphQL (NestJS + Apollo):


```ts
@Resolver(() => Order)
export class OrderResolver {
  @Query(() => Order)
  async getOrder(@Args("id") id: string) {
    return this.orderService.getOrderById(id);
  }
}
```

✅ Porta de Entrada: Cliente faz uma query getOrder(id: "123").
✅ Conversão de Formato: O id recebido é convertido para uma string.
✅ Encaminhamento: A requisição é passada para a camada de aplicação.

📌 Resumo
A Interface Layer permite que diferentes interfaces de entrada (REST, WebSockets, CLI, Mensageria, GraphQL) acessem a aplicação sem interferir no núcleo do sistema.

Essa camada facilita:

Adição de novos canais de comunicação sem modificar regras de negócio.
Troca de tecnologias sem afetar o Domain Layer.
Validação e autenticação desacopladas da aplicação.

### Agents Layer no Agentic Domain Flow
📌 Orquestração e Casos de Uso no CRM de WhatsApp

A Application Layer é a responsável por coordenar os casos de uso do sistema. Ela faz a ponte entre a Interface Layer (entrada de dados) e o Domain Layer (regra de negócio), garantindo que as operações sejam executadas da maneira correta.

📌 Função da Application Layer
Orquestração: Chama os serviços do Domínio e Infraestrutura, combinando suas funcionalidades.
Fluxos de Negócio: Implementa a lógica de casos de uso específicos, sem modificar as regras de negócio do Domínio.
Interação com Eventos: Escuta e publica eventos para processamento assíncrono.
Validação e Segurança: Antes de chamar o domínio, pode aplicar regras de acesso e validação.
📍 Estrutura no CRM de WhatsApp
Nosso CRM de WhatsApp possui dois principais domínios:

Vendas/Pedidos/Estoque
Mensagens/Chat/Tickets
Cada domínio tem casos de uso que devem ser coordenados na Application Layer.

1️⃣ Exemplo: Gerenciamento de Pedidos e Estoque
Cenário:

O cliente envia uma mensagem no WhatsApp pedindo um produto.
O sistema precisa verificar estoque, registrar o pedido e confirmar a compra.

```ts
// agents/orderAgent.ts
import { EventBus } from "../communications/eventBus";

EventBus.subscribe("OrderCreated", async (event) => {
  console.log(`Processing order for customer ${event.customerId}...`);

  // Verify stock availability
  await EventBus.publish("CheckStock", { items: event.items, orderId: event.orderId });
});
```

✅ Orquestração do fluxo: Valida estoque → Cria pedido → Dispara evento.
✅ Evita regras de negócio acopladas: Cada serviço tem sua responsabilidade.
✅ Eventos permitem escalabilidade: Outras partes do sistema podem reagir ao pedido.

2️⃣ Exemplo: Processamento de Mensagens no Chat
Cenário:

Um cliente envia uma mensagem no WhatsApp.
O sistema precisa criar um ticket de atendimento e armazenar a mensagem.

```ts
import { Injectable } from "@nestjs/common";
import { TicketRepository } from "../repositories/ticket.repository";
import { MessageRepository } from "../repositories/message.repository";
import { EventEmitter2 } from "@nestjs/event-emitter";

@Injectable()
export class ProcessMessageUseCase {
  constructor(
    private readonly ticketRepository: TicketRepository,
    private readonly messageRepository: MessageRepository,
    private readonly eventEmitter: EventEmitter2,
  ) {}

  async execute(chatId: string, customerId: string, text: string) {
    // 1️⃣ Cria um ticket se necessário
    let ticket = await this.ticketRepository.findOpenTicket(customerId);
    if (!ticket) {
      ticket = await this.ticketRepository.createTicket(customerId);
    }

    // 2️⃣ Salva a mensagem
    const message = await this.messageRepository.createMessage(chatId, customerId, text);

    // 3️⃣ Dispara evento para notificações/integrações
    this.eventEmitter.emit("message.received", { messageId: message.id });

    return message;
  }
}
```

✅ Separa lógica de negócios e orquestração.
✅ Verifica e cria tickets automaticamente.
✅ Eventos permitem automação: Notificações, análises, respostas automáticas.

3️⃣ Exemplo: Atualização do Estoque Após um Pedido
Cenário:

Quando um pedido é criado, precisamos atualizar o estoque.
Esse fluxo deve ser assíncrono para evitar lentidão na compra.

```ts
import { Injectable, OnModuleInit } from "@nestjs/common";
import { EventEmitter2, OnEvent } from "@nestjs/event-emitter";
import { StockService } from "../services/stock.service";

@Injectable()
export class OrderEventHandler {
  constructor(private readonly stockService: StockService) {}

  @OnEvent("order.created")
  async handleOrderCreated(event: { orderId: string }) {
    console.log(`Atualizando estoque para o pedido: ${event.orderId}`);

    await this.stockService.decreaseStock(event.orderId);
  }
}
```

✅ O estoque é atualizado apenas quando o pedido já foi criado.
✅ Processo assíncrono evita travamentos.
✅ Facilidade de escalabilidade: Podemos adicionar novos consumidores sem alterar o código principal.

### Communication Layer no Agentic Domain Flow
📌 
o.

📜 
```ts
const eventHandlers: Record<string, Function[]> = {};

export const EventBus = {
  subscribe: (event: string, handler: Function) => {
    if (!eventHandlers[event]) eventHandlers[event] = [];
    eventHandlers[event].push(handler);
  },
  publish: async (event: string, payload: any) => {
    if (eventHandlers[event]) {
      for (const handler of eventHandlers[event]) {
        await handler(payload);
      }
    }
  },
};
```

✅ Regra de negócio encapsulada na entidade (ex.: validação e atualização de status).
✅ Uso de Objetos de Valor (OrderItem) para garantir consistência.
✅ Sem dependências externas → Código puro e testável.

2️⃣ Exemplo: Modelagem do Ticket de Suporte (Chat Domain)
📌 Cenário:

Cada ticket representa um atendimento de suporte.
Ele pode estar aberto ou fechado.
Quando fechado, não pode mais receber mensagens.


📜 Implementação da Entidade Ticket:
```ts
export class Ticket {
  constructor(
    public readonly id: string,
    public readonly customerId: string,
    public readonly status: TicketStatus,
    public readonly createdAt: Date = new Date(),
    public readonly messages: Message[] = []
  ) {}

  // 1️⃣ Adiciona mensagem ao ticket
  addMessage(message: Message) {
    if (this.status === TicketStatus.CLOSED) {
      throw new Error("Não é possível adicionar mensagem a um ticket fechado");
    }
    this.messages.push(message);
  }

  // 2️⃣ Fecha o ticket
  close() {
    if (this.status === TicketStatus.CLOSED) {
      throw new Error("Ticket já está fechado");
    }
    this.status = TicketStatus.CLOSED;
  }
}

// Enum para Status do Ticket
export enum TicketStatus {
  OPEN = "OPEN",
  CLOSED = "CLOSED",
}

// Entidade Mensagem
export class Message {
  constructor(
    public readonly id: string,
    public readonly chatId: string,
    public readonly senderId: string,
    public readonly content: string,
    public readonly timestamp: Date = new Date()
  ) {
    if (!content) throw new Error("Mensagem não pode estar vazia");
  }
}
```

✅ Encapsula regras de negócio dentro das entidades.
✅ Garante consistência → Tickets fechados não podem receber novas mensagens.
✅ Evita acoplamento → Apenas manipula os dados necessários.

3️⃣ Exemplo: Eventos de Domínio
📌 Cenário:

Quando um pedido é criado, outros serviços podem precisar saber disso (ex.: notificações, logística).
Para evitar acoplamento direto, emitimos um evento de domínio.

📜 Implementação do Evento de Domínio:
```ts
export class OrderCreatedEvent {
  constructor(public readonly orderId: string, public readonly customerId: string) {}
}
```

📌 Agora, dentro da entidade Order, disparamos o evento:

```ts
import { EventEmitter2 } from "@nestjs/event-emitter";

export class Order {
  constructor(
    public readonly id: string,
    public readonly customerId: string,
    public readonly items: OrderItem[],
    public status: OrderStatus,
    public readonly totalAmount: number,
    private readonly eventEmitter: EventEmitter2 // Injetamos o EventEmitter
  ) {
    this.validate();
    this.eventEmitter.emit("order.created", new OrderCreatedEvent(this.id, this.customerId));
  }
}
```

✅ Baixo acoplamento → Nenhum código precisa chamar serviços externos diretamente.
✅ Escalabilidade → Novos serviços podem ouvir esse evento sem modificar a entidade Order.
✅ Alta coesão → O evento faz parte do domínio e não da infraestrutura.

4️⃣ Exemplo: Contratos de Repositórios
📌 Por que usar interfaces?

O Domínio não deve depender de bancos de dados específicos.
O repositório apenas define a estrutura, e a implementação ocorre na Infra Layer.

📜 Interface para Repositório de Pedidos:
```ts
export interface OrderRepository {
  createOrder(order: Order): Promise<Order>;
  findById(orderId: string): Promise<Order | null>;
  findByCustomer(customerId: string): Promise<Order[]>;
}
```
📌 Como isso ajuda?

No domínio, apenas chamamos os métodos do repositório sem saber a implementação.
Podemos trocar de banco de dados sem alterar a Domain Layer.
Fácil de testar, pois podemos usar mocks dos repositórios.
📌 Resumo: Vantagens da Domain Layer
✅ Código independente de infraestrutura (Banco de dados, API, framework).
✅ Regras de negócio centralizadas → Evita lógica espalhada pelo sistema.
✅ Facilidade para testes → Código puro sem dependências externas.
✅ Baixo acoplamento com a Application Layer → Comunicação via eventos.
✅ Facilidade para escalar → Podemos adicionar serviços sem mudar o código existente.

### Tools Layer no Agentic Domain Flow
📌 Implementação com Kafka no CRM de WhatsApp

A Infrastructure Layer é responsável por fornecer implementações concretas dos contratos definidos no domínio e na camada de aplicação. Aqui, implementamos repositórios para persistência e adapters para integração com sistemas externos.

No Agentic Domain Flow, essa camada: ✅ Implementa Repositórios → Concretiza a persistência (Banco de Dados, Cache).
✅ Gerencia Adapters → Comunicação com APIs externas, mensageria, notificações.
✅ Orquestra Mensageria → Envia e consome mensagens do Kafka de forma assíncrona.

📍 Kafka no CRM de WhatsApp
Nosso CRM tem dois domínios principais:

Vendas/Pedidos/Estoque → Eventos de pedidos, atualização de estoque.
Mensagens/Chat/Tickets → Processamento de mensagens em tempo real.
Usaremos o Apache Kafka para processar eventos assíncronos, garantindo que o sistema seja escalável e resiliente.

1️⃣ Produzindo Mensagens no Kafka (Pedidos)
📌 Cenário:

Quando um pedido é criado, queremos publicar um evento order.created.
Outros serviços (ex.: estoque, notificações) podem consumir esse evento.

📜 
```ts
export const logger = {
  info: (message: string) => console.log(`[INFO] ${message}`),
  error: (message: string) => console.error(`[ERROR] ${message}`),
};
```

📌 O que acontece aqui?
✅ Conectamos ao Kafka e enviamos um evento order.created.
✅ O evento é publicado com chave única (orderId) para rastreamento.
✅ Outros serviços podem consumir esse evento sem acoplamento direto.

2️⃣ Consumindo Mensagens no Kafka (Estoque)
📌 Cenário:

Quando um pedido é criado, o serviço de estoque deve atualizar a quantidade dos produtos.
O consumo desse evento será feito de forma assíncrona.
📜 Consumer Kafka para atualizar Estoque:
```ts
import { Injectable, OnModuleInit } from "@nestjs/common";
import { Kafka } from "kafkajs";
import { StockService } from "../services/stock.service";

@Injectable()
export class OrderKafkaConsumer implements OnModuleInit {
  private kafka = new Kafka({
    clientId: "crm-whatsapp",
    brokers: ["localhost:9092"],
  });

  private consumer = this.kafka.consumer({ groupId: "stock-service" });

  constructor(private readonly stockService: StockService) {}

  async onModuleInit() {
    await this.consumer.connect();
    await this.consumer.subscribe({ topic: "order.created", fromBeginning: true });

    await this.consumer.run({
      eachMessage: async ({ message }) => {
        const eventData = JSON.parse(message.value.toString());
        console.log(`📩 Evento "order.created" recebido! Atualizando estoque...`);

        await this.stockService.decreaseStock(eventData.orderId);
      },
    });
  }
}
```

📌 O que acontece aqui?
✅ O serviço de estoque escuta eventos order.created e age automaticamente.
✅ Totalmente assíncrono → O pedido pode ser criado sem esperar a atualização de estoque.
✅ Resiliência → Se o serviço cair, ele pode consumir os eventos antigos ao reiniciar (fromBeginning: true).

3️⃣ Processando Mensagens do Chat via Kafka
📌 Cenário:

Quando um usuário envia uma mensagem no WhatsApp, o sistema processa e armazena a mensagem.
Esse evento pode ser usado para respostas automáticas, analytics e integrações.
📜 Producer Kafka para mensagens do chat:
```ts
import { Injectable } from "@nestjs/common";
import { Kafka } from "kafkajs";

@Injectable()
export class ChatKafkaProducer {
  private kafka = new Kafka({
    clientId: "crm-whatsapp",
    brokers: ["localhost:9092"],
  });

  private producer = this.kafka.producer();

  async sendMessageReceivedEvent(chatId: string, customerId: string, message: string) {
    await this.producer.connect();

    await this.producer.send({
      topic: "chat.message.received",
      messages: [
        {
          key: chatId,
          value: JSON.stringify({ chatId, customerId, message, timestamp: new Date() }),
        },
      ],
    });

    console.log(`📨 Evento "chat.message.received" enviado para Kafka!`);
    await this.producer.disconnect();
  }
}
```

📌 O que acontece aqui?
✅ Cada mensagem enviada no chat gera um evento chat.message.received.
✅ O evento pode ser processado por múltiplos serviços (ex.: IA de respostas, analytics).
✅ Permite escalar o sistema de chat sem afetar o tempo de resposta do usuário.

📜 Consumer Kafka para armazenar mensagens:
```ts
import { Injectable, OnModuleInit } from "@nestjs/common";
import { Kafka } from "kafkajs";
import { MessageRepository } from "../repositories/message.repository";

@Injectable()
export class ChatKafkaConsumer implements OnModuleInit {
  private kafka = new Kafka({
    clientId: "crm-whatsapp",
    brokers: ["localhost:9092"],
  });

  private consumer = this.kafka.consumer({ groupId: "chat-service" });

  constructor(private readonly messageRepository: MessageRepository) {}

  async onModuleInit() {
    await this.consumer.connect();
    await this.consumer.subscribe({ topic: "chat.message.received", fromBeginning: false });

    await this.consumer.run({
      eachMessage: async ({ message }) => {
        const eventData = JSON.parse(message.value.toString());
        console.log(`📩 Nova mensagem recebida: ${eventData.message}`);

        await this.messageRepository.saveMessage(eventData.chatId, eventData.customerId, eventData.message);
      },
    });
  }
}
```

📌 O que acontece aqui?
✅ O serviço de mensagens processa novos eventos chat.message.received.
✅ Cada mensagem é salva no banco de dados sem bloquear outras operações.
✅ Escalabilidade → Podemos rodar múltiplos consumidores para processar mensagens em paralelo.

📌 Resumo: Vantagens da Infrastructure Layer com Kafka
✅ Desacoplamento completo → Serviços comunicam-se via eventos sem chamar diretamente outros serviços.
✅ Escalabilidade massiva → Múltiplos consumidores podem processar mensagens simultaneamente.
✅ Resiliência → Se um serviço cair, ele pode consumir eventos ao reiniciar.
✅ Performance otimizada → O processamento ocorre de forma assíncrona e distribuída.

1️⃣ MongoDB Generic Repository
📌 O que esse repositório faz?

Implementa CRUD genérico para qualquer entidade.
Usa Soft Delete ao invés de remoção direta.
Usa interfaces para garantir que a persistência seja desacoplada do domínio.
📜 Implementação do Repositório Genérico (MongoDBGenericRepository.ts):
```ts
import { Collection, ObjectId, Filter, IndexDescription, OptionalId } from "mongodb";
import { IGenericRepository } from "../interfaces/IGenericRepository";

export abstract class MongoDBGenericRepository<T extends object> implements IGenericRepository<T> {
  protected collection: Collection<T>;

  constructor(collection: Collection<T>) {
    this.collection = collection;
  }

  async insertOne(entity: T): Promise<void> {
    await this.collection.insertOne(entity as OptionalId<T>);
  }

  async findOne(filter: Filter<T>): Promise<T | null> {
    return await this.collection.findOne({ ...filter, deletedAt: { $exists: false } });
  }

  async findAll(filter: Filter<T> = {}): Promise<T[]> {
    return await this.collection.find({ ...filter, deletedAt: { $exists: false } }).toArray();
  }

  async findById(id: string): Promise<T | null> {
    return await this.collection.findOne({ _id: new ObjectId(id), deletedAt: { $exists: false } });
  }

  async updateOne(id: string, updateData: Partial<T>): Promise<void> {
    await this.collection.updateOne({ _id: new ObjectId(id), deletedAt: { $exists: false } }, { $set: updateData });
  }

  async deleteOne(id: string): Promise<boolean> {
    const result = await this.collection.deleteOne({ _id: new ObjectId(id) });
    return result.deletedCount > 0;
  }

  async softDelete(id: string): Promise<boolean> {
    const result = await this.collection.updateOne(
      { _id: new ObjectId(id), deletedAt: { $exists: false } },
      { $set: { deletedAt: new Date(), active: false } }
    );
    return result.modifiedCount > 0;
  }

  async insertMany(entities: T[]): Promise<void> {
    await this.collection.insertMany(entities as OptionalId<T>[]);
  }

  async createIndexes(indexes: IndexDescription[]): Promise<void> {
    await this.collection.createIndexes(indexes);
  }
}
```

✅ CRUD desacoplado para qualquer entidade.
✅ Soft Delete para evitar remoção permanente.
✅ Interface genérica para uso em qualquer módulo.

2️⃣ Adapter de Pagamento (MercadoPago e Stripe)
📌 O que esse adapter faz?

Usa uma única interface para dois serviços de pagamento.
Implementa os métodos processar pagamento, capturar pagamento e reembolsar.
📜 Interface para Pagamentos (IPaymentGateway.ts):
```ts
export interface IPaymentGateway {
  processPayment(amount: number, currency: string, source: string): Promise<string>;
  capturePayment(paymentId: string): Promise<boolean>;
  refundPayment(paymentId: string): Promise<boolean>;
}
```

📜 Implementação do Adapter (PaymentGateway.ts):
```ts
import { IPaymentGateway } from "../interfaces/IPaymentGateway";
import { MercadoPagoService } from "../services/MercadoPagoService";
import { StripeService } from "../services/StripeService";

export class PaymentGateway implements IPaymentGateway {
  private mercadoPago: MercadoPagoService;
  private stripe: StripeService;

  constructor() {
    this.mercadoPago = new MercadoPagoService();
    this.stripe = new StripeService();
  }

  async processPayment(amount: number, currency: string, source: string): Promise<string> {
    if (currency === "BRL") {
      return this.mercadoPago.createPayment(amount, currency, source);
    } else {
      return this.stripe.createCharge(amount, currency, source);
    }
  }

  async capturePayment(paymentId: string): Promise<boolean> {
    if (paymentId.startsWith("mp_")) {
      return this.mercadoPago.capturePayment(paymentId);
    } else {
      return this.stripe.captureCharge(paymentId);
    }
  }

  async refundPayment(paymentId: string): Promise<boolean> {
    if (paymentId.startsWith("mp_")) {
      return this.mercadoPago.refundPayment(paymentId);
    } else {
      return this.stripe.refundCharge(paymentId);
    }
  }
}
```

✅ Um único adapter para MercadoPago e Stripe.
✅ A lógica decide qual serviço usar baseada no tipo de moeda ou ID do pagamento.
✅ Baixo acoplamento → Se quisermos trocar de provedor, basta alterar a implementação interna.

3️⃣ Adapter para Envio de Mensagens (WhatsApp, Telegram, Facebook e Instagram)
📌 O que esse adapter faz?

Usa uma única interface para quatro serviços de mensagem.
Implementa envio de mensagens e recebimento.
📜 Interface para Mensagens (IMessageGateway.ts):
```ts
export interface IMessageGateway {
  sendMessage(chatId: string, message: string): Promise<void>;
  receiveMessage(): Promise<void>;
}
```

📜 Implementação do Adapter (MessagingGateway.ts):
```ts
import { IMessageGateway } from "../interfaces/IMessageGateway";
import { WhatsAppService } from "../services/WhatsAppService";
import { TelegramService } from "../services/TelegramService";
import { FacebookService } from "../services/FacebookService";
import { InstagramService } from "../services/InstagramService";

export class MessagingGateway implements IMessageGateway {
  private whatsapp: WhatsAppService;
  private telegram: TelegramService;
  private facebook: FacebookService;
  private instagram: InstagramService;

  constructor() {
    this.whatsapp = new WhatsAppService();
    this.telegram = new TelegramService();
    this.facebook = new FacebookService();
    this.instagram = new InstagramService();
  }

  async sendMessage(chatId: string, message: string): Promise<void> {
    if (chatId.startsWith("wa_")) {
      await this.whatsapp.sendMessage(chatId, message);
    } else if (chatId.startsWith("tg_")) {
      await this.telegram.sendMessage(chatId, message);
    } else if (chatId.startsWith("fb_")) {
      await this.facebook.sendMessage(chatId, message);
    } else if (chatId.startsWith("ig_")) {
      await this.instagram.sendMessage(chatId, message);
    } else {
      throw new Error("Plataforma não suportada.");
    }
  }

  async receiveMessage(): Promise<void> {
    await this.whatsapp.listenMessages();
    await this.telegram.listenMessages();
    await this.facebook.listenMessages();
    await this.instagram.listenMessages();
  }
}
```

📦 1. Testes para o MongoDBGenericRepository
📌 Objetivo:

Testar CRUD, Soft Delete e Indexes do repositório genérico.
Usaremos o MongoMemoryServer para simular um banco real.
📜 __tests__/MongoDBGenericRepository.spec.ts:

```ts
import { MongoMemoryServer } from "mongodb-memory-server";
import { MongoClient, Db, Collection } from "mongodb";
import { MongoDBGenericRepository } from "../repositories/MongoDBGenericRepository";

interface TestEntity {
  _id?: string;
  name: string;
  active?: boolean;
  deletedAt?: Date;
}

describe("MongoDBGenericRepository", () => {
  let mongoServer: MongoMemoryServer;
  let client: MongoClient;
  let db: Db;
  let collection: Collection<TestEntity>;
  let repository: MongoDBGenericRepository<TestEntity>;

  beforeAll(async () => {
    mongoServer = await MongoMemoryServer.create();
    client = await MongoClient.connect(mongoServer.getUri(), {});
    db = client.db("testdb");
    collection = db.collection<TestEntity>("test-collection");
    repository = new MongoDBGenericRepository<TestEntity>(collection);
  });

  afterAll(async () => {
    await client.close();
    await mongoServer.stop();
  });

  beforeEach(async () => {
    await collection.deleteMany({});
  });

  it("deve inserir um documento", async () => {
    const entity: TestEntity = { name: "Produto A" };
    await repository.insertOne(entity);

    const result = await collection.findOne({ name: "Produto A" });
    expect(result).toBeDefined();
    expect(result?.name).toBe("Produto A");
  });

  it("deve encontrar um documento por ID", async () => {
    const entity: TestEntity = { name: "Produto B" };
    const { insertedId } = await collection.insertOne(entity);

    const result = await repository.findById(insertedId.toString());
    expect(result).toBeDefined();
    expect(result?.name).toBe("Produto B");
  });

  it("deve atualizar um documento", async () => {
    const { insertedId } = await collection.insertOne({ name: "Produto C" });

    await repository.updateOne(insertedId.toString(), { name: "Produto C Atualizado" });

    const updated = await collection.findOne({ _id: insertedId });
    expect(updated?.name).toBe("Produto C Atualizado");
  });

  it("deve realizar soft delete", async () => {
    const { insertedId } = await collection.insertOne({ name: "Produto D" });

    const deleted = await repository.softDelete(insertedId.toString());
    expect(deleted).toBe(true);

    const result = await collection.findOne({ _id: insertedId });
    expect(result?.active).toBe(false);
    expect(result?.deletedAt).toBeDefined();
  });

  it("deve criar índices", async () => {
    await repository.createIndexes([{ key: { name: 1 }, unique: true }]);

    const indexes = await repository.listIndexes();
    expect(indexes.some(index => index.name === "name_1")).toBe(true);
  });
});
```

✅ Testa inserção, busca, atualização e soft delete.
✅ Usa MongoMemoryServer para ambiente de teste isolado.
✅ Verifica índices criados na coleção.

💳 2. Testes para o PaymentGateway
📌 Objetivo:

Testar pagamentos usando MercadoPago e Stripe.
Mockar os serviços para evitar chamadas reais.
📜 __tests__/PaymentGateway.spec.ts:

```ts
import { PaymentGateway } from "../adapters/PaymentGateway";
import { MercadoPagoService } from "../services/MercadoPagoService";
import { StripeService } from "../services/StripeService";

jest.mock("../services/MercadoPagoService");
jest.mock("../services/StripeService");

describe("PaymentGateway", () => {
  let paymentGateway: PaymentGateway;
  let mercadoPagoMock: jest.Mocked<MercadoPagoService>;
  let stripeMock: jest.Mocked<StripeService>;

  beforeEach(() => {
    mercadoPagoMock = new MercadoPagoService() as jest.Mocked<MercadoPagoService>;
    stripeMock = new StripeService() as jest.Mocked<StripeService>;

    mercadoPagoMock.createPayment.mockResolvedValue("mp_12345");
    stripeMock.createCharge.mockResolvedValue("ch_12345");

    paymentGateway = new PaymentGateway();
  });

  it("deve processar pagamento via MercadoPago", async () => {
    const paymentId = await paymentGateway.processPayment(100, "BRL", "card_123");
    expect(paymentId).toBe("mp_12345");
    expect(mercadoPagoMock.createPayment).toHaveBeenCalled();
  });

  it("deve processar pagamento via Stripe", async () => {
    const paymentId = await paymentGateway.processPayment(100, "USD", "card_123");
    expect(paymentId).toBe("ch_12345");
    expect(stripeMock.createCharge).toHaveBeenCalled();
  });

  it("deve capturar pagamento", async () => {
    mercadoPagoMock.capturePayment.mockResolvedValue(true);
    const result = await paymentGateway.capturePayment("mp_12345");
    expect(result).toBe(true);
  });

  it("deve reembolsar pagamento", async () => {
    stripeMock.refundCharge.mockResolvedValue(true);
    const result = await paymentGateway.refundPayment("ch_12345");
    expect(result).toBe(true);
  });
});
```

✅ Mocka MercadoPago e Stripe para evitar custos reais.
✅ Testa processamento, captura e reembolso de pagamento.
✅ Verifica qual gateway é usado conforme a moeda.

💬 3. Testes para o MessagingGateway
📌 Objetivo:

Testar envio de mensagens via WhatsApp, Telegram, Facebook e Instagram.
📜 __tests__/MessagingGateway.spec.ts:

```ts
import { MessagingGateway } from "../adapters/MessagingGateway";
import { WhatsAppService } from "../services/WhatsAppService";
import { TelegramService } from "../services/TelegramService";
import { FacebookService } from "../services/FacebookService";
import { InstagramService } from "../services/InstagramService";

jest.mock("../services/WhatsAppService");
jest.mock("../services/TelegramService");
jest.mock("../services/FacebookService");
jest.mock("../services/InstagramService");

describe("MessagingGateway", () => {
  let gateway: MessagingGateway;
  let whatsappMock: jest.Mocked<WhatsAppService>;
  let telegramMock: jest.Mocked<TelegramService>;
  let facebookMock: jest.Mocked<FacebookService>;
  let instagramMock: jest.Mocked<InstagramService>;

  beforeEach(() => {
    whatsappMock = new WhatsAppService() as jest.Mocked<WhatsAppService>;
    telegramMock = new TelegramService() as jest.Mocked<TelegramService>;
    facebookMock = new FacebookService() as jest.Mocked<FacebookService>;
    instagramMock = new InstagramService() as jest.Mocked<InstagramService>;

    gateway = new MessagingGateway();
  });

  it("deve enviar mensagem via WhatsApp", async () => {
    whatsappMock.sendMessage.mockResolvedValue();
    await gateway.sendMessage("wa_123", "Olá pelo WhatsApp!");
    expect(whatsappMock.sendMessage).toHaveBeenCalledWith("wa_123", "Olá pelo WhatsApp!");
  });

  it("deve enviar mensagem via Telegram", async () => {
    telegramMock.sendMessage.mockResolvedValue();
    await gateway.sendMessage("tg_123", "Olá pelo Telegram!");
    expect(telegramMock.sendMessage).toHaveBeenCalledWith("tg_123", "Olá pelo Telegram!");
  });

  it("deve enviar mensagem via Facebook", async () => {
    facebookMock.sendMessage.mockResolvedValue();
    await gateway.sendMessage("fb_123", "Olá pelo Facebook!");
    expect(facebookMock.sendMessage).toHaveBeenCalledWith("fb_123", "Olá pelo Facebook!");
  });

  it("deve enviar mensagem via Instagram", async () => {
    instagramMock.sendMessage.mockResolvedValue();
    await gateway.sendMessage("ig_123", "Olá pelo Instagram!");
    expect(instagramMock.sendMessage).toHaveBeenCalledWith("ig_123", "Olá pelo Instagram!");
  });

  it("deve lançar erro para plataforma desconhecida", async () => {
    await expect(gateway.sendMessage("xx_123", "Mensagem inválida")).rejects.toThrow(
      "Plataforma não suportada."
    );
  });
});
```

✅ Testa envio de mensagem para WhatsApp, Telegram, Facebook e Instagram.
✅ Garante que o serviço correto é chamado com base no chatId.
✅ Verifica erros para plataformas não suportadas.



✅ Baixo acoplamento → WhatsApp, Telegram, Facebook e Instagram usam a mesma interface.
✅ Lógica centralizada → O chatId determina qual serviço será chamado.
✅ Escalável → Podemos adicionar novas plataformas sem modificar o código principal.

📌 Resumo: Vantagens da Infrastructure Layer
✅ Repositório genérico → Qualquer entidade pode ser gerenciada sem reescrever código.
✅ Adapters desacoplados → MercadoPago/Stripe e WhatsApp/Telegram/Facebook/Instagram usam interfaces únicas.
✅ Facilidade para troca de tecnologia → Basta mudar a implementação sem afetar a aplicação.
✅ Escalabilidade e flexibilidade → Novos serviços podem ser adicionados facilmente.


### Integrations Layer: Orquestração Inteligente e Reativa

📦 Exemplos no CRM de Vendas via WhatsApp/Telegram
Vamos usar eventos para os seguintes fluxos:

Novo Pedido: Quando um cliente envia uma mensagem pedindo um produto.
Atualização de Estoque: Após um pedido, o estoque é atualizado.
Confirmação de Pagamento: Quando um pagamento é processado.
Envio de Mensagem: Notificar o cliente sobre status do pedido.
🚀 Tecnologias Usadas na Event Layer
Aqui está como podemos organizar a Event Layer no nosso CRM:

Tecnologia	Função

- Kafka Streams	Processamento contínuo de eventos em tempo real.
BullMQ	Gerenciamento de filas no Redis para processamento assíncrono.
Apache Airflow	Orquestração de fluxos de trabalho baseados em tempo/eventos.
AWS SQS/SNS	Fila de mensagens e notificações em nuvem.
Azure Service Bus	Fila e tópico para comunicação distribuída.
🔗 1. Implementando com Kafka Streams
📌 Cenário: Quando um pedido é criado, publicamos o evento order.created e serviços como estoque e notificações reagem.

📜 Configuração do Kafka Producer:
```ts
import axios from "axios";

export const PaymentGateway = {
  processPayment: async (orderId: string, amount: number) => {
    const response = await axios.post("https://payment-api.com/pay", {
      orderId,
      amount,
    });
    return response.data;
  },
};
```


✅ Vantagens:

Processamento em tempo real.
Capacidade de escalar consumidores horizontalmente.
Garantia de entrega com offsets.
⏳ 2. Gerenciamento de Fila com BullMQ
📌 Cenário: Quando um pagamento é confirmado, enfileiramos a atualização de estoque e a notificação ao cliente.

📜 Configuração da Fila (BullMQ):
```ts
import { Queue } from "bullmq";

const orderQueue = new Queue("order-processing", { connection: { host: "localhost", port: 6379 } });

export async function enqueueOrder(orderId: string) {
  await orderQueue.add("process-order", { orderId });
  console.log(`✅ Pedido ${orderId} enfileirado!`);
}
```

📜 Processamento da Fila (BullMQ Worker):
```ts
import { Worker } from "bullmq";

const worker = new Worker("order-processing", async job => {
  const { orderId } = job.data;
  console.log(`🚀 Processando pedido ${orderId}...`);

  // Atualiza estoque e envia notificação
});
```

✅ Vantagens:

Reprocessamento automático em caso de falha.
Controle de concorrência.
Monitoramento em tempo real.
🌬️ 3. Orquestração com Apache Airflow
📌 Cenário: Um fluxo de Airflow para processar pedidos, atualizar estoque e enviar notificações.

📜 DAG de Exemplo (Python):
```ts
from airflow import DAG
from airflow.operators.python import PythonOperator
from datetime import datetime

def process_order(**kwargs):
    order_id = kwargs["dag_run"].conf.get("order_id")
    print(f"✅ Processando pedido: {order_id}")

def update_stock(**kwargs):
    order_id = kwargs["dag_run"].conf.get("order_id")
    print(f"📦 Atualizando estoque para pedido: {order_id}")

def notify_customer(**kwargs):
    order_id = kwargs["dag_run"].conf.get("order_id")
    print(f"📨 Notificando cliente sobre pedido: {order_id}")

with DAG("order_processing_dag", start_date=datetime(2024, 2, 19), schedule_interval=None, catchup=False) as dag:
    process_order_task = PythonOperator(task_id="process_order", python_callable=process_order)
    update_stock_task = PythonOperator(task_id="update_stock", python_callable=update_stock)
    notify_customer_task = PythonOperator(task_id="notify_customer", python_callable=notify_customer)

    process_order_task >> update_stock_task >> notify_customer_task
```

✅ Vantagens:

Orquestração visual de fluxos complexos.
Reprocessamento automático em falhas.
Agendamento flexível e integração com serviços externos.
☁️ 4. Usando AWS SQS e SNS
📌 Cenário: Publicar eventos para notificações via SNS e processar via SQS.

📜 Publicando Evento SNS:
```ts
import { SNSClient, PublishCommand } from "@aws-sdk/client-sns";

const sns = new SNSClient({ region: "us-east-1" });

export async function publishOrderEvent(orderId: string) {
  const command = new PublishCommand({
    TopicArn: "arn:aws:sns:us-east-1:123456789012:OrderTopic",
    Message: JSON.stringify({ orderId }),
  });

  await sns.send(command);
  console.log(`✅ Evento "order.created" enviado para SNS!`);
}
```

📜 Consumindo Evento SQS:
```ts
import { SQSClient, ReceiveMessageCommand } from "@aws-sdk/client-sqs";

const sqs = new SQSClient({ region: "us-east-1" });

export async function receiveOrderEvents() {
  const command = new ReceiveMessageCommand({
    QueueUrl: "https://sqs.us-east-1.amazonaws.com/123456789012/OrderQueue",
    MaxNumberOfMessages: 10,
  });

  const response = await sqs.send(command);
  for (const message of response.Messages || []) {
    const orderData = JSON.parse(message.Body);
    console.log(`📥 Novo evento de pedido: ${orderData.orderId}`);
  }
}
```

✅ Vantagens:

Alta disponibilidade.
Escalabilidade automática.
Integração com outros serviços da AWS.
🔷 5. Usando Azure Service Bus
📌 Cenário: Processar eventos order.created usando filas ou tópicos no Azure.

📜 Enviando Evento para Service Bus:
```ts
import { ServiceBusClient } from "@azure/service-bus";

const sbClient = new ServiceBusClient("Endpoint=sb://seu-nome.servicebus.windows.net/");
const sender = sbClient.createSender("order-created");

export async function sendOrderCreated(orderId: string) {
  await sender.sendMessages({ body: { orderId } });
  console.log(`✅ Evento "order.created" enviado para Azure Service Bus!`);
}
```

📜 Consumindo Evento do Service Bus:
```ts
const receiver = sbClient.createReceiver("order-created");

receiver.subscribe({
  processMessage: async message => {
    console.log(`📥 Pedido recebido: ${message.body.orderId}`);
  },
  processError: async error => {
    console.error(`❌ Erro no processamento: ${error}`);
  }
});
```

✅ Vantagens:

Alta integração com serviços do Azure.
Suporte para filas e tópicos.
Processamento em lote.


📦 1. Interface Genérica para Consumers e Publishers

Para implementarmos uma interface genérica para Consumers e Publishers devemos definir quais são suas funcionalidades mínimas, além do publish para o Publisher e do consume para o Consumer eu resolvi declarar também as funções: ack e nack.


📌 O que são ack e nack?

- ack(message): Confirma o processamento da mensagem, removendo-a da fila.
- nack(message, requeue?): Indica falha no processamento.
  - Se requeue = true, a mensagem volta para a fila.
  - Se requeue = false, a mensagem é descartada ou enviada para uma Dead Letter Queue (DLQ).
📦 Como diferentes tecnologias tratam ack e nack?

## 📦 Como diferentes tecnologias tratam `ack` e `nack`?

| Mensageria           | `ack` (Confirmação)                           | `nack` (Rejeição)                                         |
|----------------------|-----------------------------------------------|------------------------------------------------------------|
| **Kafka**            | Offset é movido para frente automaticamente   | Não há `nack` nativo; controle manual do reprocessamento   |
| **RabbitMQ**         | `channel.ack(msg)` remove a mensagem da fila  | `channel.nack(msg, requeue?)` devolve ou descarta a mensagem |
| **AWS SQS**          | `DeleteMessageCommand` remove a mensagem      | Sem `nack` nativo; a mensagem retorna após o timeout        |
| **Azure Service Bus**| `completeMessage(msg)` remove a mensagem      | `abandonMessage(msg)` devolve para a fila                   |

Acho importante que a gente tenha uma forma genérica 

Essa interface garante que qualquer tecnologia siga o mesmo contrato.

📜 interfaces/IMessagePublisher.ts:
```ts
export interface IMessagePublisher {
  publish(topic: string, message: object): Promise<void>;
}
```

📜 interfaces/IMessageConsumer.ts:
```ts
export interface IMessageConsumer {
  consume(
    topic: string, 
    handler: (message: any, ack: () => void, 
    nack: (requeue?: boolean) => void) => Promise<void>
  ): Promise<void>;
}
```

🏗 2. PublisherFactory
A PublisherFactory cria instâncias de publicadores para cada tecnologia.

factories/PublishersModules.ts
```ts

import { IMessagePublisher } from "../interfaces/IMessagePublisher";
import { KafkaPublisher } from "../publishers/KafkaPublisher";
import { RabbitMQPublisher } from "../publishers/RabbitMQPublisher";
import { SNSPublisher } from "../publishers/SNSPublisher";
import { ServiceBusPublisher } from "../publishers/ServiceBusPublisher";

const PUBLISHERS = {
  "kafka": new KafkaPublisher(),
  "rabbitmq": new RabbitMQPublisher(),
  "sns": new SNSPublisher(),
  "servicebus": new ServiceBusPublisher()
}

export default PUBLISHERS;
```

📜 factories/PublisherFactory.ts:
```ts
import PUBLISHERS from "./PublishersModules"

export class PublisherFactory {
  static createPublisher(provider: string): IMessagePublisher {
    return PUBLISHERS[provider.toLowerCase()];
  }
}

```

⚙️ 3. ConsumerFactory
A ConsumerFactory cria consumidores para cada tecnologia.

📜 factories/ConsumerFactory.ts:
```ts
import { IMessageConsumer } from "../interfaces/IMessageConsumer";
import { KafkaConsumer } from "../consumers/KafkaConsumer";
import { RabbitMQConsumer } from "../consumers/RabbitMQConsumer";
import { SQSConsumer } from "../consumers/SQSConsumer";
import { ServiceBusConsumer } from "../consumers/ServiceBusConsumer";

const CONSUMERS = {
  "kafka": KafkaConsumer,
  "rabbitmq": RabbitMQConsumer,
  "sns": SQSConsumer,
  "servicebus": ServiceBusConsumer
}

export class ConsumerFactory {
  static createConsumer(provider: string, useSingleton = process.env.QUEUE_CONSUMER_SINGLETON | true): IMessageConsumer {
    const key = `consumer:${provider.toLowerCase()}`;

    if (!useSingleton) {
      return this.instantiateConsumer(provider);
    }

    return InstanceManager.getInstance<IMessageConsumer>(key, () => {
      return this.instantiateConsumer(provider);
    });
  }

  private static instantiateConsumer(provider: string): IMessageConsumer {
    return new CONSUMERS[provider.toLowerCase()]();
  }
}
```

🚀 4. Implementação dos Publishers
Vamos criar os publicadores para cada tecnologia.

📜 publishers/KafkaPublisher.ts:
```ts
import { Kafka } from "kafkajs";
import { IMessagePublisher } from "../interfaces/IMessagePublisher";

export class KafkaPublisher implements IMessagePublisher {
  private kafka = new Kafka({ clientId: "crm-app", brokers: ["localhost:9092"] });
  private producer = this.kafka.producer();

  async publish(topic: string, message: object): Promise<void> {
    await this.producer.connect();
    await this.producer.send({
      topic,
      messages: [{ value: JSON.stringify(message) }],
    });
    console.log(`✅ Mensagem publicada no Kafka - Tópico: ${topic}`);
    await this.producer.disconnect();
  }
}
```

📜 publishers/RabbitMQPublisher.ts:
```ts
import amqp from "amqplib";
import { IMessagePublisher } from "../interfaces/IMessagePublisher";

export class RabbitMQPublisher implements IMessagePublisher {
  private connection: amqp.Connection;
  private channel: amqp.Channel;

  async publish(queue: string, message: object): Promise<void> {
    this.connection = await amqp.connect("amqp://localhost");
    this.channel = await this.connection.createChannel();
    await this.channel.assertQueue(queue);
    this.channel.sendToQueue(queue, Buffer.from(JSON.stringify(message)));
    console.log(`✅ Mensagem publicada no RabbitMQ - Fila: ${queue}`);
    await this.channel.close();
    await this.connection.close();
  }
}
```

📜 publishers/SNSPublisher.ts:
```ts
import { SNSClient, PublishCommand } from "@aws-sdk/client-sns";
import { IMessagePublisher } from "../interfaces/IMessagePublisher";

export class SNSPublisher implements IMessagePublisher {
  private sns = new SNSClient({ region: "us-east-1" });

  async publish(topicArn: string, message: object): Promise<void> {
    const command = new PublishCommand({
      TopicArn: topicArn,
      Message: JSON.stringify(message),
    });
    await this.sns.send(command);
    console.log(`✅ Mensagem publicada no SNS - Tópico: ${topicArn}`);
  }
}
```

📜 publishers/ServiceBusPublisher.ts:
```ts
import { ServiceBusClient } from "@azure/service-bus";
import { IMessagePublisher } from "../interfaces/IMessagePublisher";

export class ServiceBusPublisher implements IMessagePublisher {
  private client = new ServiceBusClient(process.env.AZURE_SERVICE_BUS_CONNECTION!);
  private sender = this.client.createSender("order-topic");

  async publish(topic: string, message: object): Promise<void> {
    await this.sender.sendMessages({ body: message });
    console.log(`✅ Mensagem publicada no Azure Service Bus - Tópico: ${topic}`);
    await this.sender.close();
  }
}
```

📥 5. Implementação dos Consumers
Vamos criar os consumidores para cada tecnologia.

📜 consumers/KafkaConsumer.ts:
```ts
import { Kafka } from "kafkajs";
import { IMessageConsumer } from "../interfaces/IMessageConsumer";

export class KafkaConsumer implements IMessageConsumer {
  private kafka = new Kafka({ clientId: "crm-app", brokers: ["localhost:9092"] });
  private consumer = this.kafka.consumer({ groupId: "crm-group" });

  async consume(
    topic: string, 
    handler: (message: any, ack: () => void, 
    nack: (requeue?: boolean) => void) => Promise<void>
  ): Promise<void> {
    await this.consumer.connect();
    await this.consumer.subscribe({ topic, fromBeginning: false });

    await this.consumer.run({
      eachMessage: async ({ message, partition, topic }) => {
        const content = JSON.parse(message.value?.toString() || "{}");
        console.log(`📩 Kafka - Mensagem recebida: ${JSON.stringify(content)}`);

        // `ack` é apenas um log, pois Kafka confirma automaticamente
        const ack = () => console.log(`✅ Kafka - Mensagem processada: ${message.offset}`);

        // `nack` simulado movendo offset manualmente
        const nack = (requeue: boolean) => {
          if (requeue) {
            console.warn(`⚠️ Kafka - Mensagem não processada. Reprocessar manualmente.`);
          }
        };

        await handler(content, ack, nack);
      },
    });
  }
}

```

📜 consumers/RabbitMQConsumer.ts:
```ts
import amqp from "amqplib";
import { IMessageConsumer } from "../interfaces/IMessageConsumer";

export class RabbitMQConsumer implements IMessageConsumer {
  private connection!: amqp.Connection;
  private channel!: amqp.Channel;

  async consume(
    queue: string, 
    handler: (message: any, ack: () => void, 
    nack: (requeue?: boolean) => void) => Promise<void>
  ): Promise<void> {
    this.connection = await amqp.connect("amqp://localhost");
    this.channel = await this.connection.createChannel();
    await this.channel.assertQueue(queue);

    this.channel.consume(queue, async (msg) => {
      if (msg) {
        const content = JSON.parse(msg.content.toString());
        console.log(`📩 RabbitMQ - Mensagem recebida: ${JSON.stringify(content)}`);

        // `ack` confirma o processamento e remove a mensagem da fila
        const ack = () => {
          this.channel.ack(msg);
          console.log(`✅ RabbitMQ - Mensagem processada.`);
        };

        // `nack` devolve a mensagem para a fila se `requeue = true`
        const nack = (requeue = true) => {
          this.channel.nack(msg, false, requeue);
          console.log(`⚠️ RabbitMQ - Mensagem falhou. Reenviar? ${requeue}`);
        };

        await handler(content, ack, nack);
      }
    });
  }
}

```

📜 consumers/SQSConsumer.ts:
```ts
import { SQSClient, ReceiveMessageCommand, DeleteMessageCommand } from "@aws-sdk/client-sqs";
import { IMessageConsumer } from "../interfaces/IMessageConsumer";

export class SQSConsumer implements IMessageConsumer {
  private sqs = new SQSClient({ region: "us-east-1" });

  async consume(
    queueUrl: string, 
    handler: (message: any, ack: () => void, 
    nack: (requeue?: boolean) => void) => Promise<void>
  ): Promise<void> {
    const command = new ReceiveMessageCommand({
      QueueUrl: queueUrl,
      MaxNumberOfMessages: 10,
      WaitTimeSeconds: 5,
    });

    setInterval(async () => {
      const response = await this.sqs.send(command);
      for (const msg of response.Messages || []) {
        const body = JSON.parse(msg.Body!);
        console.log(`📩 AWS SQS - Mensagem recebida: ${JSON.stringify(body)}`);

        const ack = async () => {
          await this.sqs.send(new DeleteMessageCommand({
            QueueUrl: queueUrl,
            ReceiptHandle: msg.ReceiptHandle!,
          }));
          console.log(`✅ AWS SQS - Mensagem processada e removida.`);
        };

        const nack = (requeue = true) => {
          if (!requeue) console.warn(`⚠️ AWS SQS - Mensagem falhou e será descartada.`);
          // Mensagem será automaticamente reprocessada após timeout
        };

        await handler(body, ack, nack);
      }
    }, 5000);
  }
}

```

📜 consumers/ServiceBusConsumer.ts:
```ts
import { ServiceBusClient } from "@azure/service-bus";
import { IMessageConsumer } from "../interfaces/IMessageConsumer";

export class ServiceBusConsumer implements IMessageConsumer {
  private client = new ServiceBusClient(process.env.AZURE_SERVICE_BUS_CONNECTION!);
  private receiver = this.client.createReceiver("order-topic");

  async consume(
    topic: string, 
    handler: (message: any, ack: () => void, 
    nack: (requeue?: boolean) => void) => Promise<void>
  ): Promise<void> {
    this.receiver.subscribe({
      processMessage: async (msg) => {
        const content = msg.body;
        console.log(`📩 Azure Service Bus - Mensagem recebida: ${JSON.stringify(content)}`);

        const ack = async () => {
          await this.receiver.completeMessage(msg);
          console.log(`✅ Azure Service Bus - Mensagem processada.`);
        };

        const nack = async (requeue = true) => {
          if (requeue) {
            await this.receiver.abandonMessage(msg);
            console.warn(`⚠️ Azure Service Bus - Mensagem retornada à fila.`);
          } else {
            console.warn(`❌ Azure Service Bus - Mensagem descartada.`);
          }
        };

        await handler(content, ack, nack);
      },
      processError: async (err) => {
        console.error(`❌ Erro no processamento: ${err}`);
      },
    });
  }
}

```

🎯 6. Exemplo de Uso no CRM
Vamos integrar tudo para o fluxo de pedidos via WhatsApp.

📜 Publicando um Pedido::
```ts
import { PublisherFactory } from "./factories/PublisherFactory";

async function createOrder(orderId: string) {
  const publisher = PublisherFactory.createPublisher("kafka");
  await publisher.publish("order.created", { orderId, customer: "12345" });
}

createOrder("abc123");
```

📜 Consumindo o Pedido::
```ts
import { ConsumerFactory } from "./factories/ConsumerFactory";

async function startOrderConsumer() {
  const consumer = ConsumerFactory.createConsumer("kafka");
  await consumer.consume("order.created", async (message) => {
    console.log(`📥 Processando pedido: ${JSON.stringify(message)}`);
    // Lógica de negócio
  });
}

startOrderConsumer();
```

📜 utils/RetryFallback.ts:
```ts
type RetryHandler = (message: any) => Promise<void>;

interface RetryMetadata {
  attempts: number;
  nextRetry: Date;
}

const retryMetadata: Map<string, RetryMetadata> = new Map();

export async function retryWithFallback(
  messageId: string,
  message: any,
  handler: RetryHandler,
  nack: (requeue?: boolean) => void
): Promise<void> {
  const maxRetries = parseInt(process.env.QUEUE_MAX_RETRIES ?? "5", 10);
  const retryInterval = parseInt(process.env.QUEUE_MAX_RETRIES_TIME ?? "10", 10) * 1000;

  // Obtem tentativas anteriores (ou inicializa)
  const metadata = retryMetadata.get(messageId) || { attempts: 0, nextRetry: new Date() };
  const { attempts } = metadata;

  if (attempts >= maxRetries) {
    console.error(`❌ Mensagem ${messageId} atingiu o máximo de ${maxRetries} tentativas. Movendo para DLQ.`);
    nack(false);  // Descarte final ou envio para DLQ
    retryMetadata.delete(messageId);
    return;
  }

  // Agendamento do retry
  const nextRetry = new Date(Date.now() + retryInterval);
  retryMetadata.set(messageId, { attempts: attempts + 1, nextRetry });

  console.warn(`⚠️ Tentativa ${attempts + 1} para mensagem ${messageId}. Nova tentativa em ${retryInterval / 1000} segundos.`);

  setTimeout(async () => {
    try {
      await handler(message);
      console.log(`✅ Mensagem ${messageId} processada com sucesso após ${attempts + 1} tentativa(s).`);
      retryMetadata.delete(messageId);
    } catch (error) {
      console.error(`❌ Falha no retry ${attempts + 1} para mensagem ${messageId}.`, error);
      await retryWithFallback(messageId, message, handler, nack);  // Retry recursivo
    }
  }, retryInterval);
}

```

📜 Exemplo com KafkaConsumer:
```ts
import { Kafka } from "kafkajs";
import { IMessageConsumer } from "../interfaces/IMessageConsumer";
import { retryWithFallback } from "../utils/RetryFallback";

export class KafkaConsumer implements IMessageConsumer {
  private kafka = new Kafka({ clientId: "crm-app", brokers: ["localhost:9092"] });
  private consumer = this.kafka.consumer({ groupId: "crm-group" });

  async consume(topic: string, handler: (message: any, ack: () => void, nack: (requeue?: boolean) => void) => Promise<void>): Promise<void> {
    await this.consumer.connect();
    await this.consumer.subscribe({ topic, fromBeginning: false });

    await this.consumer.run({
      eachMessage: async ({ message }) => {
        const messageId = message.key?.toString() || Date.now().toString();
        const content = JSON.parse(message.value?.toString() || "{}");

        console.log(`📩 Kafka - Mensagem recebida: ${JSON.stringify(content)}`);

        const ack = () => {
          console.log(`✅ Kafka - Mensagem ${messageId} processada.`);
        };

        const nack = (requeue = true) => {
          console.warn(`⚠️ Kafka - Falha na mensagem ${messageId}. Reenviar: ${requeue}`);
          if (requeue) {
            retryWithFallback(messageId, content, async () => handler(content, ack, nack), nack);
          }
        };

        try {
          await handler(content, ack, nack);
        } catch (error) {
          console.error(`❌ Kafka - Erro ao processar mensagem ${messageId}:`, error);
          nack(true);
        }
      },
    });
  }
}
```

4️⃣ Exemplo com RabbitMQConsumer:
```ts
import amqp from "amqplib";
import { IMessageConsumer } from "../interfaces/IMessageConsumer";
import { retryWithFallback } from "../utils/RetryFallback";

export class RabbitMQConsumer implements IMessageConsumer {
  private connection!: amqp.Connection;
  private channel!: amqp.Channel;

  async consume(queue: string, handler: (message: any, ack: () => void, nack: (requeue?: boolean) => void) => Promise<void>): Promise<void> {
    this.connection = await amqp.connect("amqp://localhost");
    this.channel = await this.connection.createChannel();
    await this.channel.assertQueue(queue);

    this.channel.consume(queue, async (msg) => {
      if (msg) {
        const messageId = msg.properties.messageId || Date.now().toString();
        const content = JSON.parse(msg.content.toString());

        console.log(`📩 RabbitMQ - Mensagem recebida: ${JSON.stringify(content)}`);

        const ack = () => {
          this.channel.ack(msg);
          console.log(`✅ RabbitMQ - Mensagem ${messageId} processada.`);
        };

        const nack = (requeue = true) => {
          console.warn(`⚠️ RabbitMQ - Falha na mensagem ${messageId}. Reenviar: ${requeue}`);
          if (requeue) {
            retryWithFallback(messageId, content, async () => handler(content, ack, nack), nack);
          } else {
            this.channel.nack(msg, false, false); // Descarte final
          }
        };

        try {
          await handler(content, ack, nack);
        } catch (error) {
          console.error(`❌ RabbitMQ - Erro ao processar mensagem ${messageId}:`, error);
          nack(true);
        }
      }
    });
  }
}
```

5️⃣ Exemplo de Uso (Aplicação Real)
Vamos criar um consumidor simples com falha simulada e retry automático.
```ts
import { ConsumerFactory } from "./factories/ConsumerFactory";

async function startConsumer() {
  const consumer = ConsumerFactory.createConsumer("kafka");

  await consumer.consume("order.created", async (message, ack, nack) => {
    console.log(`📥 Processando mensagem: ${JSON.stringify(message)}`);

    // Simulando falha
    if (!message.orderId) {
      console.error("❌ Falha: OrderId ausente.");
      nack(true);  // Reenviar com retry automático
      return;
    }

    console.log(`✅ Pedido ${message.orderId} processado com sucesso.`);
    ack();  // Confirmação final
  });
}

startConsumer();
```


## 📊 Benefícios da Solução

| Recurso               | Descrição                                                       |
|-----------------------|-----------------------------------------------------------------|
| ✅ **Segurança**       | Valida provedores para evitar erros de execução.                |
| 🔄 **Flexibilidade**   | Alterna entre Singleton e Factory conforme necessidade.         |
| 📦 **Gerenciamento**   | Inclui método `clearInstance` para reiniciar instâncias.         |
| 💪 **Tipagem Forte**   | Usa `Record<string, { new(): IMessageConsumer }>` para classes. |
| ⚙️ **Configuração**    | Controlado via `process.env.QUEUE_CONSUMER_SINGLETON`.           |
| 🚀 **Extensibilidade** | Fácil adicionar novos provedores no objeto `CONSUMERS`.         |
| 🔒 **Eficiência**      | Singleton evita múltiplas conexões, economizando recursos.       |


## Modelagem

## 🌐 Diagrama de Integração das Camadas na Agents Layer

```mermaid
flowchart TD
  %% Entrada de Mensagem
  subgraph Entrada
    API[API]
    WhatsApp[WhatsApp Webhook]
    Kafka[Kafka Topic]
  end

  %% Camada de Mensageria
  subgraph Mensageria
    RabbitMQ[RabbitMQ Queue]
    SQS[AWS SQS Queue]
    SNS[AWS SNS Topic]
  end

  %% Agents Layer
  subgraph Agents Layer
    ExecutionAgent[Execution Agent(Processamento de Pedidos)]
    SignalAgent[Signal Agent 📣\n(Emissão de Eventos)]
    CoordinatorAgent[Coordinator Agent 🔄\n(Gerenciamento de Saga)]
    ObserverAgent[Observer Agent 👀\n(Monitoramento e Logs)]
    CognitiveAgent[Cognitive Agent 🤖\n(Análise com IA)]
  end

  %% Camada de Persistência
  subgraph Persistência
    EventStore[(Event Store 🕰️\n(Event Sourcing))]
    ReadModel[(Read Model 📊\n(CQRS - Projeções))]
    DLQ[(Dead Letter Queue ☠️)]
  end

  %% Monitoramento
  subgraph Monitoramento
    Prometheus[(Prometheus 📈\n(Métricas))]
    Jaeger[(Jaeger 🔍\n(Tracing))]
    ELK[(ELK Stack 📊\n(Logs))]
  end

  %% Fluxo de Integração
  API -->|Mensagem Recebida| Kafka & RabbitMQ & WhatsApp
  WhatsApp -->|Nova Mensagem| ExecutionAgent
  Kafka -->|Evento Criado| ExecutionAgent
  RabbitMQ -->|Comando| ExecutionAgent
  ExecutionAgent -->|Processa Pedido| EventStore
  ExecutionAgent -->|Emite Evento| SignalAgent
  SignalAgent -->|Notificação| API
  SignalAgent -->|Confirmação| Kafka

  ExecutionAgent -->|Falha?| Retry{Retry Policy?}
  Retry -- Falhou --> DLQ
  Retry -- Sucesso --> EventStore

  EventStore -->|Projeção CQRS| ReadModel
  EventStore -->|Evento Saga| CoordinatorAgent
  CoordinatorAgent -->|Inicia Fluxo Saga| ExecutionAgent
  CoordinatorAgent -->|Falha?| DLQ
  CoordinatorAgent -->|Sucesso| SignalAgent

  ObserverAgent -->|Logs e Métricas| Prometheus & Jaeger & ELK
  CognitiveAgent -->|Análise de Mensagem| EventStore

  EventStore -->|Histórico| ReadModel
  ReadModel -->|Consulta| API
```

## MicroServiços

Essa Arquitetura define uma forma lógica de como utilizar microserviços em seu sistema, cada microserviço é um Agente pois ele é reativo e contém uma Tool(Ferramenta/Lógica) própria, exemplo:

microservices/
├─ api-gateway/         # Interface Layer (Porta de Entrada)
├─ execution-agent/     # Order Service (Pedido)
├─ signal-agent/        # Notification Service (Notificação)
├─ coordinator-agent/   # Payment Service (Orquestração)
├─ observer-agent/      # Monitoring Service (Observabilidade)
├─ cognitive-agent/     # Recommendation Service (IA)
├─ event-bus/           # Comunicação entre serviços (Kafka/RabbitMQ)
├─ event-store/         # Event Sourcing (MongoDB)
└─ shared/              # Código comum (JWT, Idempotência, DLQ, Tools)


🏗️ Arquitetura de Microserviços: Visão Geral
API Gateway (Interface Layer): Recebe as requisições REST.

Microserviços:
- Order Agent: Cria pedidos (Agents Layer).
- Payment Agent: Processa pagamentos (Agents Layer).
- Stock Agent: Verifica estoque (Agents Layer).
- Notification Agent: Envia notificações (Agents Layer).
- Event Bus: Comunicação entre serviços via Kafka ou RabbitMQ (Infrasctructure Layer).
- Event Store: Registra eventos para Event Sourcing (Infrasctructure Layer).
- DLQ: Gerencia mensagens falhas (Infrasctructure Layer).


Observabilidade: Monitoramento com Prometheus, Jaeger e ELK Stack.

📜 Fluxo Completo (Exemplo: Criar um Pedido):

- O cliente envia um POST /orders para o API Gateway.
- O Order Service emite o evento order.create para o Order Agent 
  - que cria o pedido e publica o evento order.created.
- O Stock Agent recebe esse evento e verifica o estoque respondendo com stock.checked
  - enviando a quantidade total do produto no estoque.
- O Payment Service processa o pagamento (payment.processed)
  - que o Payment Agent recebe e adiciona o valor no Sistema Contábil.

Se tudo der certo, o Notification Agent notifica o cliente.
Se alguma etapa falhar, a Saga compensa a transação e cancela o pedido.

🌐 Estrutura de Pastas:

```
microservices/
├─ api-gateway/         # Interface REST com Express
├─ services
├─ order-service/       # Serviço de pedidos (Execution Agent)
├─ stock-service/       # Serviço de estoque (Signal Agent)
├─ payment-service/     # Serviço de pagamento (Coordinator Agent)
├─ notification-service/ # Serviço de notificação (Signal Agent)
├─ event-bus/           # Kafka/RabbitMQ para Event Sourcing
├─ event-store/         # MongoDB como Event Store
├─ shared/              # Código compartilhado (Tools, DLQ, Idempotência)
├─ monitoring/          # Prometheus, Jaeger, ELK para Observabilidade
└─ docker-compose.yml   # Infraestrutura dos microserviços
```

1️⃣ API Gateway: Entrada das Requisições
O API Gateway expõe os endpoints REST e roteia as requisições.

javascript
Copiar
Editar
// api-gateway/index.js
import express from "express";
import axios from "axios";

const app = express();
app.use(express.json());

// Criar pedido (rota principal)
app.post("/orders", async (req, res) => {
  const { customerId, items } = req.body;

  try {
    const response = await axios.post("http://order-service:4001/orders", { customerId, items });
    res.status(201).json(response.data);
  } catch (error) {
    console.error("Erro ao criar pedido:", error.message);
    res.status(500).json({ error: "Falha ao processar o pedido." });
  }
});

// Health Check
app.get("/health", (req, res) => res.send("API Gateway rodando 🚀"));

app.listen(4000, () => console.log("🚪 API Gateway rodando na porta 4000"));
✅ Motivo: O Gateway é o ponto central para entrada de APIs REST.

2️⃣ Order Service: Criar Pedidos (Execution Agent)
O Order Service cria pedidos e publica eventos.

javascript
Copiar
Editar
// order-service/index.js
import express from "express";
import { EventBus } from "../shared/EventBus.js";
import { EventStore } from "../shared/EventStore.js";
import { Idempotency } from "../shared/Idempotency.js";

const app = express();
app.use(express.json());

// Criar pedido
app.post("/orders", async (req, res) => {
  const { customerId, items } = req.body;
  const orderId = `order-${Date.now()}`;

  if (await Idempotency.isProcessed(orderId)) {
    return res.status(409).json({ error: "Pedido já processado." });
  }

  // Salva no Event Store (Event Sourcing)
  await EventStore.saveEvent("order.created", { orderId, customerId, items });

  // Publica evento para o Event Bus
  EventBus.publish("order.created", { orderId, customerId, items });

  res.status(201).json({ orderId, message: "Pedido criado com sucesso." });
});

// Consome resposta do estoque
EventBus.subscribe("stock.checked", async (event) => {
  const { orderId, success } = event;

  if (success) {
    console.log(`✅ Estoque confirmado para pedido #${orderId}`);
    EventBus.publish("payment.process", { orderId, amount: 100 });
  } else {
    console.warn(`❌ Estoque insuficiente para pedido #${orderId}`);
    EventBus.publish("order.failed", { orderId, reason: "Estoque insuficiente" });
  }
});

app.listen(4001, () => console.log("📦 Order Service rodando na porta 4001"));
✅ Motivo:

Execution Agent: Processa o pedido.
Event Sourcing: Salva no Event Store.
Idempotência: Evita duplicidade.
3️⃣ Stock Service: Verificar Estoque (Signal Agent)
O Stock Service verifica o estoque.

javascript
Copiar
Editar
// stock-service/index.js
import express from "express";
import { EventBus } from "../shared/EventBus.js";

const app = express();
app.use(express.json());

// Consome evento de pedido criado
EventBus.subscribe("order.created", async (event) => {
  const { orderId, items } = event;
  console.log(`🔍 Verificando estoque para pedido #${orderId}`);

  const estoqueDisponivel = Math.random() > 0.2; // Simula disponibilidade

  // Publica evento baseado na verificação
  EventBus.publish("stock.checked", { orderId, success: estoqueDisponivel });
});

app.listen(4002, () => console.log("🏷️ Stock Service rodando na porta 4002"));
✅ Motivo:

Signal Agent: Emite sinal após execução.
4️⃣ Payment Service: Processar Pagamento (Coordinator Agent)
O Payment Service gerencia a transação da Saga.

javascript
Copiar
Editar
// payment-service/index.js
import express from "express";
import { EventBus } from "../shared/EventBus.js";
import { CircuitBreaker } from "../shared/CircuitBreaker.js";

const app = express();
app.use(express.json());

EventBus.subscribe("payment.process", async (event) => {
  const { orderId, amount } = event;
  console.log(`💳 Processando pagamento de R$${amount} para pedido #${orderId}`);

  const breaker = new CircuitBreaker(async () => {
    const sucesso = Math.random() > 0.3; // Simula falha 30% das vezes
    if (!sucesso) throw new Error("Falha no pagamento.");

    EventBus.publish("payment.processed", { orderId });
  });

  breaker.execute().catch(() => {
    console.error(`❌ Pagamento falhou para pedido #${orderId}`);
    EventBus.publish("saga.failed", { orderId, reason: "Falha no pagamento" });
  });
});

app.listen(4003, () => console.log("💰 Payment Service rodando na porta 4003"));
✅ Motivo:

Coordinator Agent: Gerencia o fluxo da Saga Pattern.
Circuit Breaker: Evita falhas repetidas.
5️⃣ Notification Service: Enviar Notificações (Signal Agent)
javascript
Copiar
Editar
// notification-service/index.js
import express from "express";
import { EventBus } from "../shared/EventBus.js";

const app = express();
app.use(express.json());

// Notificar cliente após pagamento aprovado
EventBus.subscribe("payment.processed", (event) => {
  const { orderId } = event;
  console.log(`📣 Enviando notificação: Pedido #${orderId} confirmado!`);
});

app.listen(4004, () => console.log("📨 Notification Service rodando na porta 4004"));
✅ Motivo:

Signal Agent: Emite sinal após evento processado.
6️⃣ Event Bus (RabbitMQ/Kafka)
Um EventBus simples para comunicação entre microserviços.

javascript
Copiar
Editar
// shared/EventBus.js
import { EventEmitter } from "events";

export class EventBus {
  static bus = new EventEmitter();

  static publish(event, payload) {
    console.log(`📣 [EventBus] Evento publicado: ${event}`);
    this.bus.emit(event, payload);
  }

  static subscribe(event, handler) {
    console.log(`🔔 [EventBus] Assinando evento: ${event}`);
    this.bus.on(event, handler);
  }
}
✅ Motivo: Comunicação assíncrona entre microserviços.

7️⃣ Event Store (MongoDB para Event Sourcing)
javascript
Copiar
Editar
// shared/EventStore.js
import mongoose from "mongoose";

mongoose.connect("mongodb://event-store:27017/eventstore");

const eventSchema = new mongoose.Schema({
  eventType: String,
  payload: Object,
  timestamp: { type: Date, default: Date.now }
});

const Event = mongoose.model("Event", eventSchema);

export class EventStore {
  static async saveEvent(eventType, payload) {
    await Event.create({ eventType, payload });
    console.log(`💾 [EventStore] Evento salvo: ${eventType}`);
  }

  static async getEvents(eventType) {
    return await Event.find({ eventType }).lean();
  }
}
✅ Motivo: Armazena eventos para Event Sourcing e recuperação do estado.

8️⃣ Docker-Compose para Infraestrutura
yaml
Copiar
Editar
version: "3.8"

services:
  api-gateway:
    build: ./api-gateway
    ports:
      - "4000:4000"
    depends_on:
      - order-service

  order-service:
    build: ./order-service
    ports:
      - "4001:4001"
    depends_on:
      - event-store

  stock-service:
    build: ./stock-service
    ports:
      - "4002:4002"

  payment-service:
    build: ./payment-service
    ports:
      - "4003:4003"

  notification-service:
    build: ./notification-service
    ports:
      - "4004:4004"

  event-store:
    image: mongo:6.0
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db

volumes:
  mongo-data:
✅ Motivo: Orquestra todos os serviços com Docker.

9️⃣ Observabilidade: Prometheus, Jaeger e ELK
Prometheus: Coleta métricas via /metrics em cada serviço.
Jaeger: Rastreia cada requisição de ponta a ponta.
ELK Stack: Centraliza logs.
✅ Motivo: Acompanha a saúde e o desempenho dos microserviços.



Exemplo completo:

Product Listing: ProductAgent manages the product catalog.

Order Created: Customer places an order → OrderAgent creates the order.

Stock Checked: StockAgent verifies product availability.

Payment Processed: PaymentAgent charges the customer.

Order Updated: OrderAgent updates the order status.

Customer Notified: NotificationAgent informs the customer.

