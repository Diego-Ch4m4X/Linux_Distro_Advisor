# Referência Técnica para Escolha de Distribuições Linux

> Modelos de release, lifecycle, segurança, proveniência de software, desktop, infraestrutura e método de decisão — em português do Brasil.

[![Idioma: pt-BR](https://img.shields.io/badge/idioma-pt--BR-1f6feb)](#idioma-e-convencoes)
[![Edição: 0.1.0](https://img.shields.io/badge/edi%C3%A7%C3%A3o-0.1.0-8250df)](#historico-desta-edicao)
[![Snapshot: 2026-08-15](https://img.shields.io/badge/snapshot-2026--08--15-238636)](#17-recorte-temporal-e-versionamento)
[![Estado: versão inicial](https://img.shields.io/badge/estado-vers%C3%A3o_inicial-d29922)](#status-editorial)
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
- [01. Modelo mental: a classificação é multidimensional](#01-modelo-mental)
- [02. Modelo de release e cadência](#02-modelo-de-release-e-cadencia)
  - [2.1 Fixed Release](#02-01-fixed-release)
  - [2.2 Rolling Release](#02-02-rolling-release)
  - [2.3 Slow ou Curated Rolling](#02-03-slow-ou-curated-rolling)
  - [2.4 Point Release](#02-04-point-release)
  - [2.5 Branches: stable, testing e unstable](#02-05-branches-stable-testing-e-unstable)
  - [2.6 Leading edge e bleeding edge](#02-06-leading-edge-e-bleeding-edge)
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
  - [Não livre não é um único critério](#10-nao-livre-nao-e-um-unico-criterio)
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
  - [Duas correções temporais importantes](#17-duas-correcoes-temporais-importantes)
  - [Política de leitura](#17-politica-de-leitura)
- [18. Operações básicas de atualização](#18-operacoes-basicas-de-atualizacao)
  - [Debian, Ubuntu e derivados](#18-debian-ubuntu-e-derivados)
  - [Fedora e RHEL](#18-fedora-e-rhel)
  - [openSUSE](#18-opensuse)
  - [Arch Linux](#18-arch-linux)
  - [Fedora Atomic Desktops](#18-fedora-atomic-desktops)
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
  - [Gráficos, rede e terminologia](#24-graficos-rede-e-terminologia)
  - [Catálogos, mapas e comparadores](#24-catalogos-mapas-e-comparadores)
  - [Linha do tempo das distribuições Linux](#24-linha-do-tempo-das-distribuicoes-linux)
  - [Artigos, blogs e vídeos para leitura complementar](#24-artigos-blogs-e-videos-para-leitura-complementar)
- [Histórico desta edição](#historico-desta-edicao)
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

Este guia fornece o vocabulário e o método. O [Linux Distro Advisor](./quiz-linux.html) aplica uma heurística transparente sobre o mesmo modelo e devolve uma recomendação principal, duas alternativas e os trade-offs relevantes.

> O resultado do quiz é triagem técnica, não certificação, homologação nem parecer de segurança. A aprovação de produção continua dependendo de documentação upstream, matriz de compatibilidade, testes e critérios da organização.

[Voltar ao índice](#indice)

---

<a id="status-editorial"></a>

## Status editorial

Este é um material técnico independente. “Referência” significa, aqui, conteúdo versionado, auditável e sustentado por fontes primárias. Não significa documentação oficial de Debian, Fedora, Red Hat, SUSE, Canonical ou de qualquer outro projeto citado.

Documentação oficial é somente a publicada e mantida pelos respectivos projetos e fornecedores. Quando uma informação volátil é relevante, este guia aponta a fonte primária e declara a data do snapshot.

A edição **0.1.0** é a primeira publicação pública do projeto e inaugura a série 0. Enquanto o projeto permanecer em `0.x`, o catálogo, as perguntas, as matrizes e a heurística de recomendação poderão evoluir de forma incompatível entre edições. Toda mudança posterior a esta linha de base deverá ser registrada no histórico e acompanhada dos testes e das fontes aplicáveis.

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

[Voltar ao índice](#indice)

---

<a id="01-modelo-mental"></a>

## 1. Modelo mental: a classificação é multidimensional

Termos como Rolling Release, LTS, Stable, Bleeding Edge, imutável e atômico não pertencem todos ao mesmo eixo. Compará-los como categorias concorrentes produz conclusões falsas.

Uma distribuição ou edição pode ser descrita simultaneamente por várias dimensões:

| Dimensão | Pergunta correta | Exemplos |
|---|---|---|
| Release | Como mudanças chegam ao sistema? | fixed, rolling, continuous delivery |
| Cadência | Com que frequência são promovidas? | periódica, snapshots, fluxo contínuo |
| Atualidade | Quão perto os componentes ficam do upstream? | conservadora, equilibrada, leading edge |
| Lifecycle | Por quanto tempo há manutenção e em qual escopo? | curto, LTS, enterprise |
| Compatibilidade | Que contratos são preservados? | API, ABI, kABI, formato de configuração |
| Mutabilidade | Como o sistema-base pode ser alterado? | tradicional, controlado, read-only |
| Transação | Como uma mudança é ativada e revertida? | pacote a pacote, deployment, generation |
| Declaração | O operador descreve ações ou estado desejado? | imperativo, declarativo |
| Proveniência | Quem construiu e mantém o software? | repositório oficial, terceiro, upstream |
| Operação | Como o ambiente é provisionado e governado? | host manual, imagem, fleet, GitOps |

Exemplo:

~~~text
Fedora Silverblue 44
├── release: fixa, seguindo Fedora 44
├── atualidade: leading edge
├── lifecycle: aproximadamente 13 meses
├── sistema-base: image-based com rpm-ostree
├── ativação: deployment transacional
└── aplicações gráficas: preferencialmente desacopladas por Flatpak
~~~

Outro exemplo:

~~~text
Ubuntu 26.04 LTS
├── release: fixa
├── lifecycle: suporte padrão até abril de 2031
├── manutenção: atualizações controladas e correções retroportadas quando aplicável
├── sistema-base: tradicional, baseado em pacotes
└── ferramentas: APT sobre pacotes dpkg
~~~

Pergunte “como esta opção se comporta em cada dimensão relevante?”, não “qual é o tipo dela?”.

[Voltar ao índice](#indice)

---

<a id="02-modelo-de-release-e-cadencia"></a>

## 2. Modelo de release e cadência

<a id="02-01-fixed-release"></a>

### 2.1 Fixed Release

Uma Fixed Release publica versões identificáveis. Durante o lifecycle, a versão recebe correções e atualizações segundo a política do projeto; a passagem para a próxima geração é um evento explícito.

~~~text
Versão N ── correções e manutenção ──► fim do suporte
     │
     └──────── upgrade planejado ───► Versão N+1
~~~

Vantagens comuns:

- janela de suporte conhecida;
- homologação por versão;
- mudanças estruturais agrupadas;
- documentação e caminhos de upgrade definidos.

Custos comuns:

- migrações periódicas;
- componentes que podem permanecer em uma major version por bastante tempo;
- diferença entre o lifecycle do sistema e o lifecycle de aplicações externas.

Fixed não significa automaticamente conservadora, antiga ou estável. Fedora e RHEL usam releases fixas, mas atendem objetivos e cadências muito diferentes.

<a id="02-02-rolling-release"></a>

### 2.2 Rolling Release

Uma Rolling Release promove pacotes ou snapshots continuamente. O sistema instalado evolui sem uma sucessão tradicional de upgrades completos N para N+1.

~~~text
snapshot A ─► snapshot B ─► snapshot C ─► estado atual
~~~

Vantagens comuns:

- software recente;
- menos migrações grandes e espaçadas;
- boa adequação a hardware e stacks em rápida evolução.

Custos comuns:

- mudanças menores, porém mais frequentes;
- necessidade de acompanhar avisos do projeto;
- risco operacional quando o host fica meses sem atualizar;
- maior dependência de snapshot, rollback e disciplina de manutenção.

Rolling não é sinônimo de Testing, Unstable ou Bleeding Edge. O termo descreve fluxo de entrega, não qualidade.

<a id="02-03-slow-ou-curated-rolling"></a>

### 2.3 Slow ou Curated Rolling

“Slow rolling” e “curated rolling” são descrições úteis, mas não constituem um padrão universal. Projetos podem atrasar, agrupar ou submeter snapshots a testes adicionais antes da promoção.

O openSUSE Slowroll é um exemplo concreto. O Tumbleweed também usa snapshots integrados e testados, embora permaneça uma rolling de alta atualidade.

Não coloque fixed, rolling e bleeding edge numa única régua: os dois primeiros descrevem release; bleeding edge descreve atualidade e risco.

<a id="02-04-point-release"></a>

### 2.4 Point Release

Uma point release consolida correções e fornece mídia de instalação atualizada dentro de uma mesma família.

~~~text
Debian 13.0 ─► 13.1 ─► 13.2 ─► ... ─► 13.6
~~~

Na maioria dos casos, instalar 13.6 e manter uma instalação 13 atualizada levam ao mesmo conjunto de repositórios. A point release não cria necessariamente uma nova geração do produto.

Distribuições rolling também publicam ISOs numeradas. A ISO Arch 2026.08.01 e a imagem Kali 2026.2 são mídias datadas; não transformam o sistema instalado em uma release fixa.

<a id="02-05-branches-stable-testing-e-unstable"></a>

### 2.5 Branches: stable, testing e unstable

Esses nomes pertencem ao processo de cada projeto. No Debian, a promoção conceitual é:

~~~text
unstable (sid) ─► testing ─► stable
~~~

No Fedora existem releases e Rawhide. No openSUSE existem Leap, Tumbleweed e Slowroll. No Kali, o fluxo principal é kali-rolling.

Portanto, são falsas as equivalências:

- rolling = testing;
- LTS = stable em qualquer distribuição;
- bleeding edge = unstable;
- point release = modelo oposto a fixed.

<a id="02-06-leading-edge-e-bleeding-edge"></a>

### 2.6 Leading edge e bleeding edge

São rótulos informais e contextuais:

- leading edge: adoção rápida de tecnologias novas, com integração para uso regular;
- bleeding edge: máxima proximidade de novidades, aceitando maior probabilidade de regressão.

Um mesmo sistema pode ter kernel recente, biblioteca conservadora e aplicação muito recente. A análise precisa indicar o componente observado.

[Voltar ao índice](#indice)

---

<a id="03-lifecycle-lts-e-suporte-empresarial"></a>

## 3. Lifecycle, LTS e suporte empresarial

LTS significa que uma versão tem política de suporte prolongado. Enterprise lifecycle normalmente adiciona fases, escopos, compromissos contratuais, certificações e opções de extensão.

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

Lifecycle do sistema-base não prolonga automaticamente:

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

Não significa “sem bugs”. Também não prova adequação ao seu workload.

Uma distribuição pode ser estável no contrato de plataforma e ainda atualizar determinados componentes. Outra pode manter versões antigas, mas não oferecer o lifecycle necessário.

[Voltar ao índice](#indice)

---

<a id="05-backports-erratas-e-leitura-de-vulnerabilidades"></a>

## 5. Backports, erratas e leitura de vulnerabilidades

Backport é a adaptação de uma correção desenvolvida para uma versão mais nova a uma versão anterior mantida pela distribuição.

~~~text
upstream 5.1 com correção
          │
          └── adaptação downstream ─► pacote 5.0 + patch de segurança
~~~

Fornecedores também podem corrigir uma falha por rebase, patch próprio, desativação de recurso ou outra mitigação. “Backport” não é a única estratégia.

<a id="05-regra-operacional"></a>

### Regra operacional

Não determine vulnerabilidade comparando apenas a versão upstream exibida por <code>--version</code>.

Consulte:

1. advisory ou errata da distribuição;
2. tracker de CVEs do fornecedor;
3. changelog e release do pacote;
4. status do repositório e da arquitetura usada;
5. scanner que compreenda versões downstream e OVAL quando disponível.

Um scanner que compara somente versões upstream pode produzir falso positivo. Um pacote fora de suporte pode produzir falso negativo mesmo que seu número pareça recente.

[Voltar ao índice](#indice)

---

<a id="06-api-abi-e-kabi"></a>

## 6. API, ABI e kABI

<a id="06-api"></a>

### API

Application Programming Interface é o contrato usado por código-fonte para interagir com bibliotecas, serviços ou componentes.

<a id="06-abi"></a>

### ABI

Application Binary Interface inclui símbolos, calling conventions, layout de tipos e outros detalhes necessários para compatibilidade entre binários e bibliotecas.

<a id="06-kabi"></a>

### kABI

Kernel ABI pode ser relevante para módulos externos, drivers e produtos certificados. Políticas de kABI são específicas de fornecedor, versão, arquitetura e conjunto de símbolos; não representam promessa universal do kernel Linux.

Em ambientes corporativos, estabilidade pode significar:

- aplicação certificada continuar executando;
- módulo suportado continuar carregando;
- configuração manter semântica;
- biblioteca preservar símbolos;
- upgrade path documentado;
- automação não quebrar por alteração não anunciada.

Por isso, “mais recente” e “mais adequado” não são sinônimos.

[Voltar ao índice](#indice)

---

<a id="07-pacote-formato-gerenciador-repositorio-e-origem"></a>

## 7. Pacote, formato, gerenciador, repositório e origem

Esses termos não são intercambiáveis:

| Camada | Exemplo | Responde a... |
|---|---|---|
| Formato | deb, rpm, pkg.tar.zst | Como o artefato é estruturado? |
| Gerenciador de baixo nível | dpkg, rpm | Como arquivos e metadados são instalados? |
| Resolver/gerenciador | APT, DNF, Zypper, pacman | Como dependências e transações são calculadas? |
| Repositório | Debian Stable, Fedora Updates | De qual coleção o pacote vem? |
| Origem | projeto oficial, vendor, comunidade, terceiro | Quem construiu e mantém? |
| Modelo de aplicação | Flatpak, Snap, AppImage, container | Como a aplicação é distribuída e isolada? |

Uma assinatura válida comprova a relação com uma chave confiável; não prova qualidade, ausência de vulnerabilidades ou compatibilidade com sua política.

<a id="07-fontes-suplementares"></a>

### Fontes suplementares

AUR, PPA, COPR, OBS, repositórios de fornecedores e builds comunitárias resolvem necessidades legítimas, mas têm governança e garantias próprias.

- AUR distribui principalmente receitas de build; o usuário precisa revisar PKGBUILD e origem.
- PPA e COPR não herdam automaticamente o mesmo suporte do repositório oficial da distribuição.
- Flatpak desacopla a aplicação do sistema-base, mas permissões, runtime e manutenção continuam relevantes.
- AppImage simplifica portabilidade, mas atualização e proveniência precisam de processo.
- Repositório de vendor pode ser necessário para suporte, porém adiciona outro lifecycle.

<a id="07-praticas-seguras"></a>

### Práticas seguras

- prefira repositórios destinados à versão instalada;
- não misture famílias ou releases para “obter um pacote mais novo”;
- evite partial upgrades quando o projeto não os suporta;
- registre chaves, origem, prioridade e responsável;
- remova repositórios abandonados antes de upgrades maiores;
- inventarie pacotes instalados fora do gerenciador;
- valide SBOM, assinatura ou atestação quando o risco justificar.

[Voltar ao índice](#indice)

---

<a id="08-tradicional-imutavel-atomico-image-based-e-declarativo"></a>

## 8. Tradicional, imutável, atômico, image-based e declarativo

São propriedades relacionadas, mas independentes.

<a id="08-01-sistema-tradicional-baseado-em-pacotes"></a>

### 8.1 Sistema tradicional baseado em pacotes

Pacotes alteram o sistema em execução. Esse modelo é flexível, conhecido e adequado a inúmeros cenários, mas exige controle de drift e disciplina de configuração.

<a id="08-02-imutavel-ou-com-base-read-only"></a>

### 8.2 Imutável ou com base read-only

O sistema-base é modificado somente por caminhos controlados. Dados, configurações permitidas, containers e diretórios persistentes continuam mutáveis.

Imutabilidade:

- não significa que nada muda;
- não substitui controle de acesso;
- não corrige imagem vulnerável;
- não protege automaticamente dados do usuário.

<a id="08-03-atualizacao-atomica-ou-transacional"></a>

### 8.3 Atualização atômica ou transacional

A ativação deve apresentar um estado anterior ou novo, evitando um estado parcialmente aplicado no ponto de compromisso. A implementação pode usar deployment, snapshot, slot A/B ou generation.

Atomicidade da ativação não prova que a aplicação funcionará após a mudança. Migrações de banco, firmware, estado externo e compatibilidade ainda exigem teste.

<a id="08-04-image-based"></a>

### 8.4 Image-based

O sistema-base é entregue ou composto como imagem/deployment. Customização deve seguir o mecanismo suportado do projeto, não mutações arbitrárias.

<a id="08-05-declarativo"></a>

### 8.5 Declarativo

O operador descreve o estado desejado e a plataforma produz uma configuração. NixOS é o exemplo clássico, mas declaratividade também aparece em imagens, Kubernetes e ferramentas de provisionamento.

Declarativo não implica, por si só, base imutável. Imutável não implica configuração declarativa.

<a id="08-06-rollback-nao-e-backup"></a>

### 8.6 Rollback não é backup

Rollback de deployment costuma recuperar o sistema-base. Ele pode não reverter:

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

Uma recomendação de desktop precisa separar:

| Componente | Função | Exemplos |
|---|---|---|
| Desktop Environment | experiência integrada | GNOME, KDE Plasma, Xfce, Cinnamon |
| Window manager/compositor | posicionamento e composição | Mutter, KWin, Sway, Hyprland |
| Display Manager | tela e sessão de login | GDM, SDDM, LightDM |
| Protocolo/sessão | comunicação gráfica | Wayland, X11, XWayland |
| Pilha 3D | renderização e APIs | Mesa, Vulkan, driver proprietário |

O fato de uma combinação ser tecnicamente instalável não a torna a combinação mais integrada ou suportada pela edição.

Avalie separadamente:

- Wayland nativo e necessidade de XWayland;
- HDR, VRR, escala fracionária e múltiplos monitores;
- GPU híbrida, compute, CUDA/ROCm e passthrough;
- codecs e aceleração de vídeo;
- remote desktop e captura de tela;
- versão do kernel, Mesa e driver;
- sessão padrão e maturidade do desktop.

O quiz modela doze ambientes: KDE Plasma, GNOME, Cinnamon, Xfce, COSMIC, Budgie, LXQt, MATE, Pantheon, DDE, Trinity e Moksha. Window managers independentes permanecem fora desta edição.

[Voltar ao índice](#indice)

---

<a id="10-hardware-firmware-e-software-nao-livre"></a>

## 10. Hardware, firmware e software não livre

“Linux suporta meu hardware” é uma pergunta incompleta. Compatibilidade depende de:

- arquitetura e modelo exato;
- versão do kernel;
- firmware disponível;
- driver no kernel ou externo;
- Mesa/Vulkan ou driver proprietário;
- Secure Boot e assinatura de módulos;
- energia, suspensão e hibernação;
- suporte do fabricante e certificação;
- versão do sistema em que a combinação foi testada.

<a id="10-nao-livre-nao-e-um-unico-criterio"></a>

### Não livre não é um único critério

Separe:

- firmware necessário ao dispositivo;
- driver proprietário, como determinadas pilhas NVIDIA;
- codecs sujeitos a licenças e patentes;
- aplicações proprietárias;
- repositórios comunitários com conteúdo restrito.

Uma política pode aceitar firmware e rejeitar aplicações proprietárias. O recomendador trata esses itens de forma independente.

<a id="10-checklist-minimo"></a>

### Checklist mínimo

1. Inicialize uma mídia live quando aplicável.
2. Teste rede, áudio, vídeo, armazenamento, suspensão e retomada.
3. Confirme boot com Secure Boot no estado realmente desejado.
4. Teste monitor externo, dock, GPU híbrida e aceleração.
5. Verifique firmware e BIOS/UEFI disponíveis.
6. Confirme a matriz do vendor para workloads certificados.

[Voltar ao índice](#indice)

---

<a id="11-seguranca-hardening-compliance-e-certificacao"></a>

## 11. Segurança, hardening, compliance e certificação

São quatro camadas distintas:

| Camada | Objetivo | Evidência típica |
|---|---|---|
| Patching | corrigir vulnerabilidades conhecidas | advisory, errata, pacote corrigido |
| Hardening | reduzir superfície e privilégio | configuração, política MAC, benchmark |
| Compliance | demonstrar aderência a controles | scan, evidência, exceção aprovada |
| Certificação | avaliação formal em escopo definido | certificado e configuração validada |

<a id="11-selinux-e-apparmor"></a>

### SELinux e AppArmor

Ter o pacote instalado não prova que o MAC está ativo, em enforcing, com política adequada e cobertura para o workload. Também não substitui permissões Unix, capabilities, namespaces ou seccomp.

<a id="11-secure-boot-e-measured-boot"></a>

### Secure Boot e measured boot

- Secure Boot valida componentes autorizados na cadeia de inicialização.
- Measured boot registra medições, normalmente em TPM, para verificação ou attestation.
- Criptografia de disco protege dados em repouso.

São controles complementares, não equivalentes.

<a id="11-fips-cis-stig-e-common-criteria"></a>

### FIPS, CIS, STIG e Common Criteria

- modo FIPS não equivale automaticamente a módulo formalmente validado;
- certificado FIPS possui versão, módulo, binário, plataforma e configuração específicas;
- perfil CIS/STIG disponível não significa host já conforme;
- derivado ou clone não herda automaticamente certificação do fornecedor de origem;
- a auditoria precisa confirmar o escopo aplicável na data de implantação.

<a id="11-live-patching"></a>

### Live patching

Live kernel patching reduz a urgência de alguns reboots, mas não cobre toda correção, microcode, firmware, bootloader ou mudança de userspace. Alta disponibilidade depende da arquitetura do serviço, não apenas do kernel.

[Voltar ao índice](#indice)

---

<a id="12-storage-criptografia-recuperacao-e-backup"></a>

## 12. Storage, criptografia, recuperação e backup

Filesystem e resultado operacional não são sinônimos.

| Necessidade | Perguntas de validação |
|---|---|
| Integridade | há checksum de dados e metadados? scrub? correção com redundância? |
| Snapshot | é consistente com a aplicação? replica? possui retenção? |
| RAID | quais falhas tolera? como ocorre rebuild? |
| Criptografia | cobre root, dados, swap e hibernação? como é a recuperação de chave? |
| Expansão | online ou offline? quais limites e riscos? |
| Boot | o instalador e o bootloader suportam o desenho? |
| Compartilhamento | NFS/SMB/iSCSI são parte do produto ou configuração manual? |
| Backup | há cópia independente, testada e fora do domínio de falha? |

Regras essenciais:

- RAID não é backup;
- snapshot não é backup por definição;
- criptografia sem recuperação de chaves pode converter falha em perda definitiva;
- rollback do sistema não garante consistência do banco;
- deduplicação, compressão e copy-on-write têm custos;
- suporte a ZFS, Btrfs ou XFS varia entre instalador, kernel, vendor e arquitetura.

Appliances como TrueNAS e Unraid podem ser mais adequados que um Linux generalista quando storage é o produto principal. Em outros casos, um servidor generalista com equipe capaz de operar a pilha pode ser melhor.

[Voltar ao índice](#indice)

---

<a id="13-virtualizacao-containers-e-cloud-native"></a>

## 13. Virtualização, containers e cloud-native

Não trate “roda container” como uma única capacidade.

| Papel | Exemplos de requisitos |
|---|---|
| Application container host | Docker/Podman, containerd, rootless, SELinux/AppArmor |
| System containers | LXC/LXD/Incus, nesting, rede e storage |
| Hypervisor | KVM/QEMU, UI/API, live migration, HA, passthrough |
| HCI | compute, storage e rede integrados em cluster |
| Kubernetes node OS | superfície mínima, API, upgrade coordenado, imutabilidade |
| Desktop de desenvolvimento | engine local, toolboxes, integração IDE |

Proxmox VE, Harvester, Talos, Fedora CoreOS, Flatcar, Bottlerocket, Google Container-Optimized OS e Azure Container Linux não são substitutos diretos de uma estação Linux generalista. Eles concorrem quando o papel correspondente é selecionado.

Imagens otimizadas por provedor podem oferecer melhor integração, mas aumentam acoplamento. Para multicloud, portabilidade pode valer mais que integração máxima.

[Voltar ao índice](#indice)

---

<a id="14-rede-identidade-frota-observabilidade-e-continuidade"></a>

## 14. Rede, identidade, frota, observabilidade e continuidade

<a id="14-rede-e-dataplane"></a>

### Rede e dataplane

Separe:

- configuração de interfaces;
- firewall/NAT;
- bridge, bond e VLAN;
- VRF;
- VPN;
- roteamento dinâmico;
- EVPN/VXLAN;
- SR-IOV, DPDK e XDP;
- switch NOS e hardware/ASIC suportado.

Instalar FRR num servidor não transforma automaticamente esse host em appliance comparável a VyOS ou em switch NOS comparável a SONiC/Cumulus.

<a id="14-identidade"></a>

### Identidade

AD, LDAP, Kerberos, SSSD, PAM e FreeIPA/IdM não são sinônimos.

- LDAP fornece diretório/consulta.
- Kerberos fornece autenticação por tickets.
- SSSD integra identidades, cache e políticas no cliente.
- PAM organiza fluxos de autenticação.
- FreeIPA/IdM combina identidade e políticas para ambientes Linux.
- Trust AD–IdM exige desenho de DNS, Kerberos, nomes e ranges de IDs.

“Entrar no domínio” não prova aplicação completa de GPO nem gestão de frota.

<a id="14-provisionamento-e-frota"></a>

### Provisionamento e frota

Automatizar um host não equivale a governar milhares. Avalie:

- instalação desassistida e PXE;
- cloud-init ou mecanismo nativo de imagem;
- zero-touch;
- configuração por Ansible/Salt;
- conteúdo e mirrors;
- inventário e compliance;
- rollout gradual e rollback;
- air gap e proxies;
- console central e suporte multi-distribuição.

<a id="14-observabilidade"></a>

### Observabilidade

Logs, métricas, traces, profiling e telemetria de segurança/rede são sinais diferentes. Uma stack portátil baseada em OpenTelemetry ou Prometheus não possui necessariamente o mesmo grau de integração de uma plataforma nativa.

<a id="14-continuidade"></a>

### Continuidade

Separe:

- live kernel patching;
- atualização de userspace sem reinício;
- reboot rápido;
- reboot coordenado;
- HA/failover;
- quorum e fencing;
- rolling maintenance;
- live migration.

Disponibilidade é propriedade da arquitetura completa. Um sistema com live patch não corrige banco single-node, storage sem redundância ou dependência externa única.

[Voltar ao índice](#indice)

---

<a id="15-metodo-de-decisao-auditavel"></a>

## 15. Método de decisão auditável

<a id="15-etapa-1-descreva-o-workload"></a>

### Etapa 1 — descreva o workload

Registre:

- função principal e funções secundárias;
- ambiente: laptop, workstation, bare metal, VM, cloud, edge ou appliance;
- criticidade, RTO e RPO;
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

Inclua data, responsáveis, requisitos, candidatos eliminados, evidências, riscos aceitos, plano de saída e data de revisão.

[Voltar ao índice](#indice)

---

<a id="16-perfis-de-referencia"></a>

## 16. Perfis de referência

Esta tabela orienta a triagem; não substitui os requisitos.

| Cenário dominante | Candidatos iniciais | O que pode mudar a decisão |
|---|---|---|
| Desktop simples, familiar ou migração do Windows | Linux Mint, Zorin OS, Ubuntu LTS | hardware, preferência de DE, formatos de aplicativos e lifecycle |
| Workstation moderna e produtividade | Fedora Workstation, Pop!_OS, Ubuntu, openSUSE Tumbleweed | GPU, toolchain, COSMIC/GNOME/KDE e mudança tolerada |
| Gaming em desktop ou handheld | Bazzite, Nobara, Pop!_OS | GPU, Steam Gaming Mode, anti-cheat, dependência de X11 e modelo de atualização |
| Servidor comunitário conservador | Debian Stable, Ubuntu LTS | suporte comercial, certificação, stack |
| Enterprise com suporte do fabricante e certificações formais | RHEL com assinatura e suporte empresarial, SLES, Ubuntu Pro, Oracle Linux com suporte comercial | SLA, ISV, cloud, matriz de certificação, compliance e contrato |
| Ecossistema RHEL sem contrato obrigatório | AlmaLinux, Rocky Linux, Oracle Linux sem suporte contratado | compatibilidade exigida, kernel, governança, suporte opcional, certificações e lifecycle |
| Rolling com snapshots integrados | openSUSE Tumbleweed | hardware e tolerância operacional |
| Rolling derivada do Arch com instalação guiada | EndeavourOS, Manjaro | proximidade do Arch, curadoria de repositórios, AUR e responsabilidade de manutenção |
| Controle manual e aprendizado profundo | Arch Linux | tempo disponível e responsabilidade de manutenção |
| Desktop atômico | Fedora Atomic Desktops, Bazzite | workload geral ou gaming, compatibilidade com apps e fluxo de customização |
| Estado declarativo/reproduzível | NixOS | curva de aprendizado e ecossistema |
| Pentest autorizado | Kali, Parrot | escopo legal e especialidade |
| Hypervisor dedicado | Proxmox VE, Harvester | arquitetura de cluster, storage, suporte |
| Kubernetes OS | Talos, Fedora CoreOS, Flatcar | provedor, API, lifecycle e operação |
| NAS/appliance de storage | TrueNAS, Unraid | ZFS, apps, suporte, HA e escala |
| Router/firewall | VyOS, IPFire, OpenWrt | throughput, hardware, protocolos, suporte |
| Switch fabric | SONiC, NVIDIA Cumulus Linux | ASIC e matriz de hardware suportado |

<a id="16-como-interpretar-as-distribuicoes-desktop-destacadas"></a>

### Como interpretar as distribuições desktop destacadas

- **Linux Mint, Zorin OS e Pop!_OS** compartilham herança Ubuntu, mas não são equivalentes: variam em desktop, integração, calendário próprio, formatos habilitados, suporte de hardware e política de atualização.
- **EndeavourOS e Manjaro** são rolling e derivadas do ecossistema Arch, porém seguem políticas diferentes. EndeavourOS permanece mais próximo dos repositórios Arch; Manjaro mantém branches e curadoria próprias. A facilidade do instalador não elimina a responsabilidade operacional de uma rolling.
- **Bazzite** é uma imagem customizada baseada em Fedora Atomic, orientada especialmente a gaming, handhelds e HTPCs. Não deve ser descrita como “Arch rolling” nem operada como um Fedora tradicional baseado em `dnf`.
- A presença nesta tabela significa **candidato inicial plausível para um cenário**, não recomendação universal nem prêmio de popularidade.

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

Snapshot factual desta edição: **15 de agosto de 2026**, fuso America/Sao_Paulo.

<a id="17-politica-de-versionamento-da-serie-0"></a>

### Política de versionamento da série 0

A edição **0.1.0** é a linha de base da primeira publicação pública. Versões internas anteriores não integram o histórico público.

A numeração adapta a convenção do [Semantic Versioning 2.0.0](https://semver.org/lang/pt-BR/): a major zero representa desenvolvimento inicial, e `0.1.0` é o ponto de partida recomendado para a primeira edição dessa fase.

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

<a id="17-duas-correcoes-temporais-importantes"></a>

### Duas correções temporais importantes

- Em 15/08/2026, a edição estável do TUXEDO OS ainda é baseada em Ubuntu. A transição para Debian Testing foi anunciada, com open beta prevista para 16/08/2026; beta não deve ser descrita como release estável nem recomendada para produção. Consulte o [anúncio da nova base](https://www.tuxedocomputers.com/en/A-new-foundation-for-TUXEDO-OS-Switching-to-Debian.tuxedo) e o [anúncio da open beta](https://www.tuxedocomputers.com/en/The-time-has-come-The-open-beta-for-TUXEDO-OS-is-just-around-the-corner.tuxedo).
- No Google Container-Optimized OS, o milestone 133 está em beta no snapshot; o catálogo usa 129 LTS para recomendação estável. Consulte as [release notes oficiais do COS](https://docs.cloud.google.com/container-optimized-os/docs/release-notes).

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

> Exemplos educacionais. Antes de um upgrade de produção, leia as release notes, preserve backup recuperável e ensaie rollback.

<a id="18-debian-ubuntu-e-derivados"></a>

### Debian, Ubuntu e derivados

Atualização normal de pacotes:

~~~bash
sudo apt update
sudo apt upgrade
~~~

<code>apt full-upgrade</code> pode remover ou substituir pacotes para resolver dependências. Use quando a documentação do projeto indicar e revise o plano antes de confirmar. Upgrade entre releases possui procedimento próprio; não é sinônimo de executar esse comando às cegas.

<a id="18-fedora-e-rhel"></a>

### Fedora e RHEL

~~~bash
sudo dnf upgrade
~~~

Mudança de major release ou operação offline deve seguir a documentação da versão e do produto.

<a id="18-opensuse"></a>

### openSUSE

Leap, para atualização normal:

~~~bash
sudo zypper update
~~~

Tumbleweed, para sincronizar com o snapshot:

~~~bash
sudo zypper dup
~~~

<a id="18-arch-linux"></a>

### Arch Linux

~~~bash
sudo pacman -Syu
~~~

Arch não suporta partial upgrades. Leia avisos do projeto antes de intervenções que exigem ação manual.

<a id="18-fedora-atomic-desktops"></a>

### Fedora Atomic Desktops

~~~bash
rpm-ostree status
rpm-ostree upgrade
rpm-ostree rollback
~~~

Rollback seleciona deployment anterior; dados mutáveis permanecem fora dessa garantia.

<a id="18-nixos-com-channels"></a>

### NixOS com channels

~~~bash
sudo nix-channel --update
sudo nixos-rebuild switch --upgrade
~~~

<a id="18-nixos-com-flakes"></a>

### NixOS com flakes

~~~bash
nix flake update
sudo nixos-rebuild switch --flake .#HOST
~~~

<code>nixos-rebuild switch</code> sozinho reconstrói a partir das entradas já fixadas; não atualiza automaticamente todas as origens de um flake.

[Voltar ao índice](#indice)

---

<a id="19-metodologia-do-linux-distro-advisor"></a>

## 19. Metodologia do Linux Distro Advisor

<a id="19-o-que-o-catalogo-contem"></a>

### O que o catálogo contém

A edição 0.1.0 modela **95 candidatos**, não “95 distribuições puras”. O conjunto inclui distribuições, edições, canais, sistemas image-based, network operating systems e appliances Linux.

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

Não requer backend, cadastro, npm, framework ou CDN. Respostas não são transmitidas pelo código do projeto. Preferência de tema pode ser armazenada localmente quando o navegador permite; o quiz continua funcional se o armazenamento estiver bloqueado.

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
| Imutável significa inalterável | O sistema-base muda por caminhos controlados; dados continuam mutáveis. |
| Atômico e imutável são iguais | Uma propriedade descreve compromisso da mudança; a outra, mutabilidade da base. |
| Snapshot é backup | Pode compartilhar o mesmo domínio de falha e não ser application-consistent. |
| RAID é backup | RAID melhora disponibilidade diante de algumas falhas; não substitui cópia independente. |
| Secure Boot criptografa o disco | Ele verifica a cadeia de boot; criptografia é outro controle. |
| FIPS mode significa certificado | Certificação tem escopo formal e específico. |
| SELinux instalado significa protegido | Modo, política e cobertura precisam ser confirmados. |
| APT é o formato do pacote | APT resolve e gerencia; deb é o formato; dpkg é a camada de baixo nível. |
| Flatpak sempre é mais seguro | Sandbox e permissões ajudam, mas origem, runtime e manutenção continuam relevantes. |
| Entrar no AD significa aplicar toda GPO | Autenticação e gestão de políticas são capacidades diferentes. |
| Live patch elimina reboots | Apenas mudanças elegíveis são cobertas. |
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

1. atualizar snapshot e fontes primárias;
2. executar auditoria estrutural do catálogo;
3. rodar cenários determinísticos;
4. testar teclado, mobile, impressão e tema;
5. revisar links;
6. registrar mudanças de pesos e candidatos;
7. criar tag da edição.

<a id="idioma-e-convencoes"></a>

### Idioma e convenções

- idioma editorial: português do Brasil;
- nomes oficiais de produtos são preservados;
- termos em inglês recebem definição no primeiro uso;
- datas usam ISO 8601 em metadados;
- comandos e nomes de arquivo permanecem no formato original;
- MUST, SHOULD e MAY só devem ser usados quando seu significado normativo estiver declarado.

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

| Termo | Definição |
|---|---|
| ABI | *Application Binary Interface*: contrato que permite a um binário interagir com bibliotecas, kernel ou outros componentes sem recompilação, dentro das compatibilidades declaradas. |
| API | *Application Programming Interface*: interface pela qual código solicita funções ou dados de outro componente. API de código-fonte não garante, por si só, compatibilidade binária. |
| Atomic update | atualização cuja ativação é tratada como uma unidade consistente: o novo estado é aplicado por inteiro ou o estado anterior permanece recuperável. Não significa ausência de falhas depois do boot. |
| Backport | adaptação de uma correção ou funcionalidade mais nova para uma versão anterior ainda mantida, normalmente sem importar toda a versão upstream. |
| Bleeding edge | rótulo informal para software extremamente próximo do desenvolvimento upstream, com maior novidade e maior exposição a regressões. Não é sinônimo obrigatório de rolling release. |
| Bootloader | componente que localiza e inicia o kernel ou um deployment do sistema, como GRUB ou systemd-boot. |
| Certificação | avaliação formal de produto, versão, configuração e escopo contra um programa ou padrão definido. Não é sinônimo de hardening nem de compliance contínuo. |
| Compliance | conformidade demonstrável com controles, políticas ou normas aplicáveis ao ambiente. Depende de configuração, evidências e operação, não apenas do nome da distribuição. |
| Compositor | componente que combina superfícies de aplicações e produz a imagem final da tela. Em Wayland, normalmente também exerce o papel de display server e pode incorporar o gerenciamento de janelas. |
| Control plane | conjunto de funções que calcula, decide ou programa como os recursos devem operar; em redes, instrui o dataplane sobre como encaminhar tráfego. |
| CVE | identificador público para uma vulnerabilidade conhecida; o identificador não informa sozinho severidade, explorabilidade nem se o pacote downstream continua vulnerável. |
| Dataplane | também chamado *data plane* ou *forwarding plane*: caminho que processa e encaminha pacotes ou dados conforme regras programadas pelo control plane. |
| Deployment | estado instalável ou ativável do sistema-base. Em sistemas atômicos, deployments anterior e novo podem coexistir para seleção no boot ou rollback. |
| Desktop environment (DE) | conjunto integrado de shell, sessão, painel, configurações, aplicativos e componentes gráficos, como GNOME, KDE Plasma, Cinnamon ou COSMIC. |
| Display manager | serviço de login gráfico que autentica o usuário e inicia uma sessão, como GDM, SDDM ou LightDM. Não é o display server nem o desktop environment. |
| Display server | componente que coordena apresentação gráfica e entrada entre aplicações e sessão. No X11 esse papel costuma ser do X.Org Server; em Wayland, o compositor exerce esse papel. |
| Downstream | projeto ou fornecedor que integra, empacota, corrige e mantém software recebido de um projeto upstream. |
| EOL | *End of Life*: fim do período de manutenção ou suporte declarado para uma versão, canal ou produto. |
| Firmware | software de baixo nível executado ou carregado em dispositivos, como GPU, Wi-Fi, SSD e placas de rede; sua disponibilidade pode determinar suporte de hardware. |
| Fixed Release | modelo com versões identificáveis, conteúdo-base estabilizado e migrações explícitas entre releases principais. |
| Generation | estado versionado e reproduzível, comum em sistemas declarativos como NixOS. |
| Hardening | redução deliberada da superfície de ataque por configuração, remoção, restrição e aplicação de controles. Hardening não substitui patching. |
| Image-based | modelo em que o sistema-base é construído, distribuído e atualizado predominantemente como imagem coerente, em vez de depender apenas de mutações pacote a pacote no host. |
| Immutable | termo operacional para uma base protegida contra alterações ad hoc e modificada por caminhos controlados. Não significa que nenhum byte possa ser alterado em qualquer circunstância. |
| Init system | primeiro sistema de espaço de usuário responsável por iniciar, supervisionar e encerrar serviços e sessões, como systemd, OpenRC, runit, s6 ou Dinit. |
| Kernel | núcleo que gerencia CPU, memória, dispositivos, processos, isolamento e interfaces fundamentais do sistema operacional. Uma distribuição é maior que seu kernel. |
| Lifecycle | sequência de fases de uma versão ou produto: lançamento, manutenção, suporte, possíveis extensões e EOL. Deve ser verificada por edição, canal e arquitetura. |
| LTS | *Long-Term Support*: política de manutenção prolongada cujo prazo e escopo variam por projeto, edição, pacote e contrato. LTS não possui duração universal. |
| MAC (*Mandatory Access Control*) | controle de acesso obrigatório aplicado por política, como SELinux ou AppArmor, além das permissões Unix tradicionais. Neste contexto, MAC não significa endereço de rede *Media Access Control*. |
| Measured boot | processo que registra medições criptográficas dos componentes de inicialização para posterior verificação ou atestação. É diferente de apenas bloquear código não autorizado. |
| Package manager | ferramenta que resolve, instala, atualiza e remove pacotes e suas dependências, como APT, DNF, Zypper ou pacman. Não deve ser confundida com formato de pacote ou repositório. |
| Patching | aplicação de correções de segurança, defeitos ou manutenção. Pode ocorrer por atualização de pacote, backport, live patch ou substituição de imagem. |
| Point release | consolidação identificada dentro de uma família de versão, normalmente reunindo correções, mídia nova e atualizações acumuladas. Não implica nova major release. |
| Proveniência | origem, autoria, processo de build, assinatura e cadeia de manutenção de um artefato. |
| Rebase | troca da referência do sistema-base para outra versão, imagem ou branch, preservando o que o modelo declara compatível. Não é sinônimo universal de upgrade in-place. |
| Release | estado publicado e identificável de um projeto. Pode ser uma versão fixa, uma mídia de instalação ou um snapshot; o significado depende do modelo adotado. |
| Repositório | origem organizada de pacotes, metadados ou imagens consumida pelas ferramentas de atualização. Repositório oficial, comunitário e de terceiro possuem cadeias de confiança diferentes. |
| Rollback | retorno a deployment, snapshot, generation ou versão anterior. Reverte estado coberto pelo mecanismo; não substitui backup de dados. |
| Rolling | adjetivo que indica evolução contínua, mas é ambíguo quando usado sozinho. Deve ser qualificado como rolling release, rolling update ou outro mecanismo. |
| Rolling Release | modelo no qual o sistema instalado evolui continuamente pelo fluxo de pacotes, sem upgrades periódicos obrigatórios para uma nova versão completa. ISOs datadas são snapshots de instalação. |
| Rolling update / rolling deployment | estratégia operacional que substitui ou reinicia instâncias em lotes, mantendo parte do serviço disponível. É diferente do modelo de distribuição rolling release. |
| SBOM | *Software Bill of Materials*: inventário estruturado dos componentes e dependências presentes em um artefato de software. |
| Secure Boot | mecanismo de boot que verifica assinaturas e impede a execução de componentes não autorizados na cadeia coberta pela política. Não mede nem atesta sozinho o estado completo do sistema. |
| Snapshot | estado capturado em determinado ponto. Pode cobrir sistema de arquivos, volume, dataset, repositório ou conjunto de pacotes; seu escopo define o que pode ser recuperado. |
| Upstream | projeto original ou anterior na cadeia que desenvolve e publica o software recebido por distribuidores downstream. |
| Wayland | protocolo e arquitetura para comunicação entre aplicações e um compositor. Não é um desktop environment nem um único servidor universal; cada ambiente utiliza uma implementação de compositor. |
| Workload | conjunto de aplicações, serviços, processos, dados e padrões de uso que cumpre uma função e impõe requisitos mensuráveis ao sistema. “Desktop”, “banco de dados” e “nó Kubernetes” são classes amplas, não especificações completas. |
| X11 | versão 11 do protocolo X Window System, baseado em arquitetura cliente-servidor e extensões. X11 é o protocolo; X.Org Server é sua implementação mais comum em Linux. |
| X.Org Server | implementação amplamente usada do servidor X11. Não deve ser tratado como sinônimo perfeito de todo o protocolo X11. |
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
- [openSUSE Slowroll](https://en.opensuse.org/Portal:Slowroll)
- [Arch system maintenance](https://wiki.archlinux.org/title/System_maintenance)
- [NixOS release notes](https://nixos.org/manual/nixos/stable/release-notes)
- [Linux Mint — versões suportadas](https://linuxmint.com/download_all.php)
- [Zorin OS — versões e suporte](https://zorin.com/os/details/)
- [Pop!_OS 24.04 LTS — anúncio oficial](https://blog.system76.com/post/pop-os-letter-from-our-founder)
- [EndeavourOS — notícias e snapshots de instalação](https://endeavouros.com/news/)
- [Manjaro — anúncios de releases e stable updates](https://forum.manjaro.org/c/announcements/11)
- [Bazzite — atualizações, rollbacks e rebases](https://docs.bazzite.gg/Installing_and_Managing_Software/Updates_Rollbacks_and_Rebasing/)

<a id="24-arquitetura-e-operacao"></a>

### Arquitetura e operação

- [Fedora Atomic updates, upgrades and rollbacks](https://docs.fedoraproject.org/en-US/atomic-desktops/updates-upgrades-rollbacks/)
- [Nix reference manual](https://nixos.org/manual/nix/stable/)
- [Ubuntu AppArmor](https://documentation.ubuntu.com/security/apparmor/)
- [RHEL Security hardening](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/security_hardening/)
- [SUSE security certifications](https://www.suse.com/support/security/certifications/)
- [Google Container-Optimized OS release notes](https://docs.cloud.google.com/container-optimized-os/docs/release-notes)
- [Azure Container Linux overview](https://learn.microsoft.com/en-us/azure/azure-linux/azure-container-linux-overview)
- [Amazon Linux 2023 release notes](https://docs.aws.amazon.com/linux/al2023/release-notes/)

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
