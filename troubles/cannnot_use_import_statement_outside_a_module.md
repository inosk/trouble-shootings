# Cannot use impot statement outside a module

なんらかの理由で import が使えないときに出るエラー。

# 具体例

## typeorm で migration をしようとしたとき

typeorm は DataSource クラスを利用してDBの接続情報を管理する。  
app内でもtypeorm cli でも利用するが cli 利用時に発生した。

### 採用した対策

~datasource.js -> datasource.mjs と拡張子を変更することで対処。~

~node の module システムは デフォルトで commonjs で `require` を使ってimportする。~  
~その状況下でesmodule の `import/export` が記載されている場合にこのエラーがでる。~

~.mjs にすると node が esmodule のファイルとして扱ってくれるため、 `import/export`が使えるようになる。~

typeorm の cli で esmodules（+ .ts） が使えるように設定する。

- cli として `typeorm-ts-node-esm` を使うようにする

tsconfigに ts-node のオプションを追記する。

```javascript
  "ts-node": {
    "esm": true,
    "experimentalSpecifierResolution": "node"
  }
```

package.json に type: "module" を追記する

```json
{
  ...
  "type": "module", // commonjs の require ではなく、esmodules の import/export を使う設定
  ...
}
```

# 参考

https://kinsta.com/knowledgebase/cannot-use-import-statement-outside-module/#how-to-resolve-the-error-in-serverside-javascript
