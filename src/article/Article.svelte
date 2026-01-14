<script>
	import HyperparameterView from '../detail-view/Hyperparameterview.svelte';
  import Youtube from './Youtube.svelte';

	let softmaxEquation = `$$\\text{Softmax}(x_{i}) = \\frac{\\exp(x_i)}{\\sum_j \\exp(x_j)}$$`;
	let reluEquation = `$$\\text{ReLU}(x) = \\max(0,x)$$`;

  let currentPlayer;
</script>

<style>
	#description {
    margin-bottom: 60px;
    margin-left: auto;
    margin-right: auto;
    max-width: 78ch;
  }

  #description h2 {
    color: #444;
    font-size: 40px;
    font-weight: 450;
    margin-bottom: 12px;
    margin-top: 60px;
  }

  #description h4 {
    color: #444;
    font-size: 32px;
    font-weight: 450;
    margin-bottom: 8px;
    margin-top: 44px;
  }

  #description h6 {
    color: #444;
    font-size: 24px;
    font-weight: 450;
    margin-bottom: 8px;
    margin-top: 44px;
  }

  #description p {
    margin: 16px 0;
  }

  #description p img {
    vertical-align: middle;
  }

  #description .figure-caption {
    font-size: 13px;
    margin-top: 5px;
  }

  #description ol {
    margin-left: 40px;
  }

  #description p, 
  #description div,
  #description li {
    color: #555;
    font-size: 17px;
    line-height: 1.6;
  }

  #description small {
    font-size: 12px;
  }

  #description ol li img {
    vertical-align: middle;
  }

  #description .video-link {
    color: #3273DC;
    cursor: pointer;
    font-weight: normal;
    text-decoration: none;
  }

  #description ul {
      list-style-type: disc;
      margin-top: -10px;
      margin-left: 40px;
      margin-bottom: 15px;
  }
    
  #description a:hover, 
  #description .video-link:hover {
    text-decoration: underline;
  }

  .figure, .video {
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
  }
</style>

