# Webpack

[Ref Webpack](https://www.webpackjs.com/)\
[Ref Webpack start](https://www.jianshu.com/p/42e11515c10f)
[Ref Webpack React Scss](https://www.jianshu.com/p/5262ccf59e3e)
[Ref Babel loader](https://juejin.im/post/5cabf7b0e51d456e8b07dd04)
[Ref Babel loader](https://www.tangshuang.net/7427.html)
[Ref Babel loader](https://github.com/xtTech/dc-sdk-js)

https://www.jianshu.com/p/42e11515c10f
https://www.webpackjs.com/concepts/entry-points/
https://github.com/ruanyf/webpack-demos
https://www.jikexueyuan.com/course/createjs/

## Practice

1. Create project and install tools

```bash
# Init
npm init
# Webpack
npm i -D webpack webpack-cli
# Server
npm i -D webpack-dev-server
# Plugin
npm i -D html-webpack-plugin
npm i -D clean-webpack-plugin
npm i -D mini-css-extract-plugin
npm i -D css-minimizer-webpack-plugin
# Loader
npm i -D style-loader css-loader file-loader url-loader
npm i -D node-sass sass-loader
# Babel
npm i -D @babel/core @babel/cli @babel/preset-env babel-loader
npm i -D @babel/plugin-proposal-class-properties
npm i -D @babel/preset-react

## Note: dependence
npm i @babel/polyfill

touch webpack.config.js
touch .babelrc

cat _webpack.config.js.sample webpack.config.js
cat _babelrc.sample .babelrc
```

2. Config the project

```javascript
// webpack.config.js

const path = require('path'),
    webpack = require('webpack'),
    HtmlWebpackPlugin = require('html-webpack-plugin'),
    { CleanWebpackPlugin } = require('clean-webpack-plugin'),
    MiniCssExtractPlugin = require('mini-css-extract-plugin'),
    CssMinimizerWebpackPlugin = require('css-minimizer-webpack-plugin');

const pkg = require('./package.json');

module.exports = (env, argv) => {

    let wpconf = {},
        is_development = 'development' === argv.mode,
        output_prefix = `${pkg.name}-${pkg.version}.min`;

    wpconf.devtool = 'source-map';

    wpconf.entry = { index: path.resolve(__dirname, './src/index.js') };

    wpconf.output = {
        filename: `${output_prefix}.js`,
        path: path.resolve(__dirname, './dist'),
        libraryTarget: 'window',
        globalObject: 'this',
        // libraryExport: 'default',
        library: 'Ciic'
    };

    wpconf.target = ['web', 'es5',]; // Webpack 5 for IE closure

    wpconf.module = {
        rules: [
            {
                test: /\.s?css$/, use: [
                    is_development ? 'style-loader' : MiniCssExtractPlugin.loader,
                    'css-loader', 'sass-loader'
                ]
            },
            { test: /\.(png|svg|gif|jpe?g)(\?.*)?$/, use: ['file-loader'] },
            { test: /\.(otf|eot|ttf|woff2?|svg)(\?.*)?$/, use: ['file-loader'] },
            { test: /\.(js|jsx)$/, loader: 'babel-loader', exclude: /node_modules/ }
        ]
    };

    // wpconf.devServer = { host: 'ut.china.com.cn', contentBase: path.resolve(__dirname, './dist'), hot: true };
    wpconf.devServer = { host: '172.32.3.201', contentBase: path.resolve(__dirname, './dist'), hot: true, open: true };

    wpconf.plugins = [
        new CleanWebpackPlugin({
            verbose: true,
            cleanOnceBeforeBuildPatterns: ['**/*',],
        }),
        new webpack.HotModuleReplacementPlugin(),
        new HtmlWebpackPlugin({ template: path.resolve(__dirname, './src/index.html') }),
        new MiniCssExtractPlugin({ filename: `${output_prefix}.css` }),
    ];

    wpconf.optimization = {
        minimize: !is_development,
        minimizer: [
            new CssMinimizerWebpackPlugin(),
            '...',
        ]
    };

    wpconf.resolve = { alias: { webworkify: 'webworkify-webpack-dropin' } };

    return wpconf;
};
```

```json
// .babelrc
{
    "presets": [
        "@babel/preset-env"
    ],
    "plugins": [
        "@babel/plugin-proposal-class-properties"
    ]
}
```