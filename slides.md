# Continuous Integration

2016-07-26

\#study\_mtg\_engineering

Yuta Sakamaki @ SS

---

##目次

- 主要なCIツール
- smartica-apiをテストしてみた
- 使ってみた感想とか
- Jenkinsのオススメプラグイン
- 最後に

---

## 主要なCIツール

参考: [stackshareのランキング](http://stackshare.io/continuous-integration)

---

| Rank\* |                                                         Name                                                         |  Type | Price / MO. |
|:-------:|:--------------------------------------------------------------------------------------------------------------------:|:-----:|:-----------:|
|   1(2)  |           [<img src="images/jenkins.png" alt="jenkins" height="32" width="32">Jenkins](https://jenkins.io/)          |  Self |     Free    |
|   2(4)  |      [<img src="images/travis-ci.jpeg" alt="travis-ci" height="32" width="32">Travis CI](https://travis-ci.com/)     | Cloud |     $129    |
|   3(6)  |        [<img src="images/codeship.png" alt="codeship" height="32" width="32">Codeship](https://codeship.com/)        | Cloud |     $83     |
|   4(3)  |       [<img src="images/circle-ci.png" alt="circle-ci" height="32" width="32">CircleCI](https://circleci.com/)       | Cloud |     $50     |
|   5(5)  | [<img src="images/teamcity.png" alt="teamcity" height="32" width="32">TeamCity](https://www.jetbrains.com/teamcity/) |  Self |     Free    |
|   6(1)  |           [<img src="images/wercker.png" alt="wercker" height="32" width="32">wercker](http://wercker.com/)          | Cloud |     Free    |

\*カッコ内は個人的なランキング

---

## smartica-apiをテストしてみた

---

### 前提条件

- Play Framework 2.4.x (Scala) + PostgreSQL
- Scala向けの環境があっても使わずにJava8用の最小環境で `./activator` を叩く
- 実行環境はDocker Container内だったりそうじゃなかったり
- PostgreSQLはDockerで（最初から用意されていても）
- CodeshipだけはDockerが使えないっぽかったので、最初から動いてるDBを使用
- Slack通知
- デプロイ周りの機能は未使用
- テキトー

---

## 使ってみた感想とか

---

### Jenkins

![Jenkins](images/jenkins.png)

---

#### Feature

- 有名なおじさん
- OSS
- 無料
- Hudson
- ALBERTには少なくとも4匹の爺が生息している模様

---

#### Pros

- 無料で利用できる
- 細かな調整が可能
- 大体なんでも出来る
- だいぶ枯れてる
- 情報が豊富
- GUIから設定可能
- プラグインが豊富
- Ver. 2.0 で色々と改善されている？

---

#### Cons

- インフラ費がそこそこ掛かる
- 職人芸になりがち
- 枯れすぎ
- Dockerとの相性はイマイチ（プラグインはゴミ）
- 自分で色々めんどうを見る必要がある
- 当たり前のことがプラグインだったり
- GUIから設定可能
- よく怒る
- よく死ぬ
- よくゾンビクジラがうろついてる

---

### Travis CI

![Travis CI](images/travis-ci.jpeg)

---

#### Feature

- 結構有名なおじさん
- 実行環境のOSまで選択可能
- ソースコードは公開されているので、頑張ればセルフホスティングも可能？
- クラウド系CIツールの先駆者

---

#### Pros

- いい具合に枯れてる
- クラウド系CIサービスの先駆者？
- Cronジョブ機能があるっぽい
- 実行するOSを選べる

---

#### Cons

- Circle CI等に移行されているイメージ
- 他のCIサービスに比べて料金が割高
- 結構変な仕様が多い（らしい）
- SSHで入れない

---

### Codeship

![Codeship](images/codeship.png)

---

#### Feature

- YAMLでゴニョゴニョするCIサービスが多い中で、GUI中心のサービス
- といっても、結局はシェル芸？
- 最近はYAMLも導入し始めてるっぽい
- 主要なDBが最初から動いてる
- Dockerはリクエストしないと使えない？（詳しくは未調査）

---

#### Pros

- Jenkinsから移行し易い？
- 唯一、ライブラリのキャッシュを勝手に殺ってくれた
- GitHubだけでなく、BitBucketにも対応

---

#### Cons

- Dockerが何故かオプション扱い？
- オフィシャルのDockerイメージとかを使いたくても、Dockerfileがないとダメっぽい？
- 各所で日本語が表示されない
- ドキュメントが分かりにくい（見難い）

---

### CircleCI

![CircleCI](images/circle-ci.png)

---

#### Feature

- プロジェクトの内容を自動的に判別してくれる模様
- 暗黙的にジョブがセットされるので、明示的に `override` する必要がある
- 主要なDBが（ｒｙ
- 最近良く聞く気がする

---

#### Pros

- プロジェクトの自動判別が嬉しい人には嬉しい？
- とにかく安い（同等のサービス内容で、Travis CIの半額以下？）

---

#### Cons

- 暗黙的にジョブが組まれてしまうのが、個人的にはマイナス
- overrideする必要があることに気づくまでに10秒位ハマった

---

### TeamCity

![TeamCity](images/teamcity.png)

---

#### Feature

- いつもお世話になっている、IntelliJ IDEAのJetBrains社のCIツール
- smarticA!やrAprogでも前使ってた
- 1サーバ当たり3エージェント／20プロジェクトまで無料で使えるライセンスがある
- プラグインも意外とある
- Jenkinsの焼き直し的な

---

#### Pros

- Jenkinsの悪い点が改良されて、結構かゆいところに手が届く（Jenkinsに移行した感想）
- Jenkins同様、GUIベースなので取っ付き易い

---

#### Cons

- 情報が圧倒的に少ない
- 公式ドキュメントは割りと豊富だが、知りたい情報になかなか辿りつけない
- わざわざ使用する理由があまり見当たらない
- Docker連携は今のところ全く考慮されていないっぽい

---

### wercker

![wercker](images/wercker.png)

---

#### Feature

- 読み方はワーカー（worker）らしい
- Dockerネイティブ（になった）
- 何故か無料でそこそこ使える太っ腹
- 個人的には一番気に入った
- 以前はLXCベースの独自インフラ
- サービス自体は割りと以前からあるようだが、Dockerメインになって再注目されている？

---

#### Pros

- 実行環境も各種ミドルウェアも、Dockerコンテナが前提になっているので、Dockerを知っていれば学習コストが低く扱いやすい
- 無料で他のサービスの有料プラン並に使える
- 最近Workflowの仕組みが実装された（JenkinsのPipeline的な）
- VPC内のプライベート環境向けの有料プランが最近出来た？
- BitBucketでも使用可能

---

#### Cons

- たまに不安定？
- Queueが共用なので、すぐにジョブが始まらない事がある？
- 依存関係のキャッシュが一番面倒

---

## Jenkinsのオススメプラグイン

---

### Ansi Color

- オススメというか、ほぼ必須
- コンソール出力の色付き文字をちゃんと表示してくれる
- 使わないと↓のようになっちゃうアレ

```
[[36mdebug[0m] 11:26:20.256[pool-1-thread-1][ajUUdc61gC]DEBUG o.f.c.i.u.s.c.ClassPathScanner - Scanning for classpath resources at 'db/migration/default' (Prefix: 'V', Suffix: '.sql')
```

---

### Categorized Jobs View

- パターンマッチしたジョブごとにカテゴリー分けしてくれるビューを作れる
- あとから追加したジョブにも勝手に適用されるので、命名規則をちゃんと決めておけば色々捗る

---

### GitHub Pull Request Builder (GHPRB)

- GitHubのPRと連携するアレ
- 設定時のブランチには `${sha1}` を指定する例が多く出てくるが、 `${ghprbActualCommit}` にしないと変なリビジョンがビルドされたりするので注意
- `Cancel build on update` を有効にしておくと、ビルド中に再プッシュされた時に前のビルドをキャンセルしてくれるので便利（だが、Dockerゾンビ問題のせいで使えない。。）
- ビルドトリガーは、デフォルトだとCronベース（5分おき？）だが、 `GitHub Pull Request Builder` を使うと即時実行されていい感じ（↑との兼ね合いで、敢えて遅延させるのもあり）

---

### Job Config History

- XML形式でコンフィグの変更をバージョニングしてくれる
- 差分等見れて便利
- 他のサードパーティプラグインとの相性問題があるようなので、使用には注意

---

## 最後に

---

- Jenkinsに蓄積された **つらみ** は、smarticA!に蓄積された **つらみ** に相通ずるものがあります
- 基本的には、Travis CI / CircleCI / Wercker 等を使うのがよさ気だと思いました
- テスト実行にRedshift等のAWS等に依存したミドルウェアを使用していると、SGの問題などがあってめんどくさいです

---

## ご清聴、多謝
