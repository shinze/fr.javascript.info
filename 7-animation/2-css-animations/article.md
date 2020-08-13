# Les animations en CSS

Les animations CSS permettent de simples animations sans JavaScript.

JavaScript peut être utilisé pour contrôler l’animation CSS et en faire quelque chose d’encore mieux avec peu de code.

## Les transitions CSS [#css-transition]

Le principe des transitions CSS est simple. On décrit une propriété et comment ses changements devrait être animés. Quand la propriété change, le navigateur fait l’animation.

Donc, on n’a besoin que de changer une propriété. Et la transition fluide est réalisée par le navigateur.

Par exempel, le code CSS ci-dessous anime les changements de `background-color` pendant 3 secondes :


```css
.animated {
  transition-property: background-color;
  transition-duration: 3s;
}
```

Maintenant si un élément porte l’attribut class `.animated`, chaque changement de la propriété `background-color` sera animé pendant 3 secondes.

Cliquez sur le bouton ci-dessous pour animer le fond.

```html run autorun height=60
<button id="color">Click me</button>

<style>
  #color {
    transition-property: background-color;
    transition-duration: 3s;
  }
</style>

<script>
  color.onclick = function() {
    this.style.backgroundColor = 'red';
  };
</script>
```

4 propriétés permettent de décrire les transitions CSS :

- `transition-property`
- `transition-duration`
- `transition-timing-function`
- `transition-delay`

Nous les présenterons après, pour le moment notons que la propriété générale `transition` nous permet de les déclarer ensemble dans cet ordre : `property duration timing-function delay` et ainsi animer plusieurs propriétés à la fois.

Par exemple, en cliquant sur ce bouton on anime à la fois `color` et `font-size`.

```html run height=80 autorun no-beautify
<button id="growing">Cliquez ici</button>

<style>
#growing {
*!*
  transition: font-size 3s, color 2s;
*/!*
}
</style>

<script>
growing.onclick = function() {
  this.style.fontSize = '36px';
  this.style.color = 'red';
};
</script>
```

Maintenant nous découvrons chacune de ces propriétés.

## transition-property

Avec `transition-property` on peut indiquer la liste des propriétés à animer, par exemple: `left`, `margin-left`, `height`, `color`.

Certaines propriétés ne peuvent être animées, mais [beaucoup](http://www.w3.org/TR/css3-transitions/#animatable-properties-) le sont. La valeurs `all` signifie «animer toutes les propriétés».

## transition-duration

`transition-duration` permet d’indiquer un délai *avant* que l’animation ne commence. Par exemple avec `transition-delay: 1s`, l’animation commence 1 seconde après le changement.

Les valeurs négatives sont possible. L’animation commence alors par son milieu. Par exemple si `transition-duration` est à `2s` et le délai (`transition-delay`) est à `-1s` alors l’animation dure 1 seconde est commence à la moitié.
