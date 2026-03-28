# Teste Técnico Human Academy
Documentação do teste e dos prompts feitos para a Lovable IA sobre o teste técnico para vaga de Vibecoding na empresa Human Academy

Site Lovable: https://fast-approver.lovable.app
<br>
<br>

## Fast Approve
Uma solução assíncrona, login-less e em tempo real para aprovação de criativos e campanhas de Social Media entre agências e clientes.
<br>
<br>

<img width="1459" height="573" alt="image" src="https://github.com/user-attachments/assets/b7d7b8bc-5dc4-4bb5-8968-f9a71ced4df6" />
<br>
<br>

## A Ideia
O Fast Approve é um aplicativo web projetado para facilitar a relação entre o time de Criação (Designers e Social Medias) e o Cliente final. Ele substitui a informalidade e a desorganização das aprovações por aplicativos de mensagens com um fluxo estruturado.
<br>
<br>

## O Problema 
A rotina de aprovação de criativos e copys em agências de publicidade pode ser caótica e descentralizada quando realizada apenaspor mensagens.
Dessa forma, o fluxo padrão pode apresentar falhas como:

- Perda de Histórico: Peças de design são enviadas por WhatsApp, e os pedidos de alteração (ex: "Aumenta a logo", "Muda a data do evento na legenda") se perdem em áudios ou mensagens informais.
- Falta de Rastreabilidade: Não há um painel claro para a agência saber o que está travando a pauta de publicações da semana.
- Barreira de Adoção: Ferramentas de mercado tradicionais exigem que o cliente crie contas, decore senhas ou baixe aplicativos apenas para dar um "Ok" em uma arte de Instagram, o que gera frustração e atrasos.
<br>
<br>

## A Solução
Para resolver esse problema, o app conta com uma arquitetura focada inteiramente na experiência do usuário de ambas as pontas, baseada em Magic Links únicos.

1. Para a Agência:
Um painel de controle direto onde o Social Media sobe a arte finalizada e a legenda proposta. O sistema gera um link único (UUID) para aquele criativo específico e permite acompanhar o status de aprovação em tempo real (Pendente, Aprovado, Ajuste).

2. Para o Cliente:
O cliente recebe o link e acessa uma interface simples e imersiva. Sem menus complexos e sem necessidade de login. Ele visualiza a arte, lê a legenda e tem apenas duas opções: aprovar ou solicitar um ajuste detalhado.

3. Automação de Fechamento:
Ao interagir com a tela, o cliente não precisa avisar a agência. O sistema atualiza o banco de dados e dispara um Webhook integrado ao n8n, que formata e envia um e-mail dinâmico automático para a equipe de criação informando o resultado da avaliação.
<br>
<br>

# Prompts

**Prompt 1 (3:14):**

```text
Crie um aplicativo web com nome 'Fast Approve' focado em resolver o problema de agências de marketing que precisam de aprovação rápida de conteúdo em tempo real. O app não deve ter sistema de autenticação, usando uma arquitetura de rotas separadas para dividir os papéis de Agência e Cliente.
 
Design e Paleta de Cores:

Design moderno, sofisticado e minimalista. Use a seguinte paleta de cores:
- Fundo principal: #061E29
- Cor Primária (Botões principais): #1D546D
- Cor Secundária (Bordas, ícones): #5F9598
- Cor de Fundo Claro / Texto principal: #F3F4F4
 
ROTA 1: Painel da Agência (/)
- Um formulário limpo para criar um 'Novo Post para Aprovação'.
- Campos: Nome do Cliente, Legenda do Post, e Upload de Imagem ou URL da imagem.
- Uma lista de cards mostrando os posts criados.
- Cada card exibe: Status atual com cor (Amarelo=Pendente, Verde=Aprovado, Vermelho=Ajuste), a legenda, e um botão 'Copiar Link' (aponta para /aprovar/:id).
Importante: Se o status for 'Ajuste', o card deve exibir o texto do feedback que o cliente enviou.
 
ROTA 2: Visão do Cliente (/aprovar/:id)
- Uma tela limpa, sem menus, mostrando a imagem em destaque e a legenda.
Regra de Estado:
- Se o status do post for 'Pendente', exibir dois botões: 'Aprovar' e 'Solicitar Ajuste' (abre campo de texto para o feedback).

Se o status já for 'Aprovado' ou 'Ajuste', ocultar os botões e exibir um aviso read-only: 'Este post já foi avaliado.' para evitar aprovações duplicadas.

- Ao interagir (Aprovar ou Enviar Ajuste), exibir a mensagem 'Obrigado! A agência foi notificada.' e disparar um Webhook POST (deixe a função de fetch preparada no código) para notificar o sistema externo (n8n).
 
Dados: Use o Supabase integrado para gerenciar a criação dos posts e permitir que a Rota 2 carregue os dados do banco usando o ID da URL.

Estrutura de Banco de Dados:
Ative a integração nativa com o Supabase e crie a tabela posts com a seguinte estrutura:

- id (UUID, primary key)
- client_name (text)
- image_url (text)
- caption (text)
- status (text, default: 'Pendente')
- feedback_text (text, nullable)
- created_at (timestamp)


Configuração do Webhook (n8n):
Na função fetch que dispara o aviso de aprovação/ajuste na Rota 2, crie uma variável chamada VITE_N8N_WEBHOOK_URL para que eu possa colocar o link do meu n8n nas variáveis de ambiente depois. O payload (JSON) desse POST deve enviar o ID do post, o Nome do Cliente, o Status atualizado e o Feedback (se houver).

```
**Prompt 2 (1:20):**

```text
Agora, adicione uma funcionalidade para que seja possível apagar posts
```
