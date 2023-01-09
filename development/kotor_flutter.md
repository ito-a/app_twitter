#### KotorのFlutter版

#### lintの追加
lintってデフォルトで導入されるんじゃなかったけ?と思いつつ入ってないので導入。</br>

設定方法
```
app/
  analysis_options.yaml (lintのルールを設定するファイル)
```

```
dev_dependencies:
  flutter_lints: ^2.0.1
```
→ DartAnalysisで結果を確認できるようになる。(結果: 2warning 58hints)