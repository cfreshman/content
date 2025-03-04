---
title: <mfrac>
slug: Web/MathML/Element/mfrac
tags:
  - MathML
  - MathML Reference
  - MathML:Element
  - MathML:General Layout Schemata
browser-compat: mathml.elements.mfrac
---

{{MathMLRef}}

The MathML `<mfrac>` element is used to display fractions.  It can also be used
to mark up fraction-like objects such as
[binomial coefficients](https://en.wikipedia.org/wiki/Binomial_coefficient)
and [Legendre symbols](https://en.wikipedia.org/wiki/Legendre_symbol).

## Syntax

```html
<mfrac>numerator denominator</mfrac>
```

## Attributes

This element's attributes include the [global MathML attributes](/en-US/docs/Web/MathML/Global_attributes).

- `denomalign` {{deprecated_inline}}
  - : The alignment of the denominator under the fraction. Possible values are: `left`, `center` (default), and `right`.
    This attribute is deprecated and will be removed in the future. Use CSS [`text-align`](/en-US/docs/Web/CSS/text-align) instead.
- `linethickness`
  - : The thickness of the horizontal fraction line. This attributes accepts any [length values](/en-US/docs/Web/CSS/length).
    The values `medium`, `thin`, and `thick` are deprecated and will be removed in the future.
- `numalign` {{deprecated_inline}}
  - : The alignment of the numerator over the fraction. Possible values are: `left`, `center` (default), and `right`.
    This attribute is deprecated and will be removed in the future. Use CSS [`text-align`](/en-US/docs/Web/CSS/text-align) instead.

## Examples

### Simple fraction

The following MathML code should render as a fraction with numerator "a + 2" and
denominator "3 − b":

```html

<math display="block">
  <mfrac>
    <mrow>
      <mi>a</mi>
      <mo>+</mo>
      <mn>2</mn>
    </mrow>
    <mrow>
      <mn>3</mn>
      <mo>−</mo>
      <mi>b</mi>
    </mrow>
  </mfrac>
```

{{ EmbedLiveSample('simple_fraction', 700, 200, "", "") }}

### Fraction without bar

The following MathML code should render as a [binomial coefficient](https://en.wikipedia.org/wiki/Binomial_coefficient):

```html hidden
 <link
   rel="stylesheet"
   href="https://fred-wang.github.io/MathFonts/LatinModern/mathfonts.css"
  />
```

```html
<math display="block">
  <mrow>
    <mo>(</mo>
    <mfrac linethickness="0">
      <mi>n</mi>
      <mi>k</mi>
    </mfrac>
    <mo>)</mo>
  </mrow>
</math>
```

{{ EmbedLiveSample('Fraction_without_bar', 700, 200, "", "") }}

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}
