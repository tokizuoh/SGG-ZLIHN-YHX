# sgg-zlihn-yhx

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report

# run unit tests
npm run unit

# run all tests
npm test
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).
  
## 所感  
完全に時間の見積もりを見誤りました。  
今の自分の技量ではかなり時間がかかってしまい、完成には至りませんでした。  
  
## 反省点  
  
- [ ] 都道府県チェックボックスのユーザービリティの低さ  
    - gridなどで横並びに表示できるようにすればより良いと考えます。  
- [ ] グラフ生成までの手順が煩雑  
    - ページ描画時にはチェックボックスの選択に関わらず、裏で全ての都道府県の人口構成を取得して、チェックボックスで選択された都道府県の人口構成のみをグラフ描画するという形が理想だと考えます。  
    - 現状：チェックボックスで選択された都道府県の人口構成を取得 → グラフ描画  
    - 理想：都道府県の人口構成を取得 → チェックボックスで選択された → グラフ描画  
- [ ] グラフの横軸の目盛りが年度になっていない  
- [ ] グラフの縦軸の単位がわかりにくい
    - 現在：1500k  
    - 理想：150万人、など  
- [ ] グラフ描画コンポーネントではデータの処理をさせている。  
    - 処理済みのデータを渡して、グラフ描画コンポーネント自体は受け取った値を描画するのみの機能にする。  
  
## 参考にしたサイト  
- [条件付きレンダリング — Vue.js](https://jp.vuejs.org/v2/guide/conditional.html)  
  