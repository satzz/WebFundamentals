project_path：/web/fundamentals/_project.yaml book_path：/web/fundamentals/_book.yaml

{＃wf_updated_on：2017-08-04＃} {＃wf_published_on：2017-06-28＃} {＃wf_blink_components：Blink> Payments＃}

# 支払いリクエストUXの考慮事項{：.page-title}

{％include "web / _shared / contributors / mustafa.html"％}

[支払い要求API](/web/fundamentals/payments/)は、基本的にユーザー向けに構築されており、モバイルでの煩雑なチェックアウトフローを効率的に処理し、コンバージョン率の高い販売者に効率的に提供できます。

Googleが実施した調査では、 <strong>65.9％のユーザーがモバイルデバイスで購入を途中で止めた</strong>ことがわかりました。なぜ彼らが購入を断念したかについて調査で尋ねられた人のうち、最も一般的な回答は、読みにくい、理解しづらい、または長すぎるという形のものでした。

これらの課題を解決するため、Payment Request APIはより簡単なフローを作成します。 APIは、ユーザーが簡単なタップまたはクリックで商品を購入できるように、ユーザーの支払い詳細を事前に入力します。

Payment Request APIについて考える一つの方法は、基本的にブラウザがユーザーの支払い方法を管理してウェブで支払いを簡単にする方法であるということです。

## 2つのビューステート

Chrome UIでは、支払いリクエストAPIに2つの状態があります。 <em>「領収書」</em>ビューと編集ビューです。マーチャントのサイトを部分的に隠す<em>「領収書」</em>ビュー（左下に示す）。これにより、ユーザは支払詳細をすばやく確認することができます。次に、編集画面（右下に表示）があり、フルスクリーンを展開して各セクションを編集することができます。ユーザーが<em>「領収書」</em>ビューから行動をタップまたはクリックすると、編集モードに入り、そこで強制的に続行されます。

<div class="attempt-left">
  <figure>
    <img src="https://github.com/satzz/WebFundamentals/blob/master/src/content/en/fundamentals/payments/images/state01-receipt-view.png?raw=true" alt="This is an example of the 'receipt' view">
  </figure>
</div>
<div class="attempt-right">
  <figure>
    <img src="https://github.com/satzz/WebFundamentals/blob/master/src/content/en/fundamentals/payments/images/state02-edit-view.png?raw=true" alt="This is an example of the edit view">
  </figure>
</div>

## 基本チェックフロー

この例では、ユーザーがサイトに移動してアイテムを選択し、支払い要求のチェックフローを実行します。ユーザーが購入をクリックすると、ブラウザが引き継ぎます。ユーザーが以前に希望の支払い方法などの詳細を入力した場合、これらはデフォルトとして表示されます。さもなければ、ユーザーはこれらを追加できます。特定のユースケースに応じて、デフォルトが自動的に選択されるようにするか、ユーザーに入力を強制することができます。

<video controls autoplay loop muted poster="images/basic-intro-poster.png">
  <source src="videos/basic-intro.webm" type="video/webm; codecs=vp8">
  <source src="videos/basic-intro.mp4" type="video/mp4; codecs=h264">
</source></source></video>

### チェックアウトガイダンス

サインアップを要求してチェックアウトをブロックしないでください。ユーザーが既にサインインしていることの大きな利点の1つは、既に支払い方法が保存されていることです。支払いリクエストは、電子メールや電話番号を含むユーザーの支払い詳細に簡単にアクセスできるため、これらの詳細を要求するだけで、ユーザーにサインインする必要はありません。

### UXの流れを適切に処理する

Chromeは処理、キャンセル、および成功のいくつかの側面を処理しますが、ベストプラクティスとして、チェックアウトのステータスをユーザーに知らせる必要があります。例えば：

- ユーザーに支払いが打ち切られたかどうかを知らせます。

- お支払いを求めるUIを提供することで、ユーザーにチェックアウトプロセスを準備します。

- チェックアウトの進捗状況と状態をユーザーに知らせておくのは、ステッパーを使用することです。

- 支払いが完了し成功したことをユーザーに知らせる。

<div class="attempt-left">
  <figure>
    <img src="https://github.com/satzz/WebFundamentals/blob/master/src/content/en/fundamentals/payments/images/graceful-error-messaging.png?raw=true" alt="This is an example of error messages">
    <figcaption>エラーメッセージがクリアされていることを確認してください。</figcaption>
  </figure>
