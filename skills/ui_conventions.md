# UI Conventions

## テキスト配置

- ラベルは原則として中央揃えとする
- 例外がある場合は視認性を優先する

## モーダルダイアログ

- 親ウィンドウの中心座標を基準に配置する
- オフセットでずらさない
- サイズ変更時も中心基準を維持する

## 余白規約（ベース値）

- ウィンドウ外周余白は 16px をベース値とする
- セクション間は 12px をベース値とする
- コンポーネント間は 8px をベース値とする
- 上記はあくまで標準値とし、個別UIで明示的な指定がある場合はその指定を優先する
- 例外判断は視認性および操作性を基準とする

## 一貫性

- 新規UIは既存レイアウト規約に合わせる
- 個別最適化を行う場合でも全体の視覚的一貫性を損なわないこと

## 可読性固定（白飛び防止）

- 文字が背景に埋もれないよう、標準UIでは文字色を黒系で明示する
- ライトテーマを基準とし、ラベルとボタン文字は常時黒系で表示する
- 白背景と白文字の組み合わせを禁止し、背景色と文字色を同時に設計する
- 端末やプレビューなど暗背景領域は例外とし、暗背景には明色文字を明示する

## eframeの場合

- eframeでUIを実装する場合はデフォルト任せにせず、初期化時にテーマと文字色を明示設定する
- 初期化順序は同梱フォント適用の後にテーマ補正適用とする
- `ctx.set_theme(egui::Theme::Light)` を実行し、`ctx.style_mut_of(egui::Theme::Light, ...)` で文字色と背景色を同時に固定する
- ボタンやラベルへ都度色を直書きする前に、テーマ側の `override_text_color` と widget 配色を先に揃える
- 参照実装は `voicevox_daemon/src/main.rs` の `apply_custom_font` と `apply_visual_fix` 相当を基準にする

具体例:

```rust
fn apply_visual_fix(ctx: &egui::Context) {
    let base_text = egui::Color32::from_rgb(24, 24, 24);
    let strong_text = egui::Color32::from_rgb(8, 8, 8);
    let weak_text = egui::Color32::from_rgb(56, 56, 56);
    let panel_bg = egui::Color32::WHITE;
    let button_border = egui::Color32::from_rgb(51, 51, 51);

    ctx.set_theme(egui::Theme::Light);
    ctx.style_mut_of(egui::Theme::Light, |style| {
        style.visuals.override_text_color = Some(base_text);
        style.visuals.weak_text_color = Some(weak_text);
        style.visuals.widgets.noninteractive.fg_stroke.color = base_text;
        style.visuals.widgets.inactive.fg_stroke.color = base_text;
        style.visuals.widgets.hovered.fg_stroke.color = strong_text;
        style.visuals.widgets.active.fg_stroke.color = strong_text;

        style.visuals.window_fill = panel_bg;
        style.visuals.panel_fill = panel_bg;
        style.visuals.widgets.inactive.bg_stroke = egui::Stroke::new(2.0, button_border);
        style.visuals.widgets.hovered.bg_stroke = egui::Stroke::new(2.0, button_border);
        style.visuals.widgets.active.bg_stroke = egui::Stroke::new(2.0, button_border);
    });
}
```
