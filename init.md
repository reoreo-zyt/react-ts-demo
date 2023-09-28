

参考文章
    - [【前端工程化】webpack5从零搭建完整的react18+ts开发和打包环境](https://juejin.cn/post/7111922283681153038?searchId=2023092817551504A3852E8EA682CE331D)

## 配置 webpack

### 初始化项目

*初始化package.json*

```bash
yarn init -y
```

*@/package.json*

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

*安装 react 依赖*

```bash
yarn add react react-dom
```

*安装类型依赖*

```bash
yarn add @types/react @types/react-dom -D
```

*@/public/index.html*

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

*添加tsconfig.json内容*

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

*添加 @src/App.tsx 内容*

```tsx
import React from 'react'

function App() {
  return <h2>webpack5-react-ts</h2>
}
export default App
```

*添加 @src/index.tsx 内容*

```tsx
import React from 'react';
import { createRoot } from 'react-dom/client';
import App from './App';

const root = document.getElementById('root');
if(root) {
  createRoot(root).render(<App />)
}
```
