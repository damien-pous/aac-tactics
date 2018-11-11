# AAC tactics

[![Travis][travis-shield]][travis-link]
[![Contributing][contributing-shield]][contributing-link]
[![Code of Conduct][conduct-shield]][conduct-link]
[![Gitter][gitter-shield]][gitter-link]

[travis-shield]: https://travis-ci.com/coq-community/aac-tactics.svg?branch=master
[travis-link]: https://travis-ci.com/coq-community/aac-tactics/builds

[contributing-shield]: https://img.shields.io/badge/contributions-welcome-%23f7931e.svg
[contributing-link]: https://github.com/coq-community/manifesto/blob/master/CONTRIBUTING.md

[conduct-shield]: https://img.shields.io/badge/%E2%9D%A4-code%20of%20conduct-%23f15a24.svg
[conduct-link]: https://github.com/coq-community/manifesto/blob/master/CODE_OF_CONDUCT.md

[gitter-shield]: https://img.shields.io/badge/chat-on%20gitter-%23c1272d.svg
[gitter-link]: https://gitter.im/coq-community/Lobby

This Coq plugin provides tactics for rewriting universally quantified
equations, modulo associativity and commutativity of some given operator.

The implementation and underlying theory is decribed in the paper
[Tactics for Reasoning modulo AC in Coq](https://arxiv.org/abs/1106.4448).

## Meta

- Initial author(s): Thomas Braibant and Damien Pous
- Coq-community maintainer(s): Fabian Kunze
- License: [GNU Lesser General Public License v3](COPYING.LESSER)
- Compatible Coq versions: Coq 8.3 or greater

## Building and installation instructions

The easiest way to install the latest released version is via
[OPAM}(https://opam.ocaml.org/doc/Install.html):
```shell
opam repo add coq-released https://coq.inria.fr/opam/released
opam install coq-aac-tactics
```

To instead build and install the plugin manually, do:
```shell
git clone https://github.com/coq-community/aac-tactics
cd aac-tactics
make   # or make -j <number-of-cores-on-your-machine>
make install
```
Be sure to use the branch of the repository which corresponds
to the Coq version you are using.

## Documentation

After installation, definitions and tactics can be found under the `AAC_tactics` namespace.

The following example shows an application of the tactics for reasoning over Z binary numbers:
```coq
Require Import AAC_tactics.AAC.
Require AAC_tactics.Instances.
Require Import ZArith.

Section ZOpp.
  Import Instances.Z.
  Variables a b c : Z.  
  Hypothesis H: forall x, x + Z.opp x = 0.
  
  Goal a + b + c + Z.opp (c + a) = b.
    aac_rewrite H.
    aac_reflexivity.
  Qed.
End ZOpp.
```

The file [Tutorial.v](theories/Tutorial.v) provides a succinct introduction
and more examples of how to use this plugin.

The file [Instances.v](theories/Instances.v) defines several type class instances
for frequent use-cases of this plugin, that should allow you to use it off-the-shelf.
Namely, it contains instances for:

- Peano naturals	(`Import Instances.Peano`)
- Z binary numbers	(`Import Instances.Z`)
- N binary numbers	(`Import Instances.N`)
- P binary numbers	(`Import Instances.P)
- Rational numbers	(`Import Instances.Q`)
- Prop			(`Import Instances.Prop_ops`)
- Booleans		(`Import Instances.Bool`)
- Relations		(`Import Instances.Relations`)
- all of the above	(`Import Instances.All`)

To understand the inner workings of the tactics, please refer to
the `.mli` files as the main source of information on each `.ml` file.

## Acknowledgements

The initial authors are grateful to Evelyne Contejean, Hugo Herbelin,
Assia Mahboubi, and Matthieu Sozeau for highly instructive discussions.

The plugin took inspiration from the plugin tutorial "constructors" by
Matthieu Sozeau, distributed under the LGPL 2.1.
