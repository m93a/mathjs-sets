# Mathematical sets in math.js
Important sets in mathematics can be ordered into a sequence:  
[ℕ](https://en.wikipedia.org/wiki/Natural_number) ⊂ [ℕ₀](https://en.wikipedia.org/wiki/Natural_number#Notation) ⊂ [ℤ](https://en.wikipedia.org/wiki/Integer) ⊂ [𝔻](https://en.wikipedia.org/wiki/Decimal#Decimal_fractions) ⊂ [ℚ](https://en.wikipedia.org/wiki/Rational_number) ⊂ [ℝ<sub>c</sub>](https://en.wikipedia.org/wiki/Computable_number) ⊂ [ℝ](https://en.wikipedia.org/wiki/Real_number) ⊂ [ℂ](https://en.wikipedia.org/wiki/Complex_number) ⊂ [ℍ](https://en.wikipedia.org/wiki/Quaternion)

## ℕ, ℕ₀ naturals (with zero)
 * not much to say about them
 * special case of more interesting ℤ

## ℤ integers: `BigInt`
 * they form an additive group
 * EcmaScript 2020 introduced the `BigInt` type which exactly matches ℤ [[1]](https://tc39.es/ecma262/#sec-ecmascript-language-types-bigint-type)
 * the important functionality can be polyfilled [[2]](https://github.com/peterolson/BigInteger.js)
 * math.js doesn't support operations on `BigInt` yet, see [#1096](https://github.com/josdejong/mathjs/issues/1096)

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

## ℝ<sub>c</sub> computables
 * [computable numbers](https://en.wikipedia.org/wiki/Computable_number) are the real numbers that can be computed to within any desired precision by a finite, terminating algorithm
 
 * notable subsets: [constructible numbers](https://en.wikipedia.org/wiki/Constructible_number) (q, √q) and [trigonometric numbers](https://en.wikipedia.org/wiki/Trigonometric_number) (sin qπ, cos qπ)
 
   * these can be represented as `FunctionNode` with parameters from (π times) ℚ
   * math.js doesn't currently exploit this, possible progress with [#1732](https://github.com/josdejong/mathjs/issues/1732)
 * in theory it's possible to add a `Computable` type to math.js
   * user would pass to it an algorithm which, given _N_, computes the number to _N_ decimal places
   * operations that support `BigNumber` could be extended to `Computable`: they would return another `Computable` which, given _N_, would _guesstimate_ the error propagation through the operation and demand the corresponding precission from its argument, then it would variate the argument's value a little and check whether the error propagation guess was correct; if the guess was correct and the result is in given bounds, it would return the result, else it would request the argument with higher precision
   * SymPy supports most of this functionality in its [`N` function](https://docs.sympy.org/latest/modules/evalf.html)

## ℝ reals
 * most of them are non-computable, so the set is fundamentally impossible to represent by a data type
 * some special cases of non-computable numbers can be represented at least symbolically, but the usefullness of them is dubious at best
 * example of a non-computable number: [∑2<sup>−BB(i)</sup>](https://math.stackexchange.com/a/462835/142487), where BB(i) is the [Busy Beaver function](https://en.wikipedia.org/wiki/Busy_beaver)

## ℂ complex numbers
 * a very small subset of them can be represented by the `Complex` type borrowed from [complex.js](https://github.com/infusion/Complex.js)
 * a somewhat larger set (complex decimals) could be represented when [#694](https://github.com/josdejong/mathjs/issues/694) is resolved
 * the largest representable set are the complex computables ℂ<sub>c</sub> = ℝ<sub>c</sub>×ℝ<sub>c</sub>

## ℍ quaternions
 * there is currently no datatype for quaternions, but there has been some progress towards it, see [#794](https://github.com/josdejong/mathjs/pull/794)
 * quaternions, octonions and higher [Cayley–Dickson algebras](https://en.wikipedia.org/wiki/Cayley%E2%80%93Dickson_construction), as well as various [Lie groups](https://en.wikipedia.org/wiki/Lie_group) could be represented symbolicaly once [#1732](https://github.com/josdejong/mathjs/issues/1732) is implemented


# Conclusion
We found that the biggest exactly representable set of numbers in math.js is ℤ, and that is only because JavaScript itself supports it. However, utilizing `BigInt`, math.js's fractions can be quite easily extended to cover ℚ. Covering all (complex) computables is doable and some progress has been made thanks to `BigNumber`, but there's still a lot of work left to do.
