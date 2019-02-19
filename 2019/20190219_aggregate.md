# ［C++］集成体要件の変遷
集成体とは配列と条件を満たした幾つかの条件を満たしたクラス（union含む）の事で、集成体初期化を行えるものです。一様初期化構文の導入によってその他の初期化との見た目の差異が無くなりあまり意識されなくなったかもしれませんが、集成体初期化という初期化方式がなくなったわけではありません。

自前のクラスを集成体にして集成体初期化を行えるようにすることのメリットは、データメンバの初期化と参照のための煩わしい各種関数の定義をしなくて済むことです。これによって、コードとしての見た目が見やすくなり、取り扱いやすくなります。初期化と参照のための関数とは、コンストラクタ・代入演算子・getter/setterの事です。  
デメリットは、カプセル化を完全に破壊することです。なので、単にデータをまとめて可搬にするためだけの型に使用することが多いかと思います。

実はクラスが集成体になるための条件はC++11以降毎回少しづつ変化しているので、それをバージョン毎に見てみます。  
以下、staticメンバは関わってこないのでstaticメンバに関しては触れません。また、配列は特に変化がないのでクラスの条件のみを対象にしています。

### C++11
- ユーザー定義のコンストラクタを持たない
  - defaultやdelete指定された宣言はあってもok
- privateやprotectedなメンバ変数を持たない
- 仮想関数をメンバに持たない
- 継承していない
- メンバ変数が初期化されていない
  - デフォルトメンバ初期化子による初期化はng

これを満たせばC++11での集成体となれます（同時にC++14での要件も満たします）。注意点はメンバ変数が集成体である必要はないということです。

VisualStudio 2015同梱のcl.exeは部分的にC++17まで対応していますが、集成体の要件に関してはこのC++11止まりです。

### C++14
- ユーザー定義のコンストラクタを持たない
  - defaultやdelete指定された宣言はあってもok
- privateやprotectedなメンバ変数を持たない
- 仮想関数をメンバに持たない
- 継承していない

メンバ変数の初期化が解禁されました。これは疑問の余地のない当然の変更といえるでしょう。

### C++17
- ユーザー定義のコンストラクタ、explictコンストラクタ宣言、継承されたコンストラクタを持たない
  - explictでなければ、defaultやdelete指定された宣言はあってもok
- privateやprotectedなメンバ変数を持たない
- 仮想関数をメンバに持たない
- virtual, private, protectedな基底クラスを持たない

public継承に限って継承が許可されました。ただし、基底クラスのコンストラクタを継承してはいけません。しかし、基底クラスが集成体でなければならないわけではありません。  
単にpublic継承しただけでは、基底クラスのすべてのコンストラクタは隠蔽されています（言うなれば、コンストラクタは継承していません）。基底クラスのコンストラクタを使用可能にするにはusing宣言が必要です。

``` cpp
struct Base {
    Base() : m{10}
    {}

    Base(int n) : m{n}
    {}

    int m;
}

struct Derived : Base {
    //このusingによって基底クラスの全てのコンストラクタは継承される
    using Base::Base;
}

//call Base::Base(int), d.m == 30
Derived d{30};
```
継承されたコンストラクタを持たないとは、この様なusing宣言を行っていないことを意味します。

explictなコンストラクタの宣言があってはならないというのはどういうことでしょうか？ユーザー定義コンストラクタはそもそも書けないので同じことではないか？と思います。しかし、explicit defaultなコンストラクタの宣言は可能なのです。そしてその結果、集成体初期化の一貫性を破壊してしまうことになります。

``` cpp
```
これによる集成体初期化の一貫性破壊を防ぐために、explicit宣言コンストラクタを持つ場合は集成体となれない、となっているのです。

### C++20
※C++20はまだ発効前なのでこれはあくまで予定です。

- ユーザー宣言のコンストラクタ、継承されたコンストラクタを持たない
- privateやprotectedなメンバ変数を持たない
- 仮想関数をメンバに持たない
- virtual, private, protectedな基底クラスを持たない

defaultやdelete指定も含めて、あらゆるコンストラクタの宣言が禁止されました。

#### Designated initialization（指示付きの初期化子）

### 参考文献
- [N3337](https://timsong-cpp.github.io/cppwp/n3337/dcl.init.aggr)
- [N4140](https://timsong-cpp.github.io/cppwp/n4140/dcl.init.aggr)
- [N4659](https://timsong-cpp.github.io/cppwp/n4659/dcl.init.aggr)
- [p1008r1 Prohibit aggregate types with user-declared constructors](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2018/p1008r1.pdf)