# PBIレビューアプリ ユースケース図・フロー図

## 1. シーケンス図（処理の流れ）

```mermaid
sequenceDiagram
    actor PO as PO / 開発者
    participant AI as AI - たたき台生成
    actor Team as チーム全員

    Note over PO,Team: Phase 1 - 入力
    PO->>AI: やりたいことを自然言語で入力

    Note over PO,Team: Phase 2 - AI生成
    AI-->>PO: PBIチケットたたき台を自動生成
    Note right of AI: 1. PBI草案<br/>2. 受入条件<br/>3. 確認事項<br/>4. DORチェックリスト

    Note over PO,Team: Phase 3 - リファインメント
    PO->>Team: たたき台を共有
    Team->>Team: 確認事項をもとに議論
    Team->>Team: 受入条件の過不足を確認・修正
    Team->>Team: DORチェックリストで全員Ready判定

    Note over PO,Team: Phase 4 - プランニング
    PO->>Team: DOR Ready PBI のみプランニングに投入
    Note right of Team: DOR未達PBIは<br/>次回リファインメントへ
```

## 2. ユースケース図

```mermaid
graph TB
    subgraph Actors
        PO["PO / 開発者"]
        DEV["開発チーム"]
        SM["スクラムマスター"]
    end

    subgraph AI["AI - たたき台生成"]
        UC1["PBI草案を自動生成する"]
        UC2["受入条件を自動提案する"]
        UC3["確認事項を洗い出す"]
        UC4["DORチェックリストを生成する"]
    end

    subgraph TeamActivity["リファインメント - チーム活動"]
        UC5["確認事項をもとに議論する"]
        UC6["受入条件を確認・修正する"]
        UC7["DORチェックリストでReady判定する"]
    end

    PO --- UC1
    UC1 -.->|自動出力| UC2
    UC1 -.->|自動出力| UC3
    UC1 -.->|自動出力| UC4

    DEV --- UC5
    DEV --- UC6
    DEV --- UC7
    PO --- UC5
    PO --- UC6
    PO --- UC7

    SM --- UC7

    style AI fill:#e8f4f8,stroke:#2196F3,stroke-width:2px
    style TeamActivity fill:#E8F5E9,stroke:#2E7D32,stroke-width:2px
    style UC1 fill:#fff,stroke:#333
    style UC2 fill:#fff,stroke:#333
    style UC3 fill:#fff,stroke:#333
    style UC4 fill:#fff,stroke:#333
    style UC5 fill:#fff,stroke:#333
    style UC6 fill:#fff,stroke:#333
    style UC7 fill:#fff,stroke:#333
```

## 3. 全体フロー図

```mermaid
flowchart LR
    A["やりたいことを\n自然言語で入力"] --> B["AIがたたき台を生成"]
    B --> B1["PBI草案"]
    B --> B2["受入条件"]
    B --> B3["確認事項"]
    B --> B4["DORチェックリスト"]

    B1 --> C["リファインメント\nチーム全員で実施"]
    B2 --> C
    B3 --> C
    B4 --> C

    C --> D["確認事項をもとに\n議論・ブラッシュアップ"]
    D --> E["DORチェックリストで\nチーム全員がReady判定"]
    E --> F{"DOR\nReady?"}
    F -->|Ready| G["プランニング投入"]
    F -->|未達| H["次回リファインメントへ"]
    H --> C

    style A fill:#E3F2FD,stroke:#1565C0
    style B fill:#E3F2FD,stroke:#1565C0
    style B1 fill:#E3F2FD,stroke:#1565C0
    style B2 fill:#E3F2FD,stroke:#1565C0
    style B3 fill:#E3F2FD,stroke:#1565C0
    style B4 fill:#E3F2FD,stroke:#1565C0
    style C fill:#E8F5E9,stroke:#2E7D32
    style D fill:#E8F5E9,stroke:#2E7D32
    style E fill:#E8F5E9,stroke:#2E7D32
    style F fill:#FFF3E0,stroke:#E65100
    style G fill:#F3E5F5,stroke:#6A1B9A
    style H fill:#FFF3E0,stroke:#E65100
```

### フロー図の色分け

| 色 | フェーズ | 内容 |
|----|---------|------|
| 青 | AI生成 | 自然言語入力 → たたき台自動生成 |
| 緑 | リファインメント | チーム全員で議論・ブラッシュアップ・DOR判定 |
| 橙 | 判定 | Ready / 未達の分岐 |
| 紫 | プランニング | Ready PBIのみ投入 |

### AIとチームの役割分担

| 役割 | AI | チーム |
|------|-----|--------|
| PBI草案作成 | たたき台を生成 | - |
| 受入条件 | テスト可能な形で提案 | 過不足を確認・修正 |
| 確認事項 | 不明点・考慮漏れを洗い出し | 議論の材料として活用 |
| DORチェックリスト | テンプレートを生成 | チーム全員でReady判定を実施 |
