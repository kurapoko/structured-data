# 構造化データマークアップの方法

## 概要

Googleの検索エンジンが、そのページに関する情報をより詳しく理解するために使うことができます。  


例えば、下記のレシピのマークアップがある場合、  
人間が、このhtmlはレシピについての情報であるということは理解できますが、検索エンジンが理解することは困難です。
```
<section class="recipe">
  <h1>牛丼</h1>
  <img src="recipe.jpg">
  <p>おいしい牛丼です。</p>
  <p>作った人の名前</p>
  <p>調理時間：10分</p>
  <p>材料</p>
  <ul class="gredients">
    <li>牛肉</li>
    <li>お米</li>
  </ul>
  <ul class="instoruction">
    <li>煮る</li>
    <li>焼く</li>
  </ul>
</section>

```

しかし、下記のようにメタデータを付与することによって検索エンジンが、レシピ名や材料、調理手順等の情報が理解できるようになります。

```
<script type="application/ld+json">
  {
    "@context": "https://schema.org/",
    "@type": "Recipe",
    "name": "牛丼",
    "image": "recipe.jpg",
    "description": "おいしい牛丼です。",
    "author": {
      "@type": "Person",
      "name": "作った人の名前"
    },
    "prepTime": "PT0M",
    "cookTime": "PT10M",
    "totalTime": "PT10M",
    "recipeIngredient": [
      "牛肉",
      "お米"
    
    ],
    "recipeInstructions": [
      {
        "@type": "HowToStep",
        "text": "煮る"
      },
      {
        "@type": "HowToStep",
        "text": "焼く"
      }
    ]
  }
</script>

```

検索エンジンが情報を詳しく理解することによって、検索結果により詳しい情報(リッチリザルト)が表示されるようになります。
どのようなリッチリザルトが表示されるかは、下記ページにビジュアル付きで載っています。

https://developers.google.com/search/docs/guides/search-gallery?hl=ja


※上記の事例の場合のプレビューです。  
https://cutt.ly/ner9YT9


### 構造化データの種類

主にレシピや求人情報、書籍、パンくずリストなどがあります。対応可能な構造化データは下記ページに記載されています。 
(左側のメニューに表示されている内容です。)  
https://developers.google.com/search/docs/data-types/article?hl=ja  
こちらの内容は、随時、追加削除されているのでその都度確認してください。


## 事例

**DELISH KITCHEN**(動画付きレシピサイト)  
https://delishkitchen.tv/categories/9241  

### カルーセル
(検索ワード：肉料理 レシピ)で検索した場合。

https://cutt.ly/3er2S9P

構造化データマークアップをしているおかげで、DELISH KITCHENのブロックにはカルーセル形式のリンクリストが表示されています。  
(これがリッチリザルトです。)  
他サイトの検索結果表示と比較しても表示されている情報量が多いため他のサイトと比べてより多くのPVを期待できます。

### レシピ
(検索ワード：油淋鶏 レシピ)で検索した場合。

https://cutt.ly/6er9sdk



## 記載方法

主にRDFa、microdata、JSON-LDがあります。  GoogleがJSON-LDを推奨しているので、JSON-LDの記述方法を記載します。


### 基本形

```
<script type="application/ld+json">
{
  "@context": "https://schema.org/",
  "@type": "Recipe"
}
</script>

```
|key|value|概要|
|:---|:---|:---|
|@context|http://schema.org|schema.orgに準じて書くという宣言です。基本的にはこの記述です。|
|@type|Recipe、BreadcrumbListなど|構造化データの種類を記載します。|



### パンくずリスト

https://developers.google.com/search/docs/data-types/breadcrumb?hl=ja

```

<script type="application/ld+json">
{
 "@context": "http://schema.org",
 "@type": "BreadcrumbList", //パンくずリストの設定
 "itemListElement":
 [
  {
    "@type": "ListItem",
    "position": 1,
    "name": "トップ",
    "item":"https://example.com"
  },
  {
    "@type": "ListItem",
    "position": 2,
    "name": "ニュース",
    "item":"https://example.com/news"
  },
  {
    "@type": "ListItem",
    "position": 3,
    "name": "ニュース記事01",
    "item":"https://example.com/news/01"
  }
 ]
}
</script>

```
**必須プロパティ**

|key|value|
|:---|:---|
|position|パンくずリスト内のパンくずの位置。|
|name|表示されるパンくずのタイトル|
|item|表示されるパンくずのURL|



### カルーセル

```

<script type="application/ld+json">
{
 "@context": "http://schema.org",
 "@type": "BreadcrumbList", //パンくずリストの設定
 "itemListElement":
 [
  {
    "@type": "ListItem",
    "position": 1,
    "name": "トップ",
    "item":"https://example.com"
  },
  {
    "@type": "ListItem",
    "position": 2,
    "name": "ニュース",
    "item":"https://example.com/news"
  },
  {
    "@type": "ListItem",
    "position": 3,
    "name": "ニュース記事01",
    "item":"https://example.com/news/01"
  }
 ]
}
</script>

```
**必須プロパティ**

|key|value|
|:---|:---|
|position|パンくずリスト内のパンくずの位置。|
|name|表示されるパンくずのタイトル|
|item|表示されるパンくずのURL|

