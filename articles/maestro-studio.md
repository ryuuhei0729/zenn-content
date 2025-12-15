---
title: "【非エンジニアでも書ける！】Maestro Studio Desktopで爆速モバイルE2Eテスト入門"
emoji: "🎹"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["Maestro", "E2Eテスト", "モバイルアプリ", "QA", "テスト自動化"]
published: false
---

## はじめに

こんにちは、株式会社グロービスでQA業務を行っている細井です！
我々のチームでは社会人向けのe-learningアプリ（iOS/Android）を開発しております。

🍎[iOSアプリはこちらから](https://apps.apple.com/jp/app/globis-%E5%AD%A6%E3%81%B3%E6%94%BE%E9%A1%8C-%E3%83%93%E3%82%B8%E3%83%8D%E3%82%B9%E3%82%92%E4%BD%93%E7%B3%BB%E7%9A%84%E3%81%AB%E5%8B%95%E7%94%BB%E3%81%A7%E5%AD%A6%E3%81%B6/id1280172600)🍎
📱[Androidアプリはこちらから](https://play.google.com/store/apps/details?id=jp.co.globis.hodai&hl=ja)📱

ぜひインストールしてみてください☺︎

さて、この度我々のチームでは**コードを書いたことがないQAエンジニアでも実装・運用ができる**E2Eテストを実現するため、**Maestro**というテストツールを導入しました。

本記事では、その中でも特に便利な**Maestro Studio Desktop**というツールついてご紹介いたします。

:::message
**この記事でわかること**
- Maestroとは何か
- Maestro Studio Desktopとは何か
- 環境構築の手順
- 直感的なテスト作成方法
- 便利なTipsと注意点
:::

## Maestroとは？

**Maestro**は、YAML形式でモバイルアプリのUI操作やアサーションを記述できるE2Eテストツールです。
完全無料で使えて、かつ**学習時間ほぼ0**(環境構築さえ乗り越えれば…！)で利用できるのが最大の特徴です。
https://maestro.dev


何を隠そう我々のチームでも、E2Eテストは開発エンジニアやSETエンジニアではなく、**QA内で書けるようになろう！** という背景もあり、こちらを選定しました。

:::message alert
**実装していてイマイチだな〜と思っている点を先に挙げとく…**
- **プログラミング的な処理が苦手**
  - 条件分岐や繰り返し処理、変数格納などは実装が煩雑になりがち

- **レイヤー構造のUI要素の認識に難あり**
  - モーダル、ポップアップ、ドロワーなどの要素が認識されないケースがある（現在模索中…）
:::

## Maestro Studio Desktopとは？

**Maestro Studio Desktop**は、**GUIで直感的にMaestroテストを作成できるデスクトップアプリ**です。
Maestroの実装をより直感的にしてくれるサポートツール的なイメージです。
https://docs.maestro.dev/getting-started/maestro-studio-desktop

:::message alert
**注意**: Maestro Studio Desktopは現在**ベータ版**です。機能が追加・変更される可能性があるため、本番環境での使用は自己責任でお願いします。
:::


## 環境構築

Maestro / Maestro Studio Desktopを使うには、以下の準備が必要です。
絶対にどこかではつまづくと思うので、環境構築は他のエンジニアさんに手伝ってもらいましょう！非エンジニアにとってはここが正念場！

1. **Gitのインストール**
   - テストコードの管理にGitを使います
   - インストール確認: `git --version`
   - [Git公式サイト](https://git-scm.com/)

2. **テスト対象アプリのリポジトリをクローン**
   ```bash
   git clone https://github.com/your-org/your-app-repo.git
   cd your-app-repo
   ```

3. **エミュレータ/シミュレータの起動**

:::details Androidの場合
1. **Android Studioをインストール**
   - [公式サイト](https://developer.android.com/studio)からダウンロード
2. **エミュレータをセットアップ**
   - Android Studio → Device Manager → Create Device
   - 推奨: Pixel 6 / API 33以上
:::

:::details iOSの場合（Macのみ）
1. **Xcodeをインストール**
   - App Storeからダウンロード
2. **シミュレータを起動**
   - Xcode → Open Developer Tool → Simulator
   - または `open -a Simulator`
:::

### Maestroのインストール
テスト対象のレポジトリ内で、下記のコマンドをコピペしてみましょう。
```zsh
# maesroのインストール
curl -Ls "https://get.maestro.mobile.dev" | zsh

# インストール確認（バージョンが表示されればインストールOK）
maestro --version
```

### Maestro Studio DeskTopのインストール

**下記公式ページ**からダウンロードしてください。
https://docs.maestro.dev/getting-started/maestro-studio-desktop

:::message alert
**重要**: 繰り返しになりますが、Maestro Studio Desktopは**ベータ版**です。バグや予期しない動作が発生する可能性があります。問題があれば[GitHubのIssues](https://github.com/mobile-dev-inc/maestro/issues)で報告できます。
:::

## 使い方：直感的にテストを作成

### 基本の流れ

![Maestro Studio画面イメージ](/images/maestro-studio/実装.png)
*↑ 実際のMaestro Studio画面（右側の表示されているアプリはサンプル用にAIで作成しました🤖）*

1. **要素をクリック** → アプリ画面上の要素にカーソルを合わせる
2. **アクションを選択** → `tapOn` / `assertVisible` / `scrollUntilVisible` などが表示
3. **Insertで追加** → テストコードが自動生成！

### 生成されるYAMLの例

```yaml
appId: com.example.myapp
---
- launchApp
- tapOn:
   id: "email_input"
- inputText: "test@example.com"
- tapOn:
   id: "password_input"  
- inputText: "password123"
- tapOn:
   id: "login_button"
- assertVisible: "ようこそ！"
```

## 実践Tips

### 🔐 環境変数で秘匿情報を管理

メールアドレスやパスワードなど、コードに直接書きたくない情報は環境変数を使いましょう。
右上の歯車アイコンの**Environment**から設定できます
![Maestro Studio画面イメージ](/images/maestro-studio/環境.png)

### ▶️ テストの実行方法

#### 方法1: 行指定で実行

各テストコードの左側の実行ボタンを押すと、その行から下のテストが実行されます。
![Maestro Studio画面イメージ](/images/maestro-studio/行ごと実行.png)

#### 方法2: ファイルごとに実行

各ファイルの「Run Locally」もしくはサイドバーディレクトリのファイル名左側の再生ボタンをクリックすると、そのファイル全体のテストが実行されます。
![Maestro Studio画面イメージ](/images/maestro-studio/ファイル一括実行.png)

#### 方法3: 一括実行
サイドバー最上部の「Run All Tests」をクリックすると、ディレクトリ内全てのファイルを実行できます。
![Maestro Studio画面イメージ](/images/maestro-studio/全部一括実行.png)
:::message alert
※このボタンでは**直下のファイルのみ**実行対象になるため、孫関係にあるようなファイルは読み込んでくれません、、、改善求ム、、、
:::

## チームで実装を進めてみて、、、

学習コストがとにかく低いので、環境構築が完了してからは爆速で実装を進められました！
QA業務の隙間に2人で実装を進め、約2ヶ月かけて100ケースほど実装できました。
（環境構築は開発エンジニアさんにハンズオンで手伝っていただきました🙌）

また、現状は手動リグレッションテストの際や簡単な健康診断として手動実行しておりますが、ゆくゆくはCI実行なども視野に入れていこうと思います。


## 参考リンク

*公式ドキュメント*
- [Maestro公式サイト](https://maestro.mobile.dev/)
- [Maestro GitHub](https://github.com/mobile-dev-inc/maestro)
- [Maestro Studio Desktop 公式ドキュメント](https://docs.maestro.dev/getting-started/maestro-studio-desktop)

*参考記事*
- [Maestro公式ブログ](https://blog.mobile.dev/)
- [Maestro Slack コミュニティ](https://maestro-community.slack.com/) - 質問・情報交換に便利
- [Maestro Examples（サンプル集）](https://github.com/mobile-dev-inc/maestro/tree/main/maestro-test/src/test/resources/samples)

*関連ツール*
- [Android Studio](https://developer.android.com/studio) - Androidエミュレータに必要
- [Xcode](https://developer.apple.com/xcode/) - iOSシミュレータに必要（Mac限定）
