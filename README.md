# Cardanonical

<img src="./docs/assets/logo.webp" alt="Cardanonical"/>

A standardized JSON-schema to represent *any* Cardano-related objects. The schema covers objects found in blocks and transactions, from the Byron era onward.

## Overview

| schema                                                             | format |
| ---                                                                | ---    |
| [Raw schema](./cardano.json)                                       | JSON   |
| [Rendered schema](https://cardanosolutions.github.io/cardanonical) | HTML   |

## Rationale

The schema from this repository is the result of several years of experience dealing with Cardano objects and collecting feedback from builders through tools like [Ogmios](https://github.com/CardanoSolutions/ogmios). It is opinionated in a few ways, with choices that could be summarized as follows:

1. The schema and its sub-schemas try to be era-independent. That is, it tries to blend together objects from multiple Cardano era into one unified model. This is only reasonable because efforts are done for each Cardano upgrades to keep objects as backward and forward-compatible as possible. Transactions are divided into fields which are most entirely optional. While only later era may instantiate transactions carrying all fields, the schema still only describes one transaction model.

2. The previous point implies some _transformations_ and divergence compared to what one would expect from the binary format. For example, there's no MIR certificates in the schema. They are mapped to treasury withdrawal governance actions. These transformations are meant to reduce the cognitive load that comes with _undertanding the schema_ and dealing with evolutions of the chain. The schema present a final view on Cardano's objects, and thus make accomodations where needed.

3. There are few exceptions, but most exclusively around the Byron era. This is because the Byron era simply is _too different_ that some schema couldn't be unified without bringing extreme awkwardness.

4. The schema is _consistent_ in its naming. Things that represent the same concept are named the same. And things that represent different concept are named differently. This is true also between the Byron era and the others. So it is reasonable for one to expect the same sub-schema given a field name.

5. We avoid as much as possible key/value objects with arbitrary key unless they bring a real cost-saving benefit (e.g. for assets). Instead, we prefer lists with inlined fields. This is because of two main reasons: such dictionnaries are usually _ordered_ and JSON parsers do not usually preserve this ordering; and it makes parsing and processing of those datasets sometimes more cumbersome. Lists are much more universal.

6. As a means to keep the schema self-documented, we keep names full and avoid abbreviations. No `tx`, `pp`, `vk`, `pkh` or other acronyms either. The point of such schema isn't to be as compact as possible; If you need this, use a proper binary format (CBOR would be the only sensible choice given Cardano's landscape).

7. We use discriminated unions consistently where it is needed. That is, no object shall be ambiguous to parse and objects that represent sum-types shall always have a field to discriminate on.

8. Few integer values in the schema are unbounded (no `maximum` nor `minimum`). These are not oversight, but actual quantity that can grow unbounded, in particular when aggregated. Parsers are expected to use big numbers or equivalent for dealing with such unbounded quantities.

## License

 <p align="center"><a target="_blank" href="https://creativecommons.org/publicdomain/zero/1.0/?ref=chooser-v1">CC0 1.0 Universal<img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/cc.svg?ref=chooser-v1" alt=""><img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/zero.svg?ref=chooser-v1" alt=""></a></p>
