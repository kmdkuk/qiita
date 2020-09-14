---
ID: b7cceb83a84662885ce9
Title: react-native+typescript+redux
Tags: TypeScript,React,reactnative,redux
Author: Kamada Kouki
Private: false
---

# はじめに
react-native+redux
react-native+typescript
だったりギリギリのところでなにかかけている記事がとても多くて，
3つを使ってプロダクト作成するいろいろの情報がちらかっていて謎だったので自分用にまとめる．

## インストール，依存解決

```bash
npm -g install yarn
yarn global add react-native-cli
react-native init <ProjectName>
cd <ProjectName>
# typescript関係を追加
yarn add typescript @types/react @types/react-native @types/jest --dev
# redux関係を追加
yarn add redux react-redux @types/react-redux --dev
# typescriptでいい感じに起動してくれるやつを追加
yarn add react-native-typescript-transformer --dev

```

## TypeScript用にファイル変換
```bash
rm App.js
mkdir src
touch App.tsx
```
App.jsを削除
\<ProjectName>直下のindex.js以外は
.js→.ts
.jsx→.tsx
に変換
tsファイルは基本src以下に作成していく

index.jsとsrc/App.tsxを以下のように編集

```javascript:index.js
import { AppRegistry } from 'react-native';
import App from './src/App';
import { name as appName } from './app.json';

AppRegistry.registerComponent(appName, () => App);
```

```typescript:src/App.tsx
import * as React from 'react';
import { Component } from 'react';
import { Platform, StyleSheet, Text, View } from 'react-native';

const instructions = Platform.select({
  ios: 'Press Cmd+R to reload,\n' + 'Cmd+D or shake for dev menu',
  android:
    'Double tap R on your keyboard to reload,\n' +
    'Shake or press menu button for dev menu',
});

interface Props { };
export default class App extends Component<Props> {
  render() {
    return (
      <View style={styles.container}>
        <Text style={styles.welcome}>Welcome to React Native!</Text>
        <Text style={styles.instructions}>To get started, edit App.js</Text>
        <Text style={styles.instructions}>{instructions}</Text>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF',
  },
  welcome: {
    fontSize: 20,
    textAlign: 'center',
    margin: 10,
  },
  instructions: {
    textAlign: 'center',
    color: '#333333',
    marginBottom: 5,
  },
});
```

tsconfig.jsonの生成

```bash
yarn run tcs --init
# ↑でできたtsconfig.jsonを編集
```

適宜変更(正直あまり理解していない)

```javascript:tsconfig.json
{
  "compilerOptions": {
    /* Basic Options */
    "target": "es5", /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017','ES2018' or 'ESNEXT'. */
    "module": "commonjs", /* Specify module code generation: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', or 'ESNext'. */
    "jsx": "react-native", /* Specify JSX code generation: 'preserve', 'react-native', or 'react'. */
    /* Strict Type-Checking Options */
    "strict": true, /* Enable all strict type-checking options. */
    /* Additional Checks */
    "allowSyntheticDefaultImports": true, /* Allow default imports from modules with no default export. This does not affect code emit, just typechecking. */
    "esModuleInterop": true /* Enables emit interoperability between CommonJS and ES Modules via creation of namespace objects for all imports. Implies 'allowSyntheticDefaultImports'. */
  },
  "exclude": [
    "node_modules"
  ]
}

```

\<ProjectName>/直下にrn-cli.config.jsを作成

```javascript:rn-cli.config.js
module.exports = {
    getTransformModulePath() {
      return require.resolve("react-native-typescript-transformer");
    },
    getSourceExts() {
      return ["ts", "tsx"];
    }
  };
```

## 実行
```bash
react-native run-ios
# または
react-native run-android
```
↑を実行するとおそらくターミナルで自動的にMetroBundlerが起動するが，
ごちゃごちゃやってると起動しなかったりするので．

```bash
react-native start
```
で手動で起動させることができる

すでにポートが使われている場合は，起動しないので，
lsofで探してそいつをkillでたぶん起動する

```bash
lsof -i:8081
kill -9 <使っているポートのPID>
```

## 終わりに
redux感がまったくないけどまあどれをaddすればいいかメモということで，
最終版はこんな感じ
https://github.com/kmdkuk/startUp

## 参考
https://qiita.com/uryyyyyyy/items/3ad88cf9ca9393335f8c
https://qiita.com/natmark/items/0f2f88b11f49b3f28128