</div>

<div class="attempt-right">
  <figure>
    <img src="https://github.com/satzz/WebFundamentals/blob/master/src/content/en/fundamentals/payments/images/graceful-stepper.png?raw=true" alt="This is an example of steppers">
    <figcaption>このステッパーはカートから住所と支払いの確認までの進捗状況を示します。</figcaption>
  </figure>
</div>

<div class="clearfix"></div>

## カスタマイズ可能なもの

UIの一部は、ボタンの色やそれぞれのテキストラベルなど、いくつかの例外を除いてカスタマイズ可能です。

注：この例はChromeブラウザ向けです。他のブラウザには、さまざまなUI要素や「Pay」や「Cancel」などのボタンのラベルも含まれています。

別の考慮事項は、デバイスが多くのサイズで来るので、スペースは多分プレミアムであるので、ラベルを短く保ち、ブランド認知に焦点を当てる。

<div class="attempt-right">
  <figure>
    <img src="https://github.com/satzz/WebFundamentals/blob/master/src/content/en/fundamentals/payments/images/what-can-be-changed.png?raw=true" alt="This is an example what parts of the Payment Request API can be changed.">
  </figure>
</div>

1. アイコンはファビコンから取り出されます。ファビコンは高価であり、店舗では認識可能です。ベストプラクティスのために、ブラウザが最適なサイズを選択できるように、サイズの異なる複数のアイコンを用意してください。あなたは[ここでもっと学ぶ](/web/fundamentals/payments/deep-dive-into-payment-request)ことができ[ます](/web/fundamentals/payments/deep-dive-into-payment-request) 。

2. これは、 <code><title></code>タグから取得されます。これは、ユーザーが購入しているものと購入しているものを人間が判読可能な形で表示する最も強力な機会です。

3. これがドメイン/ URLです。

4. このテキスト・ラベルは、Payments Request APIを介して変更することができます。

5. 価格と通貨は合計から取られます。

6. プリセットリストから見出しを選択することができます：出荷、ピックアップまたは配送。

<div class="clearfix"> ##一般的なUXのアドバイス</div>

### ユーザステップを排除する

ユーザーが購入をするために必要な作業量を制限してください。クリアテキストのラベルを持つシンプルなUIは、ユーザーにスピードの向上を認知させます。例えば、 <em>「今すぐ購入」</em>ボタンを使用すると、アカウントを作成したりショッピングカートを作成したりせずに、簡単なワンタップ購入を行うことができます。ユーザーがより速くチェックアウトできるほど、満足度は高くなります。つまり、プロセスを完了する機会が大幅に増え、ビジネスが繰り返される可能性があります。 <em>「今すぐ購入」</em>または<em>「エクスプレスチェックアウト」</em>ボタン（ <em>「カートに追加」</em>と比較して）を使用してアイテムを購入することができるようにすることで、スピードが向上し、チェックアウトが成功します。

<div>
  <figure>
    <img src="https://github.com/satzz/WebFundamentals/blob/master/src/content/en/fundamentals/payments/images/general-ux-buy-now.png?raw=true" alt="This is an example of 'buy now' buttons">
    <figcaption>可能であれば<em>「今すぐ購入」</em>ボタンを追加して、迅速なチェックアウトの経験を作りましょう。</figcaption>
  </figure>
</div>

### UIアフォーダンス

現実の世界のユーザーは、しばしば、モバイルデバイスをスリーブまたはバンパーで巻く。人間工学と考察を考えてください。バンパーは、画面に近すぎるとタッチターゲットに当たるのを防ぐことができます。そのため、実際にはサイトやアプリの速度が実際よりも遅いという幻想を提示していないときには、です。

#### タッチターゲット

タッチターゲットが要素の視覚範囲を超えていることを確認します。たとえば、 `24 x 24`ピクセルのアイコンには、 `48 x 48`ピクセルのタッチターゲットが必要です。 。

`48 x 48 px`タッチターゲットは約9 mmで、推奨ターゲットは7〜10 mmです。画像が`48 x 48 px`より大きい場合は、ターゲットのターゲットとして画像の寸法を使用することができます。自動車障害者のような特別なニーズを持つユーザーにも、より大きなタッチターゲットを使用することが適切な場合があります。

