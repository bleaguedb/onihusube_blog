# ［翻訳］なぜそんなに確信が持てるのか？

## 前書き

この記事はC++標準化委員会の2019年12月公開の論文の1つ、Bjarne Stroustrupさんが書かれた「[P1962R0 How can you be so certain?](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2019/p1962r0.pdf)」という論文の和訳です。

この文章はC++標準化委員会における機能追加時の議論を念頭において書かれています。

私の英語力はひよこ以下なので訳の正確性には全く保証がありません。特に、細部のニュアンスの解釈は大いに間違っている可能性があります。



以下本文

## How can you be so certain? - Bjarne Stroustrup

私はJohn McPheeの本を読んでいてこの引用を見つけた。

>「これが真実だ」ではなく「だから私には、私が今見ていると思うものを見ているように思う」と言おう。  
> --- 世界最高の地質学者の一人であるDavid Loveの言葉より

少し複雑かもしれないが、考えさせられた。私はよく、一部の人々がどのように（彼らがそうであるように）その確信を得ているのか不思議に思っていたためだ。

### 我々はもっと良く考える必要がある

言語（およびライブラリ）拡張に関しては、根本的な問題の解決よりも技術的な詳細（どのキーワードが最もふさわしくないかや、機能がどのように実装されるべきかを決定するなど）を詰めるのに多くの時間を費やしているようだ。

確かに、我々は委員会初期の頃よりもはるかに優れた技術者集団となったが、大きな変更に関する議論がやや表面的なものになってはいないかと心配している。

いつの日か私は、査読済み学術論文 – それら論文の少なくとも一部は実証的な観察に基づいている – の厳密性が恋しくなるだろう。  
私は既に度々、ある機能や拡張についての利点と欠点を慎重に量る機会を失している。

たとえ完全な言語と標準ライブラリにまたがる懸念に十分に注意を払っていたとしても、我々のエビデンスへの計量が十分に徹底されており一貫しているかどうか、私は常々疑問に思っている。

- 「そう思う」は技術的な議論ではない
- 「私の会社ならやる」は決定的な議論ではない
- 「私はどうしてもそれを必要としている」は決定的な議論ではない
- 「他のモダンな言語にはそれがある」は決定的な議論ではない
- 「我々はそれを実装することができる」は必要な要件ではあるが、機能追加のための十分な理由ではない
- 「その部屋のほとんどの人はそれを気に入っていた」は十分な理由ではない

最後のポイントは反民主主義的に思えるかもしれないがそうではない。  
部屋とはどの部屋であるのか？そこにいた人々はC++標準化委員会メンバーの代表だったのか？そもそもC++のコミュニティだったのか？彼らはどんな人々だったか？それを気に入った理由は何だったのか？誰も反対しなかったのか？（反対した人がいたとしたら）その人はなぜ反対したのか？  
（そのような）部屋にいる人々は概して自分の意思でそこにおり、通常、拡張を支持する人々は現状維持を支持する人々よりもはるかに明瞭でやる気に満ち溢れている。

実際のところ、単純に票を合計して3:1の絶対多数があるかを見るのは間違っていると思う。

コンセンサスを得るにはこのような（できればこれ以上の）多数が必要であるが、それだけでは十分ではない。特に、部屋に十分に人数がおらず、そこにいる人々は週末で疲れている状態であり、主要なメンバーやベテランのメンバーが他の場所にいる場合などは十分ではない。

誰が何に対して強く反対しているのかを常に検討する価値がある。

強い支持者も強い反対者も必ずしも正しいとは限らない。時には強い感情が論理的な議論の欠如を覆い隠してしまうこともある。

我々は今後何十年も使用される言語を定義している。そのため、もう少し謙虚さが必要である。

あの頃を覚えているだろうか？

- 全ての関数をメンバ関数にすることが一般的だった？
- 仮想関数はクールだったため、全ての関数は仮想化されるべきだと多くの人が主張していた？
- publicメンバは時代遅れだったため、全てのデータメンバは隠蔽されるべきだと多くの人が主張していた？
- ガベージコレクタは不可欠だと思われていた？

私は今日のC++標準化委員会を構成する委員たちがこれらのような流行に乗ってしまったのではないかと疑っている。