<body>
  <div id="description">
    <h2>畳み込みニューラルネットワーク (CNN) とは？</h2>
    <p>
		機械学習において、分類器 (Classifier) はデータ点にクラスラベルを割り当てます。例えば、<em>画像分類器</em>は画像内に存在するオブジェクト（鳥、飛行機など）に対してクラスラベルを生成します。<em>畳み込みニューラルネットワーク</em>（Convolutional Neural Network、略してCNN）は、この問題を解決するのに優れた分類器の一種です！
	 </p>
  	<p>
  		CNNはニューラルネットワークの一種で、データ内のパターンを認識するためのアルゴリズムです。一般的にニューラルネットワークは、層 (Layer) に編成されたニューロン (Neuron) の集合で構成され、各ニューロンは学習可能な重み (Weights) とバイアス (Biases) を持っています。CNNを基本的な構成要素に分解して見てみましょう。
  	</p>
  	<ol>
  		<li><strong>テンソル (Tensor)</strong> は、n次元の行列と考えることができます。上記のCNNでは、出力層を除いてテンソルは3次元になります。</li>
  		<li><strong>ニューロン (Neuron)</strong> は、複数の入力を受け取り、単一の出力を生成する関数と考えることができます。ニューロンの出力は上記で<span style="color:#FF7577;">赤</span> &rarr; <span style="color:#60A7D7;">青</span>の<strong>活性化マップ (Activation Maps)</strong>として表現されています。</li>
  		<li><strong>層 (Layer)</strong> は、同じ演算と同じハイパーパラメータを持つニューロンの集合です。</li>
  		<li><strong>カーネル重み (Kernel Weights) とバイアス (Biases)</strong> は、各ニューロンに固有のものですが、学習フェーズ中に調整され、分類器が与えられた問題とデータセットに適応できるようにします。可視化では、<span style="color:#BC8435;">黄色</span> &rarr; <span style="color:#39988F;">緑</span>の発散カラースケールで表現されています。具体的な値は、ニューロンをクリックするか、<em>畳み込みエラスティック説明ビュー</em>でカーネル/バイアスにマウスを重ねることで、<em>インタラクティブ数式ビュー</em>で確認できます。</li>
  		<li>CNNは<strong>微分可能なスコア関数</strong>を伝達し、出力層の可視化では<strong>クラススコア</strong>として表現されます。</li>
  	</ol>
  	<p>
  		以前にニューラルネットワークを学習したことがある方には、これらの用語は馴染みがあるかもしれません。では、CNNの何が特別なのでしょうか？CNNは「畳み込み層 (Convolutional Layer)」と呼ばれる特殊な層を使用しており、画像や画像のようなデータからの学習に適しています。画像データに関しては、CNNは<a href="http://ijcsit.com/docs/Volume%207/vol7issue5/ijcsit20160705014.pdf" title="CNN Applications">画像処理、分類、セグメンテーション、物体検出</a>など、多くのコンピュータビジョンタスクに使用できます。
  	</p>
  	<p>
  		CNN Explainerでは、シンプルなCNNが画像分類にどのように使用されるかを見ることができます。ネットワークのシンプルさゆえに、その性能は完璧ではありませんが、それで問題ありません！CNN Explainerで使用されているネットワークアーキテクチャ<a href="http://cs231n.stanford.edu/" title="Tiny VGG Net presented by Stanford's CS231n">Tiny VGG</a>は、現在の最先端CNNで使用されている層や演算の多くを含んでいますが、より小規模なものです。このため、入門として理解しやすくなっています。
      </p>     

      <h2>各層の役割は？</h2>
      <p>
  		ネットワークの各層を見ていきましょう。読みながら、上の可視化をクリックしたりマウスを重ねたりして自由に操作してみてください。
      </p>
      <h4 id='article-input'>入力層 (Input Layer)</h4>
      <p>
      	入力層（最も左の層）は、CNNへの入力画像を表しています。RGB画像を入力として使用しているため、入力層にはそれぞれ赤、緑、青のチャンネルに対応する3つのチャンネルがあり、この層に表示されています。上の<img class="is-rounded" width="12%" height="12%" src="PUBLIC_URL/assets/figures/network_details.png" alt="network details icon"/>アイコンをクリックしてカラースケールを使用すると、この層や他の層の詳細情報を表示できます。
      </p>
      <h4 id='article-convolution'>畳み込み層 (Convolutional Layers)</h4>
      <p>
  		畳み込み層はCNNの基盤です。学習されたカーネル（重み）を含み、異なる画像を区別する特徴を抽出します。これがまさに分類に必要なものです！畳み込み層を操作すると、前の層と畳み込み層の間のリンクに気づくでしょう。各リンクは固有のカーネルを表し、畳み込み演算で現在の畳み込みニューロンの出力または活性化マップを生成するために使用されます。
  	</p>
  	<p>
  		畳み込みニューロンは、固有のカーネルと前の層の対応するニューロンの出力との要素ごとの内積を実行します。これにより、固有のカーネルの数だけ中間結果が生成されます。畳み込みニューロンは、すべての中間結果を学習されたバイアスと合計した結果です。
  	</p>
  	<p>
  		例えば、上のTiny VGGアーキテクチャの最初の畳み込み層を見てみましょう。この層には10個のニューロンがありますが、前の層には3個のニューロンしかありません。Tiny VGGアーキテクチャでは、畳み込み層は全結合されています。つまり、各ニューロンは前の層のすべてのニューロンに接続されています。最初の畳み込み層の最上部の畳み込みニューロンの出力に注目すると、活性化マップにマウスを重ねたときに3つの固有のカーネルがあることがわかります。
  	</p>
    <div class="figure">
      <img src="PUBLIC_URL/assets/figures/convlayer_overview_demo.gif" alt="clicking on topmost first conv. layer activation map" width=60% height=60% align="middle"/>
      <div class="figure-caption">
  		  図1. 最初の畳み込み層の最上部ノードの活性化マップにマウスを重ねると、この活性化マップを生成するために3つのカーネルが適用されていることがわかります。この活性化マップをクリックすると、各固有のカーネルで畳み込み演算が行われている様子を確認できます。
  	  </div>
    </div>

  	<p>
  		これらのカーネルのサイズは、ネットワークアーキテクチャの設計者が指定するハイパーパラメータです。畳み込みニューロンの出力（活性化マップ）を生成するには、前の層の出力とネットワークが学習した固有のカーネルとの要素ごとの内積を実行する必要があります。TinyVGGでは、内積演算はストライド1を使用します。これは、カーネルが内積ごとに1ピクセルずつシフトされることを意味しますが、これはネットワークアーキテクチャ設計者がデータセットに合わせて調整できるハイパーパラメータです。3つすべてのカーネルに対してこれを行う必要があり、3つの中間結果が生成されます。
  	</p>
    <div class="figure">
      <img src="PUBLIC_URL/assets/figures/convlayer_detailedview_demo.gif" alt="clicking on topmost first conv. layer activation map" />
      <div class="figure-caption">
        図2. 説明した活性化マップの最上部の中間結果を生成するために適用されているカーネル。
      </div>
    </div>
  	<p>
  		次に、3つすべての中間結果とネットワークが学習したバイアスを含む要素ごとの和が実行されます。その後、結果として得られる2次元テンソルが、最初の畳み込み層の最上部ニューロンの活性化マップとして上のインターフェースで表示されます。各ニューロンの活性化マップを生成するために、同じ演算を適用する必要があります。
  	</p>
  	<p>
  		簡単な計算で、最初の畳み込み層には3 x 10 = 30個の固有のカーネル（各サイズ3x3）が適用されていることがわかります。畳み込み層と前の層の間の接続は、ネットワークアーキテクチャを構築する際の設計上の決定であり、畳み込み層ごとのカーネル数に影響します。可視化をクリックして、畳み込み層の背後にある演算をより深く理解してください。上の例に従ってみてください！
    </p>
    <h6>ハイパーパラメータの理解</h6>
    <p>
    	<HyperparameterView/>
    </p>
    <ol>
    	<li><strong>パディング (Padding)</strong> は、カーネルが活性化マップを超えて拡張する場合に必要になることがよくあります。パディングは活性化マップの境界でデータを保持し、より良い性能につながります。また、<a href="https://arxiv.org/pdf/1603.07285.pdf" title="See page 13">入力の空間サイズを保持</a>するのに役立ち、アーキテクチャ設計者がより深く、より高性能なネットワークを構築できるようにします。<a href="https://arxiv.org/pdf/1811.11718.pdf" title="Outlines major padding techniques">多くのパディング技術</a>がありますが、最も一般的に使用されるアプローチはゼロパディングです。これは、その性能、シンプルさ、計算効率のためです。この技術は、入力の端に対称的にゼロを追加することを含みます。このアプローチは、<a href="https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf" title="AlexNet">AlexNet</a>などの多くの高性能CNNで採用されています。</li>
    	<li><strong>カーネルサイズ (Kernel Size)</strong>は、フィルターサイズとも呼ばれ、入力上をスライドするウィンドウの次元を指します。このハイパーパラメータの選択は、画像分類タスクに大きな影響を与えます。例えば、小さなカーネルサイズは、入力から非常にローカルな特徴を含む大量の情報を抽出できます。上の可視化でわかるように、小さなカーネルサイズは層の次元の減少も小さくなり、より深いアーキテクチャが可能になります。逆に、大きなカーネルサイズは情報の抽出が少なくなり、層の次元がより速く減少し、多くの場合、性能が低下します。大きなカーネルは、より大きな特徴を抽出するのに適しています。結局のところ、適切なカーネルサイズの選択はタスクとデータセットに依存しますが、一般的に、小さなカーネルサイズは画像分類タスクでより良い性能につながります。なぜなら、アーキテクチャ設計者は<a href="https://arxiv.org/pdf/1409.1556.pdf" title="Learn why deeper networks perform better!">より多くの層を積み重ねて、より複雑な特徴を学習</a>できるからです！</li>
    	<li><strong>ストライド (Stride)</strong> は、カーネルが一度にどれだけのピクセルをシフトするかを示します。例えば、上記の畳み込み層の例で説明したように、Tiny VGGは畳み込み層でストライド1を使用しています。これは、入力の3x3ウィンドウで内積が実行されて出力値が生成され、その後の各演算で1ピクセルずつ右にシフトされることを意味します。ストライドがCNNに与える影響はカーネルサイズに似ています。ストライドが減少すると、より多くのデータが抽出されるため、より多くの特徴が学習され、出力層も大きくなります。逆に、ストライドが増加すると、特徴抽出がより限定的になり、出力層の次元が小さくなります。アーキテクチャ設計者の責任の1つは、CNNを実装する際にカーネルが入力上を対称的にスライドすることを確認することです。上のハイパーパラメータの可視化を使用して、さまざまな入力/カーネル次元でストライドを変更し、この制約を理解してください！</li>
    </ol>
    <h4>活性化関数 (Activation Functions)</h4>
    <h6 id='article-relu'>ReLU</h6>
    <p>
    	ニューラルネットワークは現代のテクノロジーで非常に普及しています。なぜなら非常に正確だからです！今日の最高性能のCNNは、膨大な数の層で構成されており、より多くの特徴を学習できます。これらの画期的なCNNが<a href="https://arxiv.org/pdf/1512.03385.pdf" title="ResNet">驚異的な精度</a>を達成できる理由の一部は、その非線形性にあります。ReLUはモデルに不可欠な非線形性を適用します。非線形性は、非線形の決定境界を生成するために必要であり、出力が入力の線形結合として書けないようにします。非線形活性化関数がなければ、深いCNNアーキテクチャは単一の等価な畳み込み層に退化し、性能が大幅に低下します。ReLU活性化関数は、<em>Sigmoid</em>などの他の非線形関数ではなく、非線形活性化関数として特に使用されます。これは、ReLUを使用するCNNが<a href="https://arxiv.org/pdf/1906.01975.pdf" title="See page 29">経験的に</a>他の関数を使用するものよりも高速に学習できることが観察されているためです。
    </p>
    <p>
  	ReLU活性化関数は要素ごとの数学的演算です: {reluEquation}
    </p>
    <div class="figure">
    <img src="PUBLIC_URL/assets/figures/relu_graph.png" alt="relu graph" width="30%" height="30%"/>
      <div class="figure-caption">
        図3. ReLU活性化関数のグラフ。すべての負のデータを無視します。
      </div>
    </div>
    <p>
  	この活性化関数は、入力テンソルのすべての値に要素ごとに適用されます。例えば、値2.24にReLUを適用すると、2.24は0より大きいため、結果は2.24になります。上のネットワークでReLUニューロンをクリックすると、この活性化関数がどのように適用されるかを確認できます。ReLU（Rectified Linear Activation function）は、上記のネットワークアーキテクチャのすべての畳み込み層の後に実行されます。ネットワーク全体のさまざまなニューロンの活性化マップに対してこの層が与える影響に注目してください！
    </p>
    <h6 id='article-softmax'>Softmax</h6>
    <p>
    	{softmaxEquation}
    	Softmax演算は重要な目的を果たします：CNNの出力の合計が1になることを保証します。このため、Softmax演算はモデルの出力を確率にスケーリングするのに役立ちます。最後の層をクリックすると、ネットワークのSoftmax演算が表示されます。平坦化後のロジットが0から1の間にスケーリングされていないことに注目してください。各ロジット（スケーリングされていないスカラー値）の影響を視覚的に示すために、<span style="color:#FFC385;">薄いオレンジ</span> &rarr; <span style="color:#C44103;">濃いオレンジ</span>のカラースケールでエンコードされています。Softmax関数を通過した後、各クラスは適切な確率に対応するようになります！
    </p>
    <p>
    	標準的な正規化とSoftmaxの違いは何かと思うかもしれません。結局、どちらもロジットを0と1の間にリスケールします。バックプロパゲーションはニューラルネットワークの学習の重要な側面であることを忘れないでください。私たちは正解が最大の「シグナル」を持つことを望んでいます。Softmaxを使用することで、微分可能性を獲得しながら、効果的にargmaxを「近似」しています。リスケーリングは最大値を他のロジットよりも著しく高く重み付けしませんが、Softmaxはそうします。簡単に言えば、Softmaxは「より柔らかい」argmaxです。おわかりですか？
    </p>
    <div class="figure">
    <img src="PUBLIC_URL/assets/figures/softmax_animation.gif" alt="softmax interactive formula view"/>
      <div class="figure-caption">
        図4. <em>Softmaxインタラクティブ数式ビュー</em>では、ユーザーがカラーエンコードされたロジットと数式の両方を操作して、平坦化層後の予測スコアがどのように正規化されて分類スコアになるかを理解できます。
      </div>
    </div>
    <h4 id='article-pooling'>プーリング層 (Pooling Layers)</h4>
    <p>
    	異なるCNNアーキテクチャには多くのタイプのプーリング層がありますが、すべてネットワークの空間的な範囲を徐々に減少させる目的があり、これによりパラメータとネットワーク全体の計算量が削減されます。上のTiny VGGアーキテクチャで使用されているプーリングのタイプはマックスプーリング (Max-Pooling) です。
    </p>
    <p>
    	マックスプーリング演算は、アーキテクチャ設計時にカーネルサイズとストライド長を選択する必要があります。選択されると、この演算は指定されたストライドでカーネルを入力上にスライドさせ、各カーネルスライスから入力の最大値のみを選択して出力値を生成します。このプロセスは、上のネットワークでプーリングニューロンをクリックすると確認できます。
    </p>
    <p>
    	上のTiny VGGアーキテクチャでは、プーリング層は2x2カーネルとストライド2を使用しています。これらの仕様でのこの演算は、活性化の75%を破棄することになります。多くの値を破棄することで、Tiny VGGは計算効率が高くなり、過学習を回避します。
    </p>
    <h4 id='article-flatten'>平坦化層 (Flatten Layer)</h4>
    <p>
      この層は、ネットワーク内の3次元層を1次元ベクトルに変換し、分類のための全結合層の入力に適合させます。例えば、5x5x2のテンソルはサイズ50のベクトルに変換されます。ネットワークの前の畳み込み層は入力画像から特徴を抽出しましたが、今度は特徴を分類する時です。これらの特徴を分類するためにSoftmax関数を使用しますが、これには1次元の入力が必要です。これが平坦化層が必要な理由です。この層は任意の出力クラスをクリックすると確認できます。
    </p>

    <h2>インタラクティブ機能</h2>
    <ol>
    	<li><img class="icon is-rounded" src="PUBLIC_URL/assets/figures/upload_image_icon.png" alt="upload image icon"/>を選択して<strong>自分の画像をアップロード</strong>し、画像が10クラスにどのように分類されるかを理解できます。ネットワーク全体のニューロンを分析することで、活性化マップと抽出された特徴を理解できます。</li>
    	<li><img class="is-rounded" width="12%" height="12%" src="PUBLIC_URL/assets/figures/heatmap_scale.png" alt="heatmap"/>を調整して<strong>活性化マップのカラースケールを変更</strong>し、異なる抽象レベルでの活性化の影響をより深く理解できます。</li>
    	<li><img class="is-rounded" width="12%" height="12%" src="PUBLIC_URL/assets/figures/network_details.png" alt="network details icon"/>アイコンをクリックして、層の次元やカラースケールなどの<strong>ネットワークの詳細を理解</strong>できます。</li>
    	<li><img class="icon is-rounded" src="PUBLIC_URL/assets/figures/play_button.png" alt="play icon"/>ボタンをクリックして<strong>ネットワーク演算をシミュレート</strong>するか、<em>インタラクティブ数式ビュー</em>で入力または出力の部分にマウスを重ねて層スライスを操作し、マッピングと基本的な演算を理解できます。</li>
      <li><em>インタラクティブ数式ビュー</em>から<img class="icon is-rounded" src="PUBLIC_URL/assets/figures/info_button.png" alt="info icon"/>をクリックして<strong>層の機能を学習</strong>し、記事から層の詳細を読むことができます。</li>
    </ol> 

    <h2>ビデオチュートリアル</h2>
    <ul>
      <li class="video-link" on:click={currentPlayer.play(0)}>
        CNN Explainerの紹介
        <small>(0:00-0:22)</small>
      </li>
      <li class="video-link" on:click={currentPlayer.play(27)}>
        <em>概要</em>
        <small>(0:27-0:37)</small>
      </li>
      <li class="video-link" on:click={currentPlayer.play(37)}>
        畳み込み<em>エラスティック説明ビュー</em>
        <small>(0:37-0:46)</small>
      </li>
      <li class="video-link" on:click={currentPlayer.play(46)}>
        畳み込み、ReLU、プーリング<em>インタラクティブ数式ビュー</em>
        <small>(0:46-1:21)</small>
      </li>
      <li class="video-link" on:click={currentPlayer.play(82)}>
        平坦化<em>エラスティック説明ビュー</em>
        <small>(1:22-1:41)</small>
      </li>
      <li class="video-link" on:click={currentPlayer.play(101)}>
        Softmax <em>インタラクティブ数式ビュー</em>
        <small>(1:41-2:02)</small>
      </li>
      <li class="video-link" on:click={currentPlayer.play(126)}>
        魅力的な学習体験：分類の理解
        <small>(2:06-2:28)</small>
      </li>
      <li class="video-link" on:click={currentPlayer.play(149)}>
        インタラクティブチュートリアル記事
        <small>(2:29-2:54)</small>
      </li>
    </ul>
    <div class="video">
      <Youtube videoId="HnWIHWFbuUQ" playerId="demo_video" bind:this={currentPlayer}/>
    </div>

    <h2>CNN Explainerはどのように実装されていますか？</h2>
    <p>
      CNN Explainerは、<a href="https://js.tensorflow.org/"><em>TensorFlow.js</em></a>（ブラウザ内GPUアクセラレーション深層学習ライブラリ）を使用して、可視化のための学習済みモデルを読み込んでいます。インタラクティブシステム全体は、フレームワークとして<a href="https://svelte.dev/"><em>Svelte</em></a>、可視化には<a href="https://d3js.org/"><em>D3.js</em></a>を使用してJavascriptで記述されています。今日からCNNを学ぶのに必要なのはWebブラウザだけです！
    </p>

    <h2>CNN Explainerを開発したのは誰ですか？</h2>
    <p>
      CNN Explainerは
      <a href="https://zijie.wang/">Jay Wang</a>、
      <a href="https://www.linkedin.com/in/robert-turko/">Robert Turko</a>、
      <a href="http://oshaikh.com/">Omar Shaikh</a>、
      <a href="https://haekyu.com/">Haekyu Park</a>、
      <a href="http://nilakshdas.com/">Nilaksh Das</a>、
      <a href="https://fredhohman.com/">Fred Hohman</a>、
      <a href="http://minsuk.com">Minsuk Kahng</a>、
      <a href="https://www.cc.gatech.edu/~dchau/">Polo Chau</a>
      によって作成されました。これはGeorgia TechとOregon Stateの研究コラボレーションの成果です。Anmol Chhabria、Kaan Sancak、Kantwon Rogers、Georgia Tech Visualization Labのサポートと建設的なフィードバックに感謝します。この研究は、NSF grants IIS-1563816、CNS-1704701、NASA NSTRF、DARPA GARD、Intel、NVIDIA、Google、Amazonからの寄付によって一部支援されています。
    </p>
  </div>
</body>
