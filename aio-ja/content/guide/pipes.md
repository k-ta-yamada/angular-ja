# Transforming Data Using Pipes

[パイプ](guide/glossary#pipe "Definition of a pipe") を使用して、文字列、通貨額、日付、その他の表示データを変換およびフォーマットします。
パイプは、入力値を受け入れて変換された値を返すために [テンプレート式](/guide/glossary#template-expression "Definition of template expression") で使用できる単純な関数です。
たとえば、生の文字列形式ではなく、パイプを使用して日付を **April 15, 1988** として表示します。

<div class="alert is-helpful">

  このトピックで使用されているサンプルアプリについては、 <live-example></live-example> をご覧ください。

</div>

Angular は、ロケール情報を使用してデータをフォーマットする国際化 (i18n) 用の変換を含む、典型的なデータ変換用の組み込みパイプを提供します。
以下は、データのフォーマットに一般的に使用される組み込みパイプです:

* [`DatePipe`](api/common/DatePipe): ロケールのルールに従って日付値をフォーマットします。
* [`UpperCasePipe`](api/common/UpperCasePipe): テキストをすべて大文字に変換します。
* [`LowerCasePipe`](api/common/LowerCasePipe): テキストをすべて小文字に変換します。
* [`CurrencyPipe`](api/common/CurrencyPipe): 数値を通貨文字列に変換し、ロケールルールに従ってフォーマットします。
* [`DecimalPipe`](/api/common/DecimalPipe): 数値を小数点のある文字列に変換し、ロケールルールに従ってフォーマットします。
* [`PercentPipe`](api/common/PercentPipe): 数値をパーセンテージ文字列に変換し、ロケールルールに従ってフォーマットします。

<div class="alert is-helpful">

* 組み込みパイプの完全なリストについては、 [パイプAPIのドキュメント](/api/common#pipes "Pipes API reference summary") をご覧ください。
* 国際化 (i18n) の取り組みにパイプを使用する方法の詳細については、 [ロケールに基づくデータのフォーマット](/guide/i18n#i18n-pipes "Formatting data based on locale") を参照してください。

</div>

パイプを作成してカスタム変換をカプセル化し、テンプレート式でカスタムパイプを使用することもできます。

## 前提条件

パイプを使用するには、次の基本的な知識が必要です:

* [Typescript](guide/glossary#typescript "Definition of Typescript") と HTML5 プログラミング
* CSS スタイルを使用した HTML の [テンプレート](guide/glossary#template "Definition of a template")
* [コンポーネント](guide/glossary#component "Definition of a component")

## テンプレートでのパイプの使用

パイプを適用するには、次のコード例に示すように、組み込みの [`DatePipe`](api/common/DatePipe) の名前 `date` と共に、テンプレート式内でパイプ演算子 (`|`) を使用します。
この例のタブには、次のものが表示されます:

* `app.component.html` は、セパレートテンプレートで `date` を使用して誕生日を表示します。
* `hero-birthday1.component.ts` は、誕生日の値も設定するコンポーネントのインラインテンプレートの一部として同じパイプを使用します。

<code-tabs>
  <code-pane
    header="src/app/app.component.html"
    region="hero-birthday-template"
    path="pipes/src/app/app.component.html">
  </code-pane>
  <code-pane
    header="src/app/hero-birthday1.component.ts"
    path="pipes/src/app/hero-birthday1.component.ts">
  </code-pane>
</code-tabs>

コンポーネントの誕生日の値は
[パイプ演算子](guide/template-syntax#pipe) ( | ) を介して
[`date`](api/common/DatePipe) 関数に渡されます。

{@a parameterizing-a-pipe}

## パラメータとチェーンパイプを使用したデータのフォーマット

オプションのパラメーターを使用して、パイプの出力を微調整します。
たとえば、パラメーターとして EUR などの国コードを使用して [`CurrencyPipe`](api/common/CurrencyPipe "API reference") を使用できます。
テンプレート式 `{{ amount | currency:'EUR' }}` は `amount` をユーロ通貨に変換します。 
パイプ名 (`currency`) の後にコロン (`:`) とパラメーター値 (`'EUR'`) を付けます。

パイプが複数のパラメーターを受け入れる場合は、値をコロンで区切ります。
たとえば `{{ amount | currency:'EUR':'Euros '}}` は、2番目のパラメーターである文字列リテラル `'Euros '` を出力文字列に追加します。 文字列リテラルやコンポーネントプロパティなど、任意の有効なテンプレート式をパラメーターとして使用できます。

いくつかのパイプは、少なくとも 1 つのパラメーターを必要とし、 [`SlicePipe`](/api/common/SlicePipe "API reference for SlicePipe") などのより多くのオプションパラメーターを許可します。 たとえば `{{ slice:1:5 }}` は、要素 `1` で始まり要素 `5` で終わる要素のサブセットを含む新しい配列または文字列を作成します。

### 例: 日付のフォーマット

次の例のタブは、2つの異なる形式 (`'shortDate'` and `'fullDate'`) の切り替えを示しています:

* `app.component.html` テンプレートは、 [`DatePipe`](api/common/DatePipe) （名前付き `date`） のフォーマットパラメータを使用して、日付を **04/15/88** として表示します。
* `hero-birthday2.component.ts` コンポーネントは、パイプのフォーマットパラメータを `テンプレート` セクションのコンポーネントの `format` プロパティにバインドし、コンポーネントの `toggleFormat()` メソッドにバインドされたクリックイベントのボタンを追加します。
* `hero-birthday2.component.ts` コンポーネントの `toggleFormat()` メソッドは、コンポーネントの `format` プロパティを
短い形式 (`shortDate`) と長い形式 (`fullDate`) の間で切り替えます。

<code-tabs>
  <code-pane
    header="src/app/app.component.html"
    region="format-birthday"
    path="pipes/src/app/app.component.html">
  </code-pane>
  <code-pane
    header="src/app/hero-birthday2.component.ts (template)"
    region="template"
    path="pipes/src/app/hero-birthday2.component.ts">
  </code-pane>
  <code-pane
    header="src/app/hero-birthday2.component.ts (class)"
    region="class"
    path="pipes/src/app/hero-birthday2.component.ts">
  </code-pane>
</code-tabs>

Figure 1. に示すように、 **Toggle Format** ボタンをクリックすると、日付形式が **04/15/1988** と **Friday, April 15, 1988** に切り替わります。

<div class="lightbox">
  <img src='generated/images/guide/pipes/date-format-toggle-anim.gif' alt="Date Format Toggle">
</div>

**Figure 1.** ボタンをクリックすると日付形式が切り替わります

<div class="alert is-helpful">

`date` パイプのフォーマットオプションについては [DatePipe](api/common/DatePipe "DatePipe API Reference page") を参照してください。

</div>

### 例: パイプをチェーンして2つの形式を適用する

1 つのパイプの出力が次のパイプの入力になるようにパイプをチェーンすることができます。

次の例では、チェーンされたパイプが最初に日付の値にフォーマットを適用し、次にフォーマットされた日付を大文字に変換します。
`src/app/app.component.html` テンプレートの最初のタブは `DatePipe` と `UpperCasePipe` をつなげて、誕生日を **APR 15, 1988** として表示します。
`src/app/app.component.html` テンプレートの 2 番目のタブは `uppercase` にチェーンする前の `date` に `fullDate` パラメーターを渡し、 **FRIDAY, APRIL 15, 1988** を生成します。

<code-tabs>
  <code-pane
    header="src/app/app.component.html (1)"
    region="chained-birthday"
    path="pipes/src/app/app.component.html">
  </code-pane>
  <code-pane
    header="src/app/app.component.html (2)"
    region="chained-parameter-birthday"
    path="pipes/src/app/app.component.html">
  </code-pane>
</code-tabs>

{@a Custom-pipes}

## カスタムデータ変換用のパイプの作成

組み込みパイプでは提供されない変換をカプセル化するカスタムパイプを作成します。
次に、組み込みパイプを使用するのと同じ方法で、テンプレート式でカスタムパイプを使用して、入力値を出力値に変換して表示できます。

### クラスをパイプとしてマークする

クラスをパイプとしてマークし、構成メタデータを提供するには、 [`@Pipe`](/api/core/Pipe "API reference for Pipe") [デコレーター](/guide/glossary#decorator--decoration "Definition for decorator") をクラスに適用します。
パイプクラス名には [UpperCamelCase](guide/glossary#case-types "Definition of case types") (クラス名の一般的な規則) を使用し、対応する `name` 文字列には [camelCase](guide/glossary#case-types "Definition of case types") を使用します。
`name` にハイフンを使用しないでください。
詳細およびその他の例については、 [Pipe names](guide/styleguide#pipe-names "Pipe names in the Angular coding style guide") を参照してください。

組み込みパイプの場合と同様に、テンプレート式で `name` を使用します。

<div class="alert is-important">

* `NgModule` メタデータの `declarations` フィールドにパイプを含めて、テンプレートで使用できるようにします。 サンプルアプリの `app.module.ts` ファイルをご覧ください (<live-example></live-example>) 。 詳細については [NgModules](guide/ngmodules "NgModules introduction") を参照してください。
* カスタムパイプを登録します。 [Angular CLI](cli "CLI Overview and Command Reference") [`ng generate pipe`](cli/generate#pipe "ng generate pipe in the CLI Command Reference") コマンドは、パイプを自動的に登録します。

</div>

### PipeTransformインターフェースの使用

カスタムパイプクラスに [`PipeTransform`](/api/core/PipeTransform "API reference for PipeTransform") インターフェイスを実装して、変換を実行します。

Angular は最初の引数としてバインディングの値を持ち、リスト形式で 2 番目の引数としてすべてのパラメータを使って `transform` メソッドを呼び出し、変換された値を返します。

### 例: 値を指数関数的に変換する

ゲームでは、ヒーローの力を高めるために、指数関数的に値を上げる変換を実装することができます。
たとえば、ヒーローのスコアが 2 の場合、ヒーローのパワーを指数関数的に 10 ブーストすると、スコアは 1024 になります。
この変換にはカスタムパイプを使用できます。

次のコード例は 2 つのコンポーネント定義を示しています:

* `exponential-strength.pipe.ts` コンポーネントは、変換を実行する `transform` メソッドを使用して `exponentialStrength` という名前のカスタムパイプを定義します。
パイプに渡されるパラメーター (`exponent`) を `transform` メソッドの引数に定義します。

* `power-booster.component.ts` コンポーネントは、値 (`2`) と指数パラメーター (`10`) を指定してパイプを使用する方法を示しています。
Figure 2. に出力を示します。

<code-tabs>
  <code-pane
    header="src/app/exponential-strength.pipe.ts"
    path="pipes/src/app/exponential-strength.pipe.ts">
  </code-pane>
  <code-pane
    header="src/app/power-booster.component.ts"
    path="pipes/src/app/power-booster.component.ts">
  </code-pane>
</code-tabs>

<div class="lightbox">
  <img src='generated/images/guide/pipes/power-booster.png' alt="Power Booster">
</div>

**Figure 2.** `exponentialStrength` パイプからの出力

<div class="alert is-helpful">

<live-example></live-example> の `exponentialStrength` パイプの動作を調べるには、テンプレートの値とオプションの指数を変更します。

</div>

{@a change-detection}

## パイプのデータバインディングによる変更検知

パイプで [データバインディング](/guide/glossary#data-binding "Definition of data binding") を使用して、値を表示し、ユーザーアクションに応答します。
データが `String` や `Number` などのプリミティブな入力の値、または `Date` や `Array` などのオブジェクト参照の入力である場合、 Angular は入力値または参照の変更を検知するたびにパイプを実行します。

たとえば、次のコード例に示すように、前のカスタムパイプの例を変更して、 `ngModel` で双方向データバインディングを使用して量とブースト係数を入力することができます。

<code-example path="pipes/src/app/power-boost-calculator.component.ts" header="src/app/power-boost-calculator.component.ts">

</code-example>

Figure 3. に示すように、ユーザーが "normal power" の値、または "boost factor" を変更するたびに、 `exponentialStrength` パイプが実行されます。

<div class="lightbox">
  <img src='generated/images/guide/pipes/power-boost-calculator-anim.gif' alt="Power Boost Calculator">
</div>

**Figure 3.** `exponentialStrength` パイプの量とブースト係数を変更する

Angular は各変更を検知し、すぐにパイプを実行します。
これはプリミティブな入力値には問題ありません。
ただし、複合オブジェクト *内の* 何か (日付の月、配列の要素、オブジェクトのプロパティなど) を変更する場合は、変更検知のしくみと `不純な` パイプの使用方法を理解する必要があります。

### 変更検知のしくみ

Angular looks for changes to data-bound values in a [change detection](guide/glossary#change-detection "Definition of change detection") process that runs after every DOM event: every keystroke, mouse move, timer tick, and server response.
The following example, which doesn't use a pipe, demonstrates how Angular uses its default change detection strategy to monitor and update its display of every hero in the `heroes` array.
The example tabs show the following:

* In the `flying-heroes.component.html (v1)` template, the `*ngFor` repeater displays the hero names.
* Its companion component class `flying-heroes.component.ts (v1)` provides heroes, adds heroes into the array, and resets the array.

<code-tabs>
  <code-pane
    header="src/app/flying-heroes.component.html (v1)"
    region="template-1"
    path="pipes/src/app/flying-heroes.component.html">
  </code-pane>
  <code-pane
    header="src/app/flying-heroes.component.ts (v1)"
    region="v1"
    path="pipes/src/app/flying-heroes.component.ts">
  </code-pane>
</code-tabs>

Angular updates the display every time the user adds a hero.
If the user clicks the **Reset** button, Angular replaces `heroes` with a new array of the original heroes and updates the display.
If you add the ability to remove or change a hero, Angular would detect those changes and update the display as well.

However, executing a pipe to update the display with every change would slow down your app's performance.
So Angular uses a faster change-detection algorithm for executing a pipe, as described in the next section.

{@a pure-and-impure-pipes}

### Detecting pure changes to primitives and object references

By default, pipes are defined as *pure* so that Angular executes the pipe only when it detects a *pure change* to the input value.
A pure change is either a change to a primitive input value (such as `String`, `Number`, `Boolean`, or `Symbol`), or a changed object reference (such as `Date`, `Array`, `Function`, or `Object`).

{@a pure-pipe-pure-fn}

A pure pipe must use a pure function, which is one that processes inputs and returns values without side effects.
In other words, given the same input, a pure function should always return the same output.

With a pure pipe, Angular ignores changes within composite objects, such as a newly added element of an existing array, because checking a primitive value or object reference is much faster than performing a deep check for differences within objects.
Angular can quickly determine if it can skip executing the pipe and updating the view.

However, a pure pipe with an array as input may not work the way you want.
To demonstrate this issue, change the previous example to filter the list of heroes to just those heroes who can fly.
Use the `FlyingHeroesPipe` in the `*ngFor` repeater as shown in the following code.
The tabs for the example show the following:

* The template (`flying-heroes.component.html (flyers)`) with the new pipe.
* The `FlyingHeroesPipe` custom pipe implementation (`flying-heroes.pipe.ts`).

<code-tabs>
  <code-pane
    header="src/app/flying-heroes.component.html (flyers)"
    region="template-flying-heroes"
    path="pipes/src/app/flying-heroes.component.html">
  </code-pane>
  <code-pane
    header="src/app/flying-heroes.pipe.ts"
    region="pure"
    path="pipes/src/app/flying-heroes.pipe.ts">
  </code-pane>
</code-tabs>

The app now shows unexpected behavior: When the user adds flying heroes, none of them appear under "Heroes who fly."
This happens because the code that adds a hero does so by pushing it onto the `heroes` array:

<code-example path="pipes/src/app/flying-heroes.component.ts" region="push" header="src/app/flying-heroes.component.ts"></code-example>

The change detector ignores changes to elements of an array, so the pipe doesn't run.

The reason Angular ignores the changed array element is that the *reference* to the array hasn't changed.
Since the array is the same, Angular does not update the display.

One way to get the behavior you want is to change the object reference itself.
You can replace the array with a new array containing the newly changed elements, and then input the new array to the pipe.
In the above example, you can create an array with the new hero appended, and assign that to `heroes`. Angular detects the change in the array reference and executes the pipe.

To summarize, if you mutate the input array, the pure pipe doesn't execute.
If you *replace* the input array, the pipe executes and the display is updated, as shown in Figure 4.

<div class="lightbox">
  <img src='generated/images/guide/pipes/flying-heroes-anim.gif' alt="Flying Heroes">
</div>

**Figure 4.** The `flyingHeroes` pipe filtering the display to flying heroes

The above example demonstrates changing a component's code to accommodate a pipe.

To keep your component simpler and independent of HTML templates that use pipes, you can, as an alternative, use an *impure* pipe to detect changes within composite objects such as arrays, as described in the next section.

{@a impure-flying-heroes}

### Detecting impure changes within composite objects

To execute a custom pipe after a change *within* a composite object, such as a change to an element of an array, you need to define your pipe as `impure` to detect impure changes.
Angular executes an impure pipe every time it detects a change with every keystroke or mouse movement.

<div class="alert is-important">

While an impure pipe can be useful, be careful using one. A long-running impure pipe could dramatically slow down your app.

</div>

Make a pipe impure by setting its `pure` flag to `false`:

<code-example path="pipes/src/app/flying-heroes.pipe.ts" region="pipe-decorator" header="src/app/flying-heroes.pipe.ts"></code-example>

The following code shows the complete implementation of `FlyingHeroesImpurePipe`, which extends `FlyingHeroesPipe` to inherit its characteristics.
The example shows that you don't have to change anything else—the only difference is setting the `pure` flag as `false` in the pipe metadata.

<code-tabs>
  <code-pane
    header="src/app/flying-heroes.pipe.ts (FlyingHeroesImpurePipe)"
    region="impure"
    path="pipes/src/app/flying-heroes.pipe.ts">
  </code-pane>
  <code-pane
    header="src/app/flying-heroes.pipe.ts (FlyingHeroesPipe)"
    region="pure"
    path="pipes/src/app/flying-heroes.pipe.ts">
  </code-pane>
</code-tabs>

`FlyingHeroesImpurePipe` is a good candidate for an impure pipe because the `transform` function is trivial and fast:

<code-example path="pipes/src/app/flying-heroes.pipe.ts" header="src/app/flying-heroes.pipe.ts (filter)" region="filter"></code-example>

You can derive a `FlyingHeroesImpureComponent` from `FlyingHeroesComponent`.
As shown in the code below, only the pipe in the template changes.

<code-example path="pipes/src/app/flying-heroes-impure.component.html" header="src/app/flying-heroes-impure.component.html (excerpt)" region="template-flying-heroes"></code-example>

<div class="alert is-helpful">

  To confirm that the display updates as the user adds heroes, see the <live-example></live-example>.

</div>

{@a async-pipe}

## Unwrapping data from an observable

[Observables](/guide/glossary#observable "Definition of observable") let you pass messages between parts of your application.
Observables are recommended for event handling, asynchronous programming, and handling multiple values.
Observables can deliver single or multiple values of any type, either synchronously (as a function delivers a value to its caller) or asynchronously on a schedule. 

<div class="alert is-helpful">

For details and examples of observables, see the [Observables Overview](/guide/observables#using-observables-to-pass-values "Using observables to pass values"").

</div>

Use the built-in [`AsyncPipe`](/api/common/AsyncPipe "API description of AsyncPipe") to accept an observable as input and subscribe to the input automatically.
Without this pipe, your component code would have to subscribe to the observable to consume its values, extract the resolved values, expose them for binding, and unsubscribe when the observable is destroyed in order to prevent memory leaks. `AsyncPipe` is an impure pipe that saves boilerplate code in your component to maintain the subscription and keep delivering values from that observable as they arrive.

The following code example binds an observable of message strings
(`message$`) to a view with the `async` pipe.

<code-example path="pipes/src/app/hero-async-message.component.ts" header="src/app/hero-async-message.component.ts">

</code-example>

{@a no-filter-pipe}

## Caching HTTP requests

To [communicate with backend services using HTTP](/guide/http "Communicating with backend services using HTTP"), the `HttpClient` service uses observables and offers the `HTTPClient.get()` method to fetch data from a server.
The aynchronous method sends an HTTP request, and returns an observable that emits the requested data for the response.

As shown in the previous section, you can use the impure `AsyncPipe` to accept an observable as input and subscribe to the input automatically.
You can also create an impure pipe to make and cache an HTTP request.

Impure pipes are called whenever change detection runs for a component, which could be every few milliseconds for `CheckAlways`.
To avoid performance problems, call the server only when the requested URL changes, as shown in the following example, and use the pipe to cache the server response.
The tabs show the following:

* The `fetch` pipe (`fetch-json.pipe.ts`).
* A harness component (`hero-list.component.ts`) for demonstrating the request, using a template that defines two bindings to the pipe requesting the heroes from the `heroes.json` file. The second binding chains the `fetch` pipe with the built-in `JsonPipe` to display the same hero data in JSON format.

<code-tabs>
  <code-pane
    header="src/app/fetch-json.pipe.ts"
    path="pipes/src/app/fetch-json.pipe.ts">
  </code-pane>
  <code-pane
    header="src/app/hero-list.component.ts"
    path="pipes/src/app/hero-list.component.ts">
  </code-pane>
</code-tabs>

In the above example, a breakpoint on the pipe's request for data shows the following:

* Each binding gets its own pipe instance.
* Each pipe instance caches its own URL and data and calls the server only once.

The `fetch` and `fetch-json` pipes display the heroes as shown in Figure 5.

<div class="lightbox">
  <img src='generated/images/guide/pipes/hero-list.png' alt="Hero List">
</div>

**Figure 5.** The `fetch` and `fetch-json` pipes displaying the heroes

<div class="alert is-helpful">

The built-in [JsonPipe](api/common/JsonPipe "API description for JsonPipe") provides a way to diagnose a mysteriously failing data binding or to inspect an object for future binding.

</div>
