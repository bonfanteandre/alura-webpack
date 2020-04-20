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
        path: path.resolve(__dirname, 'dist') -> Diretorio onde o arquivo final será gerado (__dirname é o diretório atual)
    },
    module: { -> Regras para processaor os módulos
        rules: [ -> Lista com todas as regras para serem executadas
            {
                test: /\.js$/, -> Considerar todos os arquivos com extensao .js
                exclude: /node_modules/, -> Ignorar pasta node_modules
                use: {
                    loader: 'babel-loader' -> Utilizar o loader do Babel para processar
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
$ npm install babili-webpack-plugin@versao --saved-ev
```

Loader vs Plugin:
    - Loader: trabalhar em cada arquivo individualmente
    - Plugin: trabalha no arquivo final de bundle gerado

Instalar pacote para definir variáveis de ambiente em qualquer sistema operacional

```
$ npm install cross-env@versao --save-dev
```