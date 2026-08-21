# Referência Técnica para Escolha de Distribuições Linux

> Modelos de release, lifecycle, segurança, proveniência de software, desktop, infraestrutura e método de decisão — em português do Brasil.

[![Idioma: pt-BR](https://img.shields.io/badge/idioma-pt--BR-1f6feb)](#idioma-e-convencoes)
[![Edição: 0.1.6](https://img.shields.io/badge/edi%C3%A7%C3%A3o-0.1.6-8250df)](#historico-desta-edicao)
[![Snapshot: 2026-08-15](https://img.shields.io/badge/snapshot-2026--08--15-238636)](#17-recorte-temporal-e-versionamento)
[![Estado: pré-1.0](https://img.shields.io/badge/estado-pr%C3%A9--1.0-d29922)](#status-editorial)
[![Conteúdo: CC BY 4.0](https://img.shields.io/badge/conte%C3%BAdo-CC_BY_4.0-2ea44f)](./LICENSE)
[![Código: MIT](https://img.shields.io/badge/c%C3%B3digo-MIT-f1c40f)](./LICENSE)

---

<a id="indice"></a>

## Índice

- [Resumo executivo](#resumo-executivo)
- [Status editorial](#status-editorial)
  - [Escopo](#status-escopo)
  - [Fora de escopo](#status-fora-de-escopo)
- [Como usar este projeto](#como-usar-este-projeto)
  - [Trilhas de leitura por perfil](#como-usar-trilhas)
- [00. Conceitos fundamentais antes da escolha](#00-conceitos-fundamentais)
- [01. Modelo mental: a classificação é multidimensional](#01-modelo-mental)
- [02. Modelo de release e cadência](#02-modelo-de-release-e-cadencia)
  - [2.0 Seis dimensões de release e atualização dentro do modelo completo](#02-00-seis-dimensoes)
  - [2.1 Tabela-mestre — modelo de release dos 95 candidatos](#02-01-tabela-mestre)
  - [2.2 Fixed Release](#02-02-fixed-release)
  - [2.3 Rolling Release](#02-03-rolling-release)
  - [2.4 Semi-Rolling, Curated Rolling, Slow Rolling e modelos híbridos](#02-04-semi-rolling-curated-hibridos)
  - [2.5 Point Release e Minor Release](#02-05-point-minor-release)
  - [2.6 Branches e canais: Debian Testing/Sid, Fedora Rawhide e outros casos](#02-06-branches-e-canais)
  - [2.7 Fixed/Rolling não é Atomic/Transactional](#02-07-fixed-rolling-nao-e-atomic-transactional)
  - [2.8 Leading edge e bleeding edge](#02-08-leading-edge-e-bleeding-edge)
- [03. Lifecycle, LTS e suporte empresarial](#03-lifecycle-lts-e-suporte-empresarial)
  - [Perguntas para produção](#03-perguntas-para-producao)
- [04. Stable não significa uma única coisa](#04-stable-nao-significa-uma-unica-coisa)
- [05. Backports, erratas e leitura de vulnerabilidades](#05-backports-erratas-e-leitura-de-vulnerabilidades)
  - [Regra operacional](#05-regra-operacional)
- [06. API, ABI e kABI](#06-api-abi-e-kabi)
  - [API](#06-api)
  - [ABI](#06-abi)
  - [kABI](#06-kabi)
- [07. Pacote, formato, gerenciador, repositório e origem](#07-pacote-formato-gerenciador-repositorio-e-origem)
  - [Fontes suplementares](#07-fontes-suplementares)
  - [Práticas seguras](#07-praticas-seguras)
- [08. Tradicional, imutável, atômico, image-based e declarativo](#08-tradicional-imutavel-atomico-image-based-e-declarativo)
  - [8.1 Sistema tradicional baseado em pacotes](#08-01-sistema-tradicional-baseado-em-pacotes)
  - [8.2 Imutável ou com base read-only](#08-02-imutavel-ou-com-base-read-only)
  - [8.3 Atualização atômica ou transacional](#08-03-atualizacao-atomica-ou-transacional)
  - [8.4 Image-based](#08-04-image-based)
  - [8.5 Declarativo](#08-05-declarativo)
  - [8.6 Rollback não é backup](#08-06-rollback-nao-e-backup)
- [09. Desktop e pilha gráfica](#09-desktop-e-pilha-grafica)
- [10. Hardware, firmware e software não livre](#10-hardware-firmware-e-software-nao-livre)
  - [Arquiteturas de CPU e plataforma](#10-arquiteturas-cpu-plataforma)
  - [Firmware, microcode e drivers](#10-firmware-microcode-drivers)
  - [Não livre não é um único critério](#10-nao-livre-nao-e-um-unico-criterio)
  - [Perfis de política para software e firmware](#10-politicas-software-firmware)
  - [Checklist mínimo](#10-checklist-minimo)
- [11. Segurança, hardening, compliance e certificação](#11-seguranca-hardening-compliance-e-certificacao)
  - [SELinux e AppArmor](#11-selinux-e-apparmor)
  - [Secure Boot e measured boot](#11-secure-boot-e-measured-boot)
  - [FIPS, CIS, STIG e Common Criteria](#11-fips-cis-stig-e-common-criteria)
  - [Live patching](#11-live-patching)
- [12. Storage, criptografia, recuperação e backup](#12-storage-criptografia-recuperacao-e-backup)
- [13. Virtualização, containers e cloud-native](#13-virtualizacao-containers-e-cloud-native)
- [14. Rede, identidade, frota, observabilidade e continuidade](#14-rede-identidade-frota-observabilidade-e-continuidade)
  - [Rede e dataplane](#14-rede-e-dataplane)
  - [Identidade](#14-identidade)
  - [Provisionamento e frota](#14-provisionamento-e-frota)
  - [Observabilidade](#14-observabilidade)
  - [Continuidade](#14-continuidade)
- [15. Método de decisão auditável](#15-metodo-de-decisao-auditavel)
  - [Etapa 1 — descreva o workload](#15-etapa-1-descreva-o-workload)
  - [Etapa 2 — separe requisitos](#15-etapa-2-separe-requisitos)
  - [Etapa 3 — elimine incompatíveis](#15-etapa-3-elimine-incompativeis)
  - [Etapa 4 — pontue preferências](#15-etapa-4-pontue-preferencias)
  - [Etapa 5 — leia trade-offs e alternativas](#15-etapa-5-leia-trade-offs-e-alternativas)
  - [Etapa 6 — valide em camadas](#15-etapa-6-valide-em-camadas)
  - [Etapa 7 — produza um registro de decisão](#15-etapa-7-produza-um-registro-de-decisao)
- [16. Perfis de referência](#16-perfis-de-referencia)
  - [Como interpretar as distribuições desktop destacadas](#16-como-interpretar-as-distribuicoes-desktop-destacadas)
  - [“Gratuito” não significa “RHEL idêntico e com o mesmo serviço”](#16-gratuito-nao-significa-rhel-identico-e-com-o-mesmo-servico)
  - [Casos que não devem ser generalizados](#16-casos-que-nao-devem-ser-generalizados)
- [17. Recorte temporal e versionamento](#17-recorte-temporal-e-versionamento)
  - [Política de versionamento da série 0](#17-politica-de-versionamento-da-serie-0)
  - [Correções temporais e estados de maturidade importantes](#17-duas-correcoes-temporais-importantes)
  - [Política de leitura](#17-politica-de-leitura)
- [18. Operações básicas de atualização](#18-operacoes-basicas-de-atualizacao)
  - [Debian, Ubuntu e derivados](#18-debian-ubuntu-e-derivados)
  - [Fedora e RHEL](#18-fedora-e-rhel)
  - [openSUSE](#18-opensuse)
  - [Arch Linux](#18-arch-linux)
  - [Fedora Atomic Desktops](#18-fedora-atomic-desktops)
  - [SUSE Linux Micro e openSUSE MicroOS](#18-suse-linux-micro-opensuse-microos)
  - [NixOS com channels](#18-nixos-com-channels)
  - [NixOS com flakes](#18-nixos-com-flakes)
- [19. Metodologia do Linux Distro Advisor](#19-metodologia-do-linux-distro-advisor)
  - [O que o catálogo contém](#19-o-que-o-catalogo-contem)
  - [Modos](#19-modos)
  - [Pipeline](#19-pipeline)
  - [Interpretação do resultado](#19-interpretacao-do-resultado)
  - [Privacidade e execução](#19-privacidade-e-execucao)
  - [Limitações conhecidas](#19-limitacoes-conhecidas)
- [20. Checklist de homologação](#20-checklist-de-homologacao)
  - [Antes do laboratório](#20-antes-do-laboratorio)
  - [No laboratório](#20-no-laboratorio)
  - [Antes de produção](#20-antes-de-producao)
- [21. Erros conceituais comuns](#21-erros-conceituais-comuns)
- [22. Governança editorial, publicação e licenciamento](#22-governanca-editorial-publicacao-e-licenciamento)
  - [Estrutura canônica](#22-estrutura-canonica)
  - [Publicação](#22-publicacao)
  - [Idioma e convenções](#idioma-e-convencoes)
  - [Licenciamento](#22-licenciamento)
  - [Contribuições](#22-contribuicoes)
- [23. Glossário](#23-glossario)
- [24. Referências primárias e complementares](#24-referencias-primarias)
  - [Release e lifecycle](#24-release-e-lifecycle)
  - [Arquitetura e operação](#24-arquitetura-e-operacao)
  - [Segurança e conformidade](#24-seguranca-e-conformidade)
  - [Gráficos, rede e terminologia](#24-graficos-rede-e-terminologia)
  - [Catálogos, mapas e comparadores](#24-catalogos-mapas-e-comparadores)
  - [Linha do tempo das distribuições Linux](#24-linha-do-tempo-das-distribuicoes-linux)
  - [Artigos, blogs e vídeos para leitura complementar](#24-artigos-blogs-e-videos-para-leitura-complementar)
- [Histórico desta edição](#historico-desta-edicao)
  - [0.1.6 — 2026-08-21](#historico-0-1-6)
  - [0.1.5 — 2026-08-21](#historico-0-1-5)
  - [0.1.4 — 2026-08-21](#historico-0-1-4)
  - [0.1.3 — 2026-08-21](#historico-0-1-3)
  - [0.1.2 — 2026-08-21](#historico-0-1-2)
  - [0.1.1 — 2026-08-21](#historico-0-1-1)
  - [0.1.0 — 2026-08-15](#historico-0-1-0)

---

<a id="resumo-executivo"></a>

## Resumo executivo

Não existe uma distribuição Linux universalmente “melhor”. Existe uma opção mais adequada a um conjunto explícito de requisitos, em determinado momento, para uma equipe e um workload específicos.

Uma decisão tecnicamente defensável não começa pelo logotipo da distribuição. Ela começa por:

1. workload e ambiente de execução;
2. requisitos eliminatórios;
3. lifecycle e política de segurança;
4. compatibilidade de hardware e software;
5. modelo operacional da equipe;
6. evidências obtidas em laboratório e piloto;
7. plano de atualização, recuperação e saída.

Este guia fornece o vocabulário e o método. O QUIZ [Linux Distro Advisor](https://diego-ch4m4x.github.io/Linux_Distro_Advisor) aplica uma **heurística documentada e versionada** sobre o mesmo modelo e devolve uma recomendação principal, duas alternativas e os trade-offs relevantes.

> O resultado do quiz é triagem técnica, não certificação, homologação nem parecer de segurança. A aprovação de produção continua dependendo de documentação upstream, matriz de compatibilidade, testes e critérios da organização.

> **Snapshot factual:** uma “fotografia histórica” das informações verificadas em uma data específica; não significa que esses dados sejam os mais atuais hoje. Versões, canais, lifecycles e estados de maturidade citados nesta edição refletem **15 de agosto de 2026**. Para instalar, migrar ou homologar em outra data, valide novamente a documentação oficial da release, canal, arquitetura e produto específicos. O snapshot existe para auditabilidade histórica; ele não substitui a fonte primária atual.

[Voltar ao índice](#indice)

---

<a id="status-editorial"></a>

## Status editorial

Este é um material técnico independente. “Referência” significa, aqui, conteúdo versionado, auditável e sustentado por fontes primárias. Não significa documentação oficial de Debian, Fedora, Red Hat, SUSE, Canonical ou de qualquer outro projeto citado.

Documentação oficial é somente a publicada e mantida pelos respectivos projetos e fornecedores. Quando uma informação volátil é relevante, este guia aponta a fonte primária e declara a data do snapshot.

A edição **0.1.0** foi a primeira publicação pública do projeto e inaugurou a série 0. As revisões seguintes ampliaram precisão técnica, progressão didática, rastreabilidade e cobertura operacional sem alterar intencionalmente o contrato decisório do quiz. A edição atual, **0.1.6**, refina fronteiras terminológicas e a auditabilidade documental: separa LTS de enterprise, explicita a natureza editorial da taxonomia comparativa, melhora API/ABI/kABI, storage, supply chain OCI, priorização de vulnerabilidades, Kubernetes e NixOS, e distingue com mais rigor fato, evidência e inferência editorial. O projeto permanece em **pré-1.0**: enquanto estiver em `0.x`, catálogo, perguntas, matrizes, dados factuais e heurística ainda podem evoluir de forma incompatível entre edições. Toda mudança deve ser registrada no histórico e acompanhada das fontes e validações aplicáveis.

<a id="status-escopo"></a>

### Escopo

Este volume cobre:

- classificação de distribuições e canais;
- release, lifecycle, backports e compatibilidade;
- pacote, formato, repositório e proveniência;
- sistemas tradicionais, image-based, imutáveis, atômicos e declarativos;
- desktop e pilha gráfica;
- hardware, firmware e software não livre;
- segurança, hardening, compliance e certificação;
- storage, criptografia, recuperação e backup;
- virtualização, containers, Kubernetes, cloud, rede e observabilidade;
- identidade, gestão de frota e continuidade operacional;
- processo reproduzível para seleção e homologação;
- contrato funcional e limitações do quiz.

<a id="status-fora-de-escopo"></a>

### Fora de escopo

Shell scripting, administração Linux por comandos, preparação completa para LPIC/RHCSA/RHCE/RHCA e catálogos de ferramentas merecem volumes próprios. Aqui aparecem somente os comandos necessários para explicar atualização e lifecycle.

[Voltar ao índice](#indice)

---

<a id="como-usar-este-projeto"></a>

## Como usar este projeto

| Se você precisa... | Comece por... |
|---|---|
| Entender os conceitos | [Modelo mental](#01-modelo-mental) |
| Comparar fixed, rolling e LTS | [Release e lifecycle](#02-modelo-de-release-e-cadencia) |
| Avaliar segurança de uma versão antiga | [Backports e advisories](#05-backports-erratas-e-leitura-de-vulnerabilidades) |
| Escolher para produção | [Método de decisão](#15-metodo-de-decisao-auditavel) |
| Fazer uma triagem interativa | [Abrir o Linux Distro Advisor](./quiz-linux.html) |
| Auditar o quiz | [Metodologia do recomendador](#19-metodologia-do-linux-distro-advisor) |
| Publicar ou contribuir | [Governança editorial](#22-governanca-editorial-publicacao-e-licenciamento) |
| Reutilizar conteúdo ou código | [Consultar as licenças](./LICENSE) |

<a id="como-usar-trilhas"></a>

### Trilhas de leitura por perfil

O guia pode ser lido do início ao fim, mas não exige leitura linear. Use a trilha que melhor corresponde ao seu objetivo:

| Perfil | Ordem sugerida | Objetivo |
|---|---|---|
| **Iniciante absoluto** | Resumo → **0** → 1 → 2.0–2.3 → 3 → 4 → 15 → 16 → 21 → Glossário | formar primeiro os conceitos fundamentais e depois o vocabulário de decisão |
| **Profissional técnico** | Índice → seção do workload → 2.1 → 3/5/6 → 15 → 17 → 20 → 24 | consultar critérios, riscos e fontes sem reler fundamentos já dominados |
| **Homologação, arquitetura ou segurança** | 3 → 5 → 6 → 10 → 11 → 12–14 → 15 → 17 → 20 → 24 | produzir decisão rastreável, testar suporte e registrar evidências |
| **Auditoria do Linux Distro Advisor** | 1 → 2 → 15 → 17 → 19 → 20 → 24 | entender o modelo conceitual, o snapshot e os limites do recomendador |

> **Regra de navegação:** quando uma sigla ou conceito aparecer em uma seção especializada, a primeira ocorrência deve explicar o termo. O [Glossário](#23-glossario) funciona como referência rápida, não como pré-requisito de leitura.

[Voltar ao índice](#indice)

---

<a id="00-conceitos-fundamentais"></a>

## 0. Conceitos fundamentais antes da escolha

Esta seção estabelece o vocabulário mínimo para quem está começando. Leitores experientes podem usá-la como referência rápida e seguir diretamente para o [modelo mental](#01-modelo-mental).

> **Essencial:** **Linux não é uma distribuição.** Linux é o **kernel**. Uma distribuição combina esse kernel com componentes de espaço de usuário, ferramentas, repositórios, políticas de manutenção e integração própria.

| Conceito | Definição objetiva | Exemplo |
|---|---|---|
| **Kernel Linux** | núcleo do sistema que gerencia CPU, memória, processos, dispositivos, isolamento e interfaces fundamentais | kernel Linux usado por Debian, Fedora, Arch etc. |
| **Distribuição Linux** | projeto/produto que integra kernel Linux, bibliotecas, ferramentas, gerenciador de pacotes ou imagens, repositórios, políticas de atualização e documentação | Debian, Fedora, Arch Linux, openSUSE |
| **Edição** | variante publicada pelo mesmo projeto para um uso, desktop ou fluxo operacional específico | Fedora Workstation, Fedora Server, Fedora Kinoite |
| **Release** | estado publicado e identificável de um produto/projeto | Fedora 44, Ubuntu 26.04 LTS |
| **Branch** | linha nomeada de desenvolvimento, manutenção ou promoção | Debian `testing`, Fedora Rawhide |
| **Canal** | linha de entrega que o usuário escolhe consumir | `stable`, `testing`, `next` |
| **Imagem de instalação** | artefato usado para instalar ou inicializar um sistema; frequentemente é uma ISO, mas também pode ser imagem de disco ou outro formato | `archlinux-2026.08.01-x86_64.iso` |
| **Repositório** | fonte organizada de pacotes, metadados ou imagens consumida pelas ferramentas de instalação/atualização | Debian Stable, Fedora Updates |
| **Desktop Environment (DE)** | ambiente gráfico integrado com sessão, shell/painel, configurações e aplicações | GNOME, KDE Plasma, Cinnamon |
| **Aplicação** | software executado sobre o sistema operacional para cumprir uma função do usuário ou serviço | navegador, IDE, banco de dados |

### Mapa mental mínimo

~~~text
Linux
└── kernel

Distribuição Linux
├── kernel Linux
├── componentes de userspace
├── bibliotecas e ferramentas
├── serviços e integração
├── pacotes/imagens e repositórios
├── políticas de atualização e segurança
└── documentação e governança do projeto

Aplicações e workloads
└── executam sobre essa plataforma e possuem lifecycle próprio
~~~

> **Essencial:** a distribuição fornece uma **plataforma**. Aplicações, containers, drivers, firmware e repositórios externos podem seguir ciclos independentes e precisam ser avaliados separadamente.

### O que realmente está sendo escolhido

A unidade real de decisão não é apenas o nome da distribuição. Para uma escolha tecnicamente verificável, identifique no mínimo:

1. **produto/distribuição**;
2. **edição**, quando existir;
3. **release, branch ou canal**;
4. **arquitetura de CPU**;
5. **origem da imagem e dos repositórios**;
6. **modalidade de suporte**, quando relevante;
7. **workload** e ambiente onde o sistema será usado.

Exemplo:

~~~text
Descrição insuficiente:
"usar Fedora"

Descrição verificável:
Fedora Workstation 44
├── arquitetura: x86_64
├── origem: imagem e repositórios oficiais do Fedora
├── finalidade: workstation de desenvolvimento
└── suporte/operação: lifecycle da release Fedora + gestão da equipe local
~~~

> **Não confunda:** uma ISO com número ou data não prova que a distribuição seja Fixed Release. Em sistemas rolling, a ISO costuma ser apenas um snapshot de instalação; a instalação continua evoluindo pelo fluxo rolling.

### Instalar, atualizar e fazer upgrade não são a mesma coisa

- **Instalar** cria uma instalação ou deployment do sistema a partir de uma mídia, imagem ou processo de provisionamento.
- **Atualizar (*update*)** aplica correções e novas versões permitidas pelo canal/release atual.
- **Upgrade de release** muda explicitamente para outra geração ou referência quando o modelo exige essa transição, como Fedora 44 → 45.
- **Rebase** troca a referência/base usada para compor o sistema quando o projeto emprega esse conceito.
- **Deployment** é um estado instalável ou bootável preparado pelo mecanismo correspondente.
- **Rollback** seleciona um estado anterior ainda disponível dentro do escopo do mecanismo.
- **Restore** recupera dados/estado a partir de uma cópia ou backup.
- **Rebuild** reconstrói o estado a partir de uma especificação, configuração ou conjunto de entradas.

> **Não confunda:** *update*, *upgrade*, *rebase*, *deployment*, *rollback*, *restore* e *rebuild* descrevem operações diferentes. O procedimento correto depende do produto e da arquitetura usada.

[Voltar ao índice](#indice)

---

<a id="01-modelo-mental"></a>

## 1. Modelo mental: a classificação é multidimensional

Termos como **Rolling Release**, **LTS**, **Stable**, **Bleeding Edge**, **imutável** e **atômico** não pertencem todos ao mesmo eixo. Compará-los como se fossem categorias concorrentes produz conclusões falsas.

> **Essencial:** não tente definir uma distribuição com uma única etiqueta. **Release, lifecycle, atualidade, compatibilidade, mutabilidade e operação são perguntas diferentes** e podem combinar-se de maneiras distintas.

Neste guia, uma distribuição, edição, canal ou appliance é analisado por **dez dimensões independentes**. **Esta é uma taxonomia editorial deste projeto**, criada para organizar a comparação; não é uma classificação normativa ou universal adotada por todo o ecossistema Linux. “Host” significa a máquina ou instância do sistema Linux que está sendo avaliada; pode ser um computador físico, uma máquina virtual ou uma instância em cloud.

| Dimensão | Pergunta correta | Exemplos |
|---|---|---|
| **Release** | Como a instalação evolui entre gerações ou ao longo do tempo? | Fixed, Rolling, Semi-Rolling, Stream |
| **Cadência** | Com que frequência releases, pacotes, snapshots ou imagens são promovidos? | diária, semanal, em lotes, ~6 meses |
| **Atualidade** | Quão próximas as versões empacotadas ficam dos projetos upstream? | conservadora, equilibrada, leading edge |
| **Lifecycle** | Por quanto tempo há manutenção e em qual escopo? | curto, LTS, enterprise, suporte estendido |
| **Compatibilidade** | Que contratos técnicos precisam ser preservados? | API, ABI, kABI, formatos de configuração |
| **Mutabilidade** | Quais partes do sistema podem ser alteradas diretamente no host? | tradicional, controlado, read-only |
| **Transação** | Como uma mudança é preparada, ativada e revertida? | pacote a pacote, deployment, snapshot, generation |
| **Declaração** | O operador descreve ações ou o estado desejado? | imperativo, declarativo |
| **Proveniência** | Quem construiu, assinou, distribuiu e mantém o software? | repositório oficial, terceiro, upstream |
| **Operação** | Como o ambiente é provisionado, atualizado, observado e governado? | host manual, imagem, gestão de frota, GitOps |

Essas dimensões coexistem. Uma distribuição pode ser, ao mesmo tempo, **Fixed Release**, **leading edge**, ter **lifecycle curto**, usar **deployment atômico** e privilegiar aplicações em Flatpak. Nenhuma dessas propriedades anula as demais.

Exemplo:

~~~text
Fedora Silverblue 44
├── release: fixa, seguindo a geração Fedora 44
├── cadência: atualizações frequentes dentro dessa geração
├── atualidade: alta / leading edge
├── lifecycle: aproximadamente 13 meses para a geração Fedora
├── mutabilidade: conteúdo do SO controlado por deployment; /usr não é alterado ad hoc
├── transação: novo deployment preparado e ativado no boot
└── aplicações gráficas: preferencialmente desacopladas do host por Flatpak
~~~

Outro exemplo:

~~~text
Ubuntu 26.04 LTS
├── release: fixa
├── lifecycle: LTS, com escopos de manutenção definidos pela Canonical
├── manutenção: atualizações controladas e backports quando aplicável
├── mutabilidade: instalação tradicional baseada em pacotes
└── ferramentas: APT sobre pacotes deb/dpkg
~~~

> **Como ler o restante do guia:** a seção 2 aprofunda somente as **seis dimensões diretamente ligadas a release e atualização**: release, cadência, atualidade, lifecycle, mutabilidade e transação. As quatro dimensões restantes continuam válidas no modelo completo e são desenvolvidas principalmente em **Compatibilidade → seção 6**, **Declaração → seção 8.5**, **Proveniência → seção 7** e **Operação → seção 14**.

Pergunte **“como esta opção se comporta em cada dimensão relevante?”**, não apenas **“qual é o tipo dela?”**.

[Voltar ao índice](#indice)

---

<a id="02-modelo-de-release-e-cadencia"></a>

## 2. Modelo de release e cadência

O modelo de release é uma das propriedades mais importantes na escolha de uma distribuição, mas ele não deve ser analisado isoladamente. Palavras como *rolling*, *stable*, *atomic*, *LTS*, *enterprise* e *image-based* respondem a perguntas diferentes. Esta seção organiza essas perguntas antes de classificar qualquer candidato.

<a id="02-00-seis-dimensoes"></a>

### 2.0 Seis dimensões de release e atualização dentro do modelo completo

O modelo mental da seção 1 possui **dez dimensões**. Aqui aprofundamos apenas as seis diretamente ligadas a **como o sistema evolui e recebe mudanças**. Compatibilidade, declaração, proveniência e operação continuam válidas e são tratadas nas demais seções do guia.

| Dimensão | Pergunta correta | O que ela mede | Exemplos |
|---|---|---|---|
| **Modelo de release** | Como a instalação evolui entre gerações ou ao longo do tempo? | releases N → N+1, fluxo contínuo ou modelo híbrido | Fixed, Rolling, Semi-Rolling, Stream |
| **Cadência** | Com que frequência releases, pacotes, snapshots ou imagens são promovidos? | ritmo de entrega | diária, semanal, em lotes, ~6 meses, sob demanda |
| **Atualidade dos componentes** (*software freshness*) | Quão próximas as versões empacotadas ficam dos respectivos projetos upstream? | frescor tecnológico por componente | conservadora, equilibrada, leading edge |
| **Lifecycle** | Por quanto tempo a release, branch, canal, imagem ou componente recebe manutenção e em qual escopo? | duração e cobertura de manutenção | curto, LTS, suporte estendido |
| **Mecanismo de atualização/ativação** | Como uma mudança é preparada e colocada em uso? | forma de transação operacional | pacote a pacote, deployment atômico, snapshot transacional, A/B |
| **Mutabilidade/entrega do host** | Quais partes do sistema operacional podem ser alteradas diretamente e como o conteúdo do host é distribuído? | arquitetura operacional | tradicional, image-based, read-only/controlado, declarativo |

> **Regra central:** **não compare esses termos como se fossem alternativas.** **Fixed/Rolling** descreve como o produto evolui ao longo das gerações. **LTS** descreve uma política de manutenção prolongada aplicada a uma release ou componente específico. **Enterprise** descreve um produto/ecossistema orientado à operação empresarial e pode combinar lifecycle, suporte comercial, SLA, compatibilidade formal, certificações, ferramentas de gestão e contratos. **Atomic/Transactional** descreve como mudanças são preparadas e ativadas. **Image-based/read-only** descreve entrega e mutabilidade. **Leading edge/bleeding edge** descreve atualidade e exposição a mudanças. Esses conceitos podem coexistir.

#### Cadência

Cadência é o **ritmo de promoção das mudanças**. Duas distribuições podem ser Fixed Release e ter ritmos muito diferentes; duas rolling também podem usar políticas de promoção diferentes.

~~~text
Fedora estável  ── Fixed   ─ nova geração aproximadamente a cada 6 meses
RHEL             ── Fixed   ─ releases controladas + lifecycle empresarial longo
Arch             ── Rolling ─ pacotes em fluxo contínuo
Manjaro          ── Rolling ─ promoção Unstable → Testing → Stable
Bazzite          ── Fixed   ─ segue geração Fedora, mas imagens podem chegar diariamente
~~~

Cadência não mede, sozinha, estabilidade, qualidade ou segurança.

#### Atualidade dos componentes

Atualidade, neste guia, significa **proximidade das versões empacotadas em relação aos projetos upstream** — por exemplo kernel, Mesa, GNOME, KDE Plasma, compiladores, bibliotecas e drivers.

Uma distribuição pode ser Fixed e muito atual. Fedora é um exemplo clássico: possui releases versionadas, mas adota tecnologias novas rapidamente. Outra pode ser rolling e reter determinados pacotes por testes ou curadoria.

> **Atualidade não é um score de segurança.** Uma versão upstream aparentemente antiga pode receber backports e erratas downstream durante anos. Para vulnerabilidades, consulte a política de segurança da distribuição e a seção [Backports, erratas e leitura de vulnerabilidades](#05-backports-erratas-e-leitura-de-vulnerabilidades).

#### Lifecycle

Lifecycle mede **por quanto tempo e em qual escopo** uma release, canal ou componente é mantido. **LTS (*Long-Term Support*) não é exclusivo de distribuições**: o termo pode qualificar uma release, um kernel, um componente ou outro programa de manutenção. Sempre identifique **LTS de quê**.

**Enterprise** também não é sinônimo de lifecycle: normalmente descreve um produto/ecossistema com objetivos empresariais e pode acrescentar suporte comercial, SLA, certificações, matrizes de compatibilidade, ferramentas de gestão e condições contratuais.

#### Mecanismo de atualização e mutabilidade

Essas dimensões descrevem **como a mudança chega ao host**. Um sistema pode ser Fixed e usar atualização atômica; outro pode ser Rolling e atualizar pacotes tradicionalmente. A seção [2.7](#02-07-fixed-rolling-nao-e-atomic-transactional) mostra isso em detalhe.

<a id="02-01-tabela-mestre"></a>

### 2.1 Tabela-mestre — modelo de evolução dos 95 candidatos

Antes da tabela completa, use estes **12 casos didáticos** para reconhecer os modelos principais sem lidar imediatamente com 95 entradas:

| Sistema/canal | Modelo de release/evolução | Outra dimensão importante | O que ele ensina |
|---|---|---|---|
| Debian Stable | Fixed Release | atualidade conservadora + point releases | Fixed não significa ausência de updates |
| Ubuntu LTS | Fixed Release | lifecycle prolongado | LTS descreve suporte, não um modelo oposto a Rolling |
| Fedora Workstation | Fixed Release | leading edge | Fixed não significa software antigo |
| RHEL | Fixed Release | produto enterprise + lifecycle longo + minor releases | modelo de release, lifecycle e contrato de suporte são eixos diferentes |
| Arch Linux | Rolling Release | atualização tradicional por pacotes | exemplo clássico de rolling sem gerações N → N+1 |
| Manjaro Stable | Rolling Release curada | branches Unstable → Testing → Stable | uma branch chamada Stable pode continuar rolling |
| Debian Testing | Rolling-like / development branch | freeze e promoção para Stable | branch contínua não é automaticamente uma rolling clássica |
| Fedora Rawhide | Rolling Development | alimenta futuras releases Fedora | canal de desenvolvimento ≠ Fedora estável |
| Fedora Silverblue/Kinoite | Fixed Release | Atomic + image-based | Atomic não significa Rolling |
| Bazzite | Fixed-base / Continuous Delivery de imagens | Atomic + image-based | atualização diária de imagem não torna a base rolling |
| Nobara | Semi-Rolling / híbrido *(termo do projeto)* | curadoria Fedora | o próprio projeto usa o termo; ainda existem versões/ISOs numeradas |
| NixOS | Fixed ou canal contínuo, conforme canal | declarativo + generations | declaração e rollback são eixos independentes do release |

> **Não confunda:** a coluna “modelo” responde **como o produto evolui**. Propriedades como *Atomic*, *image-based*, *enterprise*, *LTS*, *read-only* ou *declarativo* pertencem a outros eixos.

A tabela cobre os **95 candidatos modelados pelo Linux Distro Advisor**. O catálogo não contém apenas distribuições generalistas: inclui edições, canais, sistemas image-based, cloud/container OS, network operating systems e appliances.

Para evitar ambiguidade, a coluna **Modelo de evolução segundo a taxonomia deste guia** usa categorias editoriais pequenas e controladas. **Essas categorias não constituem uma classificação oficial ou universal dos projetos upstream.** Termos como *curado*, *enterprise*, *atomic*, *image-based*, *live* e *read-only* não criam novos modelos de release; aparecem na cadência, entrega ou observação.

> **Legenda dos modelos canônicos:**
> - **Fixed Release:** existem gerações/releases identificáveis e a passagem para outra geração é delimitada.
> - **Rolling Release:** a instalação acompanha um fluxo contínuo sem upgrade periódico obrigatório de uma release completa N para N+1.
> - **Semi-Rolling / híbrido (rótulo editorial):** usado somente quando uma descrição curta precisa representar combinação de base versionada e fluxo contínuo; sempre leia a observação e prefira a descrição composicional quando disponível.
> - **Continuous Delivery / Stream:** o produto evolui por streams/canais/imagens contínuos e a dicotomia Fixed/Rolling descreve mal a operação real.
> - **Múltiplos modelos/canais:** a marca oferece linhas com comportamentos diferentes; é obrigatório declarar a edição/branch/canal.
> - **Produto versionado / appliance:** o ciclo do produto especializado é mais relevante que a taxonomia de uma distribuição generalista.
> - **Varia por build/vendor:** não existe um único modelo confiável sem identificar a build, distribuição derivada ou fornecedor específico.

> **Escopo deliberado:** a tabela compara principalmente **modelo de evolução, cadência e entrega/ativação**. **Lifecycle e atualidade não foram reduzidos a uma única célula**, porque podem variar por release, arquitetura, componente, contrato e canal. Avalie lifecycle na seção 3/17 e atualidade por componente/repositório; uma classificação única como “alta” ou “baixa” poderia produzir falsa precisão.

> **Importante:** a classificação pertence ao **produto, edição ou canal específico**, não necessariamente à marca inteira. Debian Stable e Debian Testing têm comportamentos diferentes; Fedora Workstation e Fedora Rawhide também; OpenMandriva oferece linhas Fixed e Rolling; Alpine combina releases estáveis e a branch `edge`.

| Candidato / canal | Modelo de evolução segundo este guia | Cadência | Entrega / ativação | Observação de leitura |
|---|---|---|---|---|
| **Ubuntu** | Fixed Release | releases regulares; LTS a cada 2 anos; point releases nas LTS | APT/dpkg tradicional | LTS é lifecycle, não outro modelo de release. |
| **Linux Mint** | Fixed Release | séries versionadas, normalmente sobre Ubuntu LTS | APT/dpkg tradicional | 22.x são revisões da mesma família; não é rolling. |
| **Debian Stable** | Fixed Release | release estável + point releases | APT/dpkg tradicional | Point release consolida correções; não cria uma rolling. |
| **KDE neon** | Semi-Rolling / híbrido *(editorial)* | base Ubuntu LTS fixa + KDE em ritmo próprio | APT/dpkg + Flatpak opcional | Descrição mais precisa: **base versionada + pilha KDE em fluxo independente**. |
| **Pop!_OS** | Fixed Release | releases versionadas e upgrades de geração | APT/dpkg tradicional | Atualizações frequentes dentro da release não o tornam rolling. |
| **Zorin OS** | Fixed Release | séries versionadas sobre Ubuntu LTS | APT/dpkg tradicional | Revisões 18.x permanecem na mesma família. |
| **MX Linux** | Fixed Release | base Debian Stable + snapshots/refreshes de mídia | APT/dpkg tradicional | Atualizações e ferramentas próprias não mudam a base fixed. |
| **elementary OS** | Fixed Release | releases versionadas sobre base Ubuntu LTS | APT/dpkg + Flatpak | Aplicações podem ter ritmo diferente do host. |
| **Linux Lite** | Fixed Release | releases versionadas sobre Ubuntu LTS | APT/dpkg tradicional | Foco em estabilidade e ciclo da base. |
| **Q4OS** | Fixed Release | releases versionadas sobre Debian Stable | APT/dpkg tradicional | Segue uma base conservadora e versionada. |
| **TUXEDO OS** | Fixed Release | no snapshot, release estável ainda sobre Ubuntu; transição para Debian estava anunciada | APT + Flatpak | No snapshot, a release estável ainda é Ubuntu-based. O projeto já havia anunciado a futura base Debian Testing permanente, chamada **Continuous Debian**, para preservar seu modelo híbrido; trate anúncio/beta e release estável como estados distintos. |
| **Endless OS** | Fixed Release | releases/updates controlados | OSTree / imagem + Flatpak | É image-based/OSTree. O mecanismo de imagem/atomicidade não muda o fato de o produto ser publicado em releases identificáveis. |
| **Vanilla OS** | Fixed Release | releases identificáveis | ABRoot/A-B + containers/Flatpak | A/B e image-based descrevem a ativação/entrega; a evolução continua por releases identificáveis. |
| **Devuan** | Fixed Release | releases versionadas derivadas do Debian | APT/dpkg tradicional | Difere principalmente pelo init/ecossistema sem systemd. |
| **antiX** | Fixed Release | base Debian Stable | APT/dpkg tradicional | Release conservadora; systemd-free. |
| **Peppermint OS** | Fixed Release | builds Debian/Devuan versionadas | APT/dpkg tradicional | A variante escolhida define a base concreta. |
| **Bodhi Linux** | Fixed Release | releases Ubuntu-based | APT/dpkg tradicional | Moksha não altera o modelo de release. |
| **Kali Linux** | Rolling Release | `kali-rolling`; imagens numeradas são snapshots de instalação | APT/dpkg tradicional | `kali-last-snapshot` é um canal distinto de point releases. |
| **Parrot OS** | Semi-Rolling / híbrido *(editorial)* | Debian Stable como fundação; repositório Parrot mantém ferramentas em fluxo rolling | APT/dpkg tradicional | Descrição mais precisa: **base Debian Stable + repositórios Parrot em fluxo contínuo**; não equivale a Arch nem Debian Testing. |
| **BackBox Linux** | Fixed Release | Ubuntu LTS-based | APT/dpkg tradicional | Pentest sobre base versionada. |
| **CAINE** | Fixed Release | releases versionadas Ubuntu LTS-based | APT + toolkit forense | Release versionada distribuída principalmente como ambiente live especializado em **DFIR** (*Digital Forensics and Incident Response*, forense digital e resposta a incidentes). |
| **Tsurugi Linux** | Fixed Release | releases versionadas Ubuntu LTS-based | APT + toolkit DFIR | Release versionada/live especializada em DFIR; não deve ser comparada como desktop generalista. |
| **REMnux** | Semi-Rolling / híbrido | base Ubuntu versionada + tooling atualizado por instalador | Ubuntu + Salt/tooling REMnux | O toolkit possui ciclo próprio sobre a base. |
| **Tails** | Fixed Release | releases frequentes de segurança | imagem live amnésica | Frequência alta de release não significa rolling. |
| **Whonix** | Fixed Release | releases/templates versionados sobre Debian | APT + templates/VMs | O produto é uma arquitetura de isolamento, não só uma distro desktop. |
| **Proxmox VE** | Produto versionado / appliance | major/minor releases do appliance | APT + KVM/LXC/ZFS/Ceph | Produto/appliance de virtualização com major/minor releases próprias; a taxonomia de produto é mais útil que tratá-lo como desktop generalista. |
| **Rhino Linux** | Rolling Release | fluxo contínuo sobre Ubuntu de desenvolvimento | APT + Pacstall | Ubuntu-based pode ser rolling; a **distribuição-base/origem do projeto** não determina, sozinha, o modelo de release. |
| **deepin** | Fixed Release | releases versionadas | APT/dpkg tradicional | Recebe updates dentro da geração sem virar rolling. |
| **Nitrux** | Fixed Release | releases identificáveis | arquitetura imutável/idempotente própria | A arquitetura imutável/idempotente descreve mutabilidade/entrega e não transforma o projeto em rolling. |
| **Fedora Workstation** | Fixed Release | nova geração aproximadamente a cada 6 meses | DNF/RPM tradicional | É leading edge, mas continua fixed. |
| **Fedora Atomic Desktops** | Fixed Release | segue as gerações Fedora | rpm-ostree/bootc deployment + Flatpak | Silverblue, Kinoite e demais Atomic Desktops seguem gerações Fedora fixas; atomicidade é mecanismo de atualização, não modelo de release. |
| **Red Hat Enterprise Linux (RHEL)** | Fixed Release | major/minor releases + lifecycle longo | DNF/RPM tradicional | É Fixed Release; lifecycle empresarial, minor releases e Application Streams são dimensões adicionais, não um modelo rolling. |
| **Rocky Linux** | Fixed Release | major/minor releases Enterprise Linux | DNF/RPM tradicional | É Fixed Release. Compatibilidade com o ecossistema RHEL não transfere automaticamente certificações ou suporte do fornecedor. |
| **AlmaLinux** | Fixed Release | major/minor releases Enterprise Linux | DNF/RPM tradicional | É Fixed Release com lifecycle Enterprise Linux; compatibilidade declarada e suporte devem ser avaliados separadamente. |
| **Oracle Linux** | Fixed Release | major/minor releases + lifecycle Oracle | DNF/RPM tradicional | É Fixed Release. O **RHCK (*Red Hat Compatible Kernel*)** e o **UEK (*Unbreakable Enterprise Kernel*)** são opções de kernel que podem alterar hardware, recursos e certificação sem mudar o modelo de release. |
| **CentOS Stream** | Continuous Delivery / Stream | fluxo contínuo entre Fedora e a próxima minor do RHEL | DNF/RPM tradicional | Não é Arch-style rolling; a geração Enterprise Linux continua delimitada. |
| **Fedora CoreOS** | Continuous Delivery / Stream | streams `stable`, `testing` e `next` | imagem/OSTree com atualização automática | É melhor descrito por stream de imagens do que por fixed/rolling clássico. |
| **Bazzite** | Fixed Release | segue o ciclo de releases do Fedora; imagens desktop atualizam diariamente | Fedora Atomic/bootable container + rollback | Segue as gerações Fedora, embora entregue imagens com alta frequência. Atomicidade e atualização diária de imagem não o tornam Rolling Release clássico. |
| **Nobara** | Semi-Rolling / híbrido *(termo do projeto)* | desde Nobara 41, updates normais conduzem à próxima geração quando pronta | DNF/RPM tradicional com curadoria própria | O próprio projeto usa “semi-rolling”; ainda há versões/ISOs numeradas, mas o upgrade deixou de ser um evento separado. |
| **Bluefin** | Fixed Release | Stable/LTS conforme variante | bootc/OCI + Flatpak/Homebrew/containers | Segue uma fundação Fedora versionada com fluxo contínuo de imagens; image-based/atomic é outro eixo. |
| **Aurora** | Fixed Release | Stable sobre Fedora/Universal Blue | bootc/OCI + Flatpak/Homebrew/containers | Segue uma fundação Fedora versionada com fluxo contínuo de imagens; KDE/image-based não altera o modelo de release. |
| **Fedora Asahi Remix** | Fixed Release | acompanha gerações Fedora suportadas | DNF/RPM + integração Asahi | Hardware-specific; não é um canal rolling separado. |
| **Amazon Linux** | Fixed Release | gerações suportadas + updates dentro da geração | DNF/RPM | Cloud OS com lifecycle próprio. |
| **Azure Linux** | Fixed Release | gerações alinhadas ao produto/ecossistema Azure | RPM/tarball/image conforme uso | Não confundir com Azure Container Linux image-based. |
| **openSUSE Leap** | Fixed Release | releases/minor releases versionadas | Zypper/RPM tradicional | Ciclo distinto do Tumbleweed. |
| **openSUSE Tumbleweed** | Rolling Release | snapshots continuamente promovidos e testados | Zypper/RPM tradicional | Snapshot numerado é estado da rolling, não nova major release. |
| **openSUSE Aeon** | Rolling Release | acompanha Tumbleweed com política própria | transactional/image-oriented + Flatpak | Baseado em Tumbleweed e transacional; o portal openSUSE ainda o classifica como **beta**, portanto maturidade deve ser validada antes de produção. |
| **openSUSE MicroOS** | Rolling Release | snapshots derivados do ecossistema Tumbleweed | Btrfs + transactional-update | Rolling derivado de snapshots do ecossistema Tumbleweed e com atualização transacional; Rolling e Transactional são eixos independentes. |
| **SUSE Linux Enterprise Server (SLES)** | Fixed Release | major/minor releases + lifecycle enterprise | Zypper/RPM | Fixed Release com lifecycle empresarial; suporte e extensões pertencem ao eixo de lifecycle. |
| **SUSE Linux Enterprise Desktop (SLED)** | Fixed Release | major/minor releases + lifecycle enterprise | Zypper/RPM | Fixed Release com lifecycle empresarial; desktop enterprise versionado. |
| **Regata OS** | Fixed Release | linha versionada sobre openSUSE | Zypper/RPM + ferramentas próprias | Não deve ser presumido rolling só por receber updates frequentes. |
| **Arch Linux** | Rolling Release | fluxo contínuo; ISO mensal é snapshot de instalação | pacman tradicional | Não há upgrade periódico N → N+1 do sistema completo. |
| **Manjaro** | Rolling Release | `unstable` → `testing` → `stable` | pacman + repositórios próprios | Rolling com curadoria própria: `unstable` → `testing` → `stable`. A branch `stable` continua pertencendo a uma rolling. |
| **CachyOS** | Rolling Release | fluxo contínuo Arch-based | pacman + repositórios CachyOS | ISO numerada é snapshot de instalação. |
| **Garuda Linux** | Rolling Release | fluxo contínuo Arch-based | pacman + snapshots/integrações próprias | Recursos de rollback não mudam o modelo rolling. |
| **EndeavourOS** | Rolling Release | acompanha de perto repositórios Arch | pacman + AUR opcional | Snapshots/ISOs não criam releases fixas. |
| **BlackArch** | Rolling Release | Arch-based, repositório contínuo | pacman | Especializada em segurança ofensiva. |
| **BigLinux** | Rolling Release | acompanha base Manjaro Stable | pacman + AUR + Flatpak | Rolling por acompanhar a base Manjaro; a curadoria da base altera cadência, não o modelo principal. |
| **Artix Linux** | Rolling Release | fluxo contínuo Arch-like | pacman | Escolha de init não altera o modelo rolling. |
| **SteamOS** | Continuous Delivery / Stream | updates por imagens/canais do produto | A/B/image-style no appliance | A genealogia Arch não significa que o usuário opere SteamOS como Arch genérico. |
| **NixOS** | Múltiplos modelos/canais | releases semestrais e fluxo `unstable` opcional | declarativo por generations | Canal escolhido importa mais que a marca isolada. |
| **Solus** | Rolling Release | fluxo contínuo com curadoria do projeto | eopkg tradicional | Rolling não implica copiar a cadência do Arch. |
| **Gentoo** | Rolling Release | árvore contínua + keywords stable/testing por arquitetura | Portage/emerge | Rolling e predominantemente source-based. Keywords `stable/testing` são políticas de pacotes/arquitetura, não Fixed Release. |
| **Pentoo** | Rolling Release | Gentoo-based com fluxo contínuo | Portage | Especializada em pentest. |
| **Void Linux** | Rolling Release | repositórios continuamente atualizados | XBPS | Rolling independente; não Arch-derived. |
| **Alpine Linux** | Múltiplos modelos/canais | stable versionada; `edge` é desenvolvimento contínuo | apk | É outro exemplo de marca com canais de comportamento diferente. |
| **GNU Guix System** | Múltiplos modelos/canais | channels podem acompanhar revisões continuamente | Guix generations / declarativo | Generation/rollback é mecanismo de estado, não sinônimo de rolling. |
| **Slackware Linux** | Múltiplos modelos/canais | stable versionada; `-current` prepara a próxima release | pkgtools/slackpkg | `-current` é development branch, não torna Slackware Stable rolling. |
| **Puppy Linux** | Varia por build/vendor | família de imagens e bases diferentes | live/frugal + gerência conforme Puppy | Não existe um único lifecycle/modelo universal para todos os Puppies. |
| **PCLinuxOS** | Rolling Release | fluxo contínuo | RPM + APT/Synaptic | APT aqui não é o mesmo stack dpkg do Debian. |
| **KaOS** | Rolling Release | fluxo contínuo independente | pacman + repositórios próprios | Qt-focused; catalog-only no advisor atual. |
| **Mageia** | Fixed Release | releases versionadas | RPM + DNF/urpmi conforme fluxo | Distribuição tradicional versionada. |
| **OpenMandriva Lx** | Múltiplos modelos/canais | modelo depende da edição escolhida | RPM + DNF | A marca sozinha não basta; o canal precisa ser declarado. |
| **Raspberry Pi OS** | Fixed Release | imagens/rebases periódicos sobre Debian | APT/dpkg | Lifecycle e versões dependem da base Debian adotada. |
| **Security Onion** | Produto versionado / appliance | releases e updates controlados | stack/appliance de segurança | Plataforma especializada de segurança com releases controladas; classifique o produto, não apenas a distribuição Linux subjacente. |
| **Qubes OS** | Fixed Release | releases versionadas do dom0 + templates com ciclos próprios | Xen/Qubes + templates Fedora/Debian | Host e templates têm lifecycles independentes. |
| **Harvester HCI** | Produto versionado / appliance | releases da plataforma **HCI (*Hyper-Converged Infrastructure*, infraestrutura hiperconvergente)** | Kubernetes/KubeVirt/Longhorn | Appliance/plataforma HCI versionada. Release do produto, cluster e suporte são os eixos relevantes. |
| **VyOS** | Múltiplos modelos/canais | rolling/current e linhas versionadas/LTS conforme oferta | imagem/upgrade de network OS | O canal contratado/escolhido determina o comportamento. |
| **OpenWrt** | Múltiplos modelos/canais | releases estáveis e snapshots contínuos | sysupgrade/opkg/apk conforme geração | Snapshot é canal de desenvolvimento; stable continua versionada. |
| **IPFire** | Produto versionado / appliance | releases e Core Updates do appliance | pakfire + mecanismo próprio | Appliance de rede com Core Updates e versionamento próprios; não é Rolling Release clássica. |
| **SONiC** | Varia por build/vendor | community e vendors publicam trains próprios | imagem de switch NOS | O ciclo depende da distribuição SONiC e do fornecedor. Sempre valide train de release, ASIC e hardware específicos. |
| **NVIDIA Cumulus Linux** | Fixed Release | releases suportadas do NOS | imagem/NVUE + APT/Linux networking | Ciclo comercial ligado a plataformas suportadas. |
| **Talos Linux** | Fixed Release | releases versionadas e upgrades coordenados | imutável/API-managed | Não oferece administração package-based convencional do host. |
| **Flatcar Container Linux** | Continuous Delivery / Stream | stable/beta/alpha com atualização contínua | image-based/atomic update | Entrega contínua por canais de imagem; canal contínuo não é automaticamente equivalente ao modelo Arch-style rolling. |
| **Bottlerocket** | Fixed Release | releases/imagens promovidas em ondas | host imutável com atualização de imagem | Release versionada e image-based; a promoção contínua de imagens descreve entrega, não converte automaticamente o produto em rolling. |
| **Azure Container Linux** | Continuous Delivery / Stream | imagens promovidas pelo ecossistema AKS/Azure | imutável/image-based | Fluxo de imagem é uma categoria melhor que fixed/rolling clássico. |
| **Ubuntu Core** | Fixed Release | gerações Core de longa duração | snaps transacionais/image model | Transacional descreve ativação; LTS descreve lifecycle. |
| **ChimeraOS** | Continuous Delivery / Stream | canais `stable`, `testing` e `unstable` + releases marcadas | imagem read-only via `frzr`/Btrfs; ativação no reboot | Arch-based, mas o host recebe imagens read-only via `frzr` e `pacman` não é o fluxo operacional normal; por isso, o stream de imagens descreve melhor o produto. |
| **Batocera** | Produto versionado / appliance | releases numeradas de imagem | imagem completa do appliance | Foco em retrogaming; atualização da imagem não equivale a rolling. |
| **TrueNAS** | Produto versionado / appliance | trains/releases do produto | imagem/appliance com mecanismos próprios | Storage appliance; versionamento do produto é o eixo relevante. |
| **Unraid OS** | Produto versionado / appliance | releases numeradas | imagem/appliance comercial | Não é distro generalista package-based. |
| **Container-Optimized OS (Google)** | Continuous Delivery / Stream | milestones e atualizações automáticas | image-based/auto-update | Cloud OS orientado a milestones/canais de imagem e atualização automática; pense em canal/milestone, não em rolling desktop. |
| **Athena OS** | Rolling Release | Arch-based com fluxo contínuo | pacman + tooling de segurança | Distribuição de pentest; atualidade alta não é sinônimo de estabilidade. |
| **Trisquel GNU/Linux** | Fixed Release | releases versionadas sobre Ubuntu LTS | APT/dpkg tradicional | Política de software livre é independente do modelo de release. |
| **PureOS** | Fixed Release | Debian-derived com releases próprias | APT/dpkg tradicional | Foco freedom/privacy; modelo não deve ser inferido só da origem Debian. |

---

<a id="02-02-fixed-release"></a>

### 2.2 Fixed Release

Uma **Fixed Release** publica gerações identificáveis. Durante o lifecycle, a geração recebe correções e atualizações segundo a política do projeto; a passagem para a próxima geração continua sendo um evento lógico identificável, mesmo quando a ferramenta automatiza parte do processo.

~~~text
Versão N ── correções e manutenção ──► fim do suporte
     │
     └──────── upgrade/rebase ───────► Versão N+1
~~~

Exemplos claros incluem Debian Stable, Ubuntu, Fedora estável, RHEL, Linux Mint, Zorin OS, Pop!_OS, openSUSE Leap, Rocky Linux, AlmaLinux e Oracle Linux.

Vantagens comuns:

- janela de suporte e escopo de homologação mais fáceis de associar a uma versão;
- mudanças estruturais podem ser agrupadas entre gerações;
- documentação, certificações e caminhos de upgrade podem ser vinculados a uma release concreta.

Custos comuns:

- migrações periódicas entre gerações;
- determinados componentes podem permanecer numa major version por bastante tempo;
- o lifecycle do host não prolonga automaticamente aplicações, runtimes ou repositórios externos.

**Fixed não significa conservadora, antiga ou LTS.** Fedora e RHEL são Fixed Release, mas têm objetivos, atualidade e lifecycles muito diferentes.

<a id="02-03-rolling-release"></a>

### 2.3 Rolling Release

Uma **Rolling Release** mantém a instalação evoluindo continuamente pelo fluxo de pacotes ou snapshots, sem exigir uma sucessão periódica de upgrades completos `N → N+1` como condição normal para permanecer na linha atual.

~~~text
estado A ─► estado B ─► estado C ─► estado D ─► ...
~~~

Exemplos clássicos: Arch Linux, CachyOS, EndeavourOS, Void Linux, PCLinuxOS e openSUSE Tumbleweed.

No Arch, por exemplo, uma ISO como `archlinux-2026.08.01` é uma **mídia/snapshot de instalação**, não uma “versão fixa Arch 2026.08.01”. Depois de instalada, a máquina acompanha o fluxo rolling.

Vantagens comuns:

- acesso mais rápido a kernels, Mesa, desktops, toolchains e bibliotecas novas;
- reduz a necessidade de grandes saltos periódicos de geração;
- pode ser interessante para hardware e stacks que evoluem rapidamente.

Custos comuns:

- mudanças chegam com maior frequência;
- ficar muitos meses sem atualizar pode aumentar a distância para o estado atual e complicar a manutenção;
- avisos do projeto e práticas de recuperação tornam-se especialmente importantes.

Rolling não é sinônimo de `testing`, `unstable` ou `bleeding edge`. O termo descreve **evolução contínua da instalação**, não qualidade.

<a id="02-04-semi-rolling-curated-hibridos"></a>

### 2.4 Semi-Rolling, Curated Rolling, Slow Rolling e modelos híbridos

Nem todo projeto cabe perfeitamente em Fixed ou Rolling.

**Curated Rolling** continua sendo rolling, mas o projeto segura, agrupa ou promove mudanças por etapas. Manjaro é um bom exemplo: pacotes percorrem branches próprias antes de chegar ao usuário de `stable`.

~~~text
upstream / Arch
       ↓
Manjaro Unstable
       ↓
Manjaro Testing
       ↓
Manjaro Stable
       ↓
usuário
~~~

`Stable` nesse desenho é **nome de branch/canal de promoção**, não prova de Fixed Release.

O **openSUSE Slowroll** é um caso nominal de slow rolling: deriva do Tumbleweed e desacelera mudanças maiores, preservando um fluxo contínuo para correções importantes. No snapshot desta edição ele permanece um projeto/canal em evolução e não deve ser usado como sinônimo universal de toda rolling curada.

O **Nobara** é um caso híbrido importante: desde a versão 41 o próprio projeto o descreve como **semi-rolling**. Ainda existem versões/ISOs numeradas, mas a atualização normal pode conduzir o sistema aos pacotes da geração seguinte quando eles ficam prontos; não há mais a mesma separação operacional de um upgrade de release tradicional.

O **Parrot OS** também exige leitura por camadas: a documentação atual o descreve como baseado em Debian Stable e, simultaneamente, afirma que seu repositório próprio oferece um modelo rolling para manter ferramentas de segurança atuais. Classificá-lo apenas como “Debian Fixed” ou “Arch-like Rolling” perderia essa combinação.

O **CentOS Stream** é outro caso que merece linguagem própria: há um fluxo contínuo de integração dentro de uma geração Enterprise Linux. É mais preciso descrevê-lo como **stream contínuo versionado** do que como rolling de desktop no sentido de Arch.

<a id="02-05-point-minor-release"></a>

### 2.5 Point Release e Minor Release

Point release não é uma terceira alternativa a Fixed e Rolling. Ela identifica um **estado/revisão dentro de uma família**.

~~~text
Debian 13.0 ─► 13.1 ─► 13.2 ─► ... ─► 13.6
~~~

No Debian 13, uma point release consolida correções e atualiza a mídia; uma instalação Debian 13 que já recebeu as atualizações correspondentes não precisa ser tratada como se estivesse migrando para uma nova geração.

No Ubuntu LTS, point releases também atualizam mídias e podem incorporar habilitação de hardware adequada à política da série. Linux Mint e Zorin usam numeração intermediária própria dentro de suas famílias.

No RHEL, a terminologia oficial relevante é **minor release** (`10.0`, `10.1`, `10.2`...). A função operacional se sobrepõe parcialmente ao que muita gente chama genericamente de point release, mas o guia preserva o vocabulário do fornecedor.

Distribuições rolling também podem publicar imagens numeradas. Kali 2026.2 ou uma ISO mensal do Arch não provam Fixed Release: é necessário distinguir **versão da mídia** de **modelo da instalação**.

<a id="02-06-branches-e-canais"></a>

### 2.6 Branches e canais: Debian Testing/Sid, Fedora Rawhide e outros casos

Uma **branch** é uma linha nomeada de desenvolvimento, manutenção ou promoção. `stable`, `testing`, `unstable`, `Rawhide`, `edge`, `-current` e nomes semelhantes têm significado definido por cada projeto; não existe uma semântica universal.

#### Debian Stable, Testing e Unstable

O fluxo de desenvolvimento do Debian pode ser representado assim:

~~~text
experimental
     ↓
unstable (Sid)
     ↓  pacotes que satisfazem critérios de migração
 testing
     ↓  freeze progressivo
 testing congelada
     ↓  estabilização
 stable
~~~

**Debian Stable** é uma Fixed Release.

**Debian Testing** é a branch de desenvolvimento da próxima Stable. Ela recebe continuamente pacotes vindos de `unstable` quando cumprem critérios de idade, dependências, arquiteturas e bugs críticos de release. Por isso, quando alguém acompanha permanentemente o alias `testing`, o comportamento cotidiano pode parecer rolling.

Mas existe uma diferença estrutural para Arch: Testing foi criada para **congelar, estabilizar e se transformar na próxima Debian Stable**. Portanto, neste guia a classificação preferida é **development branch / rolling-like**, e não “Rolling Release clássica”.

A escolha do nome usado em `sources.list` também importa:

~~~text
acompanhar "testing"
        │
        ├─ Forky é Testing
        │
        ├─ Forky vira Stable
        │
        └─ "testing" passa a apontar para a próxima Testing

acompanhar "forky"
        │
        ├─ Forky é Testing
        │
        └─ quando Forky vira Stable, o host continua acompanhando Forky
~~~

Isso mostra por que **branch, codename e release não são sinônimos**.

**Debian Unstable/Sid** é ainda mais explícito: é a distribuição/branch de desenvolvimento ativo onde novos pacotes entram. O próprio Debian ressalta que Sid não deve ser equiparado a uma Rolling Release pronta para uso como Arch; a descrição mais rigorosa é **Rolling Development**.

#### Fedora estável e Rawhide

As edições estáveis do Fedora — Workstation, Server e Atomic Desktops — são **Fixed Release**:

~~~text
Fedora 44 ─► Fedora 45 ─► Fedora 46
~~~

O Fedora **Rawhide**, por outro lado, é a árvore de desenvolvimento em constante evolução. Pacotes entram regularmente, tipicamente diariamente, e futuras releases Fedora são derivadas desse processo.

Portanto:

| Canal/produto | Classificação |
|---|---|
| Fedora Workstation 44 | Fixed Release |
| Fedora Silverblue/Kinoite 44 | Fixed Release + Atomic/Image-based |
| Fedora Rawhide | Rolling Development / development branch |

Dizer apenas “Fedora é rolling” seria errado; dizer que “Fedora não possui nenhum fluxo rolling” também esconderia Rawhide.

#### Outros exemplos de branches

- Manjaro: `unstable` → `testing` → `stable`, todos dentro da operação de uma rolling;
- Alpine: releases `stable` coexistem com a branch de desenvolvimento `edge`;
- Slackware: uma release estável coexistindo com `-current`, usado para preparar a próxima geração;
- Bazzite: `stable`, `testing` e `unstable` são canais de imagem e não equivalem automaticamente às branches homônimas de Debian ou Manjaro;
- Kali: `kali-rolling`, `kali-last-snapshot`, `kali-dev`, `kali-experimental` e `kali-bleeding-edge` têm funções diferentes.

A regra operacional é: **sempre leia o significado da branch no projeto específico**.

<a id="02-07-fixed-rolling-nao-e-atomic-transactional"></a>

### 2.7 Fixed/Rolling não é Atomic/Transactional

Esta é uma das distinções mais importantes da seção.

**Fixed/Rolling responde:** “como o sistema evolui entre releases ou ao longo do tempo?”  
**Atomic/Transactional responde:** “como uma mudança do conteúdo controlado do SO é preparada e ativada?”

#### O que é uma atualização atômica?

Atomicidade busca a propriedade **all-or-nothing no ponto de ativação**: em vez de deixar o host num estado parcialmente atualizado, o mecanismo prepara um novo estado coerente e só então o torna o estado de boot/execução.

~~~text
estado A em execução
       │
       ├── prepara novo estado B separadamente
       │
       └── ativa B como uma unidade
              ↓
          estado B

se necessário:
          estado A
          rollback
~~~

Isso **não significa que cada arquivo do computador esteja dentro da transação** e não significa que a aplicação funcionará automaticamente depois do boot.

#### Fedora Silverblue/Kinoite: o que exatamente é atualizado?

Nos Fedora Atomic Desktops baseados em `rpm-ostree`, é mais preciso falar em **conteúdo do sistema operacional gerenciado pelo deployment**, e não apenas “conteúdo do sistema operacional controlado”. Esse conteúdo inclui o kernel e grande parte do userspace empacotado do host — executáveis, bibliotecas, unidades/recursos do sistema, módulos e outros arquivos que compõem a árvore do SO.

A disposição conceitual é:

~~~text
/
├── /usr  → conteúdo do SO controlado pelo deployment; somente leitura no host normal
├── /etc  → configuração local gravável; mudanças locais são reconciliadas no upgrade
└── /var  → estado mutável/persistente; não é substituído pelo deployment
~~~

Assim, uma atualização pode ser visualizada como:

~~~text
BOOT ATUAL
Deployment A — Fedora 44
├── kernel A
├── bibliotecas A
├── systemd/componentes A
├── Mesa A
└── demais conteúdo gerenciado

              │ prepara sem reescrever A como uma sequência de mutações ad hoc
              ▼

Deployment B — Fedora 44 atualizado
├── kernel B
├── bibliotecas B
├── systemd/componentes B
├── Mesa B
└── demais conteúdo gerenciado

              │ reboot
              ▼

Deployment B vira o estado em execução
Deployment A pode permanecer disponível para rollback
~~~

`/var` permanece persistente entre deployments. Por isso, voltar para o Deployment A **não significa automaticamente voltar banco de dados, logs, dados de usuário ou todo estado mutável ao passado**. Rollback de SO continua diferente de backup.

Aplicações desktop também são frequentemente desacopladas do deployment por Flatpak, enquanto ambientes de desenvolvimento podem viver em Toolbx/containers. A atualização de um Flatpak ou container não é necessariamente a mesma transação do host.

#### Por que Silverblue/Kinoite não são Rolling?

Porque continuam pertencendo a gerações Fedora identificáveis:

~~~text
Fedora Silverblue 44
        │ updates atômicos dentro da geração
        ▼
Fedora Silverblue 44 atualizado
        │ upgrade/rebase
        ▼
Fedora Silverblue 45
~~~

Logo:

| Sistema | Release | Mecanismo de atualização do host |
|---|---|---|
| Fedora Workstation | Fixed | DNF/RPM tradicional |
| Fedora Silverblue/Kinoite | **Fixed** | **deployment atômico / image-based** |
| Arch Linux | **Rolling** | pacotes tradicionais com pacman |
| openSUSE Aeon/MicroOS | Rolling | snapshot transacional |

Esse quadro mostra por que `Atomic = Rolling` é uma equivalência falsa.

#### Fedora é o único ecossistema atômico?

Não. O princípio aparece com implementações diferentes:

- **Fedora Atomic Desktops** e **Fedora CoreOS** usam tecnologias OSTree/rpm-ostree/bootable images conforme a variante;
- **Bazzite**, Bluefin e Aurora usam a família Fedora Atomic/Universal Blue e imagens bootáveis;
- **SUSE Linux Micro/openSUSE MicroOS** usam atualizações transacionais: um novo snapshot Btrfs é criado, alterado separadamente e ativado após reboot quando a operação conclui;
- **Ubuntu Core** usa um modelo transacional baseado em snaps e gerações LTS;
- **Vanilla OS** emprega arquitetura image-based/A-B própria;
- Flatcar, Bottlerocket, Talos e outros container/cloud OS também tratam o host como imagem/estado controlado, cada um com sua implementação e contrato.

Portanto, `atomic`, `transactional`, `image-based`, `immutable` e `A/B` não devem ser usados como sinônimos automáticos. Eles podem cooperar para o mesmo objetivo operacional, mas descrevem propriedades ou implementações diferentes.

<a id="02-08-leading-edge-e-bleeding-edge"></a>

### 2.8 Leading edge e bleeding edge

Esses termos descrevem **atualidade e exposição a mudanças**, não modelo de release.

- **Leading edge:** adoção rápida de tecnologias novas, normalmente já integradas para uso regular. Fedora é frequentemente um exemplo útil: release fixa com software relativamente atual.
- **Bleeding edge:** proximidade ainda maior do desenvolvimento/upstream, aceitando maior probabilidade de regressões ou necessidade de intervenção. O rótulo é informal e deve ser contextualizado.

Um mesmo sistema pode ter kernel muito recente, biblioteca conservadora e aplicação atualizada por Flatpak quase imediatamente após o upstream. Por isso, atualidade deve ser avaliada **por componente, repositório ou canal**.

Não use as equivalências:

- rolling = bleeding edge;
- fixed = software antigo;
- stable = fixed;
- testing = rolling;
- atomic = rolling;
- image-based = immutable;
- LTS = fixed conservadora por definição.

[Voltar ao índice](#indice)

---

<a id="03-lifecycle-lts-e-suporte-empresarial"></a>

## 3. Lifecycle, LTS e suporte empresarial

**LTS (*Long-Term Support*)** significa política de manutenção prolongada para um **objeto específico**: por exemplo, uma release de distribuição, um kernel ou outro componente. O prazo e o escopo não são universais; sempre identifique qual objeto é LTS e quem o mantém.

**Enterprise** não é apenas um lifecycle longo. Um produto/ecossistema enterprise normalmente combina parte ou todos estes elementos: lifecycle documentado, suporte comercial, SLA, políticas de compatibilidade, erratas, matrizes de hardware/software, certificações, ferramentas de gestão e condições contratuais. Nenhum desses termos define sozinho o modelo Fixed ou Rolling.

O lifecycle deve responder pelo menos a estas perguntas:

- quando a versão entrou e sai de suporte;
- quais arquiteturas recebem atualização;
- quais repositórios e pacotes estão cobertos;
- que severidades de vulnerabilidade são tratadas;
- em qual fase são aceitas correções funcionais;
- se o suporte exige assinatura;
- como funciona a passagem para a próxima versão;
- por quanto tempo imagens de cloud e containers são mantidas.

> “Cinco anos de suporte” não implica cinco anos de manutenção idêntica para qualquer pacote instalado.

No Ubuntu, por exemplo, o escopo da manutenção padrão, o conteúdo de Main, a cobertura adicional por Ubuntu Pro/ESM e os pacotes de Universe precisam ser distinguidos. Em distribuições empresariais, add-ons e fases estendidas também possuem condições próprias.

Lifecycle do conteúdo do sistema operacional controlado não prolonga automaticamente:

- banco de dados instalado de repositório externo;
- runtime baixado do site do fornecedor;
- imagem de container de terceiro;
- plugin sem manutenção;
- firmware do equipamento;
- aplicação empacotada pelo próprio usuário.

<a id="03-perguntas-para-producao"></a>

### Perguntas para produção

1. A versão permanecerá suportada durante toda a vida prevista do serviço?
2. O repositório que contém cada pacote crítico está no escopo?
3. Há caminho testado para upgrade ou migração?
4. As aplicações e o hardware são certificados nessa combinação exata?
5. Existe contrato, comunidade ou equipe capaz de responder a incidentes?

[Voltar ao índice](#indice)

---

<a id="04-stable-nao-significa-uma-unica-coisa"></a>

## 4. Stable não significa uma única coisa

“Stable” pode designar:

- uma branch promovida para produção;
- preservação de API ou ABI;
- baixo ritmo de mudança;
- software upstream que saiu de beta;
- comportamento operacional previsível.

Não significa “sem bugs”. Também não prova adequação ao seu workload. **Nunca interprete `stable` isoladamente**: confirme se é nome de branch/canal, estágio de promoção, promessa de compatibilidade, descrição operacional ou apenas rótulo informal.

Uma distribuição pode ser estável no contrato de plataforma e ainda atualizar determinados componentes. Outra pode manter versões antigas, mas não oferecer o lifecycle necessário.

[Voltar ao índice](#indice)

---

<a id="05-backports-erratas-e-leitura-de-vulnerabilidades"></a>

## 5. Backports, erratas e leitura de vulnerabilidades

**Backport** é a adaptação de uma correção desenvolvida para uma versão mais nova a uma versão anterior ainda mantida pela distribuição.

~~~text
upstream 5.1 com correção
          │
          └── adaptação downstream ─► pacote 5.0 + patch de segurança
~~~

Fornecedores também podem corrigir uma falha por rebase, patch próprio, desativação de recurso ou outra mitigação. “Backport” não é a única estratégia.

Um **CVE (*Common Vulnerabilities and Exposures*)** é um identificador público para uma vulnerabilidade conhecida. O CVE identifica o problema; ele não informa, sozinho, se a versão instalada continua vulnerável, se existe exploração prática, se o pacote está no escopo de suporte ou se o fornecedor já aplicou uma correção downstream.

**OVAL (*Open Vulnerability and Assessment Language*)** é uma linguagem padronizada para representar informações de configuração/estado do sistema, testar condições como vulnerabilidade ou patch instalado e reportar o resultado. Conteúdo OVAL pode ser usado por scanners que compreendem versões e estados específicos do fornecedor, reduzindo a dependência de comparação ingênua de números de versão.

<a id="05-regra-operacional"></a>

### Regra operacional

Não determine vulnerabilidade comparando apenas a versão upstream exibida por <code>--version</code>.

Consulte:

1. advisory ou errata oficial da distribuição;
2. tracker de CVEs do fornecedor;
3. changelog e release do pacote;
4. status do repositório e da arquitetura usada;
5. scanner que compreenda versões downstream e, quando disponível, conteúdo OVAL/SCAP adequado ao fornecedor.

Um scanner que compara somente versões upstream pode produzir **falso positivo** — marcar como vulnerável um pacote já corrigido por backport. Um pacote fora de suporte também pode produzir **falso negativo** se o scanner interpretar incorretamente um número de versão recente como sinônimo de manutenção ativa.

> **CVE não é sinônimo de prioridade operacional.** Além do advisory do fornecedor e da severidade/CVSS quando disponível, considere exposição real do ativo, explorabilidade, existência de exploração conhecida, criticidade do workload, mitigação disponível e impacto operacional. Catálogos como o **CISA KEV (*Known Exploited Vulnerabilities*)** podem complementar — não substituir — a análise do fornecedor ao indicar vulnerabilidades com exploração conhecida.

[Voltar ao índice](#indice)

---

<a id="06-api-abi-e-kabi"></a>

## 6. API, ABI e kABI

Essas três siglas descrevem contratos diferentes de compatibilidade. Elas importam especialmente quando aplicações, bibliotecas, drivers ou módulos externos precisam continuar funcionando após atualizações.

<a id="06-api"></a>

### API

**API (*Application Programming Interface*)** é o contrato/interface por meio do qual um componente expõe funcionalidades ou dados para outro. Dependendo do contexto, pode envolver funções de biblioteca, chamadas em runtime, estruturas de dados, mensagens, protocolos ou endpoints. Manter uma API compatível permite que o código continue compilando ou interagindo conforme o contrato documentado.

<a id="06-abi"></a>

### ABI

**ABI (*Application Binary Interface*)** é o contrato binário entre componentes já compilados. Inclui símbolos exportados, layout de tipos, formatos binários e *calling conventions* — regras de baixo nível sobre como funções recebem argumentos, retornam valores e usam registradores/memória.

Uma API pode permanecer semelhante e a ABI mudar; nesse caso, um programa pode precisar ser recompilado mesmo que o código-fonte use chamadas aparentemente iguais.


Exemplo simplificado de quebra de ABI:

~~~text
aplicação compilada → espera libfoo.so.1
                         │
                         └── biblioteca passa para libfoo.so.2

Se a nova biblioteca não preservar a ABI anterior, o binário pode deixar de carregar ou exigir recompilação/rebuild, mesmo que a API de alto nível continue parecida.
~~~

O nome `libfoo.so.1` representa um **SONAME (*shared object name*)**, identificador de compatibilidade usado por bibliotecas compartilhadas no formato **ELF (*Executable and Linkable Format*)**, formato binário comum em Linux; mudar o SONAME normalmente sinaliza uma fronteira de compatibilidade e necessidade de tratar uma nova ABI. **SONAME é um mecanismo de identificação/versionamento de bibliotecas compartilhadas; não é prova completa de compatibilidade ABI em todos os aspectos.**

<a id="06-kabi"></a>

### kABI

**kABI (*kernel Application Binary Interface*)** é a parte da ABI do kernel relevante para módulos externos, drivers e outros componentes binários que interagem com o kernel. Políticas de kABI são específicas de fornecedor, versão, arquitetura e conjunto de símbolos. **O kernel Linux upstream não oferece uma ABI interna estável universal para módulos externos**; quando uma distribuição/fornecedor promete estabilidade de kABI, esse compromisso é downstream e possui escopo próprio.


Exemplo simplificado:

~~~text
kernel A + módulo externo Z → módulo carrega
          │
          └── kernel atualizado muda símbolo/contrato binário usado por Z
                                      │
                                      └── módulo pode exigir rebuild, nova versão ou suporte explícito do fornecedor
~~~

Distribuições empresariais podem preservar subconjuntos de kABI para reduzir esse tipo de ruptura, mas a política precisa ser verificada para a release, arquitetura e módulo específicos.

Em ambientes corporativos, “estabilidade” pode significar:

- aplicação certificada continuar executando;
- módulo suportado continuar carregando;
- configuração manter a mesma semântica;
- biblioteca preservar símbolos necessários;
- caminho de upgrade permanecer documentado;
- automação não quebrar por alteração incompatível não anunciada.

Por isso, “mais recente” e “mais adequado” não são sinônimos.

[Voltar ao índice](#indice)

---

<a id="07-pacote-formato-gerenciador-repositorio-e-origem"></a>

## 7. Pacote, formato, gerenciador, repositório e origem

Esses termos não são intercambiáveis:

| Camada | Exemplo | Responde a... |
|---|---|---|
| Formato de pacote | `deb`, `rpm`, `pkg.tar.zst` | Como o artefato é estruturado? |
| Gerenciador de baixo nível | `dpkg`, `rpm` | Como arquivos e metadados do pacote são registrados/instalados? |
| Resolver/gerenciador de alto nível | APT, DNF, Zypper, pacman | Como dependências, repositórios e transações são calculados? |
| Repositório | Debian Stable, Fedora Updates | De qual coleção controlada o pacote vem? |
| Origem/proveniência | projeto oficial, fornecedor, comunidade, terceiro | Quem construiu, assinou e mantém o artefato? |
| Modelo de aplicação | Flatpak, Snap, AppImage, container | Como uma aplicação é distribuída, atualizada e eventualmente isolada do host? |

Uma **verificação de assinatura bem-sucedida** comprova que os dados cobertos pela assinatura foram assinados pela chave aceita pela política local e não foram modificados depois da assinatura. Isso **não comprova**, por si só, que a chave deveria ser confiável, que o software é seguro, que está atualizado ou que atende à política da organização.

Também separe:

- **metadados assinados do repositório**: protegem a relação entre índices/metadados e o conteúdo que o gerenciador espera consumir;
- **artefato/pacote assinado individualmente**: autentica o objeto específico quando o ecossistema usa esse modelo;
- **rotação e expiração de chaves**: chaves possuem lifecycle próprio e precisam de processo de substituição, revogação e auditoria;
- **prioridade/pinning de repositórios**: regras que controlam de qual origem uma versão pode ser escolhida quando múltiplos repositórios oferecem o mesmo pacote; uma prioridade mal desenhada pode instalar versões inesperadas.

Scripts executados durante instalação, atualização ou build também fazem parte da superfície de confiança: um pacote ou receita pode executar código com os privilégios concedidos pelo processo de instalação.

<a id="07-fontes-suplementares"></a>

### Fontes suplementares

Fontes externas ao repositório principal podem ser legítimas, mas introduzem outra cadeia de manutenção:

- **AUR (*Arch User Repository*)** distribui principalmente receitas de build (`PKGBUILD`), não um repositório oficial de binários equivalentes aos pacotes oficiais do Arch; revise o `PKGBUILD` e a origem antes de construir/instalar.
- **PPA (*Personal Package Archive*)** é um repositório adicional no ecossistema Ubuntu/Launchpad. Um PPA não herda automaticamente o mesmo suporte da Canonical.
- **COPR** é um serviço comunitário de build/repositórios no ecossistema Fedora. Pacotes COPR não recebem automaticamente o mesmo suporte dos repositórios Fedora oficiais.
- **OBS (*Open Build Service*)** é uma plataforma de build/publicação usada pelo ecossistema openSUSE e por outros projetos; cada repositório mantém sua própria responsabilidade e política.
- **Flatpak** desacopla aplicações gráficas do host, usa runtimes e sandbox/permissões; origem, runtime e manutenção continuam relevantes.
- **Snap** distribui aplicações/serviços em pacotes autocontidos com canais e mecanismos de confinamento conforme o snap; canal, publisher, permissões/interfaces e lifecycle continuam relevantes.
- **AppImage** empacota uma aplicação para execução portátil; atualização, integração e proveniência precisam ser tratadas pelo projeto/usuário.
- repositório de fornecedor pode ser necessário para suporte oficial, porém adiciona outro lifecycle ao ambiente.

<a id="07-praticas-seguras"></a>

### Práticas seguras

- prefira repositórios destinados exatamente à versão instalada;
- não misture famílias ou releases apenas para “obter um pacote mais novo”;
- evite *partial upgrades* quando o projeto não os suporta;
- registre chaves, origem, prioridade e responsável por cada repositório adicional;
- remova repositórios abandonados antes de upgrades maiores;
- inventarie software instalado fora do gerenciador de pacotes;
- quando o risco justificar, valide assinatura, atestação e **SBOM (*Software Bill of Materials*)**, isto é, o inventário estruturado dos componentes presentes no artefato;
- quando disponível, use também **VEX (*Vulnerability Exploitability eXchange*)** para registrar/comunicar o estado de vulnerabilidades conhecidas em relação a um produto. SBOM responde principalmente **“quais componentes existem?”**; VEX ajuda a responder **“qual é o status conhecido desta vulnerabilidade para este produto?”**;
- em Flatpaks e containers, avalie também o **runtime/base image** e seu lifecycle: empacotar a aplicação separadamente do host não elimina dependências, vulnerabilidades nem proveniência da base;
- para **imagens OCI**, registre pelo menos registry/origem, referência imutável por **digest** quando necessário, identidade do publicador, assinatura/attestation quando disponível, cadeia de build, SBOM/VEX e política de atualização da imagem-base. Uma imagem assinada comprova vínculo/integridade segundo a política adotada; **não prova ausência de vulnerabilidades nem adequação ao workload**.

[Voltar ao índice](#indice)

---

<a id="08-tradicional-imutavel-atomico-image-based-e-declarativo"></a>

## 8. Tradicional, imutável, atômico, image-based e declarativo

São propriedades relacionadas, mas independentes.

<a id="08-01-sistema-tradicional-baseado-em-pacotes"></a>

### 8.1 Sistema tradicional baseado em pacotes

Pacotes alteram diretamente a instalação existente. Esse modelo é flexível, conhecido e adequado a inúmeros cenários, mas exige controle de **drift** — divergência acumulada entre o estado planejado e o estado real — e disciplina de configuração.

<a id="08-02-imutavel-ou-com-base-read-only"></a>

### 8.2 Imutável ou com base read-only

O **conteúdo do sistema operacional protegido pelo mecanismo de atualização** é modificado somente por caminhos controlados. Dados, configurações permitidas, containers e diretórios persistentes continuam mutáveis.

Imutabilidade:

- não significa que nada muda;
- não substitui controle de acesso;
- não corrige imagem vulnerável;
- não protege automaticamente dados do usuário;
- não impede, por si só, exploração de processos em execução, roubo de credenciais, abuso de permissões ou comprometimento de dados mutáveis.

> **Não confunda:** uma base read-only/imutável reduz alterações persistentes ad hoc na área protegida, mas **não substitui segurança em tempo de execução**. Patching, Mandatory Access Control (SELinux/AppArmor), sandbox, isolamento, privilégios mínimos, proteção de credenciais e monitoramento continuam necessários.

<a id="08-03-atualizacao-atomica-ou-transacional"></a>

### 8.3 Atualização atômica ou transacional

Uma atualização é **atômica no ponto de ativação** quando o consumidor observa o estado anterior ou o novo estado completo, sem depender de um estado intermediário parcialmente ativado. A implementação pode usar deployment, snapshot, slot A/B, generation ou outro mecanismo.

> **Não confunda:** **atomicidade não implica rollback durável**. Preservar o estado anterior, permitir retorno automático, recuperar dados ou garantir consistência da aplicação são capacidades adicionais e precisam ser verificadas separadamente.

Atomicidade da ativação também não prova que a aplicação funcionará após a mudança. Migrações de banco, firmware, estado externo, idempotência de automações e compatibilidade ainda exigem teste.

<a id="08-04-image-based"></a>

### 8.4 Image-based

O conteúdo do sistema operacional controlado pelo host é entregue ou composto predominantemente como imagem, árvore ou deployment coerente. Customizações devem seguir o mecanismo suportado do projeto, em vez de depender de mutações arbitrárias no estado em execução.

<a id="08-05-declarativo"></a>

### 8.5 Declarativo

O operador descreve o estado desejado e a plataforma produz uma configuração. NixOS é o exemplo clássico, mas declaratividade também aparece em imagens, Kubernetes e ferramentas de provisionamento.

Declarativo não implica, por si só, base imutável. Imutável não implica configuração declarativa.

> **Declarativo não significa automaticamente reproduzível.** Declaratividade descreve **como o estado desejado é especificado**. Reprodutibilidade exige controlar as entradas relevantes — versões, dependências, imagens, fontes, lockfiles e outros artefatos — de modo que o mesmo conjunto de entradas possa produzir novamente o resultado esperado.

<a id="08-06-rollback-nao-e-backup"></a>

### 8.6 Rollback não é backup

Rollback de deployment costuma recuperar **o conteúdo coberto pelo deployment**. Ele pode não reverter:

- dados de usuário;
- banco migrado;
- schema externo;
- volume persistente;
- firmware;
- segredo rotacionado;
- configuração fora do escopo da transação.

Snapshot local também não é backup se permanecer no mesmo domínio de falha.

[Voltar ao índice](#indice)

---

<a id="09-desktop-e-pilha-grafica"></a>

## 9. Desktop e pilha gráfica

Uma recomendação de desktop precisa separar as camadas da interface gráfica. Instalar um componente não significa que a distribuição o trate como combinação padrão, integrada ou plenamente suportada.

| Componente | Função | Exemplos |
|---|---|---|
| **Desktop Environment (DE)** | experiência integrada: shell, painel, configurações, sessão e aplicações | GNOME, KDE Plasma, Xfce, Cinnamon |
| **Window manager/compositor** | posiciona janelas e compõe a imagem final; em Wayland, o compositor também exerce funções de display server | Mutter, KWin, Sway, Hyprland |
| **Display manager** | tela de login gráfico e inicialização da sessão | GDM, SDDM, LightDM |
| **Protocolo/sessão gráfica** | define como aplicações, entrada e compositor/servidor gráfico se comunicam | Wayland, X11, XWayland |
| **Pilha 3D** | fornece drivers e APIs de renderização/aceleração | Mesa, Vulkan, driver proprietário |

Termos recorrentes nesta seção:

- **Mesa** é o principal conjunto open source de implementações de APIs gráficas e drivers de espaço de usuário para muitas GPUs em Linux.
- **Vulkan** é uma API gráfica e de computação de baixo nível; “suporte a Vulkan” depende do driver e do hardware, não apenas da distribuição.
- **HDR (*High Dynamic Range*)** amplia faixa de brilho/cor quando monitor, GPU, driver, compositor e aplicação suportam o fluxo completo.
- **VRR (*Variable Refresh Rate*)** permite variar dinamicamente a taxa de atualização do monitor para acompanhar a produção de quadros.
- **GPU (*Graphics Processing Unit*) híbrida** significa um sistema com mais de uma GPU, normalmente integrada + dedicada, exigindo política de seleção/offload e energia.
- **CUDA** é a plataforma proprietária de computação da NVIDIA; **ROCm** é a plataforma de computação da AMD. Compatibilidade depende de GPU, driver e versões suportadas.
- **passthrough** entrega um dispositivo físico, como uma GPU, diretamente a uma máquina virtual; exige suporte de hardware, firmware, **IOMMU (*Input-Output Memory Management Unit*)**, que controla o acesso de dispositivos à memória, e suporte do hipervisor.

Avalie separadamente:

- Wayland nativo e necessidade de XWayland para aplicações X11;
- HDR, VRR, escala fracionária e múltiplos monitores;
- GPU híbrida, computação em GPU, CUDA/ROCm e passthrough;
- codecs e aceleração de vídeo;
- desktop remoto e captura de tela;
- áudio e multimídia, incluindo maturidade de **PipeWire** — stack de áudio/multimídia usada amplamente em desktops Linux modernos — quando o workload depende de áudio profissional, Bluetooth ou roteamento avançado;
- acessibilidade, leitores de tela, métodos de entrada e navegação por teclado quando forem requisitos do usuário;
- impressão e digitalização para modelos/periféricos obrigatórios;
- **portais XDG** — interfaces intermediárias para conceder a aplicações sandboxed acesso controlado a recursos do desktop — quando Flatpak/Wayland precisar abrir arquivos, compartilhar tela ou acessar recursos do sistema;
- versão do kernel, Mesa e driver;
- sessão padrão e maturidade da combinação na distribuição escolhida.

O quiz modela doze ambientes: KDE Plasma, GNOME, Cinnamon, Xfce, COSMIC, Budgie, LXQt, MATE, Pantheon, DDE (*Deepin Desktop Environment*), Trinity e Moksha. Window managers independentes permanecem fora desta edição.


**Window managers independentes**, como i3, Sway e Hyprland, podem ser adequados para usuários que priorizam navegação por teclado, tiling e montagem manual do ambiente. Eles ficam fora do ranking padrão porque sua adequação depende fortemente da combinação individual de compositor, painel, launcher, notificações, políticas de energia e demais componentes, e não apenas de uma edição integrada fornecida pela distribuição.

[Voltar ao índice](#indice)

---

<a id="10-hardware-firmware-e-software-nao-livre"></a>

## 10. Hardware, firmware e software não livre

“Linux suporta meu hardware” é uma pergunta incompleta. Compatibilidade depende da **arquitetura de CPU**, do modelo exato do equipamento, da versão do kernel, do firmware, dos drivers, do boot, do userspace gráfico/compute e da matriz de suporte do projeto ou fornecedor.

> **Essencial:** suporte de hardware é uma propriedade da **combinação exata** `arquitetura + dispositivo + firmware + kernel + driver/userspace + release/canal`. Um componente funcionar em uma distribuição não prova que funcionará do mesmo modo em outra release ou arquitetura.

<a id="10-arquiteturas-cpu-plataforma"></a>

### Arquiteturas de CPU e plataforma

A arquitetura de CPU define o conjunto de instruções e a plataforma para a qual kernel, bootloader, bibliotecas e aplicações precisam ser compilados. Nomes comuns:

| Arquitetura | Também aparece como | Uso típico | Ponto de atenção |
|---|---|---|---|
| **x86_64** | AMD64, x64 | PCs, workstations e grande parte dos servidores | costuma ter a cobertura mais ampla de software desktop/comercial, mas isso não é garantia por produto |
| **aarch64** | ARM64 | servidores ARM, **SBCs (*Single-Board Computers*, computadores de placa única)**, notebooks e dispositivos embarcados | suporte varia muito por **SoC (*System on a Chip*, sistema em chip)**, boot, firmware e periféricos; “ARM64 suportado” não significa qualquer placa ARM |
| **riscv64** | RISC-V 64-bit | ecossistema emergente, desenvolvimento e plataformas específicas | disponibilidade de imagens, drivers, firmware e software de terceiros tende a ser mais limitada e muda rapidamente |

Arquitetura não é apenas “tipo de processador”. Ela influencia:

- imagens de instalação disponíveis;
- bootloader e firmware suportados;
- módulos e drivers externos;
- repositórios e pacotes publicados;
- containers e imagens de cloud;
- certificações de hardware e aplicações;
- ferramentas proprietárias, drivers de GPU e runtimes de computação.

> **Decisão prática:** sempre confirme a **arquitetura exata** na matriz oficial. Um software certificado em RHEL x86_64, por exemplo, não deve ser presumido certificado em RHEL aarch64.

<a id="10-firmware-microcode-drivers"></a>

### Firmware, microcode e drivers

Essas camadas são diferentes:

~~~text
BIOS/UEFI ou firmware da plataforma
          ↓
microcode da CPU
          ↓
firmware de dispositivos (Wi-Fi, GPU, SSD, placa/interface de rede...)
          ↓
driver do kernel
          ↓
libraries/userspace (Mesa, Vulkan, CUDA, ROCm...)
          ↓
aplicação
~~~

- **Microcode** é uma camada de controle interno da CPU que pode receber correções fornecidas pelo fabricante. Em x86, o kernel Linux possui suporte para carregar microcode Intel/AMD, frequentemente cedo no boot por meio de uma imagem **initramfs/initrd**, usada na fase inicial da inicialização antes de o filesystem raiz definitivo estar plenamente disponível. A distribuição normalmente empacota e integra essas atualizações.
- **Firmware de dispositivo** é código executado ou carregado em componentes como Wi-Fi, GPU, SSD, controladoras e docks. Um driver presente no kernel pode continuar inútil se o firmware necessário estiver ausente.
- **Driver** é o componente do sistema operacional que controla o dispositivo e expõe suas funções ao restante do kernel/userspace.
- **fwupd** é um daemon/framework usado por várias distribuições para descobrir e aplicar firmware de hardware compatível. O **LVFS (*Linux Vendor Firmware Service*)** é um serviço utilizado por fabricantes para publicar metadados e firmwares que clientes como `fwupd` podem consumir. Cobertura depende do fabricante e do dispositivo; nem todo firmware é distribuído por LVFS.

A atualização do sistema operacional e a atualização de firmware são ciclos diferentes. Um rollback de pacotes, deployment ou snapshot **não deve ser presumido capaz de reverter firmware já gravado no hardware**.

Para workloads de **machine learning (ML) / inteligência artificial (AI), renderização ou computação acelerada**, valide também GPU ou outro acelerador computacional, driver, CUDA/ROCm ou runtime equivalente, framework, versão do kernel e suporte de containers. “A distribuição roda Python” não prova compatibilidade com a pilha de aceleração necessária.

<a id="10-nao-livre-nao-e-um-unico-criterio"></a>

### Não livre não é um único critério

Separe:

- firmware necessário ao dispositivo;
- driver proprietário, como determinadas pilhas NVIDIA;
- codecs sujeitos a licenças e patentes;
- aplicações proprietárias;
- repositórios comunitários com conteúdo restrito.

Uma política pode aceitar firmware e rejeitar aplicações proprietárias. O recomendador trata esses itens de forma independente.

<a id="10-politicas-software-firmware"></a>

### Perfis de política para software e firmware

Esta tabela não define uma política “correta”; mostra que organizações e usuários podem aceitar categorias diferentes:

| Perfil ilustrativo | Firmware não livre | Driver proprietário | Aplicação proprietária | Uso típico |
|---|---:|---:|---:|---|
| **Software estritamente livre** | rejeita quando a política exigir pureza completa | rejeita | rejeita | ambientes orientados por critérios de liberdade/licenciamento específicos |
| **Pragmatismo de hardware** | aceita quando necessário para o dispositivo | avalia caso a caso | pode restringir | organizações que priorizam hardware funcional sem liberar automaticamente qualquer software proprietário |
| **Desktop/workstation pragmático** | aceita conforme necessidade | aceita quando necessário e suportado | aceita conforme requisito | foco em produtividade, gaming, GPU ou compatibilidade comercial |
| **Política corporativa controlada** | somente fontes aprovadas | somente versões homologadas | somente software licenciado/aprovado | ambientes governados por segurança, suporte, compliance e inventário |

> **Não confunda:** “não livre” descreve licenciamento/distribuição. Não é, sozinho, uma medida de segurança, qualidade ou suporte. A decisão deve considerar origem, manutenção, contrato, risco e requisito funcional.

<a id="10-checklist-minimo"></a>

### Checklist mínimo

1. Identifique arquitetura de CPU, modelo da placa/equipamento e revisões relevantes. Ferramentas como `lscpu`, `lspci`, `lsusb` e, quando disponível, `inxi` podem ajudar no inventário, mas a saída precisa ser confrontada com documentação do hardware e do fornecedor.
2. Inicialize uma mídia live quando aplicável.
3. Teste rede, áudio, vídeo, armazenamento, suspensão e retomada.
4. Confirme boot com Secure Boot no estado realmente desejado.
5. Teste monitor externo, dock, GPU híbrida, aceleração 3D/compute e periféricos críticos.
6. Verifique firmware da plataforma e atualizações disponíveis. **BIOS** representa o modelo legado de firmware/boot dos PCs compatíveis; **UEFI (*Unified Extensible Firmware Interface*)** é a especificação/ambiente moderno de firmware adotado pela maioria das plataformas atuais. Confirme também se `fwupd`/LVFS cobre o dispositivo quando isso fizer parte da estratégia de manutenção.
7. Confirme pacotes de microcode da CPU e política da distribuição quando aplicável.
8. Confirme a matriz do fornecedor para workload, release, arquitetura, kernel/driver e hardware exatos.
9. Em notebooks/workstations, valide autonomia, suspensão/retomada, hibernação, USB4/Thunderbolt, biometria, áudio e periféricos realmente usados; suporte parcial pode ser suficiente para um desktop de laboratório e inadequado para uma estação de produção.
10. Em ARM/aarch64, valide também boot, firmware e **device tree** — descrição estruturada do hardware usada pelo kernel em muitas plataformas ARM — quando a plataforma depender dele; suporte ao conjunto de instruções não garante suporte à placa específica.

[Voltar ao índice](#indice)

---

<a id="11-seguranca-hardening-compliance-e-certificacao"></a>

## 11. Segurança, hardening, compliance e certificação

Esses termos descrevem camadas diferentes e não devem ser usados como sinônimos:

> **Essencial:** segurança de plataforma não é um selo único. **Patching corrige falhas conhecidas; hardening reduz superfície/privilégio; compliance demonstra aderência a controles; certificação valida um escopo formal específico.**

| Camada | Objetivo | Evidência típica |
|---|---|---|
| **Patching** | corrigir vulnerabilidades e defeitos conhecidos | advisory, errata, pacote ou imagem corrigida |
| **Hardening** | reduzir superfície de ataque, privilégios e configurações inseguras | configuração, política MAC, benchmark |
| **Compliance** | demonstrar aderência a controles, políticas ou normas aplicáveis | scan, evidência, exceção formalmente aprovada |
| **Certificação** | avaliação formal de produto/configuração dentro de escopo definido | certificado, versão e configuração avaliadas |

Uma distribuição pode possuir bons mecanismos de segurança e ainda não estar conforme a política da sua organização. Da mesma forma, um produto certificado pode perder o escopo da certificação se kernel, versão, arquitetura ou configuração forem alterados fora das condições avaliadas.

Antes de selecionar controles, registre um **modelo de ameaça**: quais ativos precisam ser protegidos, contra quais adversários/eventos, por quais caminhos de ataque e com quais consequências. Sem esse contexto, “mais hardening” pode aumentar complexidade sem reduzir o risco dominante. Aplique também o **princípio do menor privilégio**: usuários, serviços, containers e processos devem receber somente os privilégios necessários para sua função.

<a id="11-selinux-e-apparmor"></a>

### SELinux e AppArmor

**MAC (*Mandatory Access Control*, controle de acesso obrigatório)** aplica políticas adicionais às permissões Unix tradicionais.

- **SELinux (*Security-Enhanced Linux*)** usa rótulos e políticas para controlar quais ações processos e objetos podem executar.
- **AppArmor** aplica perfis de segurança principalmente por caminhos/recursos associados a aplicações.

Ter o pacote instalado não prova que o controle esteja ativo, em modo de bloqueio (*enforcing*) ou com política adequada ao workload. MAC também não substitui permissões Unix, **capabilities** (privilégios granulares do kernel), **namespaces** (isolamento de recursos/processos) nem **seccomp** (filtro de chamadas de sistema).

<a id="11-secure-boot-e-measured-boot"></a>

### Secure Boot e measured boot

- **Secure Boot** verifica se componentes cobertos pela política de boot estão assinados/autorizados antes da execução.
- **Measured boot** registra medições criptográficas dos componentes inicializados, normalmente em um **TPM (*Trusted Platform Module*)**, para posterior verificação ou atestação.
- **Criptografia de disco** protege dados em repouso contra leitura não autorizada quando a chave não está disponível.

São controles complementares: verificar assinatura, medir estado e criptografar dados resolvem problemas diferentes.

<a id="11-fips-cis-stig-e-common-criteria"></a>

### FIPS, CIS, STIG e Common Criteria

Antes das ressalvas, é necessário saber o que cada termo representa:

- **FIPS 140-3 (*Federal Information Processing Standard 140-3*)** define requisitos de segurança para módulos criptográficos usados no contexto federal dos Estados Unidos e serve de base para validação formal de módulos criptográficos pelo **NIST (*National Institute of Standards and Technology*)**.
- **CIS Benchmarks**, publicados pelo **Center for Internet Security**, são recomendações consensuais de configuração segura para sistemas operacionais, cloud, containers, bancos, dispositivos e outros produtos.
- **STIGs (*Security Technical Implementation Guides*)** são guias técnicos de implementação de segurança mantidos no ecossistema do Departamento de Defesa dos Estados Unidos para sistemas e tecnologias de TI.
- **Common Criteria for Information Technology Security Evaluation** é um framework internacional para especificar e avaliar propriedades de segurança de produtos de TI; a edição CC:2022 corresponde à série ISO/IEC 15408:2022. A avaliação ocorre contra um **Security Target** do produto e, quando aplicável, **Protection Profiles**, sempre dentro de escopo e configuração formalmente definidos.

Agora as ressalvas ficam claras:

- ativar “modo FIPS” não prova que todo o ambiente esteja coberto por um módulo formalmente validado;
- validação FIPS possui módulo, versão, binário, plataforma e configuração específicos;
- existir um benchmark CIS ou STIG para o produto não significa que o host já esteja conforme;
- derivado ou clone não herda automaticamente certificação do fornecedor de origem;
- Common Criteria certifica um produto/configuração/escopo avaliado, não “qualquer Linux parecido”;
- a auditoria precisa confirmar o documento, versão e escopo válidos na data de implantação.

Para software e imagens, avalie também **cadeia de fornecimento**: origem, assinatura, atestações, SBOM/VEX quando disponíveis, proteção de chaves e repositórios, e política para módulos do kernel/artefatos externos. Esses controles complementam patching e hardening; não os substituem.

<a id="11-live-patching"></a>

### Live patching

**Live kernel patching** permite aplicar determinadas correções ao kernel em execução sem reboot imediato. Ele reduz a urgência de alguns reinícios, mas não cobre toda atualização de kernel, **microcode** de CPU, firmware, bootloader nem mudanças de **userspace** — programas e bibliotecas que executam fora do kernel.

Alta disponibilidade depende da arquitetura do serviço, não apenas do kernel: redundância, failover, banco de dados, storage e dependências externas continuam relevantes.

[Voltar ao índice](#indice)

---

<a id="12-storage-criptografia-recuperacao-e-backup"></a>

## 12. Storage, criptografia, recuperação e backup

**Sistema de arquivos (*filesystem*)** é a estrutura que organiza arquivos, diretórios, metadados e alocação de blocos em um dispositivo ou volume. O nome do filesystem, sozinho, não determina o resultado operacional: integridade, redundância, criptografia, snapshot, replicação, backup e recuperação são capacidades diferentes.

> **Essencial:** pense em storage como **camadas**. Uma decisão em uma camada não substitui controles das outras.

~~~text
hardware físico / dispositivo
          ↓
RAID ou controlador (quando existe)
          ↓
volume / LVM
          ↓
criptografia de bloco, como LUKS/dm-crypt
          ↓
filesystem
          ↓
snapshot
          ↓
replicação
          ↓
backup independente
~~~

Termos essenciais:

- **checksum** é um valor calculado para detectar alteração/corrupção de dados ou metadados;
- **scrub** é uma verificação periódica da integridade armazenada e, quando existe redundância adequada, pode permitir correção de blocos corrompidos;
- **RAID (*Redundant Array of Independent Disks*)** combina discos para desempenho e/ou tolerância a determinadas falhas; não cria automaticamente uma cópia histórica independente;
- **LVM (*Logical Volume Manager*)** adiciona uma camada de gerenciamento de volumes lógicos sobre dispositivos/volumes físicos; facilita certos desenhos de expansão, snapshots e organização, mas não substitui filesystem, RAID ou backup;
- **LUKS (*Linux Unified Key Setup*)** é um formato comum para gerenciamento de criptografia de disco em Linux; **dm-crypt** é a camada do kernel normalmente usada para realizar a criptografia de blocos;
- **snapshot** captura o estado de um volume/dataset/filesystem em um ponto no tempo; seu escopo e consistência dependem da implementação e da aplicação;
- **copy-on-write (CoW)** grava mudanças em novos blocos antes de atualizar referências, permitindo recursos como snapshots eficientes, mas com trade-offs de fragmentação, latência e amplificação de escrita;
- **scrub** é uma operação sistemática de verificação de integridade de dados e/ou metadados, conforme a implementação do sistema de armazenamento. A capacidade de **reparar** corrupção depende do mecanismo, de checksums e da existência de uma cópia redundante válida; ter scrub não implica, sozinho, recuperação automática;
- **NFS (*Network File System*)**, **SMB (*Server Message Block*)** e **iSCSI (*Internet Small Computer Systems Interface*)** são formas diferentes de oferecer armazenamento/compartilhamento em rede: NFS e SMB normalmente expõem arquivos; iSCSI expõe blocos.

| Necessidade | Perguntas de validação |
|---|---|
| Integridade | há checksums de dados/metadados? existe operação de scrub/verificação? a implementação consegue apenas detectar ou também reparar com redundância válida? a saúde do dispositivo é monitorada? |
| Snapshot | é consistente com a aplicação? replica? possui retenção? o estado capturado cobre os dados necessários? |
| RAID | quais falhas tolera? como ocorre rebuild? o controlador/filesystem entende o desenho? como o estado degradado é monitorado? |
| Volumes | LVM ou outra camada é necessária? expansão/redução é suportada pelo conjunto volume + criptografia + filesystem? |
| Criptografia | cobre o filesystem raiz (`/`), dados, **swap** e hibernação? onde ficam as chaves? existe recuperação segura do cabeçalho/chaves? |
| Expansão | é online ou offline? quais limites, janelas, riscos e pré-requisitos? |
| Boot | instalador, firmware, initramfs e bootloader suportam o desenho? existe procedimento de recuperação do boot? |
| SSD/NVMe | TRIM/discard é compatível com o desenho? endurance e indicadores SMART/NVMe são monitorados? |
| Compartilhamento | NFS/SMB/iSCSI são parte suportada do produto ou configuração manual? autenticação e permissões foram definidas? |
| Backup | existe cópia independente, testada e fora do mesmo domínio de falha? restauração foi realmente executada? |

Regras essenciais:

- RAID não é backup;
- snapshot não é backup por definição;
- **backup concluído não prova restauração possível**: backup exige retenção, independência adequada e proteção contra alteração/remoção conforme a ameaça; teste recuperação de dados e, quando necessário, reconstrução completa do serviço;
- criptografia sem recuperação de chaves pode converter uma falha operacional em perda definitiva;
- rollback do sistema não garante consistência de banco de dados; para workloads stateful, valide snapshot/backup **application-consistent** — capturado de forma coordenada com a aplicação para preservar um estado recuperável coerente — quando necessário;
- deduplicação, compressão, CoW, TRIM e snapshots possuem benefícios e custos que precisam ser medidos no workload real;
- suporte a ZFS, Btrfs, XFS, LUKS/LVM e recursos associados varia entre instalador, kernel, fornecedor, arquitetura e política de suporte;
- estratégias como **3-2-1-1-0** — por exemplo, múltiplas cópias em mídias/localizações distintas, uma cópia offline/imutável e verificação de ausência de erros — podem ser usadas como referência de desenho de backup, mas não são garantia universal: a política deve refletir RPO, RTO, ameaças, retenção, cópias imutáveis/offline e testes de recuperação.

Appliances como TrueNAS e Unraid podem ser mais adequados que um Linux generalista quando **storage é o produto principal**. Em outros casos, um servidor generalista com equipe capaz de operar a pilha pode ser mais adequado.

[Voltar ao índice](#indice)

---

<a id="13-virtualizacao-containers-e-cloud-native"></a>

## 13. Virtualização, containers e cloud-native

> **Essencial:** container, máquina virtual e sistema operacional especializado para nó Kubernetes resolvem problemas diferentes. Escolha pelo papel do workload, isolamento necessário e modelo operacional — não apenas porque todos “rodam Linux”.

**Cloud-native** descreve aplicações e plataformas desenhadas para automação, APIs, infraestrutura dinâmica e operação distribuída, frequentemente usando containers e orquestração. **Kubernetes** é uma plataforma de orquestração que agenda, mantém e atualiza workloads containerizados em um conjunto de nós.

Antes de comparar ferramentas, separe três conceitos:

- **Máquina virtual (VM)** virtualiza recursos de hardware e normalmente executa um sistema operacional convidado com **kernel próprio**.
- **Container de aplicação** isola processos por mecanismos do kernel do host, como namespaces e cgroups, e normalmente **compartilha o kernel do host**. Por isso, container não é simplesmente uma “VM menor”.
- **OCI (*Open Container Initiative*)** define especificações abertas usadas amplamente para imagens e runtimes de containers. Compatibilidade com OCI não significa, por si só, que uma imagem seja segura, suportada ou adequada ao host.
- **Rootless container** executa o runtime/container sem privilégios de root no host sempre que possível; reduz impacto de determinadas falhas, mas **não significa risco zero** nem elimina vulnerabilidades do kernel, runtime, imagem ou configuração.
- **System container** oferece um userspace mais parecido com um sistema completo, mas continua normalmente compartilhando o kernel do host; LXC/Incus são exemplos comuns.

Essas diferenças afetam isolamento, boot, compatibilidade de kernel, superfície de ataque, consumo de recursos e operação.

Não trate “roda container” como uma única capacidade:

| Papel | Exemplos de requisitos |
|---|---|
| **Application container host** | Docker/Podman, containerd, **rootless** (execução sem privilégios administrativos de `root` quando suportada), SELinux/AppArmor, cgroups |
| **System containers** | LXC/LXD/Incus, nesting, rede, storage e limites de kernel compartilhado |
| **Hypervisor** | KVM (*Kernel-based Virtual Machine*)/QEMU, interface de gestão, live migration, **HA (*High Availability*)**, passthrough |
| **HCI (*Hyper-Converged Infrastructure*)** | compute, storage e rede integrados em cluster, com quorum/fencing e lifecycle coordenado |
| **Kubernetes node OS** | kernel suportado, **cgroup v2**, runtime compatível com **CRI**, cgroup driver coerente, **CNI**, **CSI**, kubelet, upgrade coordenado, segurança MAC e drivers/accelerators quando aplicáveis |
| **Desktop de desenvolvimento** | engine local, toolboxes/containers, integração com **IDE (*Integrated Development Environment*)** e arquivos do usuário |

Proxmox VE, Harvester, Talos, Fedora CoreOS, Flatcar, Bottlerocket, Google Container-Optimized OS e Azure Container Linux não são substitutos diretos de uma estação Linux generalista. Eles concorrem quando o **papel operacional correspondente** é selecionado.

Imagens otimizadas por provedor podem oferecer integração mais profunda com a cloud, mas aumentam acoplamento. Para multicloud, portabilidade e consistência operacional podem valer mais do que integração máxima com um único provedor.

> **Kubernetes:** “executa containers” é um critério insuficiente. Valide a combinação exata de kernel, **cgroup v2**, runtime compatível com **CRI (*Container Runtime Interface*)**, cgroup driver, **CNI (*Container Network Interface*)**, **CSI (*Container Storage Interface*)**, kubelet, políticas de segurança e versão do Kubernetes. A compatibilidade pertence à matriz completa, não apenas ao nome da distribuição.

Para containers em produção, valide também a proveniência das imagens OCI: registry, digest, assinatura/attestation, SBOM/VEX, identidade do publicador e lifecycle da imagem-base.

**AI/ML e aceleradores:** treinamento e inferência dependem da combinação entre GPU/acelerador, driver, runtime de computação, framework, kernel e eventualmente imagem de container. A distribuição é uma camada dessa matriz, não a única variável.

**Edge:** em hosts próximos da origem dos dados ou usuários, arquitetura de CPU, recursos limitados, conectividade intermitente, atualização remota segura, rollback e capacidade de reconstrução podem pesar mais que variedade de pacotes desktop.

[Voltar ao índice](#indice)

---

<a id="14-rede-identidade-frota-observabilidade-e-continuidade"></a>

## 14. Rede, identidade, frota, observabilidade e continuidade

Um Linux pode executar inúmeras funções de infraestrutura, mas “é possível instalar o pacote” não significa que o sistema tenha o mesmo modelo operacional, suporte ou integração de um appliance/produto desenhado para aquela função.

<a id="14-rede-e-dataplane"></a>

### Rede e dataplane

Em redes, **control plane** decide/aprende como o tráfego deve ser tratado; **dataplane** (ou *forwarding plane*) executa o encaminhamento efetivo dos pacotes conforme essas regras.

Separe as capacidades:

- **NetworkManager** e **systemd-networkd** são exemplos de componentes usados para gerenciar interfaces e perfis de rede; a distribuição pode adotar um deles como padrão ou integrar outra camada;
- **configuração de interfaces**: endereços, rotas, **MTU (*Maximum Transmission Unit*, tamanho máximo da unidade transmitida sem fragmentação naquele enlace)**, DNS e parâmetros do link;
- **firewall/NAT (*Network Address Translation*)**: filtragem de tráfego e tradução de endereços/portas; **nftables** é um framework moderno do kernel Linux usado para filtragem, NAT e políticas relacionadas;
- **bridge**: comutação lógica de camada 2 entre interfaces;
- **bond**: agregação de múltiplas interfaces físicas para redundância e/ou distribuição de tráfego conforme o modo escolhido;
- **VLAN (*Virtual LAN*)**: segmentação lógica de camada 2 identificada por tags 802.1Q;
- **VRF (*Virtual Routing and Forwarding*)**: múltiplos contextos/tabelas de roteamento isolados no mesmo sistema;
- **VPN (*Virtual Private Network*)**: túnel/overlay que protege ou interliga tráfego sobre outra rede;
- **serviços fundamentais de rede**: IPv4/IPv6, resolução DNS, DHCP e sincronização de horário; Kerberos, certificados e vários fluxos de autenticação podem falhar quando DNS ou relógio estão incorretos;
- **roteamento dinâmico**: protocolos como **BGP (*Border Gateway Protocol*)** e **OSPF (*Open Shortest Path First*)** aprendem e/ou anunciam informações de roteamento em vez de depender somente de rotas estáticas;
- **EVPN (*Ethernet VPN*)**: control plane, normalmente baseado em BGP, usado para distribuir informações de alcance de endereços MAC/IP em redes de overlay;
- **VXLAN (*Virtual Extensible LAN*)**: encapsulamento de camada 2 sobre uma rede IP de camada 3; EVPN e VXLAN são frequentemente usados juntos, mas não são a mesma tecnologia;
- **SR-IOV (*Single Root I/O Virtualization*)**: permite que um dispositivo **PCIe (*PCI Express*)**, como uma **NIC (*Network Interface Card*, interface/placa de rede)**, exponha funções virtuais que podem ser atribuídas diretamente a máquinas virtuais ou outros consumidores, reduzindo a intervenção do host no dataplane;
- **DPDK (*Data Plane Development Kit*)**: bibliotecas e drivers para processamento de pacotes de alto desempenho em espaço de usuário, frequentemente reduzindo a dependência do caminho tradicional da pilha de rede do kernel;
- **XDP (*eXpress Data Path*)**: permite executar programas **eBPF** — tecnologia do kernel para carregar programas verificados em pontos controlados — muito cedo no caminho de recepção de pacotes, permitindo filtragem e processamento de alta velocidade;
- **switch NOS (*Network Operating System*)**: sistema operacional voltado à operação de switches; precisa ser validado junto ao **ASIC (*Application-Specific Integrated Circuit*)**, o circuito de hardware que realiza funções específicas de encaminhamento.

**FRR (*FRRouting*)** fornece protocolos de roteamento em Linux. Instalar FRR num servidor não transforma automaticamente esse host em appliance equivalente a VyOS nem em switch NOS equivalente a SONiC/Cumulus: hardware, integração, dataplane, gestão, lifecycle e suporte permanecem diferentes.

<a id="14-identidade"></a>

### Identidade

Os componentes de identidade resolvem funções diferentes:

- **AD (*Active Directory*)**: serviço de diretório e domínio da Microsoft, integrando identidade, autenticação e políticas do ecossistema Windows;
- **LDAP (*Lightweight Directory Access Protocol*)**: protocolo para consultar e modificar diretórios; LDAP não é, por si só, todo um sistema de autenticação/domínio;
- **Kerberos**: protocolo de autenticação baseado em tickets;
- **SSSD (*System Security Services Daemon*)**: integra identidades e autenticação de fontes remotas, cache e políticas no cliente Linux;
- **PAM (*Pluggable Authentication Modules*)**: framework que encadeia módulos de autenticação, conta, sessão e troca de credenciais para aplicações locais;
- **FreeIPA/IdM (*Identity Management*)**: plataforma de identidade e políticas para ambientes Linux, combinando diretório, Kerberos, certificados e outros serviços;
- **DNS (*Domain Name System*)**: resolução de nomes é parte crítica do desenho de AD, Kerberos e trusts;
- **GPO (*Group Policy Object*)**: objeto de política do Active Directory usado para aplicar configurações/regras a usuários e computadores Windows; ingressar um Linux no domínio não significa implementar toda a semântica de GPO do Windows.

Um trust AD–IdM exige desenho de DNS, Kerberos, nomes, ranges de IDs, sincronização temporal e políticas. **“Entrar no domínio” prova apenas parte da integração**, não gestão completa de frota.

<a id="14-provisionamento-e-frota"></a>

### Provisionamento e frota

Automatizar um host não equivale a governar centenas ou milhares.

- **PXE (*Preboot Execution Environment*)**: boot de instalação/provisionamento pela rede antes de existir um sistema operacional local completo;
- **cloud-init**: inicialização/configuração de instâncias a partir de metadados, muito comum em cloud;
- **zero-touch**: objetivo operacional de provisionar sem intervenção manual local depois que o equipamento é conectado/iniciado;
- **Ansible/Salt**: ferramentas de automação e gerenciamento de configuração;
- **GitOps**: prática em que o estado desejado é versionado em Git e automação/reconciliação aplica ou verifica esse estado no ambiente, preservando revisão e histórico de mudanças;
- **mirror**: cópia local/controlada de conteúdo ou repositórios upstream para reduzir dependência externa e controlar disponibilidade;
- **inventário/compliance**: saber o que existe e verificar se cada host continua aderente à política;
- **rollout gradual**: distribuir mudança em ondas, grupos ou percentuais antes de atingir toda a frota;
- **air gap**: ambiente sem conectividade direta com redes externas, exigindo processo explícito para importar conteúdo, chaves e atualizações;
- **console central**: ferramenta de inventário, política, atualização, evidência e suporte em escala.

Avalie também instalação desassistida, imagens, proxies, repositórios internos, rollback, segregação de ambientes e suporte multi-distribuição.

<a id="14-observabilidade"></a>

### Observabilidade

Sinais diferentes respondem a perguntas diferentes:

- **logs** registram eventos discretos e contexto;
- **métricas** representam valores numéricos ao longo do tempo;
- **traces** acompanham o caminho de uma transação/requisição por múltiplos componentes;
- **profiling** mede onde CPU, memória ou outros recursos são consumidos;
- **telemetria de segurança/rede** adiciona eventos e contexto específicos desses domínios.

Uma stack portátil baseada em OpenTelemetry ou Prometheus pode facilitar padronização entre distribuições, mas não possui necessariamente o mesmo grau de integração de uma plataforma nativa ou do fornecedor.

Observabilidade não é apenas coletar sinais. Defina também **SLI (*Service Level Indicator*)**, métricas que medem o comportamento relevante do serviço; **SLO (*Service Level Objective*)**, objetivos mensuráveis para esses indicadores; alertas acionáveis, retenção, cardinalidade aceitável e custo da telemetria.

<a id="14-continuidade"></a>

### Continuidade

Não confunda mecanismos de manutenção com disponibilidade do serviço:

- **live kernel patching**: aplica correções elegíveis ao kernel sem reboot imediato;
- **atualização de userspace sem reinício**: troca componentes fora do kernel, mas processos podem precisar ser reiniciados para carregar novas bibliotecas;
- **reboot coordenado**: reinicia nós em ordem controlada para preservar capacidade;
- **HA (*High Availability*)/failover**: arquitetura que mantém ou restaura o serviço transferindo função para outro nó/componente;
- **quorum**: regra para determinar se há membros/votos suficientes para uma decisão segura no cluster;
- **fencing**: isola ou desliga um nó considerado defeituoso para impedir escrita concorrente ou **split-brain**, situação em que partes isoladas de um cluster acreditam simultaneamente que devem operar como lado ativo;
- **rolling maintenance**: manutenção em lotes, preservando parte do serviço disponível;
- **live migration**: move uma máquina virtual em execução entre hosts compatíveis com interrupção mínima, quando suportado.

Disponibilidade é propriedade da arquitetura completa. Live patch não corrige banco single-node, storage sem redundância, quorum mal desenhado ou dependência externa única.

Também diferencie:

- **alta disponibilidade (HA)**: manter/restaurar serviço localmente diante de falhas;
- **tolerância a falhas**: continuar operando apesar de determinadas falhas sem perda do serviço esperado;
- **resiliência**: capacidade mais ampla de absorver falhas, degradar de forma controlada e recuperar-se;
- **recuperação de desastre (DR, *Disaster Recovery*)**: restaurar serviço/dados após evento severo que excede a operação normal de HA;
- **continuidade de negócio**: capacidade organizacional de manter funções críticas, incluindo pessoas, processos, dependências e tecnologia.

[Voltar ao índice](#indice)

---

<a id="15-metodo-de-decisao-auditavel"></a>

## 15. Método de decisão auditável

> **Decisão prática:** primeiro elimine candidatos que violam requisitos obrigatórios; só depois pontue preferências. Uma pontuação alta nunca deve “compensar” um requisito eliminatório não atendido.

<a id="15-etapa-1-descreva-o-workload"></a>

### Etapa 1 — descreva o workload

Registre:

- função principal e funções secundárias;
- ambiente: laptop, workstation, **bare metal** (host físico sem hipervisor abaixo do SO), VM, cloud, **edge** (processamento próximo à origem dos dados/usuários) ou appliance;
- criticidade, **RTO (*Recovery Time Objective*)** — tempo-alvo máximo para restaurar o serviço — e **RPO (*Recovery Point Objective*)** — perda máxima de dados tolerada, normalmente expressa como intervalo de tempo;
- escala e horizonte de uso;
- responsáveis por operação e suporte.

<a id="15-etapa-2-separe-requisitos"></a>

### Etapa 2 — separe requisitos

| Classe | Significado | Exemplo |
|---|---|---|
| Eliminatório | sem isso, o candidato não é aceitável | aplicação certificada na versão |
| Preferência | melhora a adequação, mas admite trade-off | desktop mais recente |
| Desconhecido | precisa de investigação | dock USB-C específico |
| Fora de escopo | não influencia esta decisão | preferência estética sem impacto |

Não transforme preferência em requisito obrigatório para “fazer o favorito ganhar”.

> **Regra para desconhecidos:** um requisito **eliminatório** sem evidência suficiente **não é considerado atendido**. O candidato fica **pendente de validação** e não deve ser aprovado para produção até que a evidência seja obtida. Para preferências desconhecidas, registre a incerteza e evite pontuar como se o resultado fosse conhecido.

<a id="15-etapa-3-elimine-incompativeis"></a>

### Etapa 3 — elimine incompatíveis

Exemplos legítimos:

- arquitetura de CPU sem suporte;
- lifecycle menor que o período de uso;
- certificação obrigatória ausente;
- driver ou aplicação crítica incompatível;
- modelo de atualização proibido pela política;
- appliance especializado fora do workload.

<a id="15-etapa-4-pontue-preferencias"></a>

### Etapa 4 — pontue preferências

Use pesos documentados. Uma escala simples:

| Peso | Interpretação |
|---:|---|
| 0 | indiferente |
| 1 | desejável |
| 2 | importante |
| 3 | muito importante |

O número final ordena aderência dentro daquele caso. Não é benchmark universal.

<a id="15-etapa-5-leia-trade-offs-e-alternativas"></a>

### Etapa 5 — leia trade-offs e alternativas

Uma recomendação madura sempre apresenta:

1. candidato principal;
2. versão ou canal;
3. motivos;
4. limitações;
5. duas alternativas;
6. hipótese que faria a decisão mudar.

<a id="15-etapa-6-valide-em-camadas"></a>

### Etapa 6 — valide em camadas

~~~text
documentação e matriz
        ↓
VM ou hardware de laboratório
        ↓
teste do workload e atualização
        ↓
piloto controlado
        ↓
rollout gradual com rollback
~~~

<a id="15-etapa-7-produza-um-registro-de-decisao"></a>

### Etapa 7 — produza um registro de decisão

Antes de registrar a conclusão, diferencie **tipo de evidência**:

| Tipo | Exemplo | Como interpretar |
|---|---|---|
| **Fato declarado** | lifecycle oficial publicado pelo projeto | afirmação documental da fonte |
| **Contrato formal** | certificação, SLA ou matriz ISV | obrigação/escopo formal conforme documento |
| **Comportamento observado** | teste no hardware/laboratório | evidência empírica válida para o cenário testado |
| **Inferência editorial** | “adequado como shortlist de workstation” | interpretação do guia; deve ser sustentada por critérios explícitos |

Uma inferência editorial não deve ser apresentada como declaração do fornecedor.

Inclua data, responsáveis, requisitos, candidatos eliminados, evidências, riscos aceitos, plano de saída e data de revisão. Para cada evidência relevante, registre também **fonte**, **data de verificação**, **prazo/data de revisão**, **nível de confiança** e **responsável pela validação**. Evidência envelhece: uma matriz oficial recente e específica tem peso diferente de uma postagem comunitária antiga ou de um teste informal.


Exemplo simplificado de registro preenchido:

| Campo | Exemplo |
|---|---|
| **Decisão** | Fedora KDE como workstation de desenvolvimento |
| **Data / revisão** | 2026-08-21 / revisar em 2027-02 |
| **Horizonte** | 3 anos, com upgrades planejados entre releases Fedora |
| **Eliminatórios** | GPU NVIDIA suportada; Podman/Docker; VPN corporativa; Secure Boot; IDE e toolchain exigidos |
| **Preferências** | KDE Plasma, pacotes recentes, boa integração com containers, documentação ampla |
| **Candidatos analisados** | Fedora KDE, Ubuntu LTS + KDE, openSUSE Tumbleweed |
| **Eliminados** | nenhum no papel; todos seguem para laboratório |
| **Evidências** | matriz da GPU, release notes, teste de suspensão/monitores, build do projeto, VPN, container e update em hardware real |
| **Governança da evidência** | fonte oficial quando disponível; verificada em 2026-08-21; revisão até 2026-11-21 para itens voláteis; confiança alta/média/baixa registrada por requisito; responsável identificado |
| **Riscos aceitos** | lifecycle curto exige upgrade periódico; atualização de kernel/driver precisa de validação após mudanças relevantes |
| **Alternativa** | Ubuntu LTS se o custo de upgrades frequentes superar o benefício de packages mais atuais |
| **Plano de saída** | backup independente, dados fora do host quando possível, automação de provisioning e procedimento testado de reinstalação/migração |

O exemplo não declara Fedora “melhor”; mostra como **requisitos, evidências e trade-offs** transformam uma preferência em decisão auditável.

Modelo mínimo para uma evidência/requisito crítico:

| Campo | Exemplo |
|---|---|
| ID | `HW-GPU-001` |
| Requisito | GPU oficialmente suportada no workload |
| Classe | eliminatório |
| Resultado | pendente / atendido / não atendido |
| Evidência | matriz do fornecedor + teste em hardware |
| Fonte | documento/URL oficial correspondente |
| Verificado em | 2026-08-21 |
| Revisar até | 2026-11-21 ou antes de mudança de release/driver |
| Confiança | alta / média / baixa, com justificativa |
| Escopo da evidência | produto/release/arquitetura/canal/componente aplicáveis |
| Responsável | pessoa/equipe que validou |

[Voltar ao índice](#indice)

---

<a id="16-perfis-de-referencia"></a>

## 16. Perfis de referência

Esta tabela orienta a triagem; não substitui os requisitos. **SLA (*Service Level Agreement*)** é o acordo de níveis de serviço/atendimento aplicável ao suporte; **ISV (*Independent Software Vendor*)** é o fornecedor independente de software cuja matriz pode certificar uma aplicação para determinadas combinações de distribuição, release e arquitetura.

| Cenário dominante | Exemplos que merecem investigação inicial | O que pode mudar a decisão | Não use esta linha como atalho quando... |
|---|---|---|---|
| Desktop simples, familiar ou migração do Windows | Linux Mint, Zorin OS, Ubuntu LTS | hardware, preferência de DE, formatos de aplicativos e lifecycle | houver aplicativo, periférico, política ou hardware obrigatório não homologado |
| Workstation moderna e produtividade | Fedora Workstation, Pop!_OS, Ubuntu, openSUSE Tumbleweed | GPU, toolchain, COSMIC/GNOME/KDE e mudança tolerada | o workload exigir lifecycle/certificação diferente ou estabilidade de plataforma mais longa |
| Gaming em desktop ou handheld | Bazzite, Nobara, Pop!_OS | GPU, Steam Gaming Mode, anti-cheat, dependência de X11 e modelo de atualização | o jogo/anti-cheat ou hardware crítico não tiver compatibilidade comprovada |
| Servidor comunitário conservador | Debian Stable, Ubuntu LTS | suporte comercial, certificação, stack | houver SLA, certificação ou suporte de ISV obrigatório ausente |
| Enterprise com suporte do fabricante e certificações formais | RHEL com assinatura e suporte empresarial, SLES, Ubuntu Pro, Oracle Linux com suporte comercial | SLA, ISV, cloud, matriz de certificação, compliance e contrato | produto/release/arquitetura/kernel/contrato exatos não estiverem no escopo exigido |
| Ecossistema RHEL sem contrato obrigatório | AlmaLinux, Rocky Linux, Oracle Linux sem suporte contratado | compatibilidade exigida, kernel, governança, suporte opcional, certificações e lifecycle | a organização depender de certificação ou obrigação contratual específica do fornecedor |
| Rolling com snapshots integrados | openSUSE Tumbleweed | hardware e tolerância operacional | a política proibir fluxo rolling ou a equipe não puder acompanhar manutenção contínua |
| Rolling derivada do Arch com instalação guiada | EndeavourOS, Manjaro | proximidade do Arch, curadoria de repositórios, AUR e responsabilidade de manutenção | o lifecycle/risco operacional exigido for incompatível com rolling |
| Controle manual e aprendizado profundo | Arch Linux | tempo disponível e responsabilidade de manutenção | o objetivo principal for reduzir intervenção, padronizar frota ou obter suporte formal |
| Desktop atômico | Fedora Atomic Desktops, Bazzite | workload geral ou gaming, compatibilidade com apps e fluxo de customização | ferramentas críticas dependerem de mutações do host incompatíveis com o modelo |
| Estado declarativo/reproduzível | NixOS | curva de aprendizado e ecossistema | equipe/automação não puder absorver o modelo declarativo e sua operação |
| Pentest autorizado | Kali, Parrot | escopo legal e especialidade | o workload real for desktop/servidor generalista sem necessidade das ferramentas especializadas |
| Hypervisor dedicado | Proxmox VE, Harvester | arquitetura de cluster, storage, suporte | a máquina precisar atuar como host Linux generalista em vez de appliance de virtualização |
| Kubernetes OS | Talos, Fedora CoreOS, Flatcar | provedor, API, lifecycle e operação | o host precisar de administração tradicional/interativa incompatível com o desenho do produto |
| **NAS (*Network Attached Storage*)** / appliance de storage | TrueNAS, Unraid | ZFS, apps, suporte, HA e escala | storage não for o papel principal ou integrações obrigatórias ficarem fora do produto |
| Router/firewall | VyOS, IPFire, OpenWrt | throughput, hardware, protocolos, suporte | hardware, aceleração, protocolos ou suporte não atenderem ao papel de rede |
| Switch fabric | SONiC, NVIDIA Cumulus Linux | ASIC e matriz de hardware suportado | o ASIC/plataforma não estiver explicitamente suportado pelo NOS escolhido |

<a id="16-como-interpretar-as-distribuicoes-desktop-destacadas"></a>

### Como interpretar as distribuições desktop destacadas

- **Linux Mint, Zorin OS e Pop!_OS** compartilham herança Ubuntu, mas não são equivalentes: variam em desktop, integração, calendário próprio, formatos habilitados, suporte de hardware e política de atualização.
- **EndeavourOS e Manjaro** são rolling e derivadas do ecossistema Arch, porém seguem políticas diferentes. EndeavourOS permanece mais próximo dos repositórios Arch; Manjaro mantém branches e curadoria próprias. A facilidade do instalador não elimina a responsabilidade operacional de uma rolling.
- **Bazzite** é uma imagem customizada baseada em Fedora Atomic, orientada especialmente a gaming, handhelds e HTPCs. Não deve ser descrita como “Arch rolling” nem operada como um Fedora tradicional baseado em `dnf`.
- A presença nesta tabela significa **exemplo plausível para investigação inicial**, não recomendação universal, ranking de qualidade nem prêmio de popularidade.

<a id="16-gratuito-nao-significa-rhel-identico-e-com-o-mesmo-servico"></a>

### “Gratuito” não significa “RHEL idêntico e com o mesmo serviço”

- **AlmaLinux** é livre, mantido por uma fundação sem fins lucrativos e busca [compatibilidade binária/ABI com RHEL](https://almalinux.org/blog/future-of-almalinux/). Desde 2023, o objetivo declarado não é copiar cada fonte e cada comportamento de RHEL de maneira estritamente idêntica; diferenças deliberadas devem ser verificadas nas release notes.
- **Rocky Linux** pertence ao mesmo espaço decisório de Enterprise Linux comunitário e, por simetria metodológica, deve ser avaliado junto com AlmaLinux. Governança, processo de build, fornecedores de suporte e certificações disponíveis continuam sendo critérios independentes.
- **Oracle Linux** é [gratuito para baixar, usar e redistribuir](https://www.oracle.com/linux/technologies/oracle-linux-downloads.html); suporte comercial é opcional. A distribuição mantém [compatibilidade de espaço de usuário com RHEL independentemente do kernel](https://docs.oracle.com/en/operating-systems/oracle-linux/10/relnotes10.2/ol-Compatibility.html). Em x86_64, oferece o **Red Hat Compatible Kernel (RHCK)** e o **Unbreakable Enterprise Kernel (UEK)**; em aarch64, Oracle Linux 10 oferece somente UEK. A escolha do kernel pode alterar suporte de hardware, recursos, operação e a matriz de certificação aplicável.
- **RHEL não é apenas “Linux pago”**: há [modalidades oficiais sem custo e autossuportadas](https://developers.redhat.com/articles/faqs-no-cost-red-hat-enterprise-linux) para indivíduos e desenvolvimento, sujeitas a termos e limites próprios. O que uma assinatura empresarial agrega é, entre outros itens, suporte do fornecedor, SLA, gestão de ciclo, termos de uso correspondentes e acesso ao ecossistema certificado. Portanto, compare o resultado operacional e contratual, não apenas o preço da mídia.

Quando certificação de aplicação ou hardware for requisito, valide **produto, major/minor release, arquitetura, kernel, ambiente de virtualização/cloud e modalidade de suporte** na matriz do respectivo fornecedor. Compatibilidade de ABI não transfere automaticamente uma certificação emitida para RHEL.

<a id="16-casos-que-nao-devem-ser-generalizados"></a>

### Casos que não devem ser generalizados

- Kali não é escolha padrão para aprender Linux ou operar servidor comum.
- Arch ISO possui data, mas o sistema é rolling.
- Um appliance pode liderar quando o workload é especializado e deve ser excluído fora dele.
- Uma distribuição empresarial sem contrato não oferece automaticamente o mesmo resultado operacional que uma assinatura ativa.

[Voltar ao índice](#indice)

---

<a id="17-recorte-temporal-e-versionamento"></a>

## 17. Recorte temporal e versionamento

Esta edição separa explicitamente datas com funções diferentes:

| Campo | Valor | Significado |
|---|---|---|
| **Snapshot factual** | **2026-08-15** | estado histórico usado para versões, canais, lifecycles e maturidade documentados nesta série |
| **Publicação/revisão da edição 0.1.6** | **2026-08-21** | data da revisão editorial/técnica desta edição |
| **Última validação estrutural interna** | **2026-08-21** | verificação de âncoras, links internos, fences, tabelas e invariantes documentais |
| **Última revisão semântica desta edição** | **2026-08-21** | conferência de coerência entre seção, tabela, glossário e finalidade dos itens alterados |

> **Verifique sempre na fonte primária:** o snapshot existe para reproduzir o que o projeto afirmava em **15 de agosto de 2026**. Uma instalação futura deve usar a documentação vigente do canal/release exatos, inclusive estado de suporte, arquitetura e maturidade. A data da edição **não atualiza automaticamente** os fatos preservados no snapshot.

<a id="17-politica-de-versionamento-da-serie-0"></a>

### Política de versionamento da série 0

A edição **0.1.0** é a linha de base da primeira publicação pública. Versões internas anteriores não integram o histórico público.

A numeração adapta a convenção do [Semantic Versioning 2.0.0](https://semver.org/lang/pt-BR/): a major zero representa desenvolvimento inicial, e `0.1.0` é o ponto de partida recomendado para a primeira edição dessa fase. **Esta é uma convenção editorial interna do projeto; não implica compatibilidade formal com todos os significados de Semantic Versioning aplicados a APIs de software.**

Durante a série `0.x`, o projeto adota a seguinte convenção:

- `0.1.1`, `0.1.2` e demais patches: correções factuais, editoriais ou de implementação que não alterem intencionalmente o contrato decisório;
- `0.2.0`, `0.3.0` e demais minors: novas perguntas, candidatos, capacidades ou mudanças de peso e lógica que possam alterar recomendações;
- `1.0.0`: primeira edição considerada estável quanto ao contrato editorial, à rastreabilidade dos dados e aos testes de regressão do recomendador.

Uma correção de segurança ou lifecycle deve ser publicada assim que validada, sem aguardar uma versão minor. Quando uma correção puder alterar o resultado do quiz, o changelog deve declarar esse impacto explicitamente.

| Projeto | Estado considerado no snapshot | Fonte primária |
|---|---|---|
| Debian | 13.6, família 13 “trixie” | [anúncio Debian 13.6](https://www.debian.org/News/2026/20260711) |
| Ubuntu | 26.04 LTS, suporte padrão até abr/2031 | [release notes 26.04](https://documentation.ubuntu.com/release-notes/26.04/) |
| Linux Mint | 22.3 “Zena”, série 22 suportada até 2029 | [release Linux Mint 22.3](https://linuxmint.com/rel_zena.php) |
| Zorin OS | 18.1, base Ubuntu 24.04 LTS, suporte até 1º/jun/2029 | [detalhes técnicos do Zorin OS](https://zorin.com/os/details/) |
| Pop!_OS | 24.04 LTS com COSMIC Desktop Environment Epoch 1 | [anúncio do Pop!_OS 24.04 LTS](https://blog.system76.com/post/pop-os-letter-from-our-founder) |
| Fedora | 44; lifecycle Fedora de cerca de 13 meses | [lifecycle Fedora](https://docs.fedoraproject.org/en-US/releases/lifecycle/) |
| Bazzite | canal `stable`, image-based sobre Fedora Atomic; não possui uma única versão convencional para todas as imagens | [FAQ e variantes Bazzite](https://docs.bazzite.gg/General/FAQ/) |
| RHEL | 10.2 | [datas de releases RHEL](https://access.redhat.com/articles/red-hat-enterprise-linux-release-dates) |
| AlmaLinux | 10.2; suporte ativo da família 10 até 31/mai/2030 e de segurança até 31/mai/2035 | [release notes e lifecycle do AlmaLinux](https://wiki.almalinux.org/release-notes/) |
| Rocky Linux | 10.2; suporte geral da família 10 até 31/mai/2030 e de segurança até 31/mai/2035 | [releases e lifecycle do Rocky Linux 10](https://docs.rockylinux.org/10/releases/) |
| Oracle Linux | 10.2; Premier Support da família 10 até jun/2035, Extended Support até jun/2038 e Sustaining Support por prazo indeterminado, com coberturas distintas conforme a política vigente | [documentação do Oracle Linux 10](https://docs.oracle.com/en/operating-systems/oracle-linux/10/) e [Lifetime Support Policy](https://www.oracle.com/a/ocom/docs/elsp-lifetime-069338.pdf) |
| openSUSE Leap | 16.0, manutenção de 24 meses por minor | [anúncio Leap 16.0](https://news.opensuse.org/2025/10/01/next-chapter-opens-with-leap-release/) |
| NixOS | 26.05 “Yarara” | [anúncio NixOS 26.05](https://nixos.org/blog/announcements/2026/nixos-2605/) |
| Kali | 2026.2, rolling | [anúncio Kali 2026.2](https://www.kali.org/blog/kali-linux-2026-2-release/) |
| Arch | ISO 2026.08.01; instalação permanece rolling | [downloads Arch](https://archlinux.org/download/) |
| EndeavourOS | rolling; Titan Neo é snapshot de instalação de 1º/mai/2026, não uma versão para a qual o sistema instalado precise migrar | [anúncio Titan Neo](https://endeavouros.com/news/titan-neo-with-some-fixes-and-upstream-updates-is-available/) |
| Manjaro | 26.1 “Bian-May” como mídia de instalação; sistema permanece rolling com curadoria própria | [anúncio Manjaro 26.1](https://forum.manjaro.org/t/manjaro-26-1-bian-may-released/189526) |
| Proxmox VE | 9.2 | [anúncio Proxmox VE 9.2](https://proxmox.com/en/about/company-details/press-releases/proxmox-virtual-environment-9-2) |
| TrueNAS | 25.10.6 indicada como estável | [status de software TrueNAS](https://www.truenas.com/docs/softwarestatus/) |
| Unraid | 7.3.2 | [release notes Unraid 7.3.2](https://docs.unraid.net/unraid-os/release-notes/7.3.2/) |
| OpenWrt | 25.12.5 | [release notes OpenWrt 25.12.5](https://openwrt.org/releases/25.12/notes-25.12.5) |
| Talos Linux | 1.13 | [support matrix Talos](https://docs.siderolabs.com/talos/v1.13/getting-started/support-matrix) |

> **Nota de fonte — Ubuntu 26.04 LTS:** as [release notes específicas da 26.04](https://documentation.ubuntu.com/release-notes/26.04/) indicam suporte até **abril de 2031**, enquanto a [página agregada de lifecycle da Canonical](https://ubuntu.com/about/release-cycle?product=ubuntu&release=ubuntu&version=26.04+LTS) atualmente apresenta **maio de 2031** para a manutenção padrão. Esta edição preserva abril de 2031 por priorizar a documentação específica da release no snapshot; para planejamento operacional futuro, confirme a página de lifecycle vigente na data da implantação.

<a id="17-duas-correcoes-temporais-importantes"></a>

### Correções temporais e estados de maturidade importantes

- Em **15/08/2026**, a release estável do **TUXEDO OS** ainda pertence à linha Ubuntu-based. O projeto já havia anunciado que a futura geração usaria **Debian Testing permanentemente** e chamaria esse desenho de **Continuous Debian**, preservando um modelo híbrido. Anúncio, beta e release estável devem ser tratados como estados diferentes; não reclassifique retroativamente a release estável do snapshot. Consulte o [anúncio oficial da nova base](https://www.tuxedocomputers.com/en/A-new-foundation-for-TUXEDO-OS-Switching-to-Debian.tuxedo).
- O portal oficial da openSUSE identifica **Slowroll como beta** e **Aeon como beta**. Esses estados de maturidade são diferentes de Tumbleweed (rolling) e Leap (stable) e devem ser confirmados novamente antes de recomendar produção. Consulte o [portal de distribuições openSUSE](https://en.opensuse.org/Portal:Distribution).
- No **Google Container-Optimized OS**, o milestone 133 está em beta no snapshot; o catálogo usa 129 LTS para recomendação estável. Consulte as [release notes oficiais do COS](https://docs.cloud.google.com/container-optimized-os/docs/release-notes).

<a id="17-politica-de-leitura"></a>

### Política de leitura

- Uma data de ISO não é automaticamente a versão de uma rolling.
- “Current”, “stable” e “LTS” precisam ser lidos no canal correto.
- O snapshot preserva auditabilidade histórica; a página oficial decide uma instalação futura.
- Versões no quiz devem ser atualizadas junto com fontes, testes e changelog.

[Voltar ao índice](#indice)

---

<a id="18-operacoes-basicas-de-atualizacao"></a>

## 18. Operações básicas de atualização

> **Exemplos educacionais.** Execute somente comandos correspondentes à distribuição/release instalada. `sudo` solicita execução com privilégios administrativos e exige que a conta esteja autorizada. Antes de atualizar, confirme suporte da release, origem dos repositórios, energia estável, espaço livre e um método testado de recuperação. Não interrompa deliberadamente uma transação em andamento sem conhecer o mecanismo específico.

Uma inspeção simples de espaço pode começar por:

~~~bash
df -h
~~~

Isso **não é suficiente sozinho**: em layouts separados, verifique também `/boot`, `/var`, volumes de snapshots/deployments e requisitos específicos do gerenciador. Leia release notes e avisos do projeto antes de upgrades de geração.

<a id="18-debian-ubuntu-e-derivados"></a>

### Debian, Ubuntu e derivados

Atualize os metadados, revise o que está disponível e só então aplique a atualização:

~~~bash
sudo apt update
apt list --upgradable
sudo apt upgrade
~~~

`apt` é apropriado para uso interativo; automações devem avaliar interfaces e códigos de saída próprios para scripting, frequentemente usando `apt-get` conforme a documentação aplicável. `apt list --upgradable` é uma consulta informativa e sua apresentação não deve ser tratada como interface estável de máquina.

<code>apt full-upgrade</code> pode remover ou substituir pacotes para resolver dependências. Use quando a documentação do projeto indicar e revise o plano antes de confirmar. Upgrade entre releases possui procedimento próprio; não é sinônimo de executar esse comando às cegas.

<a id="18-fedora-e-rhel"></a>

### Fedora e RHEL

Inspeção e atualização:

~~~bash
dnf check-update
sudo dnf upgrade
~~~

`dnf check-update` retorna **código de saída (*exit code*) 100** quando existem atualizações disponíveis; isso é comportamento normal, não erro. Em scripts que abortam em qualquer retorno não zero — por exemplo, shells usando `set -e` — esse comportamento precisa ser tratado explicitamente. Retorno `0` significa que não há updates aplicáveis e `1` indica erro. Mudança de major release ou operação offline deve seguir a documentação da versão e do produto.

<a id="18-opensuse"></a>

### openSUSE

Leap, para listar updates e realizar atualização normal:

~~~bash
zypper list-updates
sudo zypper update
~~~

Tumbleweed, para consultar e depois sincronizar corretamente a distribuição com o snapshot atual:

~~~bash
zypper list-updates
sudo zypper dup
~~~

<a id="18-arch-linux"></a>

### Arch Linux

Para apenas listar atualizações com segurança, use `checkupdates`, fornecido pelo pacote `pacman-contrib`, quando instalado:

~~~bash
checkupdates
sudo pacman -Syu
~~~

Arch não suporta *partial upgrades*. **Não use `pacman -Sy` isoladamente para “ver se há updates”**: sincronizar a base sem concluir uma atualização completa pode criar um estado de partial upgrade. `checkupdates` usa uma base separada justamente para consultar pendências sem alterar a base principal do pacman. Leia também os avisos do projeto antes de intervenções que exigem ação manual.

<a id="18-fedora-atomic-desktops"></a>

### Fedora Atomic Desktops

~~~bash
rpm-ostree status
rpm-ostree upgrade
rpm-ostree rollback
~~~

O `rpm-ostree upgrade` normalmente prepara um **novo deployment**; o sistema em execução pode continuar no deployment atual até o próximo reboot. `rollback` seleciona deployment anterior quando ele está disponível; dados mutáveis permanecem fora dessa garantia.

<a id="18-suse-linux-micro-opensuse-microos"></a>

### SUSE Linux Micro e openSUSE MicroOS

Em sistemas que usam `transactional-update`, uma atualização do filesystem raiz é preparada em um novo snapshot e só passa a afetar o sistema em execução após reboot.

~~~bash
sudo transactional-update
sudo reboot
~~~

Executar `transactional-update` sem argumentos solicita uma atualização do sistema. O mecanismo cria um snapshot separado, aplica nele as mudanças e, se a transação concluir, marca o novo snapshot como padrão para o próximo boot. `/var` não faz parte desse snapshot, portanto rollback do root filesystem **não equivale a restaurar todos os dados mutáveis**.

> **Aeon:** embora compartilhe tecnologias transacionais do ecossistema MicroOS, o desktop Aeon é orientado a manutenção automatizada e continua identificado como beta pelo projeto. Não generalize um procedimento manual de MicroOS/SUSE Linux Micro como rotina recomendada para Aeon sem consultar a documentação específica da edição.

<a id="18-nixos-com-channels"></a>

### NixOS com channels

~~~bash
sudo nixos-rebuild switch --upgrade
~~~

`--upgrade` atualiza o channel de sistema chamado `nixos` antes do rebuild; portanto, nesse fluxo, executar `nix-channel --update nixos` separadamente seria redundante. Se houver outros channels com aliases diferentes, eles exigem tratamento explícito.

<a id="18-nixos-com-flakes"></a>

### NixOS com flakes

~~~bash
nix flake update
nix flake check
sudo nixos-rebuild switch --flake ".#meu-host"
~~~

Substitua `meu-host` pelo nome realmente definido em `nixosConfigurations` no `flake.nix`. <code>nixos-rebuild switch</code> sozinho reconstrói a partir das entradas já fixadas; não atualiza automaticamente todas as origens de um flake.

[Voltar ao índice](#indice)

---

<a id="19-metodologia-do-linux-distro-advisor"></a>

## 19. Metodologia do Linux Distro Advisor

> **Essencial:** o quiz é uma ferramenta de **triagem e ordenação de aderência**. O score não é probabilidade, benchmark, certificação nem prova de que a primeira colocada é adequada para produção.

> **Transparência documental:** nesta série, o Advisor deve ser descrito como **heurística documentada e versionada**. O README explica pipeline, categorias e limites, mas **não contém sozinho todos os pesos, regras cruzadas e detalhes numéricos suficientes para uma implementação independente reproduzir cada score**. A auditabilidade integral é um objetivo arquitetural para a próxima série minor, com fonte de dados única e testes reproduzíveis.

<a id="19-o-que-o-catalogo-contem"></a>

### O que o catálogo contém

A edição **0.1.6** mantém **95 candidatos**, não “95 distribuições puras”. O conjunto inclui distribuições, edições, canais, sistemas image-based, network operating systems e appliances Linux.

Categorias de elegibilidade:

- **Elegível geral:** participa quando cumpre os requisitos.
- **Elegível por contexto:** só concorre no workload, hardware ou provedor associado.
- **Somente catálogo:** documentado, mas fora do ranking padrão.

Esses nomes descrevem comportamento do algoritmo, não endosso editorial.

<a id="19-modos"></a>

### Modos

- **Triagem:** percorre um subconjunto adaptativo dos critérios. Serve para estudo, desktop pessoal e formação de shortlist.
- **Análise especialista:** permite até 70 etapas condicionais para ambientes complexos.

Perguntas ocultas não contam como respondidas. Ao voltar e mudar uma decisão, respostas que deixam de ser válidas são removidas antes da pontuação.

<a id="19-pipeline"></a>

### Pipeline

~~~text
respostas
   ↓
normalização e remoção de respostas ocultas
   ↓
elegibilidade por contexto
   ↓
requisitos eliminatórios
   ↓
preferências ponderadas e regras cruzadas
   ↓
ranking relativo + motivos + trade-offs
~~~

Se nenhum candidato atender todos os requisitos eliminatórios, o quiz declara conflito e mostra a melhor aproximação entre os candidatos que continuam pertinentes ao contexto. Entradas somente catálogo e appliances de outro workload não podem “vencer” esse fallback.

<a id="19-interpretacao-do-resultado"></a>

### Interpretação do resultado

O índice:

- mede aderência relativa às respostas;
- não é probabilidade;
- não mede qualidade absoluta;
- não compara desempenho;
- não substitui benchmark;
- pode mudar quando um requisito ou o snapshot muda.

A interface apresenta separação percentual entre os primeiros colocados, candidatos excluídos por contexto e candidatos eliminados por requisito.

<a id="19-privacidade-e-execucao"></a>

### Privacidade e execução

O arquivo é standalone:

~~~text
quiz-linux.html
├── HTML
├── CSS
└── JavaScript
~~~

Não requer backend, cadastro, npm, framework ou CDN. **A propriedade de não transmitir respostas depende da implementação exata de `quiz-linux.html` e deve ser revalidada sempre que HTML/JavaScript ou dependências forem modificados.** O README, isoladamente, não prova ausência de chamadas de rede. Preferência de tema pode ser armazenada localmente quando o navegador permite; o quiz continua funcional se o armazenamento estiver bloqueado.

<a id="19-limitacoes-conhecidas"></a>

### Limitações conhecidas

- o catálogo é curado, não exaustivo;
- pesos são heurísticos e versionados;
- suporte real varia por arquitetura, região, contrato e hardware;
- o quiz não executa detecção de hardware;
- certificações precisam ser verificadas no documento formal;
- window managers independentes não são modelados;
- um candidato pode mudar de tier em edição futura;
- fontes e versões envelhecem após o snapshot.

[Voltar ao índice](#indice)

---

<a id="20-checklist-de-homologacao"></a>

## 20. Checklist de homologação

<a id="20-antes-do-laboratorio"></a>

### Antes do laboratório

- [ ] workload e proprietário definidos;
- [ ] requisitos eliminatórios separados de preferências;
- [ ] lifecycle e EOL documentados;
- [ ] repositórios e escopo de segurança confirmados;
- [ ] matriz de hardware, software e ISV verificada;
- [ ] termos de suporte e licença avaliados;
- [ ] arquitetura de backup e recuperação desenhada.

<a id="20-no-laboratorio"></a>

### No laboratório

- [ ] instalação ou provisioning reproduzível;
- [ ] boot, rede, storage, GPU e periféricos testados;
- [ ] aplicação crítica e integrações validadas;
- [ ] política MAC em modo efetivo;
- [ ] patching e reboot ensaiados;
- [ ] upgrade de release ou snapshot simulado;
- [ ] rollback e restore executados de verdade;
- [ ] logs, métricas e alertas integrados;
- [ ] desempenho e capacidade medidos com carga representativa.

<a id="20-antes-de-producao"></a>

### Antes de produção

- [ ] riscos e exceções aprovados;
- [ ] runbook e ownership definidos;
- [ ] rollout gradual e critérios de interrupção;
- [ ] canal de segurança monitorado;
- [ ] capacidade de reconstrução confirmada;
- [ ] plano de saída e próxima revisão agendados.

[Voltar ao índice](#indice)

---

<a id="21-erros-conceituais-comuns"></a>

## 21. Erros conceituais comuns

| Afirmação | Correção |
|---|---|
| Rolling é instável | Rolling descreve entrega contínua; qualidade depende do processo e da operação. |
| LTS é o oposto de rolling | LTS é lifecycle; rolling é release. |
| Stable significa sem bugs | Stable é contextual e não elimina defeitos. |
| Versão antiga é vulnerável | Verifique advisory e patch downstream, não apenas número upstream. |
| Imutável significa inalterável | O conteúdo protegido do SO muda por caminhos controlados; dados e áreas persistentes continuam mutáveis. |
| Atômico e imutável são iguais | Atomicidade descreve como uma mudança é ativada; imutabilidade/read-only descreve quais partes podem ser modificadas diretamente. |
| Container é uma VM leve | Container de aplicação normalmente compartilha o kernel do host por namespaces/cgroups; VM normalmente executa kernel próprio sobre hardware virtualizado. Isolamento, boot e superfície de ataque são diferentes. |
| Funcionar na mídia live garante que funcionará instalado | A mídia live é um teste útil, mas instalação, kernel/driver escolhido, Secure Boot, persistência, suspensão e atualizações podem produzir comportamento diferente. |
| Imagem assinada significa imagem segura | Assinatura valida integridade/autenticidade conforme a chave/política; não prova ausência de vulnerabilidades, configuração segura ou adequação ao workload. |
| Ter SBOM significa ausência de vulnerabilidades | SBOM inventaria componentes; não demonstra sozinho impacto, explorabilidade, correção ou estado de suporte. |
| Rootless significa sem risco | Rootless reduz privilégios e impacto de classes de falha, mas kernel, runtime, imagem, configuração e dados continuam formando superfície de ataque. |
| Criptografia substitui controle de acesso | Criptografia protege determinados dados/chaves em condições específicas; permissões, autenticação, MAC e segregação continuam necessários. |
| Backup concluído significa restauração garantida | Só um teste de restauração comprova que dados, chaves, dependências e procedimentos recuperam o estado necessário. |
| Snapshot é backup | Pode compartilhar o mesmo domínio de falha e não ser consistente com o estado interno da aplicação; snapshot e backup possuem escopo, retenção e independência diferentes. |
| RAID é backup | RAID melhora disponibilidade diante de algumas falhas; não substitui cópia independente. |
| Secure Boot criptografa o disco | Ele verifica a cadeia de boot; criptografia é outro controle. |
| FIPS mode significa certificado | Certificação tem escopo formal e específico. |
| SELinux instalado significa protegido | Modo, política e cobertura precisam ser confirmados. |
| APT é o formato do pacote | APT resolve e gerencia; deb é o formato; dpkg é a camada de baixo nível. |
| Flatpak sempre é mais seguro | Sandbox e permissões ajudam, mas origem, runtime e manutenção continuam relevantes. |
| Atomic significa Rolling Release | Atomic descreve a ativação da atualização; Rolling descreve como releases evoluem ao longo do tempo. Fedora Silverblue é Fixed + Atomic. |
| Entrar no AD significa aplicar toda GPO | Autenticação e gestão de políticas são capacidades diferentes. |
| Live patch elimina reboots | Apenas mudanças elegíveis são cobertas. |
| LTS significa kernel antigo | LTS descreve uma política de manutenção de um objeto específico; identifique se o termo se refere à release, kernel, componente ou serviço. |
| Imagem OCI assinada é segura | Assinatura/digest ajudam a verificar identidade/integridade segundo a política adotada; não provam ausência de vulnerabilidades nem adequação. |
| Declarativo significa reproduzível | Declaratividade descreve o estado desejado; reproduzibilidade exige controlar versões, inputs, artefatos e outras dependências relevantes. |
| O maior score é a melhor distro | O score só ordena aderência ao perfil informado. |

[Voltar ao índice](#indice)

---

<a id="22-governanca-editorial-publicacao-e-licenciamento"></a>

## 22. Governança editorial, publicação e licenciamento

<a id="22-estrutura-canonica"></a>

### Estrutura canônica

~~~text
linux-reference-br/
├── README.md
├── quiz-linux.html
└── LICENSE
~~~

O README é a fonte canônica dos conceitos, critérios e limites. O HTML implementa o questionário e carrega o catálogo factual detalhado. Mudanças que alterem a decisão devem atualizar ambos.

<a id="22-publicacao"></a>

### Publicação

O README pode ser publicado diretamente num repositório. Para executar o quiz no navegador, publique a raiz com GitHub Pages ou serviço estático equivalente. A interface comum do GitHub pode exibir o HTML como código em vez de executá-lo.

Checklist de release:

> **Invariantes recomendados para automação/CI:** o número de candidatos documentados deve coincidir com o catálogo usado pelo quiz; cada candidato deve possuir identificador único; classificações compartilhadas entre README e quiz não devem divergir; links/âncoras precisam ser válidos; mudanças factuais devem registrar fonte e data.

1. atualizar snapshot e fontes primárias quando a edição alterar dados factuais;
2. executar **auditoria estrutural**: âncoras, links internos, fences, tabelas e invariantes de contagem/schema;
3. executar **auditoria semântica**: cada linha/termo deve pertencer à seção correta e preservar o significado canônico;
4. rodar cenários determinísticos do recomendador;
5. testar teclado, mobile, impressão e tema;
6. revisar links internos e externos;
7. registrar mudanças de pesos, candidatos e fatos voláteis;
8. criar tag da edição.

> **Lição da 0.1.5:** sintaxe válida não garante conteúdo correto. A auditoria semântica foi adicionada explicitamente após a detecção de verbetes de glossário inseridos por engano na tabela de storage da 0.1.4.

<a id="idioma-e-convencoes"></a>

### Idioma e convenções

- idioma editorial: português do Brasil;
- nomes oficiais de produtos são preservados;
- termos técnicos em inglês recebem definição funcional no primeiro uso quando forem necessários à compreensão;
- siglas são expandidas no primeiro uso e acompanhadas de uma definição curta quando o nome por extenso não for autoexplicativo;
- categorias editoriais devem declarar a dimensão a que pertencem para evitar misturar release, lifecycle, atualidade, mutabilidade e mecanismo de atualização;
- datas usam ISO 8601 em metadados;
- comandos e nomes de arquivo permanecem no formato original;

<a id="22-licenciamento"></a>

### Licenciamento

O projeto adota licenciamento duplo para separar corretamente documentação e software:

| Escopo | Licença | Efeito principal |
|---|---|---|
| README, explicações, tabelas, descrições autorais e conteúdo educacional | [Creative Commons Attribution 4.0 International — CC BY 4.0](./LICENSE) | permite compartilhar e adaptar, inclusive comercialmente, desde que a autoria, a licença e as alterações sejam informadas |
| Código HTML, CSS e JavaScript do quiz | [MIT License](./LICENSE) | permite usar, copiar, modificar, distribuir e incorporar o código, preservando o aviso de copyright e a licença |

Nos arquivos mistos, a licença MIT cobre a implementação de software; a CC BY 4.0 cobre o texto editorial e as descrições autorais incorporadas. Nomes de projetos, marcas, logotipos, trechos de terceiros e conteúdos apenas referenciados permanecem sujeitos aos respectivos titulares e licenças.

Forma de atribuição sugerida para o conteúdo:

> Referência Técnica para Escolha de Distribuições Linux, por Diego (Diego-Ch4m4X), licenciada sob CC BY 4.0, com indicação da edição utilizada e das alterações realizadas.

Os termos completos e a delimitação de escopo estão no arquivo [`LICENSE`](./LICENSE).

<a id="22-contribuicoes"></a>

### Contribuições

Uma alteração factual deve informar:

- fonte primária;
- data de acesso;
- versão/canal/arquitetura;
- impacto no catálogo ou no score;
- cenário de teste;
- distinção entre fato e inferência.

Preferências pessoais sem requisito verificável não são critério de ranking.

[Voltar ao índice](#indice)

---

<a id="23-glossario"></a>

## 23. Glossário

O glossário reúne conceitos recorrentes necessários para interpretar mais de uma seção. Tecnologias citadas apenas pontualmente são definidas no primeiro uso e não precisam, necessariamente, de uma entrada própria aqui.

| Termo | Definição |
|---|---|
| A/B update | modelo de atualização que mantém dois slots/estados (A e B): o sistema executa um enquanto prepara o outro, alternando o estado de boot após a atualização. A implementação e o escopo do rollback variam por produto. |
| aarch64 | arquitetura de conjunto de instruções ARM de 64 bits, também chamada ARM64. Suporte real depende do SoC, boot, firmware, kernel, drivers e software publicado para a plataforma. |
| ABI | *Application Binary Interface*: contrato que permite a um binário interagir com bibliotecas, kernel ou outros componentes sem recompilação, dentro das compatibilidades declaradas. |
| AD | *Active Directory*: serviço de diretório/domínio da Microsoft que integra identidade, autenticação, computadores e políticas no ecossistema Windows. |
| Air gap | isolamento de uma rede/ambiente sem conectividade direta com redes externas. Atualizações, chaves e artefatos precisam entrar por processo controlado. |
| API | *Application Programming Interface*: contrato/interface por meio do qual um componente expõe funcionalidades ou dados para outro; pode envolver funções, chamadas em runtime, estruturas, mensagens, protocolos ou endpoints. Compatibilidade de API não garante, por si só, compatibilidade ABI. |
| Application-consistent | estado de snapshot/backup capturado em coordenação com a aplicação para preservar consistência lógica suficiente para recuperação, não apenas consistência do filesystem/bloco. |
| ASIC | *Application-Specific Integrated Circuit*: circuito integrado desenvolvido para funções específicas; em switches, normalmente executa encaminhamento de pacotes em hardware. |
| Atomic update | atualização cuja ativação ocorre como uma unidade no ponto de compromisso: o consumidor observa o estado anterior ou o novo estado completo, não um estado parcialmente ativado. Rollback, preservação do estado anterior e recuperação são capacidades adicionais. |
| Atualidade dos componentes | grau de proximidade das versões empacotadas em relação aos projetos upstream (*software freshness*). Pode variar por kernel, biblioteca, desktop, driver ou aplicação e não constitui, por si só, medida de segurança. |
| Backport | adaptação de uma correção ou funcionalidade mais nova para uma versão anterior ainda mantida, normalmente sem importar toda a versão upstream. |
| Backup | cópia de dados/estado mantida com independência suficiente para recuperação diante dos cenários de falha definidos. Backup só é confiável quando o processo de restauração é testado. |
| Bleeding edge | rótulo informal para software extremamente próximo do desenvolvimento upstream, com maior novidade e maior exposição a regressões. Não é sinônimo obrigatório de rolling release. |
| Bootloader | componente que localiza e inicia o kernel ou um deployment do sistema, como GRUB ou systemd-boot. |
| Branch | linha nomeada de desenvolvimento, manutenção ou promoção dentro de um projeto ou repositório. Pode representar estados/canais como `stable`, `testing`, `unstable`, `Rawhide`, `edge` ou `-current`. Uma branch pode ter regras próprias de migração, freeze e suporte; seu nome não determina sozinho modelo de release, estabilidade operacional ou lifecycle, e o significado é específico de cada projeto. |
| Cadência | ritmo ou frequência com que releases, pacotes, snapshots ou imagens são publicados ou promovidos para um canal. Cadência diária, semanal ou semestral não determina sozinha se o sistema é Fixed ou Rolling. |
| Certificação | avaliação formal de produto, versão, configuração e escopo contra um programa ou padrão definido. Não é sinônimo de hardening nem de compliance contínuo. |
| Channel / canal | linha nomeada de entrega consumida pelo usuário, como `stable`, `testing`, `beta` ou `next`. O significado é específico de cada projeto e pode representar imagem, pacote ou release. |
| CIS Benchmarks | recomendações consensuais de configuração segura publicadas pelo Center for Internet Security para sistemas operacionais, cloud, containers, aplicações e outros produtos. |
| Compliance | conformidade demonstrável com controles, políticas ou normas aplicáveis ao ambiente. Depende de configuração, evidências e operação, não apenas do nome da distribuição. |
| Compositor | componente que combina superfícies de aplicações e produz a imagem final da tela. Em Wayland, normalmente também exerce o papel de display server e pode incorporar o gerenciamento de janelas. |
| Control plane | conjunto de funções que calcula, decide ou programa como os recursos devem operar; em redes, instrui o dataplane sobre como encaminhar tráfego. |
| Copy-on-write (CoW) | estratégia em que uma alteração é gravada em novos blocos antes de atualizar referências, facilitando snapshots e compartilhamento eficiente, com custos operacionais que dependem do workload. |
| CNI | *Container Network Interface*: especificação/ecossistema usado para integrar rede de pods/containers em plataformas como Kubernetes. |
| CRI | *Container Runtime Interface*: API usada pelo kubelet para interoperar com runtimes de containers compatíveis. |
| CSI | *Container Storage Interface*: especificação para integração de sistemas de storage com orquestradores como Kubernetes. |
| Curated Rolling | Rolling Release em que o projeto intercala testes, promoção ou retenção deliberada de mudanças antes de entregá-las ao usuário final, sem deixar de operar como fluxo rolling. |
| CVE | identificador público para uma vulnerabilidade conhecida; o identificador não informa sozinho severidade, explorabilidade nem se o pacote downstream continua vulnerável. |
| CVSS | *Common Vulnerability Scoring System*: método padronizado de pontuação de severidade técnica. Ajuda a priorizar, mas não substitui contexto de exposição, explorabilidade, criticidade do ativo, KEV ou advisory do fornecedor. |
| Dataplane | também chamado *data plane* ou *forwarding plane*: caminho que processa e encaminha pacotes ou dados conforme regras programadas pelo control plane. |
| Deployment | estado instalável ou bootável do conteúdo do sistema operacional controlado por um mecanismo de deployment. Em sistemas atômicos, estados anterior e novo podem coexistir para seleção no boot ou rollback; dados mutáveis fora do escopo do deployment não são necessariamente revertidos. |
| Digest | identificador derivado criptograficamente do conteúdo de um artefato, usado para referenciar exatamente uma imagem/objeto quando o ecossistema suporta esse modelo. Digest comprova identidade do conteúdo referenciado, não segurança do conteúdo. |
| Desktop environment (DE) | conjunto integrado de shell, sessão, painel, configurações, aplicativos e componentes gráficos, como GNOME, KDE Plasma, Cinnamon ou COSMIC. |
| Display manager | serviço de login gráfico que autentica o usuário e inicia uma sessão, como GDM, SDDM ou LightDM. Não é o display server nem o desktop environment. |
| Display server | componente que coordena apresentação gráfica e entrada entre aplicações e sessão. No X11 esse papel costuma ser do X.Org Server; em Wayland, o compositor exerce esse papel. |
| Distribuição Linux | produto/projeto que integra kernel Linux, componentes de userspace, ferramentas, repositórios ou imagens, políticas de atualização e documentação para formar uma plataforma utilizável. |
| Domínio de falha | conjunto de componentes que podem ser afetados pelo mesmo evento de falha. Duas cópias no mesmo domínio de falha não oferecem a mesma independência de uma cópia externa. |
| Downstream | projeto ou fornecedor que integra, empacota, corrige e mantém software recebido de um projeto upstream. |
| Enterprise | qualificador de produto/ecossistema orientado à operação empresarial, podendo envolver lifecycle, suporte comercial, SLA, compatibilidade formal, certificações, gestão e contratos. Não define sozinho Fixed/Rolling nem possui significado universal fora do contexto do fornecedor. |
| Edição | variante de um projeto/produto destinada a um uso, desktop ou fluxo operacional específico; pode compartilhar a mesma geração com outras edições. |
| ELF | *Executable and Linkable Format*: formato comum de executáveis, objetos e bibliotecas compartilhadas em Linux e outros sistemas Unix-like. |
| EOL | *End of Life*: fim do período de manutenção ou suporte declarado para uma versão, canal ou produto. |
| ESM | *Expanded Security Maintenance*: serviço do Ubuntu Pro que amplia o escopo/período de manutenção de segurança de releases Ubuntu LTS conforme a política do produto. |
| FIPS 140-3 | *Federal Information Processing Standard 140-3*: padrão de requisitos de segurança para módulos criptográficos, utilizado no programa federal de validação correspondente nos Estados Unidos. |
| Firmware | software de baixo nível executado ou carregado em dispositivos, como GPU, Wi-Fi, SSD e placas de rede; sua disponibilidade pode determinar suporte de hardware. |
| Fixed Release | modelo com versões identificáveis, conteúdo-base estabilizado e migrações explícitas entre releases principais. |
| fwupd | daemon/framework de atualização de firmware em Linux. Pode consumir metadados e payloads de fontes como o LVFS quando o fabricante/dispositivo é suportado. |
| Generation | estado versionado e reproduzível, comum em sistemas declarativos como NixOS. |
| GitOps | prática operacional em que o estado desejado é versionado em Git e automação/reconciliação aplica ou verifica esse estado no ambiente, preservando revisão e histórico de mudanças. |
| GPO | *Group Policy Object*: objeto de política do Active Directory usado para aplicar configurações e regras a usuários/computadores Windows. Ingressar um Linux no domínio não implica implementar toda a semântica de GPO. |
| HA | *High Availability*: desenho para manter ou restaurar serviço apesar de falhas por redundância, failover e coordenação de componentes. Não é propriedade automática de uma distribuição. |
| Hardening | redução deliberada da superfície de ataque por configuração, remoção, restrição e aplicação de controles. Hardening não substitui patching. |
| HCI | *Hyper-Converged Infrastructure*: arquitetura que integra compute, storage e rede/virtualização em uma plataforma de cluster gerenciada de forma conjunta. |
| Host | máquina ou instância que executa o sistema operacional em análise; pode ser física, virtual ou cloud. Em virtualização, também pode significar o sistema que hospeda VMs/containers. |
| Idempotência | propriedade de uma operação que pode ser repetida e convergir para o mesmo resultado desejado sem acumular efeitos indevidos. Atomicidade não implica idempotência. |
| Image-based | modelo em que o conteúdo do sistema operacional controlado pelo host é construído, distribuído e atualizado predominantemente como imagem, árvore ou deployment coerente, em vez de depender apenas de mutações pacote a pacote na instalação em execução. |
| Imagem ISO | arquivo no formato ISO 9660 ou derivado usado frequentemente como mídia inicializável/instalação. Em uma rolling, a data/número da ISO normalmente identifica a mídia, não uma geração fixa do sistema instalado. |
| Immutable | termo operacional para uma base protegida contra alterações ad hoc e modificada por caminhos controlados. Não significa que nenhum byte possa ser alterado em qualquer circunstância. |
| Init system | primeiro sistema de espaço de usuário responsável por iniciar, supervisionar e encerrar serviços e sessões, como systemd, OpenRC, runit, s6 ou Dinit. |
| initramfs / initrd | imagem usada na fase inicial do boot para disponibilizar drivers, módulos e ferramentas necessários antes de montar/usar plenamente o filesystem raiz definitivo. |
| ISV | *Independent Software Vendor*: fornecedor independente de software. Matrizes de ISV são relevantes para certificar/suportar aplicações em combinações específicas de sistema, release e arquitetura. |
| kABI | *kernel Application Binary Interface*: conjunto de contratos binários do kernel relevantes para módulos/drivers externos. Políticas de estabilidade são específicas de fornecedor, release, arquitetura e símbolos. |
| Kernel | núcleo que gerencia CPU, memória, dispositivos, processos, isolamento e interfaces fundamentais do sistema operacional. Uma distribuição é maior que seu kernel. |
| Kernel module | componente carregável que estende funcionalidades do kernel, como drivers e filesystems. Módulos externos podem depender de kABI e políticas de assinatura/Secure Boot. |
| Lifecycle | sequência de fases de uma versão ou produto: lançamento, manutenção, suporte, possíveis extensões e EOL. Deve ser verificada por edição, canal e arquitetura. |
| LTS | *Long-Term Support*: política de manutenção prolongada aplicada a um objeto específico, como release, kernel ou componente. Prazo e escopo variam; sempre identifique **LTS de quê** e quem mantém esse objeto. |
| LVFS | *Linux Vendor Firmware Service*: serviço usado por fabricantes para publicar metadados e firmwares consumidos por clientes como `fwupd`; não cobre automaticamente todo hardware existente. |
| MAC (*Mandatory Access Control*) | controle de acesso obrigatório aplicado por política, como SELinux ou AppArmor, além das permissões Unix tradicionais. Neste contexto, MAC não significa endereço de rede *Media Access Control*. |
| Major release | geração principal de um produto, normalmente identificada pelo primeiro componente de versão (`9`, `10`, etc.) e associada a mudanças maiores de plataforma/lifecycle conforme a política do fornecedor. |
| Measured boot | processo que registra medições criptográficas dos componentes de inicialização para posterior verificação ou atestação. É diferente de apenas bloquear código não autorizado. |
| Microcode | camada de controle interno da CPU que pode receber correções do fabricante; distribuições podem carregar microcode atualizado no boot conforme arquitetura e pacotes suportados. |
| Minor release | revisão identificada dentro da mesma major release, como `RHEL 10.1` ou `10.2`. O guia preserva o termo oficial do fornecedor; sua função pode se sobrepor ao que outros projetos chamam de point release. |
| NOS | *Network Operating System*: sistema operacional orientado à operação de dispositivos de rede, especialmente switches/roteadores, integrando hardware, dataplane, protocolos e gestão. |
| OVAL | *Open Vulnerability and Assessment Language*: linguagem estruturada para representar estado/configuração de sistemas, testar condições como vulnerabilidade/patch e reportar resultados de avaliação. |
| Package manager | ferramenta que resolve, instala, atualiza e remove pacotes e suas dependências, como APT, DNF, Zypper ou pacman. Não deve ser confundida com formato de pacote ou repositório. |
| Package pinning | política de prioridade/preferência que controla de qual repositório ou versão um pacote pode ser selecionado quando múltiplas origens estão disponíveis. |
| Patching | aplicação de correções de segurança, defeitos ou manutenção. Pode ocorrer por atualização de pacote, backport, live patch ou substituição de imagem. |
| Point release | consolidação identificada dentro de uma família de versão, normalmente reunindo correções, mídia nova e atualizações acumuladas. Não implica nova major release. |
| Proveniência | origem, autoria, processo de build, assinatura e cadeia de manutenção de um artefato. |
| RAID | *Redundant Array of Independent Disks*: combinação de múltiplos discos para desempenho e/ou tolerância a determinadas falhas. RAID não substitui backup independente. |
| Rebase | troca da referência do conteúdo do sistema operacional controlado pelo mecanismo para outra versão, imagem ou branch, preservando o que o modelo declara compatível. Não é sinônimo universal de upgrade in-place. |
| Recuperação de desastre (DR) | processo/arquitetura para restaurar serviços e dados após evento severo que excede a operação normal de alta disponibilidade. |
| Release | estado publicado e identificável de um projeto. Pode ser uma versão fixa, uma mídia de instalação ou um snapshot; o significado depende do modelo adotado. |
| Repositório | origem organizada de pacotes, metadados ou imagens consumida pelas ferramentas de atualização. Repositório oficial, comunitário e de terceiro possuem cadeias de confiança diferentes. |
| Reprodutibilidade | capacidade de reconstruir novamente um resultado suficientemente equivalente a partir do mesmo conjunto controlado de entradas. Não é sinônimo de declaratividade. |
| Resiliência | capacidade de absorver falhas, degradar de forma controlada e recuperar-se preservando funções críticas dentro dos objetivos definidos. |
| riscv64 | arquitetura RISC-V de 64 bits. A disponibilidade de imagens, drivers, firmware e software de terceiros varia e deve ser confirmada por projeto/hardware. |
| Rollback | retorno a deployment, snapshot, generation ou versão anterior. Reverte estado coberto pelo mecanismo; não substitui backup de dados. |
| Rolling | adjetivo que indica evolução contínua, mas é ambíguo quando usado sozinho. Deve ser qualificado como rolling release, rolling update ou outro mecanismo. |
| Rolling Development | branch de desenvolvimento ativo que recebe mudanças continuamente e alimenta futuras releases, como Debian Unstable/Sid ou Fedora Rawhide. Não deve ser confundida automaticamente com uma Rolling Release pronta para uso regular. |
| Rolling Release | modelo no qual o sistema instalado evolui continuamente pelo fluxo de pacotes, sem upgrades periódicos obrigatórios para uma nova versão completa. ISOs datadas são snapshots de instalação. |
| Rolling update / rolling deployment | estratégia operacional que substitui ou reinicia instâncias em lotes, mantendo parte do serviço disponível. É diferente do modelo de distribuição rolling release. |
| Root filesystem | filesystem raiz montado em `/`, ponto de partida da árvore de diretórios do sistema Linux. Pode estar sobre volume, criptografia e storage físico distintos. |
| Rootless container | container/runtime executado sem privilégios de root no host sempre que o mecanismo permite. Reduz privilégios e impacto de determinadas falhas, mas não elimina risco do kernel, runtime, imagem ou configuração. |
| RPO | *Recovery Point Objective*: quantidade máxima de perda de dados tolerada após incidente, normalmente expressa como intervalo de tempo entre o estado recuperado e a falha. |
| RTO | *Recovery Time Objective*: tempo-alvo máximo para restaurar um serviço após indisponibilidade. |
| Runtime de container | software responsável por executar containers conforme o modelo/ecossistema usado; exemplos de baixo nível incluem `runc` e `crun`. Runtime, kernel e configuração influenciam o isolamento real. |
| SBC | *Single-Board Computer*: computador de placa única que integra CPU/SoC, memória e interfaces em uma mesma placa; suporte Linux depende do modelo, boot, firmware e periféricos. |
| SBOM | *Software Bill of Materials*: inventário estruturado dos componentes e dependências presentes em um artefato de software. |
| Secure Boot | mecanismo de boot que verifica assinaturas e impede a execução de componentes não autorizados na cadeia coberta pela política. Não mede nem atesta sozinho o estado completo do sistema. |
| Scrub | operação sistemática de verificação de integridade de dados/metadados conforme a implementação do storage. Reparação depende de checksums, mecanismo suportado e cópia redundante válida; scrub sozinho não garante recuperação. |
| Semi-Rolling | rótulo **editorial/contextual**, sem definição universal, usado quando um produto combina propriedades de base versionada e fluxo contínuo. Prefira descrever a combinação concreta e preserve o termo quando o próprio projeto o utiliza. |
| SLA | *Service Level Agreement*: acordo que define níveis de serviço, atendimento, disponibilidade ou tempos de resposta entre as partes. |
| Slow Rolling | Rolling Release que atrasa deliberadamente mudanças maiores ou agrega mais etapas de teste em relação a uma rolling de referência, mantendo fluxo contínuo de manutenção. |
| Snapshot | estado capturado em determinado ponto. Pode cobrir filesystem, volume, dataset, repositório ou conjunto de pacotes; seu escopo define o que pode ser recuperado. **Snapshot factual**, neste guia, significa fotografia histórica das informações verificadas em uma data, não estado automaticamente atual. |
| STIG | *Security Technical Implementation Guide*: guia técnico de implementação de segurança do ecossistema do Departamento de Defesa dos Estados Unidos para tecnologias específicas. |
| Stream | linha contínua de integração/entrega organizada por canal ou geração. É usada quando a dicotomia Fixed/Rolling descreve mal o produto, como em determinados sistemas de imagens ou no CentOS Stream. |
| Taint do kernel | marcação do kernel Linux indicando determinadas condições, como carregamento de módulos proprietários/out-of-tree ou eventos específicos; ajuda suporte/diagnóstico a identificar estado não totalmente equivalente ao kernel upstream padrão. |
| TPM | *Trusted Platform Module*: componente de segurança, físico ou firmware conforme a plataforma, que fornece funções protegidas para chaves, medições e atestação. |
| Transactional update | atualização preparada em estado/snapshot separado e ativada como uma unidade quando a operação conclui. A tecnologia concreta pode ser Btrfs snapshots, deployment OSTree, A/B ou outro mecanismo. |
| Upstream | projeto original ou anterior na cadeia que desenvolve e publica o software recebido por distribuidores downstream. |
| Userspace | conjunto de processos, bibliotecas e aplicações que executam fora do kernel. Atualizar userspace não significa atualizar o kernel, e vice-versa. |
| VEX | *Vulnerability Exploitability eXchange*: formato/conceito para comunicar o estado de vulnerabilidades conhecidas em relação a um produto, complementando inventários como SBOM. Não substitui advisory, análise de risco ou status do fornecedor. |
| VM | *Virtual Machine*: ambiente que virtualiza hardware e normalmente executa um sistema operacional convidado com kernel próprio. |
| Wayland | protocolo e arquitetura para comunicação entre aplicações e um compositor. Não é um desktop environment nem um único servidor universal; cada ambiente utiliza uma implementação de compositor. |
| Workload | conjunto de aplicações, serviços, processos, dados e padrões de uso que cumpre uma função e impõe requisitos mensuráveis ao sistema. “Desktop”, “banco de dados” e “nó Kubernetes” são classes amplas, não especificações completas. |
| X.Org Server | implementação amplamente usada do servidor X11. Não deve ser tratado como sinônimo perfeito de todo o protocolo X11. |
| X11 | versão 11 do protocolo X Window System, baseado em arquitetura cliente-servidor e extensões. X11 é o protocolo; X.Org Server é sua implementação mais comum em Linux. |
| x86_64 | arquitetura x86 de 64 bits, também chamada AMD64/x64. É comum em PCs e servidores, mas suporte de software e certificação ainda precisa ser confirmado por produto/release. |
| XWayland | servidor X11 completo que executa como cliente de um compositor Wayland para compatibilidade com aplicações X11. Aplicação via XWayland não é aplicação Wayland nativa. |

[Voltar ao índice](#indice)

---

<a id="24-referencias-primarias"></a>

## 24. Referências primárias e complementares

Esta referência adota uma hierarquia de evidências:

1. documentação de release, lifecycle, segurança e compatibilidade publicada pelo próprio projeto ou fornecedor;
2. especificações, padrões e documentação dos componentes upstream;
3. catálogos, artigos, blogs, vídeos e avaliações comunitárias para descoberta, contexto e pontos de teste;
4. opinião, popularidade e page views nunca são usados isoladamente para eliminar, pontuar ou recomendar uma distribuição.

<a id="24-release-e-lifecycle"></a>

### Release e lifecycle

- [TUXEDO OS — anúncio da futura base Debian Testing / Continuous Debian](https://www.tuxedocomputers.com/en/A-new-foundation-for-TUXEDO-OS-Switching-to-Debian.tuxedo)
- [openSUSE — portal de distribuições e estados de maturidade (Tumbleweed, Slowroll, MicroOS, Aeon, Leap)](https://en.opensuse.org/Portal:Distribution)

- [Debian releases](https://www.debian.org/releases/)
- [Debian Security Tracker](https://security-tracker.debian.org/tracker/)
- [Ubuntu release cycle](https://ubuntu.com/about/release-cycle)
- [Ubuntu Security Notices](https://ubuntu.com/security/notices)
- [Fedora lifecycle](https://docs.fedoraproject.org/en-US/releases/lifecycle/)
- [Fedora Atomic Desktops](https://fedoraproject.org/atomic-desktops/)
- [RHEL lifecycle](https://access.redhat.com/support/policy/updates/errata)
- [Red Hat Security Advisories](https://access.redhat.com/security/security-updates/security-advisories)
- [RHEL sem custo — modalidades e limites do programa Developer](https://developers.redhat.com/articles/faqs-no-cost-red-hat-enterprise-linux)
- [AlmaLinux — releases e lifecycle](https://wiki.almalinux.org/release-notes/)
- [AlmaLinux — política de compatibilidade binária/ABI com RHEL](https://almalinux.org/blog/future-of-almalinux/)
- [Rocky Linux — releases e lifecycle](https://docs.rockylinux.org/10/releases/)
- [Oracle Linux 10 — documentação e release information](https://docs.oracle.com/en/operating-systems/oracle-linux/10/)
- [Oracle Linux 10 — compatibilidade de espaço de usuário com RHEL](https://docs.oracle.com/en/operating-systems/oracle-linux/10/relnotes10.2/ol-Compatibility.html)
- [Oracle Linux 10 — kernels fornecidos e diferenças por arquitetura](https://docs.oracle.com/en/operating-systems/oracle-linux/10/relnotes10.0/ol10.0-ShippedKernels.html)
- [Oracle Linux — download, uso e redistribuição gratuitos](https://www.oracle.com/linux/technologies/oracle-linux-downloads.html)
- [Oracle Linux — Lifetime Support Policy](https://www.oracle.com/a/ocom/docs/elsp-lifetime-069338.pdf)
- [openSUSE roadmap](https://en.opensuse.org/openSUSE:Roadmap)
- [openSUSE Leap 16 — modelo fixed e lifecycle](https://en.opensuse.org/Portal:Leap)
- [openSUSE Tumbleweed — rolling release](https://en.opensuse.org/Portal:Tumbleweed)
- [openSUSE Slowroll](https://en.opensuse.org/Portal:Slowroll)
- [Arch system maintenance — rolling release e upgrades completos](https://wiki.archlinux.org/title/System_maintenance)
- [Kali branches — `kali-rolling`, `kali-last-snapshot` e canais de desenvolvimento](https://www.kali.org/docs/general-use/kali-branches/)
- [Manjaro — modelo de desenvolvimento Rolling Release](https://wiki.manjaro.org/index.php?title=The_Rolling_Release_Development_Model)
- [Manjaro — Stable, Testing e Unstable branches](https://wiki.manjaro.org/index.php?title=Switching_Branches)
- [NixOS release notes](https://nixos.org/manual/nixos/stable/release-notes)
- [Linux Mint — versões suportadas](https://linuxmint.com/download_all.php)
- [Zorin OS — versões e suporte](https://zorin.com/os/details/)
- [Pop!_OS 24.04 LTS — anúncio oficial](https://blog.system76.com/post/pop-os-letter-from-our-founder)
- [EndeavourOS — notícias e snapshots de instalação](https://endeavouros.com/news/)
- [Manjaro — anúncios de releases e stable updates](https://forum.manjaro.org/c/announcements/11)
- [Bazzite — atualizações, rollbacks e rebases](https://docs.bazzite.gg/Installing_and_Managing_Software/Updates_Rollbacks_and_Rebasing/)
- [Debian Testing — funcionamento, aliases e codename](https://wiki.debian.org/DebianTesting)
- [Debian Unstable/Sid — Rolling Development, não Rolling Release](https://wiki.debian.org/DebianUnstable)
- [Debian Developer’s Reference — fluxo unstable → testing → freeze → stable](https://www.debian.org/doc/manuals/developers-reference/developers-reference.html)
- [Fedora Rawhide — desenvolvimento em constante evolução](https://fedoraproject.org/wiki/Releases/Rawhide)
- [Nobara — semi-rolling desde a versão 41](https://wiki.nobaraproject.org/general-usage/troubleshooting/upgrade-nobara)
- [CachyOS — rolling-release](https://wiki.cachyos.org/cachyos_basic/why_cachyos/)
- [Parrot OS — base Debian Stable e repositório com modelo rolling](https://parrotsec.org/docs/introduction/what-is-parrot/)
- [ChimeraOS — FAQ e mecanismo de atualização por imagem `frzr`](https://github.com/ChimeraOS/chimeraos/wiki/FAQ)

<a id="24-arquitetura-e-operacao"></a>

### Arquitetura e operação

- [Fedora Atomic updates, upgrades and rollbacks](https://docs.fedoraproject.org/en-US/atomic-desktops/updates-upgrades-rollbacks/)
- [rpm-ostree — filesystem, `/usr`, `/etc`, `/var` e deployments](https://coreos.github.io/rpm-ostree/administrator-handbook/)
- [Fedora Silverblue — Atomic Desktop](https://fedoraproject.org/atomic-desktops/silverblue/)
- [Bazzite FAQ — Fedora Atomic, update cycle e rollback](https://docs.bazzite.gg/General/FAQ/)
- [SUSE Linux Micro — transactional updates e snapshots Btrfs](https://documentation.suse.com/sle-micro/6.2/html/Micro-transactional-updates/index.html)
- [Nix reference manual](https://nixos.org/manual/nix/stable/)
- [Ubuntu AppArmor](https://documentation.ubuntu.com/security/apparmor/)
- [RHEL Security hardening](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/security_hardening/)
- [SUSE security certifications](https://www.suse.com/support/security/certifications/)
- [Google Container-Optimized OS release notes](https://docs.cloud.google.com/container-optimized-os/docs/release-notes)
- [Azure Container Linux overview](https://learn.microsoft.com/en-us/azure/azure-linux/azure-container-linux-overview)
- [Amazon Linux 2023 release notes](https://docs.aws.amazon.com/linux/al2023/release-notes/)
- [fwupd — documentação oficial](https://fwupd.github.io/)
- [Linux Vendor Firmware Service (LVFS) — visão geral](https://lvfs.readthedocs.io/en/latest/intro.html)
- [Linux kernel — microcode loader x86](https://www.kernel.org/doc/html/latest/arch/x86/microcode.html)
- [Kubernetes — Container Runtime Interface (CRI)](https://kubernetes.io/docs/concepts/containers/cri/)
- [Kubernetes — cgroup v2](https://kubernetes.io/docs/concepts/architecture/cgroups/)
- [NixOS Manual — atualização por channels e `nixos-rebuild --upgrade`](https://nixos.org/manual/nixos/stable/)
- [Open Container Initiative — specifications](https://opencontainers.org/)

<a id="24-seguranca-e-conformidade"></a>

### Segurança e conformidade

- [NIST — FIPS 140-3: Security Requirements for Cryptographic Modules](https://csrc.nist.gov/pubs/fips/140-3/final)
- [NIST — OVAL, Open Vulnerability and Assessment Language](https://csrc.nist.gov/glossary/term/open_vulnerability_and_assessment_language)
- [NIST — SCAP 1.4 e linguagens de checklist/avaliação](https://csrc.nist.gov/projects/security-content-automation-protocol/scap-releases/scap-1-4)
- [Center for Internet Security — CIS Benchmarks](https://www.cisecurity.org/cis-benchmarks-overview)
- [DoD Cyber Exchange — Security Technical Implementation Guides (STIGs)](https://public.cyber.mil/stigs/)
- [Common Criteria Portal — visão geral e certificação de produtos](https://www.commoncriteriaportal.org/)
- [Common Criteria — CC:2022 / documentos oficiais](https://www.commoncriteriaportal.org/cc/index.cfm)
- [CISA — Vulnerability Exploitability eXchange (VEX), materiais e requisitos](https://www.cisa.gov/sites/default/files/2024-03/VEX_feb2024.pdf)
- [CISA — Known Exploited Vulnerabilities (KEV) Catalog](https://www.cisa.gov/known-exploited-vulnerabilities-catalog)

<a id="24-graficos-rede-e-terminologia"></a>

### Gráficos, rede e terminologia

- [Wayland — visão geral oficial](https://wayland.freedesktop.org/)
- [Wayland — arquitetura comparada ao X](https://wayland.freedesktop.org/architecture.html)
- [XWayland — suporte a aplicações X11](https://wayland.freedesktop.org/docs/book/Xwayland.html)
- [X11 Protocol — especificação do X.Org](https://xorg.freedesktop.org/releases/X11R7.7/doc/xproto/x11protocol.html)
- [RFC 7426 — control plane e forwarding/data plane](https://datatracker.ietf.org/doc/html/rfc7426)

<a id="24-catalogos-mapas-e-comparadores"></a>

### Catálogos, mapas e comparadores

- [DistroWatch](https://distrowatch.com/): útil para descobrir projetos, releases, notícias, versões de pacotes e páginas históricas. Seus dados devem ser confirmados na fonte oficial antes de alterar o catálogo.
- [FAQ do DistroWatch](https://distrowatch.com/dwres.php?resource=faq): explica o escopo e o Page Hit Ranking. O ranking mede acessos às páginas do site entre seus visitantes; **não mede instalações, participação de mercado, qualidade, segurança nem aderência ao workload**.
- [Distrochooser em português](https://distrochooser.de/pt-br): questionário independente voltado à orientação inicial. É uma referência comparativa de UX e metodologia, não fonte factual do lifecycle das distribuições.
- [Código e limitações declaradas do Distrochooser](https://github.com/distrochooser/distrochooser): o próprio projeto descreve os resultados como sugestões, não como cálculo infalível.

<a id="24-linha-do-tempo-das-distribuicoes-linux"></a>

### Linha do tempo das distribuições Linux

<p align="center">
  <a href="https://upload.wikimedia.org/wikipedia/commons/1/1b/Linux_Distribution_Timeline.svg">
    <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/1b/Linux_Distribution_Timeline.svg/330px-Linux_Distribution_Timeline.svg.png" width="330" alt="Prévia da linha do tempo comunitária das distribuições Linux">
  </a>
</p>

- [Abrir a linha do tempo em SVG e alta resolução](https://upload.wikimedia.org/wikipedia/commons/1/1b/Linux_Distribution_Timeline.svg)
- [Página do arquivo, histórico, autoria e licença](https://commons.wikimedia.org/wiki/File:Linux_Distribution_Timeline.svg)
- [Código-fonte que mantém a linha do tempo](https://github.com/FabioLolix/LinuxTimeline)

> **Nota editorial:** esta é uma visualização comunitária, não uma imagem oficial conjunta de todas as distribuições. A revisão atualmente servida no Wikimedia foi gerada a partir da linha do tempo 24.10 e publicada em fevereiro de 2025. Ela é excelente para genealogia e contexto histórico, mas não comprova atividade atual, versão, suporte ou compatibilidade. O arquivo é disponibilizado sob GFDL 1.3 ou posterior e contém marcas sujeitas aos respectivos titulares.

<a id="24-artigos-blogs-e-videos-para-leitura-complementar"></a>

### Artigos, blogs e vídeos para leitura complementar

- [Red Hat — What’s the best Linux distro for you?](https://www.redhat.com/en/topics/linux/whats-the-best-linux-distro-for-you): visão orientada a workload, comunidade e suporte enterprise; deve ser lida como perspectiva de fornecedor.
- [LWN.net](https://lwn.net/): jornalismo técnico sobre kernel, distribuições e ecossistema; útil para contexto, sem substituir advisories e documentação de release.
- [Linux Foundation no YouTube](https://www.youtube.com/user/TheLinuxFoundation): palestras e fundamentos do ecossistema Linux e open source.
- [Learn Linux TV no YouTube](https://www.youtube.com/learnlinuxtv): tutoriais, administração e avaliações de distribuições; conteúdo comunitário que deve ser confrontado com documentação oficial.
- [The Linux Experiment no YouTube](https://www.youtube.com/c/TheLinuxExperiment): notícias, testes e opiniões sobre desktop Linux; útil para observar experiência de uso, não para certificar compatibilidade.
- **Diolinux e Diocast — conteúdo em português:**
  - [O que você PRECISA saber sobre Rolling Release e Point Release](https://www.youtube.com/watch?v=_KBsY1okvCM): introdução audiovisual aos modelos discutidos nas seções 2.2 e 2.4; use a terminologia canônica deste guia para distinguir modelo de distribuição, mídia de instalação e estratégia de rollout.
  - [Qual o tipo de Linux certo pra você? — Diocast](https://www.youtube.com/watch?v=S0ArPaTYaGI): conversa sobre perfis, necessidades e escolha de distribuição; útil como contraponto comunitário ao método auditável da seção 15.
  - [O que é Linux? Explicação COMPLETA 2026](https://www.youtube.com/watch?v=CT6BZBzbpWA): visão introdutória sobre kernel, sistema operacional, distribuições e presença do Linux no ecossistema tecnológico.
  - [As melhores (e piores) distros para começar no Linux em 2026](https://www.youtube.com/watch?v=dLTmYcAM7mA): avaliação editorial orientada à experiência de iniciantes. Use-a para identificar critérios e pontos de teste, não como ranking universal nem fonte de lifecycle, segurança ou compatibilidade.

O HTML contém referências adicionais junto às capacidades especializadas. Links comerciais, comunitários, artigos, vídeos e rankings não substituem a matriz formal do fornecedor nem os testes no hardware e workload reais.

[Voltar ao índice](#indice)

---

<a id="historico-desta-edicao"></a>

## Histórico desta edição

<a id="historico-0-1-6"></a>

### 0.1.6 — 2026-08-21

Revisão de precisão terminológica e auditabilidade documental, sem alteração intencional dos pesos, perguntas, elegibilidade ou algoritmo do Linux Distro Advisor.

- separadas explicitamente as noções de **LTS**, **enterprise** e lifecycle, com regra “LTS de quê?”;
- declarada a taxonomia multidimensional/modelos híbridos como **taxonomia editorial deste guia**, não classificação normativa universal;
- refinadas as descrições de KDE neon, Parrot OS e Nobara para reduzir falsa precisão no rótulo Semi-Rolling;
- ampliado o mapa mental inicial e consolidado o quadro operacional `update`/`upgrade`/`rebase`/`deployment`/`rollback`/`restore`/`rebuild`;
- refinadas API, SONAME e kABI, incluindo a ausência de ABI interna estável universal no kernel Linux upstream;
- adicionada priorização de vulnerabilidades além de CVE/CVSS, com contexto operacional e KEV;
- ampliada a proveniência de **imagens OCI** com registry, digest, assinatura/attestation, SBOM/VEX e lifecycle de base;
- acrescentada comparação equilibrada de Snap/Flatpak e distinção entre assinatura, integridade e segurança;
- corrigida a definição de **scrub** e explicitado que a pilha de storage mostrada é apenas um exemplo de camadas possíveis;
- adicionada a distinção **declarativo ≠ reproduzível**;
- modernizados critérios de Kubernetes com cgroup v2, CRI, CNI, CSI, kubelet e matriz de compatibilidade;
- simplificado o fluxo NixOS com channels para `nixos-rebuild switch --upgrade` e adicionado `nix flake check` no fluxo com flakes;
- reformulada a declaração de privacidade para depender de auditoria da implementação real do HTML, não do README isoladamente;
- substituída a expressão **heurística transparente** por **heurística documentada e versionada**, declarando os limites atuais de reprodutibilidade do score;
- introduzida distinção formal entre **fato declarado**, **contrato formal**, **comportamento observado** e **inferência editorial**;
- ampliados glossário, referências e erros conceituais comuns com os conceitos desta revisão;
- mantido o snapshot factual de **2026-08-15** e preservado o contrato funcional do quiz.

---

<a id="historico-0-1-5"></a>

### 0.1.5 — 2026-08-21

Revisão corretiva, didática e de governança, sem alteração intencional do algoritmo, pesos, perguntas ou elegibilidade do Linux Distro Advisor.

- corrigido erro editorial da 0.1.4 que havia inserido indevidamente `SONAME` e `SoC` na tabela de storage;
- acrescentada auditoria **semântica** separada da auditoria estrutural para detectar itens corretos em Markdown, porém conceitualmente deslocados;
- atualizado o estado editorial/badge para **pré-1.0**;
- criada a seção **0. Conceitos fundamentais antes da escolha**, definindo kernel Linux, distribuição, edição, release, branch, canal, imagem de instalação, repositório, desktop environment e aplicação;
- explicitada a unidade real da decisão: produto/distribuição + edição + release/branch/canal + arquitetura + origem + suporte + workload;
- refinada a definição de atualização atômica para separar atomicidade de rollback, recuperação e consistência de dados/aplicações;
- ampliada a seção de proveniência com metadados assinados, assinatura de artefatos, rotação de chaves, pinning/prioridades, scripts de instalação e runtimes/base images;
- adicionado VEX como complemento de SBOM para comunicação do estado de vulnerabilidades;
- ampliadas, de forma controlada, as validações de desktop, hardware, containers e rede;
- reestruturada a seção de storage em camadas e adicionados LVM, LUKS/dm-crypt, recuperação, SSD/NVMe, application-consistent e teste real de restore;
- definida regra explícita: requisito eliminatório desconhecido permanece pendente e não pode ser considerado atendido sem evidência;
- adicionados data, validade/revisão, confiança e responsável à governança das evidências do método de decisão;
- alterada a seção de perfis para **exemplos que merecem investigação inicial**, com coluna de condição em que a linha não deve ser usada como atalho;
- separadas data do snapshot factual, data da edição, validação estrutural e revisão semântica;
- adicionados pré-requisitos de segurança operacional à seção de comandos e refinamentos específicos para APT, DNF, rpm-ostree e NixOS flakes;
- ampliado o glossário com conceitos fundamentais e operacionais recorrentes, incluindo distribuição, edição, ISO, domínio de falha, package pinning, VEX, runtime de container, DR e resiliência;
- mantido o snapshot factual de **2026-08-15** e preservado o contrato decisório do quiz.

---

<a id="historico-0-1-4"></a>

### 0.1.4 — 2026-08-21

Revisão de progressão didática e completude operacional, sem alteração intencional do algoritmo, pesos, perguntas ou elegibilidade do Linux Distro Advisor.

- adicionadas trilhas de leitura específicas para iniciante, profissional técnico, homologação/segurança e auditoria do recomendador;
- adicionado aviso global de snapshot factual logo no início do guia, preservando 15/08/2026 como recorte histórico;
- adicionada matriz didática com 12 casos representativos antes da tabela-mestre de 95 candidatos;
- introduzidos quadros seletivos **Essencial**, **Não confunda** e **Decisão prática** sem repetir o recurso em todas as seções;
- ampliada a seção de compatibilidade com exemplos concretos de quebra de ABI/SONAME e de impacto de kABI em módulos externos;
- reforçado que imutabilidade/read-only não substitui segurança em tempo de execução;
- adicionada nota sobre window managers independentes e motivo de exclusão do ranking padrão;
- ampliada a seção de hardware com x86_64, aarch64 e riscv64, impactos de arquitetura e matriz de suporte;
- diferenciados BIOS/UEFI, microcode, firmware de dispositivos, drivers e userspace; adicionados `fwupd` e LVFS;
- adicionada nota operacional sobre pilhas de AI/ML/accelerators sem transformar o guia em curso especializado;
- adicionados perfis ilustrativos de política para firmware, drivers e aplicações não livres;
- adicionado conceito de GitOps à gestão de frota e uma nota concisa sobre critérios de edge;
- adicionado exemplo preenchido de registro de decisão auditável na etapa 7;
- ampliada a seção 18 com comandos seguros de inspeção antes de atualizar: `apt list --upgradable`, `dnf check-update`, `zypper list-updates` e `checkupdates`, incluindo a ressalva de partial upgrades no Arch;
- ampliado o glossário com x86_64, aarch64, riscv64, microcode, fwupd, LVFS, SONAME e GitOps;
- adicionadas fontes primárias para fwupd/LVFS, microcode do kernel, DNF check-update e Arch checkupdates;
- mantido o snapshot factual de 15/08/2026 e preservado integralmente o contrato decisório do quiz.

---

<a id="historico-0-1-3"></a>

### 0.1.3 — 2026-08-21

Revisão técnica e didática integral, sem alteração intencional do algoritmo de recomendação do quiz.

- reconciliado explicitamente o modelo completo de 10 dimensões da seção 1 com o subconjunto de 6 dimensões aprofundado na seção 2;
- normalizada a tabela-mestre dos 95 candidatos para uma taxonomia canônica pequena, separando modelo de release de curadoria, lifecycle, atomicidade, image-based e estado de appliance;
- declarado por que lifecycle e atualidade não são comprimidos em uma única coluna da tabela-mestre, evitando falsa precisão;
- reforçadas as explicações de Debian Testing/Sid, Fedora Rawhide e distinção entre branch, codename, canal e release;
- adicionadas definições no primeiro uso para CVE/OVAL, API/ABI/kABI, fontes suplementares de pacotes, conceitos de desktop/gráficos, segurança, storage, virtualização, rede, identidade, frota e continuidade;
- ampliada a seção de segurança com definições de FIPS 140-3, CIS Benchmarks, STIG e Common Criteria antes das ressalvas de compliance/certificação;
- reescrita a seção de rede com definições precisas de VRF, EVPN, VXLAN, SR-IOV, DPDK, XDP, NOS, ASIC e FRR;
- adicionada distinção fundamental entre VM, container de aplicação e system container;
- adicionados RTO, RPO, SLA e ISV no primeiro uso e no glossário;
- adicionada operação de `transactional-update` para SUSE Linux Micro/openSUSE MicroOS, com ressalva específica para Aeon;
- documentados Slowroll e Aeon como projetos em estado beta segundo o portal openSUSE consultado nesta revisão;
- ampliado o glossário com termos estruturais de release, operação, segurança, storage e infraestrutura;
- adicionada seção de referências primárias para FIPS, OVAL/SCAP, CIS Benchmarks, STIG e Common Criteria;
- removida a regra editorial de MUST/SHOULD/MAY enquanto esses termos não forem usados normativamente no corpo do guia;
- preservado o snapshot factual de 2026-08-15 e mantida a separação entre estado histórico do snapshot e anúncios de transição futuros.

---

<a id="historico-0-1-2"></a>

### 0.1.2 — 2026-08-21

Revisão conceitual aprofundada da seção 2, sem alteração do snapshot factual de versões (2026-08-15), dos 95 candidatos, dos pesos ou da lógica de pontuação do quiz.

- reorganizada a seção 2 para começar por seis dimensões independentes: modelo de release, cadência, atualidade dos componentes, lifecycle, mecanismo de atualização/ativação e mutabilidade/entrega;
- adicionada tabela-mestre cobrindo os 95 candidatos do Linux Distro Advisor, incluindo distribuições, edições, canais, sistemas image-based, cloud/container OS, NOS e appliances;
- ampliada a explicação de Fixed Release, Rolling Release, Semi-Rolling, Curated/Slow Rolling, streams e modelos híbridos;
- detalhada a distinção entre point release e minor release, preservando a terminologia específica de Debian, Ubuntu e RHEL;
- adicionada explicação aprofundada de Debian Testing, Debian Unstable/Sid, alias `testing`, codename e processo `unstable → testing → freeze → stable`;
- adicionada a distinção entre Fedora estável e Fedora Rawhide, evitando classificar a família Fedora inteira como Fixed ou Rolling sem qualificar o canal;
- detalhada a classificação de Bazzite, Nobara, CachyOS, Parrot OS, CentOS Stream, NixOS, OpenMandriva e outros casos híbridos;
- criada seção específica para demonstrar que Fixed/Rolling e Atomic/Transactional pertencem a eixos independentes;
- substituída, nessa explicação, a expressão genérica “conteúdo do sistema operacional controlado” por uma descrição explícita do conteúdo do SO controlado pelo deployment, incluindo a separação conceitual entre `/usr`, `/etc` e `/var` no rpm-ostree;
- adicionados exemplos de atomicidade/transacionalidade fora do Fedora, incluindo Bazzite/Universal Blue e SUSE/openSUSE MicroOS;
- adicionados ao glossário os termos **Branch**, **Cadência** e **Atualidade dos componentes**, além de refinamentos em **Atomic update**, **Deployment** e **Image-based**;
- ampliadas as referências primárias para Debian Testing/Sid, Fedora Rawhide, rpm-ostree, Nobara, CachyOS, Parrot OS, Bazzite e SUSE transactional updates.

<a id="historico-0-1-1"></a>

### 0.1.1 — 2026-08-21

Revisão de correção conceitual e editorial, sem alteração do snapshot factual de versões (2026-08-15) nem da lógica de pontuação do quiz.

- adicionada tabela comparativa no início da seção 2 com distribuições, edições e canais de referência confirmados em fontes primárias;
- explicitado que fixed release, rolling release, point release, branches e leading/bleeding edge pertencem a dimensões diferentes e podem coexistir;
- refinada a definição de rolling para não pressupor disponibilidade universal de snapshots ou rollback;
- refinada a seção de Slow/Curated Rolling, registrando o status beta do openSUSE Slowroll no snapshot e distinguindo-o da curadoria por branches do Manjaro;
- corrigida a explicação de point release para separar o comportamento do Debian, do Ubuntu LTS e do Kali `kali-last-snapshot`;
- ampliada a seção de branches com Manjaro e Kali;
- adicionadas referências primárias específicas para openSUSE Leap/Tumbleweed, Kali e Manjaro;
- registrada a divergência entre páginas oficiais da Canonical sobre abril/maio de 2031 no suporte padrão do Ubuntu 26.04 LTS, preservando a fonte específica da release como referência desta edição;
- preservado explicitamente o snapshot factual de 2026-08-15 para manter a auditabilidade histórica.

<a id="historico-0-1-0"></a>

### 0.1.0 — 2026-08-15

Primeira publicação pública e linha de base da série 0.

- README estabelecido como fonte canônica conceitual do projeto;
- 24 tópicos técnicos, índice navegável hierárquico com subtópicos e retorno ao índice ao final de cada tópico;
- modelo multidimensional de distribuição, release, lifecycle, segurança, proveniência, desktop e operação;
- separação entre pacote, formato, gerenciador, repositório e origem;
- separação entre patching, hardening, compliance e certificação;
- cobertura de desktop, hardware, storage, infraestrutura, rede, identidade, frota e continuidade;
- catálogo inicial com 95 candidatos e dois modos de análise;
- respostas condicionais normalizadas e fallback explícito para conflito de requisitos;
- snapshot factual identificado em 2026-08-15;
- perfis e snapshot com Linux Mint, Zorin OS, Pop!_OS, EndeavourOS, Manjaro, Bazzite, AlmaLinux, Rocky Linux e Oracle Linux;
- glossário ampliado com workload, lifecycle, rolling, Wayland, XWayland, X11, dataplane e conceitos correlatos;
- fontes complementares classificadas por nível de autoridade, incluindo DistroWatch, Distrochooser, linha do tempo, Diolinux/Diocast e outros canais técnicos;
- licenciamento CC BY 4.0 para o conteúdo e MIT para o código.

---

> **Regra de ouro:** escolha por workload, manutenção, lifecycle, ecossistema, suporte, arquitetura e capacidade operacional. Registre evidências, teste o caminho de atualização e prove a recuperação antes de chamar uma plataforma de pronta para produção.

[Voltar ao índice](#indice)