<div>
  <figure>
    <img src="https://github.com/satzz/WebFundamentals/blob/master/src/content/en/fundamentals/payments/images/affordance-touch-targets.png?raw=true" alt="This is an example of desired touch targets">
    <figcaption>最低48ピクセルのタッチターゲットを与えることで、ユーザーがより小さなUI要素をタップまたはクリックするのを助けることができます。</figcaption>
  </figure>
</div>

### ユーザーのフィードバックメカニズム

購入が成功したことをユーザーに確認するだけでなく、フィードバックを提供して、ユーザーに自分の経験の状態を再確認します。フィードバックの一例は、アクションが完了すると表示される[「Snackbar」UI](https://material.io/guidelines/components/snackbars-toasts.html)です。平均的な読者は、毎分200〜400語を読み、理解することができます。文字列の長さに応じて、ユーザーが読むために十分な長さのUIを画面に残しておきます.5語の文章では約3秒です。

<video class="attempt-left" controls autoplay loop muted poster="images/basic-intro-poster.png"></video>

  <source src="videos/components_snackbar.webm" type="video/webm; codecs=vp8">
  <source src="videos/components_snackbar.mp4" type="video/mp4; codecs=h264">

<div class="clearfix"></div>
</source></source>


### パフォーマンス

Googleで行われた別の調査では、最初のダイアログが十分に速く描画されないため、70％のユーザーがチェックアウトから脱落したことがわかりました。あなたが使用して、チェックアウトの流れを改善するために、貧弱なネットワーク条件をテストを確認してください[灯台](/web/tools/lighthouse)と[デベロッパーツール](/web/tools/chrome-devtools/network-performance/)ネットワークエミュレーションを。

### 言語

アクションボタンの呼び出しの言語が改善されているため、ユーザーはそのフローでどのようなアクションが取られているか理解できます。彼らがチェックアウトの流れにおいて一貫していることは重要です。最も理にかなったラベルを使用してください。

<div class="attempt-left">
  <figure>
    <img src="https://github.com/satzz/WebFundamentals/blob/master/src/content/en/fundamentals/payments/images/delivery-options-delivery.png?raw=true" alt="This is an example of a label titled 'delivery'">
  </figure>
</div>


<div class="attempt-right">
  <figure>
    <img src="https://github.com/satzz/WebFundamentals/blob/master/src/content/en/fundamentals/payments/images/delivery-options-pickup.png?raw=true" alt="This is an example of a label titled 'pickup'">
  </figure>
</div>
Depending on the context the label <em>"Pickup"</em> may be more 
appropriate than the label <em>"Delivery"</em>.


### ユーザーにフォールバックオプションを提供する

支払い依頼の購入をブロックしないでください。ブラウザでサポートされていないか、または失敗した場合、支払いリクエストAPIが利用できない場合、エラーメッセージではなく、デフォルトのチェックアウトにシームレスにフォールバックします。

ユーザーがチェックアウトフォームにアクセスできるすべての方法のユーザーフローを作成することをお勧めします。設計が必要な共通のエントリポイント。

- ユーザーはすぐにチェックアウトに向かいます。

- ユーザーはホームページから開始し、カートを表示し、チェックアウトに向かいます。

- ユーザは、ホームページから開始し、アイテムを見て、今購入するように指示される。

- ユーザーはホームページから開始し、クリックしてカートにアイテムを追加し、チェックアウトに向かいます。

- ユーザーはアイテムから開始し、チェックアウトに向かいます。

<div>
  <figure>
    <img src="https://github.com/satzz/WebFundamentals/blob/master/src/content/en/fundamentals/payments/images/user-flow-diagram.png?raw=true" alt="This is an illustrative example 
    of the above, different paths a user 
    takes when checking out">
  <figcaption>これは、ユーザがチェックアウトする際にとる、上記の異なるパスの説明的な例です</figcaption>
  </figure>
</div>

## フローの例

### ゲストチェックアウト

この例では、ユーザーにチェックアウトを許可し、IDコードと領収書を電子メールで送信します。あるいは、 `window.print();`を呼び出すボタンを提供することもできます`window.print();`ユーザーにPDFを提供します。

<video controls autoplay loop muted poster="images/basic-intro-poster.png">
  <source src="videos/basic-intro.webm" type="video/webm; codecs=vp8">
  <source src="videos/basic-intro.mp4" type="video/mp4; codecs=h264">
</source></source></video>

### 今すぐ購入

この例では、すべての商品に「今すぐ購入」があります。ユーザーが「今すぐ購入」をタップまたはクリックすると、まず特定の納品日とカスタムメッセージを尋ねて、チェックアウトプロセスを開始します。プロセスの次のステップでは、Payment Request APIが起動され、その後に確認ページが表示されます。

<div>
  <figure>
    <img src="https://github.com/satzz/WebFundamentals/blob/master/src/content/en/fundamentals/payments/images/general-ux-buy-now.png?raw=true" alt="This is an example of 'buy now' buttons">
    <figcaption>可能であれば<em>「今すぐ購入」</em>ボタンを追加して、迅速なチェックアウトの経験を作りましょう。</figcaption>
  </figure>
</div>

### 配達

ユーザーのデフォルトアドレスを自動選択することは技術的に可能ですが、ユーザーが優先アドレスを選択できるようにすることがベストプラクティスです。これにより、商人は、住所が正しいことと配送可能であることを確認することができます。ユーザーが住所を変更できるようにすることで、配送オプションを更新および変更することができます。たとえば、ユーザーがローカルの場合は、ローカルの送料を提供できますが、ユーザーが国際的な住所を選択した場合は、国際料金の配送オプションを変更することができます。

アドレスを自動選択することにより、不必要な摩擦を発生させることができます。たとえば、デフォルトの住所が納品または出荷できない場所にある場合、ユーザーが実際の支払い操作を開始した場合にのみ、その住所が取得されます。ユーザーが購入を完全に放棄する原因となる可能性があるため、すべてのコストをかけて妨害の経験を作成しないでください。

### 配送オプション

実際の日付に応じてユーザー配信オプションを指定します。日付のような文脈上のデータは、ユーザに文脈が与えられ、アイテムがいつ配信されるのかについてさらに安心感を与える。

<div class="attempt-left">
  <figure>
    <img src="https://github.com/satzz/WebFundamentals/blob/master/src/content/en/fundamentals/payments/images/delivery-options-with-dates.png?raw=true" alt="This is an example of delivery options with dates">
    <figcaption>ユーザーが自分の経験を計画しカスタマイズするのに役立つ日付を付けます。</figcaption>
  </figure>
</div>

<div class="clearfix"></div>

### 追加情報の追加

商人がトランザクションの前または後に追加の情報を追加する必要がある場合が多いユースケースがあります。ロイヤリティカード番号または割引クーポンなど。

### ギフトカード

ギフトカードはPayment Request APIで直接動作しませんので、事前にカードコードを入力できるフィールドを追加することをおすすめします。まだギフトカードを追加したら、さらに支払が必要な場合は、支払いリクエストAPIを起動することができます。以下の図は、このフローを示しています。

<div>
  <figure>
    <img src="https://github.com/satzz/WebFundamentals/blob/master/src/content/en/fundamentals/payments/images/gift-card-checkout-flow.png?raw=true" alt="This is an example of giftcard workflow 
    mentioned above">
  </figure>
</div>

## 支払いリクエストAPIフローのカスタマイズ

### 支払いリクエストAPIを呼び出す前に

支払いリクエストAPIでは、バウチャーとロイヤルティコードは直接処理されません。代わりに、支払い要求APIを起動する前に、ユーザーにコードまたはロイヤリティカードのスキームを追加する機能を提供します。実際には、これは、ユーザーが最初に<em>「今すぐ購入」を</em>クリックすると、最初にクーポンまたはロイヤリティカード番号を追加できるチェックアウトページに移動することを意味します。以下はその例です。

<div>
  <figure>
    <img src="https://github.com/satzz/WebFundamentals/blob/master/src/content/en/fundamentals/payments/images/wooCommerce.png?raw=true" alt="This is an example of WooCommerce">
    <figcaption>この例の<a href="https://woocommerce.paymentrequest.show/">WooCommerceでは</a> 、ユーザーはトランザクションを完了する前にクーポンコードを追加します。 （ <a href="https://woocommerce.paymentrequest.show/">デモのウェブサイト</a>は、偽のクレジットカード番号「4242 4242 4242 4242」のみで動作します）</figcaption>
  </figure>
</div>

加盟店の法務チームがチェックアウトプロセス中に表示される利用規約（TOS）などの法的テキストボックスを必要とする場合は、支払いリクエストAPIのフローが開始される前に表示されることをお勧めします。理想的には、購入ボタンまたはリンクの横にTOSを置くことで、ユーザーが取った手順とアクションを最小限に抑えることができます。しかし、これが不可能な場合もあるので、ユーザーが「今すぐ購入」ボタンをクリックした後の解決策の1つは、TOSの承認を求めるプロセスの追加ステップを提供することです。次のステップは、PR支払要求フローを開始することです。

### 支払請求の途中

支払いオプションが選択された後、追加のUI要素を提供することができます。そのため、APIのデフォルトのスピナーを表示する代わりに、次の例のように独自のスピナーを表示できます。支払詳細を処理したら、選択した任意のUIを提示することができます。

したがって、この視覚的な例では：

- ユーザは「今すぐ購入」をタップし、商人は関数show（）を呼び出します。

- 次に、加盟店は、支払い要求APIまたはそれ自身のUIを用いて支払いを処理する。

- すべての条件が満たされると、販売者は確認ページを表示します。

この[テクニックの](/web/fundamentals/payments/deep-dive-into-payment-request#completing_the_transaction)詳細については、 [こちらをご覧ください](/web/fundamentals/payments/deep-dive-into-payment-request#completing_the_transaction)

<div>
  <figure>
    <img src="https://github.com/satzz/WebFundamentals/blob/master/src/content/en/fundamentals/payments/images/show-to-promise.png?raw=true" alt="This is an example of the different flow you can take after firing a 'show()'' function">
  </figure>
</div>

### 支払い/購入が処理された後。

商人が、配送指示やショッピング体験に関するフィードバックなど、非標準的な情報をユーザから追加する機会をユーザに与える機会があることがあります。これはPayment Request APIで直接行うことはできませんが、支払いが成功した場合は`textfield`などのUI要素を表示することは可能です。

<div>
  <figure>
    <img src="https://github.com/satzz/WebFundamentals/blob/master/src/content/en/fundamentals/payments/images/mid-way-flow.png?raw=true" alt="This is an example of a 'textfield' appearing after the checkout flow.">
  </figure>
</div>

コード例：

```
request.show()
.then(function(paymentResponse) {
    // Process payment here.
    // Close the UI:
    paymentResponse.complete('success').then(function() {
        // Request additional shipping address details.
        const additionalDetailsContainer = document.getElementById('additional-details-container');
        additionalDetailsContainer.style.display = 'block';
        additionalDetailsContainer.focus();
    }).catch(function(error) {
        // Handle error.
    });
})
.catch(function(error) {
    // Handle error.
});
```

よりユニークなユースケースのコードはここにあります（https://github.com/w3c/payment-request-info/wiki/ CodeExamples＃show-additional-user-interface-after-payment）。

## 既存のユーザー

既存の顧客の場合は引き続き支払いリクエストAPIを使用できますが、経験を調整して既存のログインした顧客に新しいUIを提示する必要があります。たとえば、「Express pay」ボタン、「Pay with new card」ボタン、または「Ship to new address」ボタンを使用して支払いリクエストAPIを導入することができます。

目標は、ユーザーが不安なく商人のサイトですばやく商品を購入できることを示すことです。支払いリクエストAPIはすばやく効率的ですが、既存のユーザーはこの新しい支払いフローで何らかの教育を必要とするため、速度と効率性を表すテキストラベルとUIで新しいUXパターンを導入することに重点を置いています。

## コードデモとブラウザのサポート

この[デモ](https://paymentrequest.show/demo/)では、さまざまな機能やUXパターンを試すことができます。支払いリクエストAPIは現在、

- AndroidでChrome 53以上
- デスクトップとiOSのChrome 61以上
- デスクトップのエッジ15以上

[caniuse.comで](https://caniuse.com/#search=payments)他のすべてのブラウザの最新ステータスを追跡できます。

## ステッカーシート

独自のフローを作成するには、 [Github Repo](https://goo.gl/daxhRa)からPayment Request APIステッカーシートをダウンロードして[ください](https://goo.gl/daxhRa) 。