今日、我々は多くの流行に囲まれているが、その中で何が長期的に有用で何が「単に流行っているだけ」なのかを判断するのは依然として困難である。そして、現在流行しているものに心を奪われるのは簡単なことだ。  
どの問題に解決する価値があり、それはどれほど流行に左右されるのか。

議論のスピードを変えろと主張しているわけではない。例えば、コンセプトは遅すぎたし、`<=>`（一貫比較）は早すぎたと思っている。

私は、我々がより組織的で慎重で一貫した推論を行うことを提案する。

### 提案

解決策を提案するよりも問題を指摘する方が簡単であろう。

瓶に詰められるような設計の「魔法の源」はなく、実際に使用されるための言語設計（これは我々が話していること）は単純な予測可能プロセスではない。  
「問題」とは何か、どの問題に対処が必要か、多くの解決策のうちどれが最善なのか、についての完全な合意を得ることはできない。

ただし、それら対応策毎に過去の対応において上手く行ったことや、さらなる対応を必要とするような長引く問題の原因などについてを学ばなければならない。

異なる提案にはそれぞれ異なる種類、異なる量の仕事が必要となる。

少なくとも議論・検討の初期段階では使用パターンとインターフェースに目を向けるべきであり、実装詳細にはあまり目を向けないでおくことを提案する。  
ユーザーインターフェースと使用モデルがクリーンであるならば、実装は何年（何十年）かかけて改善される傾向にある。
「どのように（実装される）？」よりも、「何が？」「なぜ？」（必要か）に集中する必要がある。

現在の技術の下では、最適なパフォーマンスよりも安定したインターフェースの方が重要である。標準は何十年にも渡って安定であることが要求される点で、多くのプロダクトと異なるためだ。

私は、最近のC++標準化委員会での議論は詳細を詰めるのに集中しすぎており、プログラマーに「どのように？」を明確にさせることで使用パターンを進化させ実装を改善する、ということを難しくさせているように感じている。

確かに、詳細を詰めより良い一般的なインターフェースの選択をすることは意思決定プロセスの問題ではなく設計の問題であるが、より良いインターフェースを選択することで実装の議論において可能なすべての実装詳細を列挙するのではなく、そのインターフェースのもとで可能ないくつかの代替案にまとめることができるので標準化がシンプルになる。

完全に分離された言語（ライブラリ）機能は存在せず（少なくとも現在は存在していないはず）、そのような機能間の相互作用は最も難しい問題の一つであるが、多くの場合は問題としても有用なものとしても過小評価されている。  
我々は、そのような他の言語機能や標準ライブラリコンポーネントとの相互作用を常に考慮しなければならない。特に、そのような機能が開発中である場合はこれはとても難しい。

このことは、C++に大きな改善が施された後、常に小さなクリーンアップと小さなサポート機能の追加が必要になる理由の一つである。  
大規模な機能の導入から結果として得られる教訓を初めに予想することは出来ない。  
可能なあらゆるニーズ（初めは予想されていないような）に対応するために、機能はあまり精巧なものにしてはならない。

まず、できない。次に、やろうとすれば、永遠にそれと付き合わなければならないほどの肥大化が生じる。

C++標準化委員会における「委員会による設計（より正確には委員の連合による設計）」プロセスでは一般に、表明されたすべてのニーズを包含するような設計に落ち着く傾向にあり、そのように設計された承認当初の機能は酷いもので、肥大化している。表明されたすべてのニーズはほとんどのC++プログラマにとって（直接・間接的にも）現実的で重要なものではないためだ。

我々は、最小限の機能からスタートしてフィードバックに基づいて機能を成長させていくべきだ。  
「最小限の機能」には、何が基本的で、何がクリーンで、何が不可欠であるのかに焦点を当てる必要がある。
オリジナルのUnixのことを考えてみてほしい（そして、現代の派生と比較してみてほしい）。

ある機能の、最初の最小限のコアから成熟したファシリティへの成長とは、一般化と他の言語・ライブラリの（すでに成長を終えた）ファシリティとの統合でなければならない。
パッチの上にパッチを重ね続けるような特殊ケースを追加するものであってはならない。  
そのようなパッチをしなければならない特殊ケースが発生したとしたら、初期設計に欠陥があったということだ。例えば、（C++標準完成間近のような）土壇場で行われる機能の「改善」が気がかりです。

