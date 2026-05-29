# 00 — Seleção de Escopo

## 0. Seleção de Escopo

Este documento apresenta o recorte de escopo que será utilizado no trabalho de modelagem da plataforma **ArenaStream**. Como o sistema completo possui diversos fluxos, atores e funcionalidades, a modelagem não contemplará todos os requisitos levantados no Documento de Requisitos. Em vez disso, serão selecionadas três fatias verticais representativas, permitindo uma análise mais profunda dos fluxos mais importantes do sistema.

Uma fatia vertical representa um fluxo completo, com início, processamento e resultado observável. Dessa forma, a escolha das fatias considera não apenas telas ou entidades isoladas, mas casos de uso completos que atravessam diferentes partes da plataforma.

As fatias escolhidas foram selecionadas com base nos seguintes critérios:

- presença de funcionalidades classificadas como **Must Have** na priorização MoSCoW;
- interação entre mais de um subsistema ou tipo de usuário;
- existência de regras de negócio relevantes, como validação de pagamento, geração de QR Code, atualização em tempo real e bloqueio de reutilização de ingresso.

---

## 0.1 Subsistemas e Atores Identificados

Com base nos documentos anteriores, a plataforma ArenaStream foi organizada em três subsistemas principais.

| Subsistema | Descrição | Atores envolvidos |
|---|---|---|
| Subsistema do Torcedor | Responsável pela busca de eventos, compra de ingressos, acesso ao QR Code, acompanhamento de placar e transmissão | Torcedor |
| Subsistema de Gestão do Evento | Responsável pelo cadastro de eventos, criação de partidas, configuração de ingressos, preços, publicação e atualização de placar | Administrador do Evento |
| Subsistema de Operação ao Vivo | Responsável pela validação de ingresso, controle de acesso, operação de check-in e apoio às funcionalidades em tempo real | Operador de Check-in, Operador de Transmissão |

---

## 0.2 Fatias Verticais Selecionadas

## Fatia 1 — Compra de ingresso online com geração de QR Code

### Histórias cobertas

- **US-SUB1-004** — Como torcedor, eu quero comprar ingressos online, para que eu garanta meu acesso ao evento sem precisar ir ao local antecipadamente.
- **US-SUB1-005** — Como torcedor, eu quero receber confirmação da compra, para que eu tenha segurança de que meu ingresso foi emitido corretamente.
- **US-SUB1-006** — Como torcedor, eu quero acessar meu ingresso digital com QR Code, para que eu consiga entrar no evento de forma prática e segura.
- **US-SUB2-006** — Como administrador do evento, eu quero configurar tipos de ingressos, lotes e preços, para que eu gerencie a estratégia comercial do evento.

### Justificativa da escolha

Esta fatia foi escolhida por representar o fluxo comercial central da ArenaStream. Sem a compra de ingressos, a plataforma perde uma de suas principais propostas de valor. O fluxo envolve o torcedor, o sistema interno da plataforma, o controle de disponibilidade de ingressos e a integração com um gateway de pagamento externo.

Além disso, essa fatia contém regras de negócio importantes, como validação de disponibilidade, confirmação de pagamento, emissão de ingresso único e geração de QR Code. Também é necessário garantir que a venda não seja duplicada quando dois torcedores tentam comprar o mesmo ingresso ou lote ao mesmo tempo.

### O que o grupo espera aprender

Com essa fatia, o grupo espera aprender como modelar um fluxo transacional com integração externa, regras de consistência e geração de um artefato digital importante para o sistema. Essa fatia será útil para representar classes como `Torcedor`, `Evento`, `Partida`, `TipoIngresso`, `Ingresso`, `Pagamento` e `QRCode`.

---

## Fatia 2 — Atualização e exibição do placar ao vivo

### Histórias cobertas

- **US-SUB2-004** — Como administrador do evento, eu quero criar partidas vinculadas a um evento, para que eu organize a programação esportiva disponível ao público.
- **US-SUB2-005** — Como administrador do evento, eu quero definir local, data e horário da partida, para que os torcedores saibam quando e onde ela acontecerá.
- **US-SUB2-007** — Como administrador do evento, eu quero publicar ou suspender uma partida, para que eu controle sua disponibilidade ao público.
- **US-SUB2-009** — Como administrador do evento, eu quero atualizar o placar em tempo real, para que os torcedores recebam o andamento mais recente da partida.
- **US-SUB1-008** — Como torcedor, eu quero acompanhar o placar em tempo real, para que eu fique atualizado durante a partida.

### Justificativa da escolha

Esta fatia foi escolhida porque representa uma funcionalidade essencial para a experiência do torcedor durante o evento. O fluxo começa com o administrador criando e publicando uma partida, passa pela atualização do placar durante o jogo e termina com a exibição do placar atualizado para os torcedores.

Essa fatia envolve mais de um tipo de usuário, pois o administrador altera o estado da partida e o torcedor consome a informação em tempo real. Também envolve regras de negócio relevantes, como permitir atualização de placar apenas em partidas publicadas ou em andamento, registrar horário da última atualização e manter o placar consistente para todos os usuários conectados.

### O que o grupo espera aprender

Com essa fatia, o grupo espera aprender como modelar um fluxo com atualização em tempo real e transições de estado da partida. Essa fatia permite explorar estados como `Criada`, `Publicada`, `Em andamento`, `Encerrada` e `Suspensa`, além de mostrar a relação entre administrador, partida e torcedor.

---

## Fatia 3 — Validação de ingresso no check-in com bloqueio de reutilização

### Histórias cobertas

