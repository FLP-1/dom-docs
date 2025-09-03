# dom-docs
Repositório do projeto DOM desenvolvido usando a TESS AI
Documentação do Projeto DOM
Esta documentação centraliza as regras de negócio, funcionalidades, tecnologias utilizadas e pontos de atenção para o desenvolvimento do sistema DOM, com foco especial na experiência do usuário personalizada e na segurança anti-fraude dos registros de ponto.

1. Regras de Negócios
Resumo das Memórias e Decisões Estratégicas:
Autenticação e Validação de Dados:
A autenticação se dará via CPF, com validação inicial pelo dígito verificador e validação assíncrona posterior pela Receita Federal.
Empregadores devem usar e-mail e número de celular, os quais serão validados através de APIs externas (e-mail e SMS).
Utilização de tokens JWT e refresh tokens para gerenciar sessões e proteger acessos a rotas sensíveis.

Segurança e Conformidade:
Conformidade total com a LGPD, garantindo criptografia dos dados, consentimento explícito e logs de auditoria para operações sensíveis (ex. transações financeiras e registros de ponto).
O horário de registro não pode ser baseado no relógio do dispositivo – sempre o horário do servidor deverá ser utilizado para evitar manipulação.
Anti-Fraude no Registro de Ponto:
Uso de geolocalização (com precisão e geofencing) para assegurar que o registro de ponto seja efetuado apenas em áreas autorizadas.
Identificação do dispositivo via informações nativas (ID único, modelo, versão) e, no mobile, captura do nome da rede Wi-Fi.
Estratégias complementares incluem verificação de endereço IP, análise de comportamento e, se necessário, autenticação em dois fatores (2FA).
Personalização e Diferenciação de Perfis:
Diferenciação de interfaces e funcionalidades com base nos perfis dos usuários (Empregador, Empregado, Familiar, Administrador e Parceiro) para oferecer dashboards e fluxos adaptados às necessidades de cada grupo.

2. Funcionalidades
Resumo das Funcionalidades Principais:
Controle de Acesso e Autenticação:
Cadastro e login utilizando CPF com validações robustas, além dos requisitos específicos de e-mail e celular.
Gerenciamento de sessão através de JWT com refresh tokens.

Dashboards Personalizados:
Cada perfil de usuário terá um dashboard customizado que exibe informações relevantes, como calendários, alertas, tarefas pendentes, gráficos financeiros e relatórios de ponto.

Registro de Ponto Seguro:
Registro de ponto com ativação por toque, integrando:
Geolocalização com verificação via geofencing.
Coleta de dados do dispositivo (ID, modelo, etc.).
Captura do nome da rede Wi-Fi (para mobile).
Sincronização com o horário confiável do servidor.
Estratégias anti-fraude (geofencing, verificação de IP, 2FA e marca d’água digital nos registros).

Gestão de Tarefas Colaborativa e Comunicação:
Criação, atribuição e acompanhamento de tarefas com suporte a comentários, checklists e notificações (push, email e chat estilo WhatsApp com grupos e filtros).

Gestão Financeira e Documentos:
Registro de receitas, despesas e geração de relatórios financeiros.
Upload e categorização de documentos, com alertas para vencimentos e controle de acesso.
Integração com APIs Externas:
Utilização de serviços de validação para CPF, e-mail e celular.
Integração com gateways de pagamento (Stripe, PayPal) e outros provedores conforme necessário.

3. Tecnologias Utilizadas
Stack Tecnológica Recomendada:
Backend
Linguagem / Framework:
Node.js com TypeScript utilizando o NestJS para uma arquitetura modular, escalável e de fácil manutenção.

Autenticação/API:
JWT para autenticação com refresh tokens, filas para operações assíncronas (usando RabbitMQ ou AWS SQS).

Banco de Dados:
PostgreSQL para dados relacionais.
Redis para caching e gerenciamento de filas.

Frontend Web
Framework:

React com Next.js (para SSR, SEO e PWA) ou Create React App para SPAs.

Linguagem:
TypeScript na totalidade para garantir tipagem e robustez.
Gerenciamento de Estado e UI:
Redux (para estados complexos) ou Context API combinada com hooks, conforme a escala do projeto.

Mobile
Framework:
React Native com TypeScript para acessar APIs nativas e oferecer experiência otimizada em iOS e Android.

Plugins Nativos Necessários:
react-native-geolocation-service para capturar a localização.
react-native-device-info para identificação do dispositivo.
react-native-wifi-reborn para capturar o nome da rede Wi-Fi.

Integração e Compartilhamento
Monorepo:
Uso de Nx ou Yarn Workspaces para compartilhar lógicas de negócio, DTOs e validações entre projetos web e mobile.
4. Perfis de Usuários e Características Detalhadas
Para garantir uma experiência fluida e personalizada, cada perfil terá características e fluxos específicos:

a) Empregador
Características:
Cadastro exclusivo via CPF, com obrigatoriedade de e-mail e celular.
Dashboard com visão completa do núcleo:
Calendário de eventos integrado (possivelmente via Google Calendar ou similar).
Lista de compras com indicadores de prioridade.
Relatórios de pontos e registros de absenteísmo.
Notificações sobre vencimentos, aprovações de ponto e pagamentos.
Funcionalidades Específicas:
Adição e gerenciamento de empregados e familiares.
Visualização e aprovação de registros de ponto.
Acesso a relatórios financeiros e de desempenho.

