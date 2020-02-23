# Mathematical sets in math.js
Important sets in mathematics can be ordered into a sequence:  
ℕ ⊂ ℕ₀ ⊂ ℤ ⊂ 𝔻 ⊂ ℚ ⊂ ℝ<sub>c</sub> ⊂ ℝ ⊂ ℂ ⊂ ℍ

## ℕ, ℕ₀ naturals (with zero)
 * not much to say about them
 * special case of more interesting ℤ

## ℤ integers: `BigInt`
 * they form an additive group
 * EcmaScript 2020 introduced the `BigInt` type which exactly matches ℤ [[1]](https://tc39.es/ecma262/#sec-ecmascript-language-types-bigint-type)
 * the important functionality can be polyfilled [[2]](https://github.com/peterolson/BigInteger.js)

## 𝔻 decimals: `BigNumber`
 * there is no standard notation for this set in mathematics, so I chose 𝔻
 * _d_ ∈ 𝔻 ⇔ ∃ _m_, _e_ ∈ ℤ: _d_ = _m_ · 10<i>ᵉ</i>
 * math.js uses the type `BigNumber` borrowed from [decimal.js](https://github.com/MikeMcl/decimal.js/)
   * strictly speaking, `BigNumber` has a hard limit on _m_ (up to 10<sup>10⁹</sup> [[3]](https://mikemcl.github.io/decimal.js/#precision)) and _e_ (up to 9·10<sup>15</sup> [[4]](https://mikemcl.github.io/decimal.js/#minE)), so it's not a cover 𝔻
   * `NaN` and ±∞ are also included in `BigNumber` [[5]](https://mikemcl.github.io/decimal.js/#isFinite)
 
## ℚ rationals: new `Fraction`
 * _a_ ∈ ℚ ⇔ ∃ _p_, _q_ ∈ ℤ: _a_ = <sup><i>p</i></sup>／<sub><i>q</i></sub>
 * math.js uses the type `Fraction` borrowed from [fraction.js](https://github.com/infusion/Fraction.js)
   * right now, `Fraction` has similar limits to `BigNumber`, but there are plans to fix it – once [#28](https://github.com/infusion/Fraction.js/issues/28) is fixed, `Fraction` will be able to represent any number from ℚ
   * `NaN` and ±∞ are also valid in `Fraction` [[6]](https://runkit.com/embed/kjq3h7w3txz1)
