# NAVODILA ZA DRUGO DOMAČO NALOGO

Naloga obsega razširitev sistema tipov jezika Lambda in dopolnitev dokaza o varnosti za razširjen sistem tipov.

## IZGRADNJA IN UPORABA JEZIKA LAMBDA

Jezik Lambda se zgradi tako kot v prvi domači nalogi. V tej domači nalogi uporabljamo zgolj neučakano operacijsko semantiko, zaradi česar način izvajanja ni več argument pri uporabi jezika. Zaradi preglednosti je izpis malih korakov izklopljen (kdor želi, lahko odkomentira primerne dele kode v `lambda.ml`).

Lambdo tako kot prej zgradite z `ocamlbuild lambda.native` oz. preko taska `Compile Lambda` in zaženete kot `./lambda.native ime_datoteke.lam` oz. preko taska `Lambda`.

## RAZŠIRITEV SISTEMA TIPOV

Jezik Lambda že vsebuje operacijsko (neučakano) operacijsko semantiko iz prejšnje domače naloge. Prav tako je sistem tipov že dopolnjen s konstruktoma `ProdTy` in `ListTy`, ki predstavljata tip produktov in seznamov.

Vaše naloge so:

1. Dopolnite funkcije v datoteki `infer.ml`.
2. V datoteko `error_example.lam` napišite primer, kjer sistem tipov vrne napako `Cannot solve bool list = (int * int) -> bool`.
3. V datoteko `typing_example.lam` napišite primer funkcije, za katero algoritem izračuna tip `('a -> ('b -> 'c)) -> (('a * 'b) -> 'c)`.

## DOPOLNITEV DOKAZA O VARNOSTI

Na predavanjih ste spoznali izrek o varnosti, sestavljen iz napredka in ohranitve.

Dopolniti morate dokaz za vse konstrukte, ki smo jih dodali v jezik pri razširitvi s pari in seznami. V repozitoriju že imate delno dopolnjeno datoteko `varnost.lean`, pri čemer morate dopolniti zgolj dokaza za `preservation` in `progress`, v dokazu lem o substituciji in šibitvi lahko pustite `sorry`.

** NALOGO LAHKO REŠITE SAMOSTOJNO ALI V DOKAZOVALNIKU LEAN **

V primeru, da ne boste dokazovali v LEANu, datoteke z dokazom prav tako dodajte v mapo `formalizacija`. Dokazi so zaželeni v markdownu (v ta namen imate že delno dopolnjeno datoteko `varnost.md`) ali LaTeXu, sprejete pa bodo tudi na roke napisane in skenirane rešitve (v tem primeru se prosim potrudite, da bo vaša pisava dovolj čitljiva).

Predpostavite lahko veljavnost leme o šibitvi (`Γ ⊦ e : A ⟹ Γ, x : B ⊦ e : A`).

# DODATNA NALOGA
Poskusite dokazati lemo o šibitvi. Premislite, kako bi popravili naš sistem, da bi jo lahko dokazali.
