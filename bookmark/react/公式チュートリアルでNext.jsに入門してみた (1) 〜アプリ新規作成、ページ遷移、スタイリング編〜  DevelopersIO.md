---
title: "公式チュートリアルでNext.jsに入門してみた (1) 〜アプリ新規作成、ページ遷移、スタイリング編〜 | DevelopersIO"
source: "https://dev.classmethod.jp/articles/introduction-to-nextjs/"
author:
  - "[[若槻龍太]]"
published: 2022-01-02
created: 2025-10-15
description:
tags:
  - "clippings"
image: "https://devio2023-media.developers.io/wp-content/uploads/2021/12/eyecatch_NextJS_1200x630.png"
---
こんにちは、CX事業本部 IoT事業部の若槻です。

今回は、現在注目されているフロントエンドフレームワーク **Next.js** への入門のために、次の **公式チュートリアル** を数回のシリーズに分けてこなしていき、基本的な機能に触れていこうと思います。

- [Create a Next.js App | Learn Next.js](https://nextjs.org/learn/basics/create-nextjs-app)

Next.jsは、オープンソースで提供される **Reactベースのフロントエンドフレームワーク** です。

- [Next.js by Vercel - The React Framework](https://nextjs.org/)

[こちら](https://nextjs.org/learn/basics/create-nextjs-app) によるとNext.jsの特徴は次のようなものがあり、 **プロダクション環境で必要とされるあらゆる機能** と、 **最高の開発者エクスペリエンス** を提供できるように設計されています。

> - An intuitive page-based routing system (with support for dynamic routes)
> - Pre-rendering, both static generation (SSG) and server-side rendering (SSR) are supported on a per-page basis
> - Automatic code splitting for faster page loads
> - Client-side routing with optimized prefetching
> - Built-in CSS and Sass support, and support for any CSS-in-JS library
> - Development environment with Fast Refresh support
> - API routes to build API endpoints with Serverless Functions
> - Fully extendable

以下は意訳したもの。

> - 直感的なページベースのルーティングシステム（ **dynamic routes** のサポート）
> - **static generation(SSG)** と **server-side rendering(SSR)** の両方のpre-rendering方式をページごとにサポート
> - ページのロード高速化のための自動コード分割
> - プリフェッチを最適化した **client-side routing**
> - CSSとSassのビルトインサポートおよびCSS-in-JSライブラリのサポート
> - Development環境での **Fast Refresh** のサポート
> - Serverless Functionsを使用したAPIエンドポイントを構築するための **API routes**
> - 完全な拡張可能性

チュートリアルを進めていく中でこれら特徴について触れていければと思います。

## やってみた

それではチュートリアルに沿ってNext.jsアプリを作成してみます。参考にしたのは次の公式チュートリアルです。

- [Create a Next.js App | Learn Next.js](https://nextjs.org/learn/basics/create-nextjs-app)

本記事(1)では、チュートリアルのうち「Create a Next.js App」、「Navigate Between Pages」および「Assets, Metadata, and CSS」をやっていき、Next.jsの **アプリ新規作成** 、 **ページ遷移** および **スタイリング** について理解を深めていきます。

- **(1) 〜アプリ新規作成、ページ遷移、スタイリング編〜**
	- **Create a Next.js App**
	- **Navigate Between Pages**
	- **Assets, Metadata, and CSS**
- [(2) 〜Pre-rendering、データフェッチ編〜](https://dev.classmethod.jp/articles/introduction-to-nextjs-part-2/)
	- Pre-rendering and Data Fetching
- [(3) 〜Dynamic Routes、API Routes編〜](https://dev.classmethod.jp/articles/introduction-to-nextjs-part-3/)
	- Dynamic Routes
	- API Routes
- [(4) 〜デプロイ編〜](https://dev.classmethod.jp/articles/introduction-to-nextjs-part-4/)
	- Deploying Your Next.js App

### Create a Next.js App

ここではアプリケーションの新規作成を行います。

#### Setup

##### Create a Next.js app

`create-next-app` コマンドを使用してNext.jsアプリケーションを新規作成します。

```
$ npx create-next-app nextjs-blog --use-npm --example "https://github.com/vercel/next-learn/tree/master/basics/learn-starter"
```

`--example` オプションではテンプレートとするソースコードを指定できます。

`--use-npm` オプションを指定するとnpmを使用してブートストラップすることを明示的に指定できます。npm以外だとyarnが使用可能です。

- [Options - Create Next App | Next.js](https://nextjs.org/docs/api-reference/create-next-app#options)

##### Run the development server

次のコマンドを実行して、アプリケーションの開発サーバーを起動します。

```
$ cd nextjs-blog
$ npm run dev

> @ dev /Users/wakatsuki.ryuta/projects/cm-rwakatsuki/nextjs-tutorial/nextjs-blog
> next dev

ready - started server on 0.0.0.0:3000, url: http://localhost:3000
event - compiled client and server successfully in 1618 ms (158 modules)
Attention: Next.js now collects completely anonymous telemetry regarding usage.
This information is used to shape Next.js' roadmap and prioritize features.
You can learn more, including how to opt-out if you'd not like to participate in this anonymous program, by visiting the following URL:
https://nextjs.org/telemetry

wait  - compiling / (client and server)...
event - compiled client and server successfully in 265 ms (174 modules)
```

起動後に `http://localhost:3000` にアクセスすると、アプリケーションにアクセスできました！ ![](https://www.evernote.com/l/APdyB41W6l1HZ6TTqwahq8lWMfDRY1ZhI2oB/image.png)

##### Editing the Page

`index.js` ファイルを次のように `<h1>` の内容を `Welcome to` から `Learn` へ変更します。

```
import Head from 'next/head'

export default function Home() {
  return (
    <div className="container">
      <Head>
        <title>Create Next App</title>
        <link rel="icon" href="/favicon.ico" />
      </Head>

      <main>
        <h1 className="title">
          Learn <a href="https://nextjs.org">Next.js!</a>
        </h1>
```

ファイルの編集を保存すると、ページの更新などをせずに即座に変更が反映されました！ ![](https://www.evernote.com/l/APdWE45kBWRCZqP775aPyrRwFPAKmwPXQjoB/image.png)

これが開発サーバーの **Fast Refresh** による即時反映とのこと。

- [Basic Features: Fast Refresh | Next.js](https://nextjs.org/docs/basic-features/fast-refresh)

> Fast Refresh is a Next.js feature that gives you instantaneous feedback on edits made to your React components. Fast Refresh is enabled by default in all Next.js applications on 9.4 or newer. With Next.js Fast Refresh enabled, most edits should be visible within a second, without losing component state.

### Navigate Between Pages

ここではページ遷移を実装します。

#### Pages in Next.js

新しいページを作成します。

`pages/posts/` に次のファイル `first-post.js` を作成します。

```
export default function FirstPost() {
  return <h1>First Post</h1>
}
```

`http://localhost:3000/posts/first-post` にアクセスすると、新しいページにアクセスできました。 ![](https://www.evernote.com/l/APfsvbv9mPJKzaaN-8oaa_s74P1tk5WrewIB/image.png)

これが **ページベースのルーティング** です。ソースコード上のページファイルのパス配置が、アプリケーション上のページのURLのパスに対応しているため、直感的な実装が可能となっていますね！

#### Link Component

`Link` Componentを使って **client-side navigation** によるページ間の遷移を実装してみます。

`pages/index.js` ファイルで、 `Link` のインポートの記述を追記します。また `<h1>` の内容を次の通り変更します。

```
import Head from 'next/head'
import Link from 'next/link' //追記

export default function Home() {
  return (
    <div className="container">
      <Head>
        <title>Create Next App</title>
        <link rel="icon" href="/favicon.ico" />
      </Head>

      <main>
        <h1 className="title">
          Read{' '}
          <Link href="/posts/first-post">
            <a>this page!</a>
          </Link>
        </h1>
```

また `pages/posts/first-post.js` ファイルを次の通り変更し、トップページに戻るためのリンクを設置します。

```
import Link from 'next/link'

export default function FirstPost() {
  return (
    <>
      <h1>First Post</h1>
      <h2>
        <Link href="/">
          <a>Back to home</a>
        </Link>
      </h2>
    </>
  )
}
```

するとアプリケーションでLinkを使用したページ間の遷移ができるようになりました！ ![nextjs-page-link](https://user-images.githubusercontent.com/57384023/147879192-41241c2d-f3af-48a8-8f48-f58181f03fda.gif)

このclient-side navigationによるページ間の遷移は、 **JavaScriptが表示ページとURLをクライアント側だけで変更する** ので、 **通常のページ遷移よりも高速** となります。

ここではアプリケーションのスタイリングを実装します。

#### Assets

##### Download Your Profile Picture

適当なプロフィール画像データを `public/images` 配下に `profile.jpg` として配置します。 ![](https://www.evernote.com/l/APeNBkUg45ZGuap2DEHQlWVHGRyFQB-Ok1EB/image.png)

##### Unoptimized Image

標準的なHTMLでは画像の追加は次のように `<img>` を使用して行います。

```
<img src="/images/profile.jpg" alt="Your Name" />
```

しかしこの場合だとNext.jsにおいては画像サイズの最適化やレイジーロードは行われません。

##### Using the Image Component

Next.jsでは `Image` を使用することにより画像サイズの最適化やレイジーロードを自動で行うことができます。

```
import Image from 'next/image'

const YourComponent = () => (
  <Image
    src="/images/profile.jpg" // Route of the image file
    height={144} // Desired size with correct aspect ratio
    width={144} // Desired size with correct aspect ratio
    alt="Your Name"
  />
)
```

このコードは後ほどまた出てくるので、まだファイルには追加せずにおきます。

#### Metadata

##### Adding Head to first-post.js

`<Head>` を使用するとページのメタデータを設定することができます。

`pages/posts/first-post.js` ファイルで、 `Head` のインポートの記述を追記します。また `<Head>` の記述を次の通り追加します。

```
import Link from 'next/link'
import Head from 'next/head'  //追記

export default function FirstPost() {
  return (
    <>
      <Head>
        <title>First Post</title>
      </Head>
      <h1>First Post</h1>
      <h2>
        <Link href="/">
          <a>Back to home</a>
        </Link>
      </h2>
    </>
  )
}
```

`http://localhost:3000/posts/first-post` にアクセスすると、 `<title>` で指定した通りページのタブを変更できました。 ![](https://www.evernote.com/l/APcpFOl1mdZGBroBKiwNsdJPpKNpYcR3UDIB/image.png)

#### Third-Party JavaScript

##### Adding Third-Party JavaScript

標準的なHTMLではサードパーティスクリプトの追加は次のように `<script>` を使用して行います。下記はFacebookソーシャルプラグインを追加している例です。

```
<Head>
  <title>First Post</title>
  <script src="https://connect.facebook.net/en_US/sdk.js" />
</Head>
```

しかしこの場合だとFacebook SDKのフェッチにより他のページコンテンツの読み込みが遅れる可能性があります。

##### Using the Script Component

Next.jsでは `Script` を使用することにより、スクリプトの読み込みや実行を最適化することができます。

`pages/posts/first-post.js` ファイルで、 `Script` のインポートの記述を追記します。また `<Script>` の記述を次の通り追加します。

```
import Link from 'next/link'
import Head from 'next/head'
import Script from 'next/script'  //追記

export default function FirstPost() {
  return (
    <>
      <Head>
        <title>First Post</title>
      </Head>
      <Script
        src="https://connect.facebook.net/en_US/sdk.js"
        strategy="lazyOnload"
        onLoad={() =>
          console.log(\`script loaded correctly, window.FB has been populated\`)
        }
      />
```

`strategy` で `lazyOnload` を指定するとスクリプトのレイジーロードを行うようにすることができます。これによりページ表示速度を向上できます。

`http://localhost:3000/posts/first-post` にアクセスすると、SDKロード時の処理が実行できていますね。 ![](https://www.evernote.com/l/APeeHjQvaehAvYpoi55UVlQOpOS7gz58YscB/image.png)

#### CSS Styling

`pages/index.js` の中では、すでに次の記述が使われています。

```
<style jsx>{\`
  …
\`}</style>
```

これは **styled-jsx** というCSS-in-JSライブラリを使用したもので、これによりReactコンポーネント内でCSSを記述できるようになります。Next.jsではstyled-jsをビルトインでサポートしています。

#### Layout Component

ここではレイアウトを変更してみます。

プロジェクトトップに `components` ディレクトリを作成し、 `layout.module.css` ファイルを作成します。これはCSSモジュールファイルで、ファイル名末尾は`.module.css` とする必要があります。

```
.container {
  max-width: 36rem;
  padding: 0 1rem;
  margin: 3rem auto 6rem;
}
```

同じく `components` 配下に次の `layout.js` ファイルを作成します。

```
import styles from './layout.module.css'

export default function Layout({ children }) {
  return <div className={styles.container}>{children}</div>
}
```

`pages/posts/first-post.js` を次のように変更します。 `Layout` コンポーネントをimportして、すべてのタグを囲います。（先程の `Script` の記述は削除して構いません。）

```
import Head from 'next/head'
import Link from 'next/link'
import Layout from '../../components/layout'

export default function FirstPost() {
  return (
    <Layout>
      <Head>
        <title>First Post</title>
      </Head>
      <h1>First Post</h1>
      <h2>
        <Link href="/">
          <a>Back to home</a>
        </Link>
      </h2>
    </Layout>
  )
}
```

`http://localhost:3000/posts/first-post` にアクセスすると、 `<Layout>` 内の要素をセンターに配置できました。 ![](https://www.evernote.com/l/APfMck-ZzVNIUJQnQtuxShVztZd6SiooHYYB/image.png)

#### Global Styles

ここでは、グローバルに適用できるCSSスタイルを作成します。

プロジェクトトップに `styles` ディレクトリを作成し、次の `global.css` ファイルを作成します。

```
html,
body {
  padding: 0;
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, Segoe UI, Roboto, Oxygen, Ubuntu,
    Cantarell, Fira Sans, Droid Sans, Helvetica Neue, sans-serif;
  line-height: 1.6;
  font-size: 18px;
}

* {
  box-sizing: border-box;
}

a {
  color: #0070f3;
  text-decoration: none;
}

a:hover {
  text-decoration: underline;
}

img {
  max-width: 100%;
  display: block;
}
```

`pages` 配下に次の `_app.js` ファイルを作成します。これにより上述のCSSをグローバルに適用します。

```
import '../styles/global.css'

export default function App({ Component, pageProps }) {
  return <Component {...pageProps} />
}
```

`http://localhost:3000/posts/first-post` にアクセスすると、スタイルを適用できていますね。 ![](https://www.evernote.com/l/APdcJCJRZ7tOP5zbV8ByfRtgm6Uak1EoyGcB/image.png)

#### Polishing Layout

ここまで作成したアプリケーションのレイアウトを改善していきます。

##### Update components/layout.module.css

`components/layout.module.css` を次の通り更新します。

##### Create styles/utils.module.css

次の通り `styles/utils.module.css` を作成します。

```
.heading2Xl {
  font-size: 2.5rem;
  line-height: 1.2;
  font-weight: 800;
  letter-spacing: -0.05rem;
  margin: 1rem 0;
}

.headingXl {
  font-size: 2rem;
  line-height: 1.3;
  font-weight: 800;
  letter-spacing: -0.05rem;
  margin: 1rem 0;
}

.headingLg {
  font-size: 1.5rem;
  line-height: 1.4;
  margin: 1rem 0;
}

.headingMd {
  font-size: 1.2rem;
  line-height: 1.5;
}

.borderCircle {
  border-radius: 9999px;
}

.colorInherit {
  color: inherit;
}

.padding1px {
  padding-top: 1px;
}

.list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.listItem {
  margin: 0 0 1.25rem;
}

.lightText {
  color: #666;
}
```

##### Update components/layout.js

`components/layout.js` を次の通り更新します。

```
import Head from 'next/head'
import Image from 'next/image'
import styles from './layout.module.css'
import utilStyles from '../styles/utils.module.css'
import Link from 'next/link'

const name = 'Your Name'
export const siteTitle = 'Next.js Sample Website'

export default function Layout({ children, home }) {
  return (
    <div className={styles.container}>
      <Head>
        <link rel="icon" href="/favicon.ico" />
        <meta
          name="description"
          content="Learn how to build a personal website using Next.js"
        />
        <meta
          property="og:image"
          content={\`https://og-image.vercel.app/${encodeURI(
            siteTitle
          )}.png?theme=light&md=0&fontSize=75px&images=https%3A%2F%2Fassets.vercel.com%2Fimage%2Fupload%2Ffront%2Fassets%2Fdesign%2Fnextjs-black-logo.svg\`}
        />
        <meta name="og:title" content={siteTitle} />
        <meta name="twitter:card" content="summary_large_image" />
      </Head>
      <header className={styles.header}>
        {home ? (
          <>
            <Image
              priority
              src="/images/profile.jpg"
              className={utilStyles.borderCircle}
              height={144}
              width={144}
              alt={name}
            />
            <h1 className={utilStyles.heading2Xl}>{name}</h1>
          </>
        ) : (
          <>
            <Link href="/">
              <a>
                <Image
                  priority
                  src="/images/profile.jpg"
                  className={utilStyles.borderCircle}
                  height={108}
                  width={108}
                  alt={name}
                />
              </a>
            </Link>
            <h2 className={utilStyles.headingLg}>
              <Link href="/">
                <a className={utilStyles.colorInherit}>{name}</a>
              </Link>
            </h2>
          </>
        )}
      </header>
      <main>{children}</main>
      {!home && (
        <div className={styles.backToHome}>
          <Link href="/">
            <a>← Back to home</a>
          </Link>
        </div>
      )}
    </div>
  )
}
```

##### Update pages/index.js

`pages/index.js` を次の通り更新します。

```
import Head from 'next/head'
import Layout, { siteTitle } from '../components/layout'
import utilStyles from '../styles/utils.module.css'

export default function Home() {
  return (
    <Layout home>
      <Head>
        <title>{siteTitle}</title>
      </Head>
      <section className={utilStyles.headingMd}>
        <p>[Your Self Introduction]</p>
        <p>
          (This is a sample website - you’ll be building a site like this on{' '}
          <a href="https://nextjs.org/learn">our Next.js tutorial</a>.)
        </p>
      </section>
    </Layout>
  )
}
```

`http://localhost:3000/posts/first-post` にアクセスすると、「Assets, Metadata, and CSS」で追加したプロフィール画像を表示できました。 ![](https://www.evernote.com/l/APeuwnRqaJVMoLb_cxJLwoFdquMj8LiB8x0B/image.png)

## おわりに

公式チュートリアルでNext.jsに入門してみた (1) 〜アプリ新規作成、ページ遷移、スタイリング編〜 でした。

次回はこちらです。

- [公式チュートリアルでNext.jsに入門してみた (2) 〜Pre-rendering、データフェッチ編〜 | DevelopersIO](https://dev.classmethod.jp/articles/introduction-to-nextjs-part-2/)

以上

この記事をシェアする