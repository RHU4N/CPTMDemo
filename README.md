# CPTM Demo

Protótipo front-end em Vue 3 para acompanhamento de inspeções operacionais. A aplicação simula o fluxo de acesso de inspetores e administradores, preenchimento de formulários, histórico de inspeções, sincronização de registros e visualização de usuários em um mapa.

## Funcionalidades

- tela de carregamento inicial com identidade CPTM x FATEC;
- login de usuário e administrador;
- fluxo de primeiro acesso com código de ativação;
- criação de novas inspeções;
- formulário de inspeção com 10 perguntas paginadas em grupos de 5;
- respostas de texto e seleção por checkbox;
- listagem de inspeções por status;
- edição, exclusão, envio e cancelamento de inspeções;
- histórico com filtros por status e linha;
- área administrativa para usuários, logs e chamados;
- mapa com localização de inspetores usando Leaflet;
- indicação visual de usuários online e offline.

## Perfis demonstrativos

O login atual é local e utiliza credenciais fixas no componente `login.vue`:

| Perfil        | Usuário | Senha   |
| ------------- | ------- | ------- |
| Administrador | `admin` | `admin` |
| Usuário       | `user`  | `user`  |

No fluxo de **Primeiro Acesso**, o código válido de demonstração é:

```text
1234
```

Esses valores são apenas para prototipação e não devem ser usados em um sistema real.

## Fluxo da aplicação

```text
Carregamento
	↓
Login ────────────────┐
	│                  │
	├── Primeiro acesso│
	│                  ↓
	├── Usuário      User
	└── Administrador Admin
```

Após o login, o usuário comum acessa inspeções e histórico. O administrador também visualiza usuários no mapa e possui atalhos adicionais para gerenciamento, logs e chamados.

## Formulário de inspeção

O formulário contém 10 itens divididos em duas páginas de cinco perguntas. Os tipos de resposta são:

- texto livre;
- checkbox com opções de situação.

Algumas perguntas são marcadas como somente visualização, como sistema de freios e documentação. Ao finalizar, a inspeção é adicionada à lista com status `pendente`.

Os status utilizados no protótipo são:

- `pendente`;
- `pendenteSync`;
- `sync`;
- `enviado`.

## Estrutura do projeto

```text
CPTMDemo/
├── index.html
├── package.json
├── vite.config.js
├── public/
└── src/
	 ├── App.vue                    # Controle das telas principais
	 ├── main.js
	 ├── style.css
	 ├── assets/                    # Logos e imagens
	 └── components/
		  ├── loading.vue            # Tela de carregamento
		  ├── login.vue              # Login local
		  ├── firstAccess.vue        # Ativação de conta
		  ├── user.vue               # Área do inspetor
		  ├── admin.vue              # Área administrativa
		  ├── InspectionForm.vue     # Formulário paginado
		  ├── InspectionList.vue     # Lista e ações das inspeções
		  ├── Historico.vue          # Filtros do histórico
		  ├── mapa.vue               # Mapa Leaflet
		  ├── AdminUsers.vue
		  ├── AppHeader.vue
		  ├── AppButton.vue
		  └── ...
```

## Tecnologias

- Vue 3;
- Vite;
- JavaScript;
- Leaflet;
- OpenStreetMap tiles.

## Requisitos

- Node.js;
- npm;
- navegador moderno com suporte a geolocalização, caso queira testar a localização do administrador.

## Instalação

Na raiz do projeto, instale as dependências:

```bash
npm install
```

## Como executar

Inicie o servidor de desenvolvimento:

```bash
npm run dev
```

O Vite exibirá no terminal o endereço local da aplicação, normalmente `http://localhost:5173`.

Para gerar a versão de produção:

```bash
npm run build
```

Para visualizar o build localmente:

```bash
npm run preview
```

## Mapa

O componente `mapa.vue` usa Leaflet para mostrar inspetores em coordenadas de teste na região de São Paulo. A localização do administrador é obtida pelo navegador quando o usuário concede permissão; em caso de recusa, o protótipo usa uma coordenada padrão.

O mapa utiliza tiles do OpenStreetMap e, portanto, depende de conexão com a internet para carregar o mapa base e alguns ícones.

## Observações

- O projeto não possui backend ou banco de dados conectado.
- Inspeções e usuários são dados estáticos mantidos em memória nos componentes Vue.
- Criar, editar, excluir e sincronizar registros altera apenas o estado da sessão atual.
- A tela de logs e a tela de chamados são placeholders visuais.
- O botão de configurações apresenta apenas um alerta demonstrativo.
- O componente de histórico mantém uma lista própria de exemplo, separada da lista principal do usuário.
- O login e o código de ativação são inseguros por serem valores fixos no código.

## Objetivo

Praticar desenvolvimento de interfaces com Vue 3, componentes reutilizáveis, formulários paginados, estados de inspeção, geolocalização e mapas interativos.