- **US-SUB3-001** — Como operador de check-in, eu quero validar o ingresso por QR Code, para que eu autorize a entrada apenas de ingressos válidos.
- **US-SUB3-002** — Como operador de check-in, eu quero bloquear reutilização do mesmo ingresso, para que não ocorram acessos duplicados ao evento.
- **US-SUB3-004** — Como operador de check-in, eu quero registrar horário e status da entrada do torcedor, para que haja rastreabilidade operacional do acesso.
- **US-SUB1-006** — Como torcedor, eu quero acessar meu ingresso digital com QR Code, para que eu consiga entrar no evento de forma prática e segura.

### Justificativa da escolha

Esta fatia foi escolhida por representar um momento crítico da operação presencial do evento. Mesmo que a compra online funcione corretamente, o sistema precisa garantir que o ingresso seja validado de forma rápida, segura e sem permitir reutilização indevida.

O fluxo envolve o torcedor, que apresenta o QR Code, o operador de check-in, que realiza a leitura, e o sistema, que verifica o estado do ingresso. A regra de negócio principal é impedir que um ingresso já utilizado seja aceito novamente, além de registrar o horário e o resultado da validação.

### O que o grupo espera aprender

Com essa fatia, o grupo espera aprender como modelar validações de segurança, mudança de estado de uma entidade e rastreabilidade operacional. Essa fatia também ajuda a representar classes e entidades como `Ingresso`, `QRCode`, `ValidacaoEntrada`, `OperadorCheckin` e `Partida`.

---

## 0.3 Cobertura dos Critérios Obrigatórios

| Critério obrigatório | Fatia 1 | Fatia 2 | Fatia 3 |
|---|---|---|---|
| Pelo menos uma fatia Must Have | Sim | Sim | Sim |
| Envolve mais de um subsistema ou tipo de usuário | Sim | Sim | Sim |
| Possui regras de negócio não triviais | Sim | Sim | Sim |

As três fatias escolhidas atendem aos critérios obrigatórios do trabalho. A Fatia 1 representa o núcleo comercial da plataforma, a Fatia 2 representa a experiência em tempo real do torcedor e a Fatia 3 representa a operação presencial e a segurança no acesso ao evento.

---

## 0.4 Requisitos do Trabalho 2 que Serão Modelados

| Fatia | Requisitos modelados |
|---|---|
| Fatia 1 — Compra de ingresso com QR Code | US-SUB1-004, US-SUB1-005, US-SUB1-006, US-SUB2-006 |
| Fatia 2 — Placar ao vivo | US-SUB2-004, US-SUB2-005, US-SUB2-007, US-SUB2-009, US-SUB1-008 |
| Fatia 3 — Check-in e validação de ingresso | US-SUB3-001, US-SUB3-002, US-SUB3-004, US-SUB1-006 |

---

## 0.5 Requisitos que Não Serão Modelados

As seguintes histórias de usuário do Trabalho 2 não serão modeladas neste trabalho, pois não fazem parte das três fatias verticais selecionadas.

### Subsistema do Torcedor

- **US-SUB1-001** — Buscar eventos por modalidade, data, time ou localização.
- **US-SUB1-002** — Visualizar detalhes de uma partida.
- **US-SUB1-003** — Criar conta e autenticar-se na plataforma.
- **US-SUB1-007** — Consultar histórico de ingressos e eventos.
- **US-SUB1-009** — Assistir à transmissão ao vivo quando disponível.
- **US-SUB1-010** — Configurar alertas para times ou eventos favoritos.

Essas histórias são importantes para a plataforma, mas foram deixadas fora do recorte por serem fluxos de apoio ou funcionalidades que podem ser tratadas em versões posteriores da modelagem. Login, busca e visualização de detalhes serão considerados pré-condições ou funcionalidades auxiliares para as fatias escolhidas.

### Subsistema de Gestão do Evento

- **US-SUB2-001** — Autenticar-se no painel administrativo.
- **US-SUB2-002** — Cadastrar um novo evento esportivo.
- **US-SUB2-003** — Editar informações do evento.
- **US-SUB2-008** — Visualizar vendas e ocupação por partida.
- **US-SUB2-010** — Vincular e gerenciar transmissão ao vivo da partida.

Essas histórias não serão detalhadas porque o foco da modelagem estará nos fluxos de compra, placar e validação de acesso. O cadastro básico e a autenticação serão tratados como pré-condições, enquanto relatórios e transmissão ao vivo ficam fora do escopo da modelagem principal.

### Subsistema de Operação ao Vivo

- **US-SUB3-003** — Utilizar modo de contingência em caso de falha de conexão.
- **US-SUB3-005** — Consultar lista de presentes e ausentes.
- **US-SUB3-006** — Iniciar a transmissão ao vivo.
- **US-SUB3-007** — Encerrar a transmissão ao vivo.
- **US-SUB3-008** — Monitorar o status da transmissão.
- **US-SUB3-009** — Receber alertas de falha técnica.
- **US-SUB3-010** — Registrar incidentes operacionais detalhados.

Essas funcionalidades são relevantes para a operação completa do sistema, porém foram excluídas do recorte por ampliarem demais o escopo. A validação do ingresso foi priorizada por envolver segurança, mudança de estado e impacto direto na entrada do torcedor no evento.

---

## 0.6 Conclusão do Recorte

O recorte definido permite modelar três dimensões fundamentais da ArenaStream:

1. **Dimensão comercial**, representada pela compra de ingresso e geração de QR Code.
2. **Dimensão informacional em tempo real**, representada pela atualização e exibição do placar ao vivo.
3. **Dimensão operacional e de segurança**, representada pela validação do ingresso no check-in.

Essas fatias são suficientes para representar aspectos centrais do domínio da plataforma, envolvendo entidades persistentes, fluxos entre usuários, integrações externas, transições de estado e regras de negócio relevantes. Dessa forma, o trabalho mantém profundidade na modelagem sem tentar representar superficialmente todo o sistema.