設計において回避不能な不確実性にアプローチするために2つの基本的な方法がある

- 誰もが役に立つと感じるまで「改善」し続ける
- 原則的で基本的なものだけが残るまで機能を削ぎ落とす

（どちらを選んだとしても）これらのアプローチの後では、得られた経験により変更（プレリリース）と追加（リリース後にできるすべてのこと）の必要性が明らかになるだろう。私は疑いもなく2番目のアプローチを支持しており、これこそが原則とフィードバックに基づいた適切なエンジニアリングであると考えている。

以前のハッキングと政治を考える。標準化のプロセスには確かに妥協が必要だが、そうした妥協が単に合意が取れなかっただけであるとか機能の肥大化させただけ、となることが無いように保証しなければならない。

ある問題についてを検討するときは、我々は常に「誰が利益を得るか？」「どのように利益をもたらすか？」「その利益にはどれほど意義があるのか？」を明確にするようにしなければならない。例えば[P1700R0](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2019/p1700r0.html)のように。


また、「誰が新しい問題に苦しむことになるのか？」も明確かつ具体的であること。「平均的なユーザーは〜できない」という主張はどちらも欠けている。

提出された提案の利点は誰にとっても自明でもなければ明白でも無い。その問いに対する最初の回答を用意するのは提案者の仕事である（すべての提案にはコストがかかっていることに注意されたい。委員会の時間、実装、文書化、教育、古い実装に対する文書化処理など）。

通常、目標の設定は機能の詳細設計と実装よりもはるかに困難だ。我々は優れた技術者であり、一度目標が設定されれば必要な作業を行うための理論と経験を持ち合わせている。しかし残念なことに、我々はその目標を明確にしその合意を得ることが得意では無い。多くの場合、そのような合意は複雑な要件に帰着してしまう。

言語設計は製品開発では無く、我々には基本的な優先事項を決定するより高いレベルの管理者が存在していない。所属する会社がそのような事業を行なっている訳でも無い限り、我々の中でこのような（言語設計のような）職場経験を持つ人はほとんど居ない。ここ数年、そのような製品開発のアナロジーによって規格化を進めることが度を越しているように思う。

議論もデータもそれ自体では決定的では無い。我々は巧妙な議論で自分を騙すことに長け過ぎていて、データには常に解釈が必要である。学術文献が得意なことの一つは実験によるデータの解釈だ。

我々の経験は必然的に狭く、不完全だ。C++の世界はあまりに広大で、誰もがその全てを知ることはできない。にも関わらず、問題とその解決のスタイルは時間とともに変化していく。  
あなたの所属する会社や業界に対する認識は必ずしも正しく無いかもしれず、仮に正しかったとしてもC++コミュニティ全体にとっては決定的では無いかもしれない。
あなたのニーズを完全に満たそうとすることは、C++コミュニティ全体にとって害かもしれない。これは我々全員に当てはまる事だ。

いくつもの「完璧な」言語は失敗してきた、注意を怠ればC++もまた失敗するかもしれない。我々には柔軟さと責任感が必要だ。すなわち、我々の設計は世界が変化しても連動するようなものでなくてはならない（世界が変化した場合に、ではなく）。
重要な問題に正しく対処し、それが将来の機能改善を妨げないことを100％確約できないため、あらゆる設計にはリスクが伴うことを理解しなければならない。しかし、それが我々を麻痺させるものであってはならない。何もしないこともまた（良きにせよ悪しきにせよ）結果をもたらす。リスクを取ることは不可避であるが、それは意図的にリスクを考慮した上でのものにしなければならない。

完璧を主張していては進歩できない。機能することが分かっているものに基づいて慎重に初期設計を進め、あとから磨き上げる必要がある。これは、どこに行きたいかについてかなり明確な考えを持っている場合にのみ可能である。そのような大まかな展望が欠けていれば、拡張は単なるハッキングであり、パッチにパッチを重ねることになるだろう。現在稼働している大規模なシステムは、常にいくつかの小さなシステムの仕事の結果を受けている。

