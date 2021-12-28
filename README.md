# webpack-basic-config

## 1 - **Preparar o ambiente**

```markdown
nvm use
npm install
npm run start
```

## 2 - **Configuração inicial do webpack**

```markdown
//Instalação das dependências de desenvolvimento do webpack
npm install webpack@5.28.0 --save-dev
npm install webpack-cli@4.5.0 --save-dev
```

```jsx
const path = require('path');

module.exports = {
	entry: './app/src/js/app.js',
	output: {
		filename: 'bundle.js',
		path: path.resolve(__dirname, 'app/dist'),
	},
};
```

```json
//Adicionar o comando de build no package.json
"build:dev": "webpack --mode development --config webpack.config.js"
```

## 3 - Instalação de plugins no webpack

```json
//Plugin que permite gerenciar a criação automatizada de arquivos html
npm install html-webpack-plugin@5.3.1 --save-dev
```

```jsx
//Deve incluir abaixo de output
plugins: [
	new HtmlWebpackPlugin({
		template: './app/src/app.html', //arquivo de exemplo
		filename: 'index.html', //arquivo de saida
		hash: true, //gera um hash para atualizar as versões do bundle
	}),
],
```

```jsx
//Permite copiar arquivos css para a pasta dist
npm install copy-webpack-plugin@8.1.0 --save-dev
```

```jsx
//adicionar abaixo do HtmlWebpackPlugin
new CopyWebpackPlugin({
	patterns: [
		{ from: './app/src/css', to: 'css' },
	],
}),
```

## **4 - Trabalhando com módulos e dependencias de css**

```jsx
//Para ter uma dependencia de css externa e atualizada
npm install bootstrap@4.6.0 --save
```

```jsx
//Remover as referencias de css do app.html
//Remover a pasta css de src/css
```

```jsx
//remover
new CopyWebpackPlugin({
	patterns: [
		{ from: './app/src/css', to: 'css' },
	],
}),
```

```jsx
import './bootstrap/dist/css/bootstrap.css';
```

```jsx
//recursos necessários para a configuração
npm install css-loader@5.2.0 --save-dev
npm install style-loader@2.0.0 --save-dev
```

```jsx
module: {
	rules: [
		{ test: /\.css$/, use: ['style-loader', 'css-loader'] },
	],
},
```

```jsx
//para gerar um bundle.js e um bundle.css 
//para otimizar o carregamento e renderização do css
npm install mini-css-extract-plugin@1.4.2 --save-dev
```

```jsx
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

//em plugins adicione
new MiniCssExtractPlugin({
	filename: 'style.css',
}),
//em module>rules modifica a regra para:
{ test: /\.css$/, use: [MiniCssExtractPlugin.loader, 'css-loader'] },
```

## 5 - Build para produção

```jsx
//adiciona o comando em scripts
"build:prod": "webpack --mode production --config webpack.config.js"
```

```jsx
//para minificar o bundle final pracisamos instalar um plugin especifico
npm install css-minimizer-webpack-plugin@1.3.0 --save-dev
```

```jsx
const CssMinimizeWebpackPlugin = require('css-minimize-webpack-plugin');

//abaixo de module
optimization: {
	minimizer: true,
	minimizer: [
		new CssMinimizeWebpackPlugin(),
		'...', //para manter todas as outras propriedades default do webpack
	]
},
```

```jsx
//para concatetar de uma forma mais robusta precisamos importar um modulo do próprio webpack
const webpack = require('webpack');

//em plugins adicione
new webpack.ModuleConcatenationPlugin(),
```

```jsx
npm run build:prod
```

## 6 - Webpack dev server

```jsx
//servidor de desenvolvimento do webpack
//conta com hot reload
npm install webpack-dev-server@3.11.2 --save-dev
```

```json
//adicionar os scripts em package.json
"start:dev": "webpack serve --mode development --config webpack.config.js",
"start:prod": "webpack serve --mode production --config webpack.config.js",
```

```jsx
//adicionar as configurações no final do arquivo
devServer: {
	contentBase: path.resolve(__dirname, 'dist'),
	port:  3000,
},

//remover o arquivo src/index.html
//renomear o arquivo src/app.html para serc/index.html
//alterar as configurações para o novo arquivo
new HtmlWebpackPlugin({
	template: './app/src/index.html',
	filename: 'index.html',
	hash: true,
}),
```
