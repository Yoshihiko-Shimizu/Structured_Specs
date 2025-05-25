# Structured_Specs

このリポジトリは、USDM（Universal Specification Describing Manner）に基づいて要求仕様をYAML形式で記述するための **JSON Schema定義** を提供します。構造化された要求と仕様の記述により、ドキュメントの品質と保守性を向上させます。

---

## 背景

USDMによる要求仕様はExcelなどのスプレッドシートで記述されることが前提にされていますが、次のような課題があります：

- テキストデータではないためGitなどの構成管理サービスの利用のハードルが高く、変更管理が難しい
- 階層構造や入れ子の構造を持つ情報（要求と仕様の関連、グルーピングなど）がスプレッドシートでは、見た目の表現に縛られてしまう
- スキーマ定義やバリデーション、補完支援などのツールとの連携が難しく、自動化・機械処理が困難

このリポジトリでは、USDMをYAMLとJSON Schemaで定式化することで、構造化・再利用・バリデーションのしやすい要求仕様管理を可能にします。

---

## スキーマ構成

```yaml
feature:
  feature_name: string                   　　# 機能の名称（例: "印刷機能"）
  req_groups:                            　　# 複数の要求をまとめる論理グループ（任意）
    - group_name: string                　　 # 要求グループ名（例: "操作に関する要求"）
      description: markdown             　　 # 要求グループの説明（任意）
      requirements:                      
        - req_id: string              　　   # 要求ID（例: "CH-02"）
          requirement: markdown        　　  # 要求の内容（なにをしたいか）
          reason: markdown            　　   # 要求の理由（なぜ必要か）
          description: markdown       　　   # 補足説明（背景、用語など）
          spec_groups:                　　   # 要求を満たす仕様のグループ
            - group_name: string      　　   # 仕様グループの名前（例: "チェックのタイミング"）
              description: markdown   　　   # 仕様グループの説明（任意）
              specifications:
                - spec_id: string      　　  # 仕様ID（例: "CH-02-1"）
                  specification: markdown　　# 実現手段・実装内容
                  reason: markdown       　　# なぜこの仕様かの理由（任意）
                  description: markdown  　　# 補足（任意）
```

### 主な定義要素

| 要素名 | 説明 |
|--------|------|
| `feature` | 機能単位の要求・仕様のまとまり。1つの文書単位を表す |
| `requirement` | システムに対する抽象的な期待。目的・理由・説明・関連仕様を含む |
| `requirement_group` | 複数の要求を目的別にグルーピング |
| `specification` | 要求に対する具体的な実現手段や挙動の定義 |
| `specification_group` | 関連する複数の仕様を観点別にグルーピング |

---

## 例

以下の例は、技術評論社刊『【改訂第２版】［入門＋実践］要求を仕様化する技術・表現する技術～仕様が書けていますか？』（著：清水吉男、ISBN: 978-4-7741-4257-9）で示されている要求記述をもとに構成されています。
参考：https://gihyo.jp/book/2010/978-4-7741-4257-9

```yaml
feature:
  feature_name: "印刷機能"
  requirements:
    - req_id: "CH-02"
      requirement: "印刷開始時にインク残量を確認し、完了不能なら通知する"
      reason: "印刷内容を知る利用者に早期に状況を伝えるため"
      description: "途中で止まるのではなく、事前に察知させる"
      spec_groups:
        - group_name: "チェックのタイミング"
          specifications:
            - spec_id: "CH-02-1"
              specification: "印刷開始操作完了時にインク残量をプリンターと交信して取得"
              reason: "印刷ボリュームとの比較が必要"
        - group_name: "確認操作"
          specifications:
            - spec_id: "CH-02-2"
              specification: "警告表示後にユーザーの確認操作を要求し、処理を継続"
              description: "インク交換後に確認操作をすることで再開可能とする"
```


---

## 使用方法

VS Codeなどで [YAML Language Server](https://github.com/redhat-developer/vscode-yaml) を使用し、以下のように `$schema` を設定することで補完・バリデーションが可能です。

```yaml
# yaml-language-server: $schema=.schema/schema.yaml
```

---

## 関連技術・仕様

- [JSON Schema Draft-07](https://json-schema.org/specification.html)
- [USDMとは](https://affordd.jp/wp-content/uploads/et2018/ET2018_03.pdf)
- [RedHat YAML Language Server](https://github.com/redhat-developer/vscode-yaml)

---

## 今後の予定

- 要求/仕様の記述表現の拡張（例：ASIS-TOBEの記述、事前/事後状態の記述、トレーサビリティマトリクス）
- CLIツールによるYAML→EXCEL変換（仕様書自動生成）
- 生成AIとの連携による要求・仕様のドラフト自動生成（自然言語からYAML構造への変換補助）

---

## 貢献方法

Pull Request / Issue いつでも歓迎です。ご提案・フィードバックをお待ちしております。


## ライセンス

MIT License