2019年9月現在、カルーセルが使えるのは、下記コンテンツタイプです

|コンテンツタイプ|概要|URL|
|:---|:---|:---|
|Recipe|レシピサイトなど|https://developers.google.com/search/docs/data-types/recipe?hl=ja|
|Course|スクールや教室などのコース|https://developers.google.com/search/docs/data-types/course?hl=ja|
|Articl|スクールや教室などのコース|https://developers.google.com/search/docs/data-types/course?hl=ja|

### 動画付きレシピ

※kurashiruのレシピを参考。  
https://www.kurashiru.com/recipes/795b460a-e0ed-4028-ba16-0db03c66bdfe

```
<script type="application/ld+json">{
  "@context": "http://schema.org",
  "@type": "Recipe",
  "author": {
    "@type": "Organization",
    "url": "https://www.kurashiru.com/files/kurashiru_large_logo.png",
    "name": "クラシル オフィシャル"
  },
  "mainEntityOfPage": {
    "@type": "WebPage",
    "@id": "https://www.kurashiru.com/recipes/795b460a-e0ed-4028-ba16-0db03c66bdfe"
  },
  "recipeCategory": "主食・おかず",
  "name": "ピリ辛 サムギョプサルうどん",
  "headline": "「ピリ辛 サムギョプサルうどん」の作り方を簡単で分かりやすいレシピ動画で紹介しています。",
  "image": "https://video.kurashiru.com/production/videos/795b460a-e0ed-4028-ba16-0db03c66bdfe/compressed_thumbnail_square_normal.jpg?1529054789",
  "datePublished": "2018-07-04",
  "dateModified": "2018-07-04",
  "publisher": {
    "@type": "Organization",
    "name": "kurashiru",
    "logo": "https://www.kurashiru.com/files/kurashiru_large_logo.png"
  },
  "description": "豚バラ肉を使って、ピリ辛味に仕上げたうどんのご紹介です。辛いのがお好きな方や、刺激のあるものが食べたい時におすすめですよ。豚バラ肉が入ることで食べ応えも十分あります。お好きな野菜を入れて具材を増やしても美味しいですよ。ぜひお試しください。",
  "prepTime": "PT0M",
  "cookTime": "PT20M",
  "totalTime": "PT20M",
  "recipeYield": "1 servings",
  "recipeIngredient": ["うどん 1玉", "お湯 適量", "豚バラ肉 80g", "塩こしょう ひとつまみ", "ごま油 小さじ1", "キムチ 50g", "サンチュ 1枚", "ゆで卵 1/2個", "コチュジャン 大さじ1", "酢 大さじ1/2", "しょうゆ 大さじ1/2", "ごま油 小さじ1", "すりおろしニンニク 小さじ1/2", "砂糖 小さじ1/2", "白いりごま 適量"],
  "recipeInstructions": [
    {"@type": "HowToStep", "text": "ボウルにタレの調味料を入れ、よく混ぜ合わせます。"},
    {"@type": "HowToStep", "text": "豚バラ肉は4cm幅に切ります。"},
    {"@type": "HowToStep", "text": "中火で熱したフライパンにごま油をひきます。2を焼き、火が通ったら塩こしょうを振って火から下ろします。"},
    {"@type": "HowToStep", "text": "鍋にお湯を沸かして、うどんをパッケージの表記通りに茹でます。お湯を切って流水にさらし、水気を切ります。"},
    {"@type": "HowToStep", "text": "1に4を入れ、よく混ぜ合わせます。"},
    {"@type": "HowToStep", "text": "お皿に5を盛り、サンチュ、3、キムチ、ゆで卵を乗せ、白いりごまをかけて完成です。"}
  ],
  "video": {
    "@type": "VideoObject",
    "name": "ピリ辛 サムギョプサルうどん",
    "description": "豚バラ肉を使って、ピリ辛味に仕上げたうどんのご紹介です。辛いのがお好きな方や、刺激のあるものが食べたい時におすすめですよ。豚バラ肉が入ることで食べ応えも十分あります。お好きな野菜を入れて具材を増やしても美味しいですよ。ぜひお試しください。",
    "thumbnail": "https://video.kurashiru.com/production/videos/795b460a-e0ed-4028-ba16-0db03c66bdfe/compressed_thumbnail_square_normal.jpg?1529054789",
    "thumbnailUrl": "https://video.kurashiru.com/production/videos/795b460a-e0ed-4028-ba16-0db03c66bdfe/compressed_thumbnail_square_normal.jpg?1529054789",
    "contentURL": "https://video.kurashiru.com/production/videos/795b460a-e0ed-4028-ba16-0db03c66bdfe/original.mp4",
    "uploadDate": "2018-07-04T11:02:55+09:00",
    "duration": "36S",
    "height": "1080",
    "width": "1080",
    "author": {
      "@type": "Organization",
      "url": "https://www.kurashiru.com/files/kurashiru_large_logo.png",
      "name": "クラシル オフィシャル"
    }
  }
}</script>
```

レシピページのプロパティの説明は下記ページに記載されております。  
https://developers.google.com/search/docs/data-types/recipe?hl=ja


