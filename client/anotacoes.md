## Anotações sobre Webpack

Instalalção do Webpack (precisa do núcleo do transpilador Babel, pois irá utilizá-lo para compilar/transpilar os arquivos)

```
$ npm install webpack@versao babel-core@versao --save-dev
```

Instalação do Babel Loader (loader do Babel para o Webpack)

```
$ npm install babel-loader@versao --save-dev
```

Arquivo de configuração do Webpack: webpack.config.js

Exemplo do webpack.config.js:

```
const path = require('path'); -> Inclui módulo para trabalhar com o path
const babili = require('babili-webpack-plugin'); -> Utilizando plugin Babili

-> Só executa o código caso exista uma variável de ambiente com valor 'production' (definida no script no package.json)
if (process.env.NODE_ENV == 'production') {
    plugins.push(new babili());
}

module.exports = {
    entry: './app-src/app.js', -> Ponto de entrada da aplicação (primeiro módulo carregado)
    output: { -> Configurações de saída
        filename: 'bundle.js', -> Nome do arquivo final que será gerado
        path: path.resolve(__dirname, 'dist'), -> Diretorio onde o arquivo final será gerado (__dirname é o diretório atual)
        publicPath: 'dist' -> Diretório público acessado pelo Webpack Dev Server
    },
    module: { -> Regras para processaor os módulos
        rules: [ -> Lista com todas as regras para serem executadas
            {
                test: /\.js$/, -> Considerar todos os arquivos com extensao .js
                exclude: /node_modules/, -> Ignorar pasta node_modules
                use: {
                    loader: 'babel-loader' -> Utilizar o loader do Babel para processar
                }
            },
            {
                test: /\.css$/,
                use: {
                    loader: 'stye-loader!css-loader' -> Loaders para arquivos CSS. "!" serve para executar mais de um (executa da direita para a esquerda)
                }
            }
        ]
    }
}
```

Criando um script no package.json para executar o Webpack:

```
 "build-dev": "webpack --config webpack.config.js"
```

Instalar babili para minificação de arquivos (UglifyJS não suporte sintaxe do ES2015 - ES6 - para frente)

```
$ npm install babili-webpack-plugin@versao --save-dev
```

Loader vs Plugin:
    - Loader: trabalhar em cada arquivo individualmente
    - Plugin: trabalha no arquivo final de bundle gerado

Instalar pacote para definir variáveis de ambiente em qualquer sistema operacional

```
$ npm install cross-env@versao --save-dev
```

Instalação do Webpack Dev Server

```
$ npm install webpack-dev-server@versao --save-dev
```

Script para rodar o Webpack Dev Server

```
 "start": "webpack-dev-server"
```

Instalando o Bootstrap pelo npm

```
$ npm install bootstrap@versao --save
```

Importando o Boostrap na aplicação

Como não estamos usando ./ ou ../ para acessar, os arquivos serão buscados dentro de node_modules

```
import 'bootstrap/dist/css/boostrap.css';
import 'bootstrap/dist/css/boostrap-theme.css';
```

Para isso precisamos instalar loaders que lidam com CSS

```
$ npm install css-loader@versao style-loader@versao --save-dev
```

Após instalar, precisamos configurá-los no webpack.config.js

```
    {
        test: /\.css$/,
        loader: 'style-loader!css-loader'
    }
```

Além disso, é necessário instalar loaders para as fontes

```
$ npm install url-loader@versao file-loader@versao --save-dev
```

Configurando os loaders de fonte no webpack.config.js

```
{ 
    test: /\.(woff|woff2)(\?v=\d+\.\d+\.\d+)?$/, 
    loader: 'url-loader?limit=10000&mimetype=application/font-woff' 
},
{ 
    test: /\.ttf(\?v=\d+\.\d+\.\d+)?$/, 
    loader: 'url-loader?limit=10000&mimetype=application/octet-stream'
},
{ 
    test: /\.eot(\?v=\d+\.\d+\.\d+)?$/, 
    loader: 'file-loader' 
},
{ 
    test: /\.svg(\?v=\d+\.\d+\.\d+)?$/, 
    loader: 'url-loader?limit=10000&mimetype=image/svg+xml' 
}   
```

Instalar plugin para gerar CSS em separado no build e evitar FOUC (Flash of Unstyled Content)

```
$ npm install extract-text-webpack-plugin@versao --save-dev
```

Configurando o plugin extract-text-webpack-plugin

```
const extractTextPlugin = require('extract-text-webpack-plugin'); -> Importar o plugin
```

```
plugins.push(new extractTextPlugin('styles.css')); -> Adicionar na lista de plugins
```

No array de regras:

```
    {
        test: /\.css$/,
        use: extractTextPlugin.extract({
            fallback: 'style-loader',
            use: 'css-loader'
        })
    }
```

Após isso é necessário fazer uma configuração para que o arquivo CSS gerado seja minificado

Para isso podemos utilizar dois plugins

```
$ npm install optimize-css-assets-webpack-plugin@versao cssnano@versao --save-dev
```

Para lidar com dependências que ficam no "escopo global" precisamos utilizar um plugin que já vem com o webpack o Provide Plugin

Para configurar ele precisamos alterar o arquivo webpack.config.js

```
    const webpack = require('webpack'); -> Importar o webpack

    plugins.push(new webpack.ProvidePlugin({ -> Configurar as dependências globais
        '$': 'jquery/dist/jquery.js',
        'jQuery': 'jquery/dist/jquery.js'
    }));
```

Plugin parar gerar inclusão de assets automaticamente

```
$ npm install html-webpack-plugin@<versao> --save-dev
```

Configurando o plugin no webpack.config.js

```
const htmlWebPackPlugin = require('html-webpack-plugin');

plugins.push(new htmlWebPackPlugin({
    hash: true, -> Hash para atualizar o cache
    minify: {
        html5: true,
        collapseWhitespace: true,
        removeComments: true
    },
    filename: 'index.html', -> Arquivo que irá gerar
    template: __dirname + '/main.html' -> Template que irá pegar como base
}));
```