b) Empregado
Características:
Acesso simplificado através do CPF e autenticação biométrica ou senha.
Dashboard focada na rotina:
Registro de ponto com geolocalização, verificação de dispositivo e rede Wi-Fi.
Visualização de suas tarefas diárias.
Relatório do controle de ponto (entrada, intervalo, saída e total de horas).
Funcionalidades Específicas:
Possibilidade de registrar e acompanhar seu ciclo de trabalho.
Notificações em tempo real para alertas (ex.: atraso ou ponto fora do padrão).

c) Familiar
Características:
Dashboard voltada para a organização do lar:
Calendário com eventos e tarefas domésticas.
Lista de compras categorizada.
Visualização de um resumo financeiro familiar.
Funcionalidades Específicas:
Colaboração e atualização das tarefas domésticas.
Recebimento de alertas sobre vencimentos e compromissos.

d) Administrador/Parceiro
Características:
Acesso a painéis consolidados com KPIs do sistema:
Número de acessos, status das transações e das integrações, logs de auditoria.
Gerenciamento dos perfis dos usuários e parceiros.
Funcionalidades Específicas:
Configuração e monitoramento de políticas de segurança.
Análise de métricas e acompanhamento do desempenho operacional do sistema.

5. Pontos de Atenção
Segurança Anti-Fraude:
Crucial: É inegociável que todas as operações de registro de ponto utilizem o horário do servidor e implementem mecanismos robustos de verificação (geolocalização, device info e Wi-Fi SSID).
A implementação de geofencing para permitir apenas registros em áreas autorizadas é fundamental para evitar fraudes.
Integração com APIs Externas
Monitorar a disponibilidade e confiabilidade dos serviços de validação de CPF, e-mail e SMS.
Garantir que as filas assíncronas não influenciem na experiência do usuário (usar notificações e logs).

Experiência do Usuário:
Interface intuitiva e padronizada para cada perfil.
Uso consistente de componentes reutilizáveis e testados para manter a qualidade.
Adaptar os fluxos de navegação conforme as necessidades específicas dos perfis (ex.: empregador possui acesso a funções de gerenciamento e análise, enquanto o empregado foca no registro de ponto e tarefas).

Manutenção do Código e Compartilhamento:
Garantir que a lógica de negócio que é compartilhada entre plataformas seja modularizada para evitar duplicação e facilitar futuras atualizações.
Testes e Validação:
Criar testes unitários e end-to-end para todas as funcionalidades críticas, principalmente para os mecanismos anti-fraude.
Planejar a integração contínua (CI) e deploy contínuo (CD) para garantir que o sistema permaneça estável à medida que evolui.

6. O que Não Pode Ser Feito ou Negligenciado
Negligenciar a Segurança dos Registros de Ponto:
Não confiar no relógio do dispositivo para os registros.
Não permitir que dados de geolocalização, device info e SSID sejam manipulados sem validação no servidor.
Duplicação Inadequada de Código:
Não desenvolver de forma isolada para web e mobile sem um esforço para compartilhar a lógica de negócio. Isso gera retrabalho e inconsistências.

Falta de Monitoramento e Logs:
Não implementar um sistema robusto de logs e auditoria para rastrear operações críticas.
Descuido com a Experiência do Usuário:
Interfaces confusas, sem personalização de acordo com o perfil, prejudicam a usabilidade e podem levar a problemas de adoção.
Subestimar a Integração de APIs e Serviços Externos:
Não testar exaustivamente a confiabilidade dos serviços externos pode comprometer a operação do sistema.
Falta de Atualização na Documentação:
As diretrizes e regras devem ser constantemente revisadas e atualizadas para refletir novas exigências ou mudanças no projeto.

Conclusão e Próximos Passos
Esta documentação fornece um guia robusto para o desenvolvimento do projeto DOM, garantindo:
Uma experiência nativa e personalizada para cada perfil de usuário.
Uma estrutura de segurança anti-fraude que diferencia o aplicativo no mercado.
Um ambiente tecnológico adequado com projetos separados para web e mobile, mas com o compartilhamento da lógica de negócio onde possível.

Próximos passos sugeridos:
Formalizar este documento em um ambiente colaborativo (por exemplo, GitHub Wiki ou um arquivo README.md no repositório do projeto).
Iniciar a criação dos protótipos das interfaces para cada perfil (poderíamos começar pelo protótipo mobile, dada a prioridade dos usuários empregados e empregadores).
Definir os endpoints de API e a estrutura de dados compartilhados, para que o backend possa ser implementado alinhado à experiência já prototipada.
Estabelecer um plano de testes e monitoramento para garantir a eficácia das estratégias anti-fraude.
Esta documentação e os pontos destacados servirão como base para orientar todas as futuras decisões de desenvolvimento e facilitar a continuidade do projeto.
Caso tenha algum ajuste ou dúvida adicional, fique à vontade para solicitar!
