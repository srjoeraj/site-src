---
title: "コンテンツの例"
date: 2020-08-09
draft: false
description: "コンテンツがどのように作成され、構成されるべきかを示すいくつかの例"
summary: "コンテンツがどのように構成されるべきかを示すいくつかの例です。"
slug: "content-examples"
tags: ["content", "example"]
---

ドキュメントを順番に読んできたのなら、Congoで利用可能な機能と設定についてはすべて知っているはずです。このページでは、それらをまとめて、あなたがHugoプロジェクトで使いたくなるような例をいくつか紹介します。

{{< alert >}}
**ヒント:** もしあなたがHugoに慣れていないのであれば、[Hugo docs](https://gohugo.io/content-management/page-bundles/)をチェックし、ページバンドルとリソースの概念について学んでください。
{{< /alert >}}

このページで紹介する例はさまざまなシナリオに適用できますが、個々のプロジェクトで特定のコンテンツ項目をフォーマットする方法について、いくつかのアイデアが得られることを願っています。

## ブランチページ

Hugoのブランチページバンドルは、ホームページ、セクションリスト、Taxonomyページのような項目をカバーしています。ブランチバンドルについて覚えておくべき重要なことは、このコンテンツタイプのファイル名は **`_index.md`** であるということです。

Congoはブランチページで指定されたフロントマターを尊重し、デフォルト設定を上書きします。例えば、ブランチページで `title` パラメーターを設定すると、ページタイトルを上書きすることができます。

### ホームページ

|              |                      |
| ------------ | -------------------- |
| **Layout:**  | `layouts/index.html` |
| **Content:** | `content/_index.md`  |

Congoのホームページは、ホームページレイアウト設定パラメーターによって包括的なデザインが制御されるという点で特別です。これについては[ホームページレイアウト]({{< ref "homepage-layout" >}})セクションで詳しく説明しています。

このページにカスタムコンテンツを追加したい場合は、 `content/_index.md` ファイルを作成するだけです。このファイルにあるものはすべてホームページに含まれます。

**例:**

```yaml
---
title: "Congoへようこそ！"
description: "これはホームページにコンテンツを追加するデモです"
---
私のウェブサイトへようこそ！立ち寄ってくれて本当に嬉しいです。
```

_この例では、カスタムタイトルを設定し、ページ本文にいくつかの追加テキストを追加します。ショートコード、画像、リンクを含め、どのようなMarkdownフォーマットのテキストでも構いません。_

### リストページ

|              |                              |
| ------------ | ---------------------------- |
| **Layout:**  | `layouts/_default/list.html` |
| **Content:** | `content/../_index.md`       |

リストページは、セクション内のすべてのページをグループ化し、訪問者が各ページに到達するための方法を提供します。ブログやポートフォリオは、記事やプロジェクトをグループ化したリストページの例です。

リストページの作成は、 `content` 内にサブディレクトリを作成するのと同じくらい簡単です。例えば、"Projects"セクションを作成するには、 `content/projects/` を作成します。そして、プロジェクトごとにMarkdownファイルを作成します。

デフォルトではリストページが生成されますが、コンテンツをカスタマイズするために、この新しいディレクトリに`_index.md`ページも作成してください。

```shell
.
└── content
    └── projects
        ├── _index.md          # /projects
        ├── first-project.md   # /projects/first-project
        └── another-project
            ├── index.md       # /projects/another-project
            └── project.jpg
```

Hugoは、 `content/projects` 内のページのURLを適宜生成します。

ホームページと同じように、 `_index.md` ファイルのコンテンツは生成されたリストインデックスに出力されます。Congoはこのセクションのすべてのページをリストします。

**例:**

```yaml
---
title: "Projects"
description: "私のプロジェクトについて"
cascade:
  showReadingTime: false
---
このセクションには、私が現在取り組んでいるすべてのプロジェクトが含まれています。
```

_この例では、特別な `cascade` パラメーターを使って、このセクション内のサブページの読書時間を非表示にしています。こうすることで、どのプロジェクトページでも読書時間が表示されなくなります。これは、個々のページにデフォルトのテーマパラメーターを含めなくても、セクション全体のデフォルトのテーマパラメーターを上書きすることができる素晴らしい方法です。_

このサイトの[サンプル]({{< ref "samples" >}})はリストページの一例です。

### Taxonomyページ

|                  |                                  |
| ---------------- | -------------------------------- |
| **List layout:** | `layouts/_default/taxonomy.html` |
| **Term layout:** | `layouts/_default/term.html`     |
| **Content:**     | `content/../_index.md`           |

Taxonomyページには、TaxonomyのリストとTaxonomyのTermという2つの形式があります。リストはTaxonomy内の各Termのリストを表示し、Termは指定されたTermに関連するページのリストを表示します。

Termは少し混乱しやすいので、`animals` というTaxonomyを使って例を探ってみましょう。

まず、HugoでTaxonomyを使うには設定が必要です。 `config/_default/taxonomies.toml` に設定ファイルを作成し、Taxonomyの名前を定義します。

```toml
# config/_default/taxonomies.toml

animal = "animals"
```

HugoはTaxonomyを単数形と複数形でリストすることを想定しているので、単数形の `animal` と複数形の `animals` を追加して、例のTaxonomyを作成します。

これで `animals` Taxonomyが存在することになったので、個々のコンテンツに追加する必要があります。フロントマターに挿入するだけです:

```yaml
---
title: "ライオンの巣へ"
description: "今週はライオンについて学びます"
animals: ["lion", "cat"]
---
```

これで `animals` Taxonomyの中に `lion` と `cat` というTermができたことになります。

この時点では明らかではありませんが、Hugoはこの新しいTaxonomyリストとTermのページを生成します。デフォルトでは、リストは `/animals/` に、Termページは `/animals/lion/` と `/animals/cat/` になります。

リストページはTaxonomyに含まれるすべてのTermをリストアップします。この例では、 `/animals/` に移動すると、 `lion` と `cat` のリンクがあるページが表示され、訪問者はそれぞれのTermページに移動できます。

TermページはそのTermが含まれるすべてのページをリストアップします。これらのTermリストは基本的に通常の[リストページ](#リストページ)とほとんど同じように動作します。

Taxonomyページにカスタムコンテンツを追加するには、Taxonomy名をサブディレクトリとして、 `content` 内に `_index.md` ファイルを作成するだけです。

```shell
.
└── content
    └── animals
        ├── _index.md       # /animals
        └── lion
            └── _index.md   # /animals/lion
```

これらのコンテンツファイルにあるものは生成されたTaxonomyページに配置されます。他のコンテンツと同じように、フロントマターはデフォルトを上書きするために使うことができます。このように、 `lion` という名前のタグがあっても、 `title` を"Lion"に上書きすることができます。

これが実際にどのように見えるかは、このサイトの[Tags]({{< ref "tags" >}})をチェックしてください。

## リーフページ

|                           |                                 |
| ------------------------- | ------------------------------- |
| **Layout:**               | `layouts/_default/single.html`  |
| **Content (standalone):** | `content/../page-name.md`       |
| **Content (bundled):**    | `content/../page-name/index.md` |

Hugoのリーフページは基本的に標準的なコンテンツページです。サブページを含まないページとして定義されます。例えば、アバウトページや、ウェブサイトのブログセクションにある個々のブログ記事などです。

リーフページについて覚えておくべき最も重要なことは、ブランチページとは異なり、リーフページはアンダースコアなしで `index.md` と名前をつけるべきということです。リーフページはまた、セクションのトップレベルにまとめて一意な名前をつけることができるという点で特別です。

```shell
.
└── content
    └── blog
        ├── first-post.md     # /blog/first-post
        ├── second-post.md    # /blog/second-post
        └── third-post
            ├── index.md      # /blog/third-post
            └── image.jpg
```

画像などをページに含める場合、ページバンドルを使用する必要があります。ページバンドルは `index.md` ファイルを含むサブディレクトリを使って作成します。ショートコードやその他のテーマロジックの多くは、リソースがページと一緒にバンドルされていることを前提としているので、コンテンツと一緒に独自のディレクトリにグループ化することが重要です。

**例:**

```yaml
---
title: "初めてのブログ投稿"
date: 2022-01-25
description: "私のブログへようこそ！"
summary: "私について、そして私がなぜこのブログを始めたのか、もっと知ってください。"
tags: ["welcome", "new", "about", "first"]
---
_これ_ が私のブログ記事の内容です。
```

リーフページには様々な[フロントマター]({{< ref "front-matter" >}})パラメーターがあり、それらを使って表示方法をカスタマイズすることができます。

### 外部リンク

Congoには、外部ページへのリンクを記事リストに表示できる特別な機能があります。これは、Mediumのようなサードパーティのウェブサイトや研究論文にコンテンツがあり、Hugoのサイトにコンテンツを複製することなくリンクを張りたい場合に便利です。

外部リンク記事を作成するには、特別なフロントマターを設定する必要があります:

```yaml
---
title: "私のMediumの記事"
date: 2022-01-25
externalUrl: "https://medium.com/"
summary: "私はMediumに記事を書きました。"
showReadingTime: false
_build:
  render: "false"
  list: "local"
---
```

`title` や `summary` のような通常のフロントマターパラメーターとともに、 `externalUrl` パラメーターはこの記事が普通の記事ではないことを伝えるために使われます。ここで指定されたURLは、訪問者がこの記事を選択したときに誘導される場所になります。

さらに、このコンテンツの通常のページが生成されないように（外部URLにリンクしているので、ページを生成する意味がありません！）、Hugoの特別なフロントマターパラメーターである `_build` を使用しています。

テーマには、このような外部リンク記事を簡単に生成するためのアーキタイプが含まれています。新しいコンテンツを作るときに `-k external` を指定するだけです。

```shell
hugo new -k external posts/my-post.md
```

### シンプルページ

|                   |                                |
| ----------------- | ------------------------------ |
| **Layout:**       | `layouts/_default/simple.html` |
| **Front Matter:** | `layout: "simple"`             |

Congoにはシンプルなページのための特別なレイアウトも含まれています。シンプル・レイアウトは全幅のテンプレートで、特別なテーマ機能なしにMarkdownコンテンツをページに配置するだけです。

シンプルレイアウトで利用できる唯一の機能はパンくずリストと共有リンクです。これらの動作は通常のページと同様に[フロントマター]({{< ref "front-matter" >}})パラメーターを使って制御することができます。

特定のページでシンプルレイアウトを有効にするには、 `layout` フロントマター変数に `"simple"` という値を追加します:

```yaml
---
title: "ランディングページ"
date: 2022-03-08
layout: "simple"
---
このページのコンテンツは全幅になりました。
```

## カスタムレイアウト

Hugoの利点のひとつは、サイト全体や個々のセクション、ページのカスタムレイアウトを簡単に作成できることです。

レイアウトは通常のHugoのテンプレート規則に従います。詳細は[Hugo公式ドキュメント](https://gohugo.io/templates/introduction/)をご覧ください。

### デフォルトレイアウトのオーバーライド

上で説明した各コンテンツタイプには、各タイプのページを生成するために使用されるレイアウトファイルが記載されています。このファイルをローカルプロジェクトに作成すると、テーマテンプレートを上書きするので、ウェブサイトのデフォルトスタイルをカスタマイズするために使用することができます。

例えば、 `layouts/_default/single.html` ファイルを作成すれば、リーフページのレイアウトを完全にカスタマイズすることができます。

### カスタムセクションレイアウト

また、個々のコンテンツセクションのカスタムレイアウトを作成するのも簡単です。これは、特定のコンテンツを特定のスタイルで一覧表示するセクションを作りたい場合に便利です。

特殊なレイアウトでプロジェクトを一覧表示するカスタム「Projects」ページを作成する例を見てみましょう。

これを行うには、通常のHugoコンテンツルールを使用してコンテンツを構成し、プロジェクト用のセクションを作成します。さらに、コンテンツと同じディレクトリ名を使い、 `list.html` ファイルを追加して、プロジェクトセクション用の新しいレイアウトを作成します。

```shell
.
└── content
│   └── projects
│       ├── _index.md
│       ├── first-project.md
│       └── second-project.md
└── layouts
    └── projects
        └── list.html
```

この `list.html` ファイルはデフォルトのリストテンプレートをオーバーライドします。このファイルを見る前に、まず個々のプロジェクトファイルを見てみましょう。

```yaml
---
title: "Congo"
date: 2021-08-11
icon: "github"
description: "Tailwind CSSで作られたHugoのテーマ"
topics: ["Hugo", "Web", "Tailwind"]
externalUrl: "https://github.com/jpanther/congo/"
---
```

_この例では、各プロジェクトにメタデータを割り当て、リストテンプレートで使用できるようにしています。ページのコンテンツはありませんが、それを含めることを妨げるものも何もありません。あなたのカスタムテンプレートなのですから！_

プロジェクトが定義されたので、各プロジェクトの詳細を出力するリストテンプレートを作成することができます。

```go
{{ define "main" }}
  <section class="mt-8">
    {{ range .Pages }}
      <article class="pb-6">
        <a class="flex" href="{{ .Params.externalUrl }}">
          <div class="mr-3 text-3xl text-neutral-300">
            <span class="relative inline-block align-text-bottom">
              {{ partial "icon.html" .Params.icon }}
            </span>
          </div>
          <div>
            <h3 class="flex text-xl font-semibold">
              {{ .Title }}
            </h3>
            <p class="text-sm text-neutral-400">
              {{ .Description }}
            </p>
          </div>
        </a>
      </article>
    {{ end }}
  </section>
{{ end }}
```

これは非常にわかりやすい例ですが、このセクションの各ページ（つまり各プロジェクト）を順に見ていき、各プロジェクトへのHTMLリンクをアイコンと一緒に出力していることがわかります。各プロジェクトのフロントマターのメタデータは、どの情報を表示するかを決定するために使われます。

関連するスタイルとクラスが利用可能であることを確認する必要があり、Tailwind CSSを再コンパイルする必要があるかもしれないことを覚えておいてください。これについては、[高度なカスタマイズ]({{< ref "advanced-customisation" >}})セクションで詳しく説明します。

このようなカスタムテンプレートを作成する場合、デフォルトのCongoテンプレートがどのように動作するかを見て、それをガイドとして使用するのが最も簡単です。[Hugo docs](https://gohugo.io/templates/introduction/)はテンプレートの作成についてもっと学ぶための素晴らしいリソースです。