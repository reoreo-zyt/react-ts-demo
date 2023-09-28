

参考文章
    - [【前端工程化】webpack5从零搭建完整的react18+ts开发和打包环境](https://juejin.cn/post/7111922283681153038?searchId=2023092817551504A3852E8EA682CE331D)

## 配置 webpack

### 初始化项目

#### 初始化 package.json

`root/`

```bash
yarn init -y
```

`root/package.json`

```bash
├── build
|   ├── webpack.base.js # 公共配置
|   ├── webpack.dev.js  # 开发环境配置
|   └── webpack.prod.js # 打包环境配置
├── public
│   └── index.html # html模板
├── src
|   ├── App.tsx 
│   └── index.tsx # react应用入口页面
├── tsconfig.json  # ts配置
└── package.json
```

#### 安装 react 依赖

`root/`

```bash
yarn add react react-dom
```

#### 安装类型依赖

`root/`

```bash
yarn add @types/react @types/react-dom -D
```

#### 添加业务内容

`root/public/index.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>webpack5-react-ts</title>
</head>
<body>
  <!-- 容器节点 -->
  <div id="root"></div>
</body>
</html>
```

`root/tsconfig.json`

```json
{
  "compilerOptions": {
    "target": "ESNext",
    "lib": ["DOM", "DOM.Iterable", "ESNext"],
    "allowJs": false,
    "skipLibCheck": false,
    "esModuleInterop": false,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "module": "ESNext",
    "moduleResolution": "Node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react" // react18这里也可以改成react-jsx
  },
  "include": ["./src"]
}
```

`root/src/App.tsx`

```tsx
import React from 'react'

function App() {
  return <h2>webpack5-react-ts</h2>
}
export default App
```

`root/src/index.tsx`

```tsx
import React from 'react';
import { createRoot } from 'react-dom/client';
import App from './App';

const root = document.getElementById('root');
if(root) {
  createRoot(root).render(<App />)
}
```

### 配置 webpack

#### webpack 公共配置

- *配置入口出口文件*
- *配置 loader 解析 ts 和 jsx*
- *resolve 解析文件后缀（引入时可以不加文件后缀）*
- *html-webpack-plugin 引入静态资源到 html 中*

`root/build/webpack.base.js`

```js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
    entry: join(__dirname, '../src/index.tsx'),
    output: {
        filename: 'static/js/[name].js',
        path: join(__dirname, '../dist'),
        clean: true,
        publicPath: '/', // 打包后文件的公共前缀路径
    },
    module: {
        rules: [
            {
                test: /.(ts|tsx)$/, // 匹配.ts, tsx文件
                use: {
                    loader: 'babel-loader',
                    options: {
                        // 预设执行顺序由右往左,所以先处理ts,再处理jsx
                        presets: [
                            '@babel/preset-react',
                            '@babel/preset-typescript'
                        ]
                    }
                }
            }
        ]
    },
    resolve: {
        extensions: ['.js', '.tsx', '.ts'],
    },
    plugins: [
        new HtmlWebpackPlugin({
            template: path.resolve(__dirname, '../public/index.html'), // 模板取定义root节点的模板
            inject: true, // 自动注入静态资源
        })
    ]
}
```

#### webpack 开发环境配置

`root/build/webpack.dev.js`

```bash
webpack-dev-server webpack-merge -D
```
