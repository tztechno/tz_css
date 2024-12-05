    <style>
        .image-container {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 10px;
            padding: 10px;
        }
        .image-container img {
            width: 100%;
            height: auto;
            border: 1px solid #ccc;
            border-radius: 5px;
        }
    </style>

元画像の大きさがばらばらの場合、行列の幅はどうなる？
列幅は一定、行高さはまちまち

------------------------------------------------------------------

元画像全体を表示しつつ行列を揃えたい
列幅は一定、行高も一定（一定のaspect比）


    <style>
        .image-container {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 10px;
            padding: 10px;
        }
    .image-container img {
        aspect-ratio: 16 / 9; /* 例: 16:9 に統一 */
        width: 100%; /* 横幅をセルに合わせる */
        height: auto; /* 自動調整 */
        border: 1px solid #ccc;
        border-radius: 5px;
        }
        
    </style>
------------------------------------------------------------------

元画像の大きさがばらばらの場合、**行列内の各セルの幅は揃いますが、セルの高さは揃いません**。具体的には以下のような挙動になります：

### 1. **幅は固定される**
- `grid-template-columns: repeat(5, 1fr);` の指定により、各列の幅は均等（`1fr` で1単位分）に分割されます。
- **セル（画像が配置される枠）の幅は固定される**ため、ページ全体の横幅に応じて列の幅が調整されます。

### 2. **高さは画像の比率に依存**
- CSSの `width: 100%; height: auto;` により、画像の幅がセルの幅に収まるように拡縮されます。
- 高さは画像のアスペクト比を保ちながら調整されるため、**元画像の縦横比が異なる場合、セルごとの高さがバラバラになります**。

---

### 例
#### 元画像のサイズ
- 画像1: 800x600（横長）
- 画像2: 600x800（縦長）
- 画像3: 1024x768（横長）

#### 表示結果
| 画像 | 幅       | 高さ     |
|------|----------|----------|
| 画像1 | 100%（セル幅にフィット） | 自動調整（600 ÷ 800 の比率で計算） |
| 画像2 | 100%（セル幅にフィット） | 自動調整（800 ÷ 600 の比率で計算） |
| 画像3 | 100%（セル幅にフィット） | 自動調整（768 ÷ 1024 の比率で計算） |

結果として、行ごとにセルの高さが揃わない不規則な見た目になります。

---

### 解決策
セルの高さを揃えたい場合は、以下のような方法を考慮します：

#### 1. **画像をクロップする**
CSSの `object-fit` プロパティを使用すると、セル内で画像をクロップできます。
```css
.image-container img {
    width: 100%;
    height: 100px; /* 固定高さ */
    object-fit: cover; /* 縦横比を無視してトリミング */
    border: 1px solid #ccc;
    border-radius: 5px;
}
```
これにより、画像の表示部分をセルのサイズに合わせることができます。

---

#### 2. **セルの高さを揃える（余白を埋める）**
CSSの `align-items: stretch;` を `display: grid` に組み合わせて、セル全体に画像をストレッチ表示できます。
```css
.image-container {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    gap: 10px;
    padding: 10px;
    align-items: stretch; /* セルの高さを揃える */
}

.image-container img {
    width: 100%;
    height: 100%; /* セル全体に引き伸ばし */
    object-fit: contain; /* 縦横比を保ちながらフィット */
    border: 1px solid #ccc;
    border-radius: 5px;
}
```

---

#### 3. **アスペクト比を揃える**
CSSで全ての画像を同じアスペクト比に揃える。
```css
.image-container img {
    aspect-ratio: 4 / 3; /* 全画像を4:3の比率に統一 */
    width: 100%;
    height: auto;
    border: 1px solid #ccc;
    border-radius: 5px;
}
```

これにより、元画像のアスペクト比に関係なく、見た目が統一されます。

---

### 結論
元画像の比率が異なる場合、デフォルト設定では高さが揃わず見た目に不揃いが生じます。行列の幅と高さを揃えるには、以下の方法のいずれかを適用してください：
1. **クロップ (`object-fit: cover`)**
2. **高さをセル全体に合わせる (`height: 100%; object-fit: contain`)**
3. **アスペクト比を指定 (`aspect-ratio`)**
