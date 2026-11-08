# 第3回 解答

## 演習

- 第1回（素のLLM）とエージェントの回答の違い：
一般的な「3レベル」の説明ではなく、AIE2026の講座の内容に沿った「3レベル」の説明をした。
- `--trace` で見えたツール呼び出し（ツール名と引数）：
--- trace ---
[tool call]   search_glossary({'query': 'LoRA'})
[tool call]   search_glossary({'query': 'QLoRA'})
- 1つの質問での LLM 呼び出し回数（`requests`）：
requests=2
- 追加したツールと、docstring に書いた説明：
追加したツール：make_quiz
docstring に書いた説明：指定した回の用語から 1つを選び、確認問題と答えを返す。「第2回の確認問題を出して」のように、復習用の問題を求められたときに使う。
## 宿題

- 取り組んだレベル：基礎
- やったこと：
1.docstringの変更
「第2回の確認問題を出して」のように、復習用の問題を求められたときに使う。の記載を削除。
2.INSTRUCTIONSの変更
回答の最後に、関係する回（第N回）を示す。を削除
- 結果：
・変更前
第2回の内容から確認問題をお出しします！

**【確認問題】**
「量子化」の意味は？

（答えのヒント：モデルの重みとビット数、メモリや計算量に関係する技術です。考えてみてくださいね！）

関係する回：第2回

--- trace ---
[tool call]   make_quiz({'lecture': 2})
[tool return] make_quiz -> {"question": "「量子化」の意味は？", "answer": "モデルの重みを少ないビット数（例：16ビットから4ビット）で表し、メモリ使用量や計算量を減らす技術。精度は少し下がることがある。"}…
[usage] RunUsage(cost=Decimal('0.0004578'), details={'text_prompt_tokens': 876}, input_text_tokens=876, input_tokens=876, output_tokens=78, requests=2, tool_calls=1)

・変更1(docstringの変更)
第2回（講座の用語集より）から確認問題を1問出します！

**【確認問題】**
「ランク（LoRAの $r$）」の意味は何でしょうか？

---
第2回

--- trace ---
[tool call]   make_quiz({'lecture': 2})
[tool return] make_quiz -> {"question": "「ランク（LoRAのr）」の意味は？", "answer": "LoRAで使う低ランク行列の大きさを決めるハイパーパラメータ。大きいほど表現力が上がるが、学習するパラメータとメモリも増える。"}…
[usage] RunUsage(cost=Decimal('0.0003996'), details={'text_prompt_tokens': 832}, input_text_tokens=832, input_tokens=832, output_tokens=60, requests=2, tool_calls=1)

・変更2(INSTRUCTIONSの変更)
第2回から確認問題を1問出しますね！

**【第2回 確認問題】**
Q. 「教師ありファインチューニング（SFT）」とは、どのような学習方法でしょうか？

考えてみてくださいね。答え合わせをしたいときはいつでも声をかけてください！

--- trace ---
[tool call]   make_quiz({'lecture': 2})
[tool return] make_quiz -> {"question": "「教師ありファインチューニング（SFT）」の意味は？", "answer": "入力と望ましい出力のペア（例：質問と模範回答）を使ってモデルを追加学習させる方法。指示に従う振る舞いを身につけさせる代表的な手法。"…
[usage] RunUsage(cost=Decimal('0.0004372'), details={'text_prompt_tokens': 849}, input_text_tokens=849, input_tokens=849, output_tokens=73, requests=2, tool_calls=1)

- 気づき・疑問：
docstringの「「第2回の確認問題を出して」のように、復習用の問題を求められたときに使う」を削除しても回答は変わらないのではないかと思ったが、【確認問題】の後の一言がなくなった。
これがどのように関連しているのか、それとも関連していないのか（答えの揺らぎなのか？）がよく解らなかった。
INSTRUCTIONS「回答の最後に、関係する回（第N回）を示す。」を削除すると、回答に関係する回がなくなったのは予想通りの変化だった。
INSTRUCTIONSに記載された内容を参照して回答を作成していること、記載内容が回答作成に与える影響を確認することができた。