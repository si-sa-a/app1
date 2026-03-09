# PBIレビューアプリ ユースケース図・フロー図

## 1. シーケンス図（処理の流れ）

```mermaid
sequenceDiagram
    actor PO as PO / 開発者
    participant AI as AI - レビューアプリ
    actor Team as チーム

    Note over PO,Team: Phase 1 - PBI作成
    PO->>AI: やりたいことを自然言語で入力
    AI-->>PO: PBI たたき台を自動生成
    Note right of AI: ユーザーストーリー<br/>受け入れ基準<br/>影響範囲

    Note over PO,Team: Phase 2 - DORチェック
    AI-->>PO: DOR自動チェック結果
    Note right of AI: 達成項目 / 不足項目<br/>+ 追記ガイド
    PO->>AI: 不足項目を追記・修正
    AI-->>PO: DORチェック再実行

    Note over PO,Team: Phase 3 - 非同期レビュー
    PO->>Team: PBIを共有 - レビュー依頼
    Team->>PO: コメント・確認事項のフィードバック
    PO->>AI: フィードバックを反映して更新
    AI-->>PO: 更新後のDORチェック結果

    Note over PO,Team: Phase 4 - リファインメント
    PO->>Team: リファインメント実施 - Ready PBIのみ
    Team-->>PO: ブラッシュアップ・合意形成
    Team->>Team: DOR Ready 最終判定

    Note over PO,Team: Phase 5 - プランニング
    PO->>Team: Ready PBI をプランニングに投入
```

## 2. ユースケース図

```mermaid
graph TB
    subgraph Actors
        PO["PO / 開発者"]
        DEV["開発チーム"]
        SM["スクラムマスター"]
    end

    subgraph System["AI PBIレビューアプリ"]
        UC1["PBIを自然言語から作成する"]
        UC2["DOR達成状況を自動チェックする"]
        UC3["不足項目の追記ガイドを表示する"]
        UC4["PBIにコメント・フィードバックする"]
        UC5["DOR達成状況を一覧で確認する"]
        UC6["PBIを修正・更新する"]
    end

    PO --- UC1
    PO --- UC2
    PO --- UC6

    UC2 -.->|include| UC3

    DEV --- UC4
    DEV --- UC2

    SM --- UC5

    style System fill:#e8f4f8,stroke:#2196F3,stroke-width:2px
    style UC1 fill:#fff,stroke:#333
    style UC2 fill:#fff,stroke:#333
    style UC3 fill:#fff,stroke:#333
    style UC4 fill:#fff,stroke:#333
    style UC5 fill:#fff,stroke:#333
    style UC6 fill:#fff,stroke:#333
```

## 3. 全体フロー図

```mermaid
flowchart LR
    A["やりたいことを\n自然言語で入力"] --> B["AIがPBI\nたたき台を生成"]
    B --> C["DOR\n自動チェック"]
    C --> D{"DOR\n達成?"}
    D -->|未達| E["不足項目を\n追記・修正"]
    E --> C
    D -->|達成| F["チームに\nレビュー依頼"]
    F --> G{"未解決の\n質問あり?"}
    G -->|あり| H["フィードバック\n反映・修正"]
    H --> C
    G -->|なし| I["リファインメント"]
    I --> J["DOR Ready\n最終判定"]
    J --> K["プランニング\n投入"]

    style A fill:#E3F2FD,stroke:#1565C0
    style B fill:#E3F2FD,stroke:#1565C0
    style C fill:#FFF3E0,stroke:#E65100
    style D fill:#FFF3E0,stroke:#E65100
    style E fill:#FFF3E0,stroke:#E65100
    style F fill:#E8F5E9,stroke:#2E7D32
    style G fill:#E8F5E9,stroke:#2E7D32
    style H fill:#E8F5E9,stroke:#2E7D32
    style I fill:#F3E5F5,stroke:#6A1B9A
    style J fill:#F3E5F5,stroke:#6A1B9A
    style K fill:#F3E5F5,stroke:#6A1B9A
```

### フロー図の色分け

| 色 | フェーズ | 内容 |
|----|---------|------|
| 青 | PBI作成 | 自然言語入力 → AI自動生成 |
| 橙 | DORチェック | 自動判定 → 不足項目の修正ループ |
| 緑 | チームレビュー | 非同期フィードバック → 反映 |
| 紫 | リファインメント・プランニング | Ready判定 → スプリント投入 |
