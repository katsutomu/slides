# Domain領域を守るための一つの手段

[@katsutomu](https://github.com/katsutomu)

---

### アジェンダ
- なぜ我々は設計をするのか
- 設計をする時にどのようなことを心がけるべきか
- 達成するための手段

---

### なぜ我々は設計をするのか
- ユーザーやクライアントの利益を最大化し不利益を最小化するため
- 開発チームの力を最大化するため
- 長期的に開発/運用を継続するため

---

### 現在から未来を通してプロダクトに関わる全てのステークホルダーのため

---

### 何を心がけるべきか

- ドメインを守る

---

### 何故ならば

- ステークホルダーの関心事の多くはドメインに詰まっている

---

### 故に

- ドメインを他の関心事から隔離し守ることが大事

---

### 例えば

---

## DDDの青ぼん
![DDD](https://i0.wp.com/www.ajlopez.com/images/articles/dddlayered.png)

---

## DDDのオレンジ本
![IDDD](https://cdn-ak.f.st-hatena.com/images/fotolife/d/dackdive/20170904/20170904055134.png)

---

## クリーンアーキテクチャ
![Clean](https://camo.qiitausercontent.com/aa4e4f88dd885f486d7ab736311f886a7538a1df/68747470733a2f2f71696974612d696d6167652d73746f72652e73332e616d617a6f6e6177732e636f6d2f302f36303435372f38666534633336362d343636652d653434632d613761362d6633613734336636393138332e706e67
)
---


## DDDの青ぼん
![DDD](https://i0.wp.com/www.ajlopez.com/images/articles/dddlayered.png)

---

## 旧来のアーキテクチャでの課題

- ドメインレイヤーがインフラレイヤーに依存している

---

## 達成するための一つの手段

- 依存性逆転の原則

---

```
class Hoge {
    val repository:HogeRepository
    init {
        repository = HogeRepository(ApiClient())
    }
    fun list() {
        repository.list()
    }
}
```

---

```
class HogeRepository @Inject constructor(private val apiClient: ApiClient)  {
    fun listById(id: Int): Observable<List<Coupon>> {
        return apiClient.list(id)
    }
}
```

---

### 図にすると

---?image=assets/befor.png

---

```
interface HogeRepositoryInterface {
  fun listById(id: Int): Observable<List<Coupon>>
}
```

---

```
class HogeRepositoryImpl constructor(private val apiClient: ApiClient) : HogeRepositoryInterface {
    fun listById(id: Int): Observable<List<Coupon>> {
        return apiClient.list(id)
    }
}
```

---

```
class Hoge constructor(private repository:HogeRepositoryInterface) {
    fun list() {
        repository.list()
    }
}
```


---

+++?image=assets/befor.png
<!-- .slide: data-background-transition="none" -->
+++?image=assets/after.png


---


### まとめ

- なぜ我々は設計をするのか
  - 現在から未来を通してプロダクトに関わる全てのステークホルダーのため
- 設計をする時にどのようなことを心がけるべきか
  - ドメイン領域を守る
- 達成するための一つの手段
  - 依存性逆転の原則

---

### しめ


---



