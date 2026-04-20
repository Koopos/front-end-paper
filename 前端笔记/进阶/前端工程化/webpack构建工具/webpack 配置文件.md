
Webpack 的配置主要通过一个 `webpack.config.js` 文件来完成，它用于定义打包规则、入口、输出、插件等。下面给你一个清晰、实用的入门讲解 + 示例。

---

# 一、基础配置示例

```js
const path = require('path');

module.exports = {
  mode: 'development', // development | production

  entry: './src/index.js', // 入口文件

  output: {
    path: path.resolve(__dirname, 'dist'), // 输出目录
    filename: 'bundle.js', // 输出文件名
  },

  module: {
    rules: [
      {
        test: /\.css$/, // 匹配 CSS 文件
        use: ['style-loader', 'css-loader'],
      },
      {
        test: /\.js$/, // 匹配 JS 文件
        exclude: /node_modules/,
        use: 'babel-loader',
      },
    ],
  },

  plugins: [],

  devServer: {
    static: './dist',
    hot: true, // 热更新
  },
};
```

---

# 二、核心配置项解释

## 1. entry（入口）

指定 Webpack 从哪个文件开始打包：

```js
entry: './src/index.js'
```

也可以多入口：

```js
entry: {
  app: './src/app.js',
  admin: './src/admin.js',
}
```

---

## 2. output（输出）

```js
output: {
  path: path.resolve(__dirname, 'dist'),
  filename: '[name].[contenthash].js',
}
```

常见占位符：

- `[name]`：入口名
    
- `[hash]`：构建 hash
    
- `[contenthash]`：文件内容 hash（推荐）
    

---

## 3. module.rules（加载器 loader）

Webpack 默认只认识 JS，需要 loader 处理其他资源。

### 常见 loader：

#### 处理 CSS

```js
{
  test: /\.css$/,
  use: ['style-loader', 'css-loader'],
}
```

#### 处理图片

```js
{
  test: /\.(png|jpg|gif)$/,
  type: 'asset/resource',
}
```

#### 处理 ES6（Babel）

```js
{
  test: /\.js$/,
  exclude: /node_modules/,
  use: 'babel-loader',
}
```

---

## 4. plugins（插件）

用于扩展功能：

### 常用插件：

#### HTML 模板

```js
const HtmlWebpackPlugin = require('html-webpack-plugin');

plugins: [
  new HtmlWebpackPlugin({
    template: './src/index.html',
  }),
]
```

#### 清理目录

```js
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

plugins: [
  new CleanWebpackPlugin(),
]
```

---

## 5. mode（模式）

```js
mode: 'development' // 或 'production'
```

区别：

- development：调试友好、未压缩
    
- production：自动压缩优化
    

---

## 6. devServer（开发服务器）

```js
devServer: {
  static: './dist',
  port: 3000,
  open: true,
  hot: true,
}
```

---

# 三、进阶配置

## 1. 路径别名（alias）

```js
resolve: {
  alias: {
    '@': path.resolve(__dirname, 'src'),
  },
}
```

使用：

```js
import Foo from '@/components/Foo';
```

---

## 2. 自动补全扩展名

```js
resolve: {
  extensions: ['.js', '.json'],
}
```

---

## 3. 代码分割（优化）

```js
optimization: {
  splitChunks: {
    chunks: 'all',
  },
}
```

---

## 4. Source Map（调试）

```js
devtool: 'source-map'
```

---

# 四、常见项目结构

```
project/
├── src/
│   ├── index.js
│   ├── style.css
├── dist/
├── webpack.config.js
├── package.json
```

---

# 五、安装常用依赖

```bash
npm install webpack webpack-cli webpack-dev-server -D
npm install css-loader style-loader -D
npm install html-webpack-plugin -D
npm install babel-loader @babel/core @babel/preset-env -D
```

---

# 六、一个更完整的实战配置（简化版）

```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  mode: 'development',

  entry: './src/index.js',

  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: '[name].[contenthash].js',
    clean: true,
  },

  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader'],
      },
      {
        test: /\.js$/,
        use: 'babel-loader',
        exclude: /node_modules/,
      },
    ],
  },

  plugins: [
    new HtmlWebpackPlugin({
      template: './src/index.html',
    }),
  ],

  devServer: {
    static: './dist',
    hot: true,
  },
};
```

---

# 如果你想更深入 👇

我可以帮你按场景细化：

- Vue / React 项目 webpack 配置
    
- 打包优化（性能提升）
    
- 从 0 搭建企业级配置
    
- Vite vs Webpack 对比（是否该换）
    

直接说你的使用场景 👍