設計と改善について完全に自由な選択肢はない。
「世の中」には数十億行のコードがあり、数百万の教科書や人気のブログがあり、多くの古い知識は数百万の頭の中に保存されている。
また、新旧のファシリティが円滑に相互運用されるためには、既存の言語機能と型システムを尊重する必要がある。

言語が安定であることは特徴であると同時に設計上の重大な制約でもある。古いコードや設計アプローチが「進歩」の邪魔になると常にイライラするが、人々はコードが壊れることを**本当に**嫌います。
多くのコードは非標準の機能に依存している、もしくは機能が非常に曖昧であり間違いなくバグっている（コード、コンパイラ、または標準）ため、ある程度の破損は避けられない。

私は多くの人の態度を要約できる
- 「C++は複雑すぎる。もっと小さく、単純で、きれいにする必要がある。
- そして、この2つの機能を追加してほしい。
- そして、**何をしたとしても私のコードを壊さないで！**」

私もそう思うが、もちろんこれは不可能だ。ユーザーは古いバージョンをサポートするコンパイラを要求するため、機能の非推奨化でさえ実際には機能しなかった。主要な機能を廃止することは不可能であり、小さな機能を廃止することは大した益のない面倒なことである。  
20年前のC++コードは今日でも実行できる、これはC++を利用する大きな理由でもある。その理由の1つは、今日書いたコードは20年後にも動作するという期待を抱かせるためだ。

互換性は常に過敏な問題である。言語自体が保証できる範囲を超えてシンプルさと正しさを両立するために、コーディングガイドラインと静的解析に注力することをお勧めする。[C++コアガイドライン](https://github.com/isocpp/CppCoreGuidelines/blob/master/CppCoreGuidelines.md)はそのいい例だと思っている。

我々は型安全でリソース安全なC++を書くことができ、そうすべきだ。
言語の進化はこの理想を支えるものでなくてはならない（「The Design and Evolution of C++.」や「 [Direction for ISO C++](https://wg21.link/p0939)」によって文書化されている）。  
既存の巨大なコードベースに型安全性とリソース安全性を確保するための手法や新機能を適用するのは非常に困難だが、ひどく互換性のないバージョンの言語を扱うよりもはるかに管理しやすい。

通常、実装経験は設計の改善に有効だが、実装作業は設計を凍結する傾向にあり、代替案や改善は無視されるかもしれない。一般に、基本的要件と原則が明確に記述され（できれば書面で）、主要なユースケースが選択される前に実装してしまうのは賢明ではない。

使用経験の報告は最も価値があるが、大規模に取得することは困難であり不可能だ。通常、我々は自分達で選んだ、（通常その道を極めたC++愛好家の）小さなグループの経験で間に合わせなければならない。ただし、実装経験と同様に（単一の小さくまとまったグループのみが関与する場合は特に）初期の経験報告は疑ってかかる必要がある。

「あと2つの機能だけ」を望むのは委員会のメンバーだけではない。300人以上のメンバーで構成される委員会があり、メンバーは皆基本的にC++に入れたい機能を1つか2つは持っていて、多くのメンバーは更にいくつか持っている。
あまりに多くの機能を追加してしまえば「C++が沈む」という意見を私は変えていない（[P0977R0 Remember the Vasa!](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2018/p0977r0.pdf)）。実際P0977を書いて以降、新しい提案は洪水のように増加していると思う。

我々はあまりにも急ぎすぎている。我々は多くのことをするか歩みを遅くすることができるが、両方を行いながら品質と一貫性を維持することはできはしない。我々はより抑制的で選択的にならなければならない。

全ての設計には長所、短所、および制限がある。可能性のある問題や代替案について真剣かつ誠実に議論しないまま設計を提示してはならない。可能性のある問題について調査するのは提案者の仕事の一つであり、「販売するだけの仕事」は知的に誠実ではない。

「対立陣営」の人々によって書かれたコルーチンに関する「賛否両方の立場からの論文」は非常に有益だった（[Coroutines: Use-cases and Trade-offs](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2019/p1493r0.pdf)、[Coroutines: Language and Implementation Impact ](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2019/p1492r0.pdf)）。