# gemma-3-4b-vlm-in-the-world

概要
このリポジトリでは、SakanaAI/JA-VG-VQA-500 データセットと Google の Gemma3 モデル (google/gemma-3-4b-it) を利用し、
日本語ビジョンランゲージ（VQA: Visual Question Answering）推論を行うためのサンプルコードを提供しています。

ファイル構成
main.py
JA-VG-VQA-500 データセットから画像データと対応する複数の Q&A を取得し、Gemma3 へ入力 → モデル出力を JSONL 形式で保存するサンプルスクリプト。

環境構築手順
Python のインストール
3.8 以上を推奨します（3.7 以下の場合、一部のライブラリで問題が起きる可能性があります）。

必要パッケージのインストール
以下のように pip を利用してインストールします:

bash
コピーする
pip install --upgrade pip
pip install torch transformers datasets Pillow
transformers : Gemma3 モデルの読み込みに利用

datasets : JA-VG-VQA-500 の読み込みに利用

Pillow : 画像の保存や操作に利用

HF Token の用意（必要に応じて）
Gemma3 モデルを読み込む際、Hugging Face Hub の認証が必要な場合があります。
事前に Hugging Face 公式 でアクセストークンを取得し、
端末上で huggingface-cli login を行う、またはスクリプト内に use_auth_token=True として埋め込んでください。

使い方
データセットとモデルを読み込み、推論を実行する
以下のコマンドでスクリプトを実行します:

bash
コピーする
python main.py
スクリプトでは、JA-VG-VQA-500 の test スプリットから順に (画像・質問) を読み込みます。

Pillow (PIL) の EXIF 処理によるエラーを回避するために decode=False を指定し、画像を raw bytes として読み込んでいます。

読み込んだバイナリ画像を一時ファイルに保存 → Gemma3 にファイルパスを渡す形で推論を行います。

出力ファイル (gemma3_output.jsonl)
推論結果は gemma3_output.jsonl に書き出されます。

image_id : 入力画像の ID

qa_id : 質問ごとのユニーク ID

url : 画像の外部 URL

question : 質問文

pred_answer : Gemma3 の推論回答

gt_answer : データセットのアノテーションとしての正解（比較用）

出力例（1 QA あたり1行の JSON 形式）:

json
コピーする
{
  "image_id": 100,
  "qa_id": 10674688,
  "url": "https://example.com/image.jpg",
  "question": "どんな天候ですか？",
  "pred_answer": "晴れの日",
  "gt_answer": "晴天"
}
トラブルシューティング
PIL.ExifTags に関するエラー
デフォルト設定では、datasets が自動で画像を PIL Image に変換し、EXIF 情報にアクセスしようとしてエラーになるケースがあります。
そのため、本スクリプトでは decode=False を使い、EXIF 処理を回避しています。

Hugging Face Token が無い/認証エラー
必要に応じて use_auth_token=False に切り替えるか、huggingface-cli login でトークンを設定してください。

Out of Memory エラー

Gemma3 4B モデルはある程度の GPU メモリを要します。

もしメモリ不足の場合は device_map="auto" を利用して CPU と GPU を自動的に切り分けるか、より大きな GPU インスタンスを利用してください。

一時ファイルの削除
スクリプトでは推論後に temp_***.jpg を削除していますが、環境によってはファイルが残る場合があります。
必要に応じて手動でクリーンアップしてください。

ライセンス
JA-VG-VQA-500 データセットは SakanaAI/JA-VG-VQA-500 のライセンスに従います。

Gemma3 モデルは Google 提供のライセンスに従ってご利用ください。

謝辞
Hugging Face やコミュニティによる変換・ホスティングの貢献

SakanaAI/JA-VG-VQA-500 データセット

その他多くのオープンソースプロジェクト
