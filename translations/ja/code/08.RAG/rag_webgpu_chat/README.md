Phi-3-mini WebGPU RAG Chatbot

## WebGPUとRAGパターンを紹介するデモ
Phi-3 Onnx Hostedモデルを使用したRAGパターンは、情報検索を強化した生成アプローチを活用し、Phi-3モデルの力をONNXホスティングと組み合わせて効率的なAI展開を実現します。このパターンは、ドメイン固有のタスクにモデルを微調整するのに役立ち、品質、コスト効果、および長い文脈理解のバランスを提供します。これはAzure AIのスイートの一部であり、さまざまな業界のカスタマイズニーズに応えるために、見つけやすく、試しやすく、使用しやすい多くのモデルを提供しています。Phi-3モデル（Phi-3-mini、Phi-3-small、Phi-3-mediumを含む）は、Azure AIモデルカタログで利用可能であり、セルフマネージドまたはHuggingFaceやONNXなどのプラットフォームを通じて微調整および展開できます。これは、アクセス可能で効率的なAIソリューションに対するMicrosoftのコミットメントを示しています。

## WebGPUとは
WebGPUは、ウェブブラウザから直接デバイスのグラフィックス処理ユニット（GPU）に効率的にアクセスできるように設計された最新のウェブグラフィックスAPIです。これはWebGLの後継を意図しており、いくつかの主要な改善点を提供します：

1. **最新のGPUとの互換性**: WebGPUは、Vulkan、Metal、Direct3D 12などのシステムAPIを活用し、現代のGPUアーキテクチャとシームレスに動作するように構築されています。
2. **性能向上**: 一般的なGPU計算と高速な操作をサポートし、グラフィックスレンダリングと機械学習のタスクの両方に適しています。
3. **高度な機能**: WebGPUは、より高度なGPU機能にアクセスでき、複雑で動的なグラフィックスおよび計算ワークロードを可能にします。
4. **JavaScriptの負荷軽減**: さらに多くのタスクをGPUにオフロードすることで、WebGPUはJavaScriptの負荷を大幅に軽減し、より良いパフォーマンスとスムーズな体験を提供します。

現在、WebGPUはGoogle Chromeなどのブラウザでサポートされており、他のプラットフォームへのサポート拡大が進行中です。

### 03.WebGPU
必要な環境：

**サポートされているブラウザ:** 
- Google Chrome 113+
- Microsoft Edge 113+
- Safari 18 (macOS 15)
- Firefox Nightly.

### WebGPUを有効にする:

- Chrome/Microsoft Edgeで 

`chrome://flags/#enable-unsafe-webgpu` フラグを有効にします。

#### ブラウザを開く:
Google ChromeまたはMicrosoft Edgeを起動します。

#### フラグページにアクセス:
アドレスバーに `chrome://flags` と入力し、Enterキーを押します。

#### フラグを検索:
ページ上部の検索ボックスに 'enable-unsafe-webgpu' と入力します。

#### フラグを有効にする:
結果リストで #enable-unsafe-webgpu フラグを見つけます。

その隣のドロップダウンメニューをクリックして、「有効」を選択します。

#### ブラウザを再起動:
フラグを有効にした後、変更を反映させるためにブラウザを再起動する必要があります。ページ下部に表示される再起動ボタンをクリックします。

- Linuxの場合、 `--enable-features=Vulkan` でブラウザを起動します。
- Safari 18 (macOS 15) はデフォルトでWebGPUが有効になっています。
- Firefox Nightlyでは、アドレスバーに about:config と入力し、`set dom.webgpu.enabled to true` します。

### Microsoft Edge用のGPU設定

WindowsでMicrosoft Edge用の高性能GPUを設定する手順は次のとおりです：

- **設定を開く:** スタートメニューをクリックして設定を選択します。
- **システム設定:** システムに移動し、ディスプレイを選択します。
- **グラフィックス設定:** 下にスクロールしてグラフィックス設定をクリックします。
- **アプリを選択:** 「優先設定を設定するアプリの選択」の下でデスクトップアプリを選択し、参照をクリックします。
- **Edgeを選択:** Edgeのインストールフォルダ（通常は `C:\Program Files (x86)\Microsoft\Edge\Application`）に移動し、`msedge.exe` を選択します。
- **優先設定を設定:** オプションをクリックし、高性能を選択して保存をクリックします。
これにより、Microsoft Edgeが高性能GPUを使用してパフォーマンスが向上します。
- **マシンを再起動** してこれらの設定を反映させます。

### コードスペースを開く:
GitHubのリポジトリに移動します。
コードボタンをクリックして、コードスペースで開くを選択します。

まだコードスペースを持っていない場合は、新しいコードスペースをクリックして作成できます。

**Note** コードスペースにNode環境をインストールする
GitHubコードスペースからnpmデモを実行するのは、プロジェクトをテストおよび開発するための優れた方法です。ここでは、始めるためのステップバイステップガイドを示します：

### 環境を設定する:
コードスペースが開いたら、Node.jsとnpmがインストールされていることを確認します。次のコマンドを実行して確認できます：
```
node -v
```
```
npm -v
```

インストールされていない場合は、次のコマンドを使用してインストールできます：
```
sudo apt-get update
```
```
sudo apt-get install nodejs npm
```

### プロジェクトディレクトリに移動する:
ターミナルを使用してnpmプロジェクトがあるディレクトリに移動します：
```
cd path/to/your/project
```

### 依存関係をインストールする:
次のコマンドを実行して、package.jsonファイルに記載されているすべての必要な依存関係をインストールします：

```
npm install
```

### デモを実行する:
依存関係がインストールされたら、デモスクリプトを実行できます。これは通常、package.jsonのscriptsセクションに指定されています。たとえば、デモスクリプトがstartという名前の場合、次のコマンドを実行できます：

```
npm run build
```
```
npm run dev
```

### デモにアクセスする:
デモにウェブサーバーが関与している場合、CodespacesはアクセスするためのURLを提供します。通知を確認するか、ポートタブを確認してURLを見つけてください。

**Note:** モデルはブラウザにキャッシュされる必要があるため、読み込みに時間がかかる場合があります。

### RAGデモ
マークダウンファイル `intro_rag.md` to complete the RAG solution. If using codespaces you can download the file located in `01.InferencePhi3/docs/` をアップロードします。

### ファイルを選択する:
「ファイルを選択」ボタンをクリックして、アップロードしたいドキュメントを選択します。

### ドキュメントをアップロードする:
ファイルを選択した後、「アップロード」ボタンをクリックして、RAG（情報検索を強化した生成）のためにドキュメントをロードします。

### チャットを開始する:
ドキュメントがアップロードされると、その内容に基づいてRAGを使用したチャットセッションを開始できます。

**免責事項**:
この文書は機械ベースのAI翻訳サービスを使用して翻訳されています。正確さを期すために努めていますが、自動翻訳にはエラーや不正確な点が含まれる可能性があります。元の言語で書かれた文書を権威ある情報源と見なすべきです。重要な情報については、専門の人間による翻訳をお勧めします。この翻訳の使用に起因する誤解や誤訳について、当社は一切の責任を負いません。