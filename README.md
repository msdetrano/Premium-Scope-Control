Premium Scope Control 🛡️
Um dashboard interativo e encapsulado para gestão de escopo e recursos de clientes (Squad Premium). Desenvolvido especificamente para ser injetado em páginas do Wiki.js (2.x), utilizando Shadow DOM para evitar conflitos de CSS com o Vuetify/Vue.js nativos da plataforma.

📸 Preview
(Adicione aqui um print da tela final do seu projeto)

🚀 Funcionalidades
Isolamento Total: Utiliza Shadow DOM para garantir que o CSS do Wiki.js não quebre o layout do dashboard e vice-versa.

Gestão Visual: Tabela de status com ícones "soft" (estilo pastel) para indicar cobertura de serviços (Redes, S.O., Bancos, etc.).

Estatísticas em Tempo Real: Cards superiores calculam automaticamente a porcentagem de cobertura do time baseada nos dados da tabela.

Modo de Segurança: Botão de "Bloqueio/Edição" para prevenir cliques acidentais.

CRUD Básico (Em Memória):

Adicionar: Insere novos clientes na lista.

Editar: Clique nos ícones para alternar entre ✅ (Ativo) e ❌ (Inativo).

Excluir: Pressão longa (0.8s) no nome do cliente para remover.

Responsivo: Layout adaptável para telas menores com rolagem horizontal na tabela.

🛠️ Como Instalar no Wiki.js
Este projeto não requer build (npm/webpack). Ele é injetado diretamente nas propriedades da página do Wiki.js.

1. Preparar o HTML (Editor)
No editor da página (modo Raw HTML ou Markdown), adicione apenas o container anfitrião:

HTML
<div id="premium-dashboard-host" style="width: 100%; min-height: 800px; display: block;"></div>
2. Configurar CSS (Page Properties > Scripts/Styles > CSS)
Adicione este CSS para limpar a área de visualização do Wiki e permitir largura total:

CSS
/* Remove a barra lateral direita e força largura total */
aside.v-navigation-drawer--right {
    display: none !important;
}

.v-main__wrap > .container,
.contents,
.page-content {
    max-width: 100% !important;
    padding: 0 !important;
    margin: 0 !important;
    background-color: #f8fafc !important;
}

#premium-dashboard-host {
    display: block;
    width: 100%;
    margin-top: 20px;
}
3. Injetar o Script (Page Properties > Scripts/Styles > JS)
Copie o conteúdo do arquivo dashboard.js (ou o script final que geramos) e cole na área de Scripts.

Nota: O script utiliza uma IIFE (function(){ ... })() para evitar vazamento de variáveis no escopo global do Wiki.

💻 Estrutura do Código
O código foi desenhado para resolver o problema de "CSS Bleeding" (vazamento de estilo) comum em SPAs como o Wiki.js.

Tecnologias Usadas
Shadow DOM (attachShadow({mode: 'open'})): Cria uma árvore DOM separada. O CSS definido dentro dela não afeta o resto da página.

Vanilla JS: Sem dependências externas pesadas (apenas FontAwesome via CDN).

CSS in JS: Os estilos são definidos em uma constante const styles e injetados dentro do Shadow Root.

Trecho Principal (Inicialização Segura)
Para garantir que o script rode apenas quando o Wiki terminar de carregar o Vue.js:

JavaScript
function init() {
    const host = document.getElementById('premium-dashboard-host');
    
    // Se o elemento ainda não existe, tenta novamente em 100ms
    if (!host) { setTimeout(init, 100); return; }
    
    // Evita duplicar se já foi renderizado
    if (host.shadowRoot) return;

    // Cria o isolamento
    const shadow = host.attachShadow({ mode: 'open' });
    render(shadow);
}
🎨 Personalização
Você pode alterar facilmente as cores e ícones editando as constantes no início do script:

Ícones
Utiliza a classe do Font Awesome 6.4.0.

JavaScript
const icons = ['fa-sitemap', 'fa-terminal', 'fa-database', 'fa-lock', 'fa-cloud-arrow-up', 'fa-layer-group'];
Cores (Tema)
JavaScript
const CONFIG = {
    // ...
    gradient: 'linear-gradient(135deg, #1e40af 0%, #2563eb 100%)', // Gradiente do Header
    colors: {
        primary: '#2563eb', // Cor principal (Azul Edge)
        // ...
    }
};
📝 Como Usar
Modo Leitura (Padrão): O painel inicia bloqueado. Você pode visualizar os dados, mas não pode alterar nada.

Modo Edição:

Clique no botão 🔒 BLOQUEADO. Ele mudará para 🔓 EDITANDO.

Agora você pode clicar nos botões de status (✅/❌) para alterar.

Clique em + Adicionar para inserir um novo cliente.

Excluir Cliente:

Com o modo de edição ativo, clique e segure (pressione por 1 segundo) no nome do cliente que deseja remover. Confirme a ação no alerta.

👤 Autor
Marcos Detrano Technical Leader of Operations - Edge UOL

Este projeto é de uso interno para o Squad Premium.
