# Izrek o varnosti za λ-račun

## Definicija jezika

### Sintaksa

Vzemimo λ-račun, ki se ujema s tistim v Leanu, torej brez celih števil in z enotskim tipom:

    e ::= x
        | ()
        | true
        | false
        | if e then e1 else e2
        | λx. e
        | e1 e2

### Operacijska semantika

Za vrednosti vzamemo:

    v ::= ()
        | true
        | false
        | λx. e

Operacijsko semantiko podamo z malimi koraki:

    e  ~>  e'
    -----------------------------------------------
    if e then e1 else e2  ~>  if e' then e1 else e2

    -------------------------------
    if true then e1 else e2  ~>  e1

    --------------------------------
    if false then e1 else e2  ~>  e2

    e1  ~>  e1'
    -----------------
    e1 e2  ~>  e1' e2

    e2  ~>  e2'
    -----------------
    v1 e2  ~>  v1 e2'

    -----------------------
    (λx. e) v  ~>  e[v / x]

### Tipi

Tipi so:

    A, B ::= unit
           | bool
           | A -> B

Pravila za določanje tipov pa so:

    (x : A) ∈ Γ
    -----------
    Γ ⊢ x : A

    -------------
    Γ ⊢ () : unit

    ---------------
    Γ ⊢ true : bool

    ----------------
    Γ ⊢ false : bool

    Γ ⊢ e : bool   Γ ⊢ e1 : A   Γ ⊢ e2 : A
    ---------------------------------------
    Γ ⊢ if e then e1 else e2 : A

    Γ, x : A ⊢ e : B
    ------------------
    Γ ⊢ λx. e : A -> B

    Γ ⊢ e1 : A -> B   Γ ⊢ e2 : A
    -----------------------------
    Γ ⊢ e1 e2 : B

## Izrek o varnosti

### Lema (o substituciji)

Če velja `Γ ⊢ e : A` in `Γ, x : A ⊢ e' : B` in , tedaj velja `Γ ⊢ e'[e / x] : B`.

#### Dokaz

Z indukcijo na izpeljavo `Γ, x : A ⊢ e' : B`.

### Trditev (napredek)

Če velja `⊢ e : A`, tedaj:
1. je `e` vrednost ali
2. obstaja `e'`, da velja `e ~> e'`.

#### Dokaz

Z indukcijo na predpostavko o določenem tipu.
Če je zaključek zadnjega uporabljenega pravila:

* `⊢ x : A`, imamo protislovje, saj je kontekst prazen.

* `⊢ () : unit`, imamo vrednost (1).

* `⊢ true : bool`, imamo vrednost (1).

* `⊢ false : bool`, imamo vrednost (1).

* `⊢ if e then e1 else e2 : A`, mora veljati `⊢ e : bool`.
    Po indukciji dobimo dva primera:
    1. `e` je vrednost, torej `()`, `true`, `false` ali `λx. e`.
    Ker velja `⊢ e : bool`, prva in zadnja možnost odpadeta.
    Če je `e = true`, velja `if e then e1 else e2 ~> e1`,
    če je `e = false`, velja `if e then e1 else e2 ~> e2`.
    2. Obstaja `e'`, da velja `e ~> e'`, zato velja tudi `if e then e1 else e2 ~> if e' then e1 else e2`.

    V vseh primerih izraz torej lahko naredi korak (2).

* `⊢ λx. e : A -> B`, imamo vrednost (1).

* `⊢ e1 e2 : B`, mora veljati `⊢ e1 : A -> B` in `⊢ e2 : A` za nek `A`.
    Po indukciji za `e1` dobimo dva primera:
    1. `e1` je vrednost `v1`. V tem primeru po indukciji za `e2` dobimo dva primera:
        1. Tudi `e2` je vrednost `v2`. Ker velja `⊢ e1 : A -> B`, mora veljati `e1 = λx. e` za neka `x` in `e`. Tedaj velja `e1 e2 = (λx. e) v2 ~> e[v2 / x]`.
        2. Obstaja `e2'`, da velja `e2 ~> e2'`, zato velja tudi `e1 e2 = v1 e2 ~> v1 e2'`.
    2. Obstaja `e1'`, da velja `e1 ~> e1'`, zato velja tudi `e1 e2 ~> e1' e2`.

    V vseh primerih izraz torej lahko naredi korak (2).

### Trditev (ohranitev)

Če velja `Γ ⊢ e : A` in `e ~> e'`, tedaj velja tudi `Γ ⊢ e' : A`.

#### Dokaz

Z indukcijo na predpostavko o koraku.
Če je zaključek zadnjega uporabljenega pravila:

* `if e then e1 else e2 ~> if e' then e1 else e2`, mora veljati `e ~> e'`,
  iz `Γ ⊢ if e then e1 else e2 : A` pa sledi
  `Γ ⊢ e : bool`, `Γ ⊢ e1 : A` in `Γ ⊢ e2 : A`.
  Po indukcijski predpostavki velja `Γ ⊢ e' : bool`, iz česar sledi tudi
  `Γ ⊢ if e' then e1 else e2 : A`.

* `if true then e1 else e2 ~> e1`,
  iz `Γ ⊢ if e then e1 else e2 : A` sledi `Γ ⊢ e1 : A`, kar želimo.

* `if false then e1 else e2 ~> e2`
  iz `Γ ⊢ if e then e1 else e2 : A` sledi `Γ ⊢ e2 : A`, kar želimo.

* `e1 e2 ~> e1' e2`, mora veljati `e1 ~> e1'`,
  iz `Γ ⊢ e1 e2 : A` pa sledi
  `Γ ⊢ e1 : B -> A` in `Γ ⊢ e2 : B` za nek `B`.
  Po indukcijski predpostavki velja `Γ ⊢ e1' : B -> A`, iz česar sledi tudi
  `Γ ⊢ e1' e2 : A`.

* `v1 e2 ~> v1 e2'`, mora veljati `e2 ~> e2'`,
  iz `Γ ⊢ v1 e2 : A` pa sledi
  `Γ ⊢ v1 : B -> A` in `Γ ⊢ e2 : B` za nek `B`.
  Po indukcijski predpostavki velja `Γ ⊢ e2' : B`, iz česar sledi tudi
  `Γ ⊢ v e2' : A`.

* `(λx. e) v ~> e[v / x]`,
  iz `Γ ⊢ (λx. e) v : A` sledi
  `Γ ⊢ (λx. e) : B -> A` in `Γ ⊢ v : B` za nek `B`.
  Iz prvega sledi `Γ, x : B ⊢ e : A`,
  z drugim pa prek leme o substituciji izpeljemo `Γ ⊢ e[v / x] : A`.
