# Bestiário RPG

Um aplicativo minimalista de Bestiário de RPG com operações CRUD completas, desenvolvido com Vue.

## Funcionalidades

- **Listar Monstros**: Exibe uma grade responsiva com os monstros cadastrados.
- **Buscar (Read)**: Permite filtrar os monstros pelo nome através de uma barra de busca.
- **Adicionar (Create)**: Formulário simples para criar um novo monstro informando Nome, Classe e Nível.
- **Editar (Update)**: Modo de edição *inline* que permite alterar as informações de um monstro diretamente no card.
- **Deletar (Delete)**: Remoção de monstros da lista com apenas um clique.

## Tecnologias Utilizadas

- **Vue 3**: Utilizando Composition API (`<script setup>`).
- **Vite**: Ferramenta de build rápida e eficiente.
- **Tailwind CSS**: Utilizado exclusivamente via CDN para estilização utilitária rápida, garantindo uma interface moderna e bonita sem configuração pesada.

## Como Executar o Projeto Localmente

1. Clone este repositório ou baixe os arquivos.
2. Navegue até o diretório do projeto:
   ```bash
   cd dev-web-projeto-vue
   ```
3. Instale as dependências:
   ```bash
   npm install
   ```
4. Inicie o servidor de desenvolvimento:
   ```bash
   npm run dev
   ```
5. Acesse no seu navegador, geralmente no endereço `http://localhost:5173/`.
