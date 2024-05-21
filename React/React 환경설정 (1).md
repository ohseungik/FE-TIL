# React 환경설정

## 1. CRA 없이 리액트를 시작해야 하는 이유

create-react-app(CRA)을 이용하면 쉽게 리액트 프로젝트를 시작할 수 있지만, 설정을 커스터마이징하기 어렵습니다. 이 때문에, 직접 설정을 구성하는 방법을 선택할 수 있습니다.

## 2. 구축 순서

1. `package.json` 생성
2. `React`, `Typescript` 설치
3. `webpack`, `webpack plugins`, `loader`, `devServer` 설치
4. `webpack`, `typescript` 설정
5. `prettier` 설정
6. `src` 디렉터리 생성
7. `public` 디렉터리 생성
8. `package.json` 설정

### 1. `package.json` 생성

```bash
npm init -y
```

### 2. React, Typescript 설치

```bash
npm i react react-dom
npm i -D typescript @types/react @types/react-dom
tsc --init
```

### 3. webpack, webpack plugins, loader, devServer 설치

```bash
npm i webpack webpack-cli --dev
npm i html-webpack-plugin webpack-dev-server ts-loader --dev
```

### 4. 4. webpack, typescript 설정

#### webpack.common.js

```javascript

const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  mode: 'development',
  entry: './src/index.tsx',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist'),
    clean: true,
  },
  resolve: {
    extensions: ['.ts', '.tsx', '.js', '.jsx'],
  },
  module: {
    rules: [
      {
        test: /\.(js|ts|tsx)$/i,
        exclude: /node_modules/,
        use: {
          loader: 'ts-loader',
        },
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './public/index.html',
    }),
  ],
  devtool: 'inline-source-map',
  devServer: {
    static: './dist',
    hot: true,
    open: true,
  },
};
```
#### webpack.dev.js

```javascript
const { merge } = require('webpack-merge');
const common = require('./webpack.common.js');

module.exports = merge(common, {
  mode: 'development',
  devtool: 'eval',
  devServer: {
    historyApiFallback: true,
    port: 3000,
    hot: true,
  },
});
```

#### webpack.prod.js

```javascript
const { merge } = require('webpack-merge');
const common = require('./webpack.common.js');

module.exports = merge(common, {
  mode: 'production',
  devtool: 'hidden-source-map',
});
```

### 5. prettier 설정

```json
{
  "endOfLine": "auto",
  "singleQuote": true,
  "trailingComma": "all",
  "printWidth": 100
}
```

### 6. src 디렉터리 생성

#### src/index.tsx

```typescript
import App from './App';
import React from 'react';
import ReactDOM from 'react-dom/client';

const root = ReactDOM.createRoot(document.getElementById('root') as HTMLElement);
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

#### src/App.tsx

```typescript
const App = () => {
  return <></>;
};

export default App;
```

### 7. public 디렉터리 생성

#### public/index.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>CRA 없이 React 시작하기</title>
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>
```

### 8. package.json 필드 설정

```json
{
  "name": "my-react-app",
  "version": "0.0.1",
  "main": "index.tsx",
  "repository": "https://github.com/your-repo/client.git",
  "author": "your-github-id <your-email@example.com>",
  "license": "MIT",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack-dev-server --config webpack.dev.js --open --hot",
    "build": "webpack --config webpack.prod.js",
    "start": "webpack --config webpack.dev.js"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
  "devDependencies": {
    "html-webpack-plugin": "^5.5.3",
    "ts-loader": "^9.4.3",
    "typescript": "^5.1.3",
    "webpack": "^5.86.0",
    "webpack-cli": "^5.1.4",
    "webpack-dev-server": "^4.15.1"
  }
}

```






