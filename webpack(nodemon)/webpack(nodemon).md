-   [webpack](https://www.npmjs.com/package/webpack) — сборщик собственной персоной
-   [webpack-cli](https://www.npmjs.com/package/webpack-cli) — позволяет работать с Webpack'ом из консоли  
-   [webpack-dev-server](https://www.npmjs.com/package/webpack-dev-server) — небольшой сервер для разработки  
-   [webpack-notifier](https://www.npmjs.com/package/webpack-notifier) — будет показывать нам уведомления, если вдруг что-то где-то сломаем  
-   [clean-webpack-plugin](https://www.npmjs.com/package/clean-webpack-plugin) — при каждой сборке автоматически очищает папку для итоговых файлов
-   [uglifyjs-webpack-plugin](https://www.npmjs.com/package/uglifyjs-webpack-plugin) — для продовой версии будет сжимать и минифицировать JS-файлы
- webpack-dev-middleware —
- webpack-hot-middleware —
- webpack-node-externals —


`npm i webpack webpack-cli webpack-dev-server webpack-dev-middleware webpack-hot-middleware webpack-node-externals --save-dev`

## Файл webpack.config.client.js

```js
const path = require('path')

const webpack = require('webpack')

const CURRENT_WORKING_DIR = process.cwd()

  

const config = {

    name: 'browser',

    mode: 'development',

    devtool: 'eval-source-map',

    entry: [

        'webpack-hot-middleware/client?reload=true',

        path.join(CURRENT_WORKING_DIR, 'client/main.js')

    ],

    output: {

        path: path.join(CURRENT_WORKING_DIR, '/dist'),

        filename: 'bundle.js',

        publicPath: '/dist/'

    },

    module: {

        rules: [

            {

                test: /\.jsx?$/,

                exclude: /node_modules/,

                use: ['babel-loader']

            },

            {

                test: /\.(ttf|eot|svg|gif|jpg|png)(\?[\s\S]+)?$/,

                use: 'file-loader'

            }

        ]

    },

    plugins: [

        new webpack.HotModuleReplacementPlugin(),

        new webpack.NoEmitOnErrorsPlugin()

    ],

    resolve: {

        alias: {

            'react-dom': '@hot-loader/react-dom'

        }

    }

}

  

module.exports = config
```

- **mode** — устанавливает process.env.NODE_ENV в заданное значение и говорит Webpack использовать свои встроенные оптимизации соответствующим образом. Если это значение не задано явно, то по умолчанию значение "production". Его также можно установить через командную строку, передав передав значение в качестве аргумента CLI
- **devtool** — определяет, как создаются карты исходных текстов, если они вообще создаются. Как правило, карта карта исходников обеспечивает способ сопоставления кода в сжатом файле обратно к исходному положению в исходном файле для облегчения отладки.
- **entry** — указывает входной файл, с которого Webpack начинает упаковку, в данном случае с файла main.js в папке client.
- **output** — указывает путь вывода для упакованного кода, в данном случае задан как dist/bundle.js.
- **publicPath** — позволяет указать базовый путь для всех активов в приложении.
- **module** — устанавливает правило regex для расширения файла, которое будет использоваться для транспонирования, и папки, которые будут исключены. Инструментом транспонирования, который будет использоваться в данном случае, является babel-loader.
- **HotModuleReplacementPlugin** — включает горячую замену модулей для **react-hot-loader**.
- **NoEmitOnErrorsPlugin** — позволяет пропускать эмиттинг при наличии ошибок компиляции.
- Мы также добавляем псевдоним **Webpack** для указания ссылок **react-dom** на версию **@hot-loader/react-dom**.

Код приложения на стороне клиента будет загружен в браузер из пакета кода в файле bundle.js.

## Файл webpack.config.server.js

```js
const path = require('path')

const CURRENT_WORKING_DIR = process.cwd()

const nodeExternals = require('webpack-node-externals')

  

const config = {

    name: "server",

    entry: [ path.join(CURRENT_WORKING_DIR, './server/server.js') ],

    target: "node",

    output: {

        path: path.join(CURRENT_WORKING_DIR, '/dist/'),

        filename: "server.generated.js",

        publicPath: '/dist/',

        libraryTarget: "commonjs2"

    },

    externals: [nodeExternals()],

    module: {

        rules: [

            {

                test: /\.js$/,

                exclude: /node_modules/,

                use: [ 'babel-loader' ]

            },

            {

                test: /\.(ttf|eot|svg|gif|jpg|png)(\?[\s\S]+)?$/,

                use: 'file-loader'

            }

        ]

    }

}

  

module.exports = config
```

- Параметр **mode** не задается здесь явно, но будет передаваться по мере необходимости при при выполнении команд Webpack в отношении запуска для разработки или сборки для производства.
- Webpack начинает обвязку из папки server с server.js, затем выводит обвязанный код в server.generated.js в папке dist. Во время сборки будет предполагаться среда **CommonJS**, поскольку мы указываем **commonjs2** в **libraryTarget**, поэтому вывод будет назначен в module.exports.

Мы запустим код на стороне сервера, используя сгенерированный пакет в файле server.generated.js

## Файл webpack.config.client.production.js

```js
const path = require('path')

const webpack = require('webpack')

const CURRENT_WORKING_DIR = process.cwd()

  

const config = {

    mode: "production",

    entry: [

        path.join(CURRENT_WORKING_DIR, 'client/main.js')

    ],

    output: {

        path: path.join(CURRENT_WORKING_DIR, '/dist'),

        filename: 'bundle.js',

        publicPath: "/dist/"

    },

    module: {

        rules: [

            {

                test: /\.js$|jsx/,

                exclude: /node_modules/,

                use: [

                    'babel-loader'

                ]

            },

            {

                test: /\.(ttf|eot|svg|gif|jpg|png)(\?[\s\S]+)?$/,

                use: 'file-loader'

            }

        ]

    }

}

  

module.exports = config
```

Это настроит Webpack для пакетирования кода React для использования в производственном режиме. Конфигурация здесь аналогична конфигурации на стороне клиента для режима разработки, но без плагина горячей загрузки и конфигурации отладки, поскольку они не потребуются в производстве.

После создания конфигураций комплектации мы можем добавить конфигурацию для автоматического запуска этих сгенерированных комплектов при обновлении кода во время разработки, используя **Nodemon**.

## Nodemon (файл nodemon.json)

```json
{

    "verbose": false,

    "watch": [ "./server" ],

    "exec": "webpack --mode=development --config webpack.config.server.js && node ./dist/server.generated.js"

}
```

Эта конфигурация настроит nodemon на отслеживание изменений в файлах сервера во время разработки, а затем выполнит команды компиляции и сборки по мере необходимости.