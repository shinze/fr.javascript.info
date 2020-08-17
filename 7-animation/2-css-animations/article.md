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

Ici l’animation fait passer les chiffres de 0 à 9 en utilisant la propriété `translate` :

[codetabs src="digits"]

La propriété `transform` est animé de cette manière :

```css
#stripe.animate {
  transform: translate(-90%);
  transition-property: transform;
  transition-duration: 9s;
}
```

Dans l’exemple ci-dessus JavaScript ajoute l’attribut class `.animate` à l’élément -- et l’animation commence :

```js
stripe.classList.add('animate');
```

Nous pouvons aussi la commencer «à la moitié», à partir d’un chiffre exact, comme par exemple à la seconde qui correspond en utilisant une valeur négative appliquée à la propriété `transition-delay`.

Ici, si vous cliquez sur le chiffre -- ça lance l’animation à la seconde courante :

[codetabs src="digits-negative-delay"]

En JavaScript, c’est une ligne en plus :

```js
stripe.onclick = function() {
  let sec = new Date().getSeconds() % 10;
*!*
  // Ici, par exemple, -3s lance l’animation à partir de la 3e seconde
  stripe.style.transitionDelay = '-' + sec + 's';
*/!*
  stripe.classList.add('animate');
};
```

## transition-timing-function

Cette fonction décrit comment l’animation se distribue sur la durée. Est ce cette animation commence doucement pour enfin ralentir ou le contraire.

À première vue cette propriété semble compliquée. Mais devient très simple si vous lui accordez un peu de temps.

Cette propriété accepte deux types de valeurs : une courbe de Bézier ou des étapes. Commençons par la courbe qui est plus courante.

### La courbe de Bézier

La fonction temporelle peut être renseignée avec une [courbe de Bézier](/bezier-curve) ayant 4 poignées de contrôle remplissant les conditions suivantes :

1. Une première poignée : `(0,0)`;
2. Une dernière poignée: `(1,1)`;
3. Les poignées intermédiaires la valeur de `x` doit se situer dans l’intervalle `0..1`, `y` peut avoir n’importe quelle valeur.

La syntaxe des courbes de Bézier en CSS : `cubic-bezier(x2, y2, x3, y3)`. Ici, seuls les 2nd et 3e points sont spécifiés car le 1er est à `(0,0)` et le 4e à `(1,1)`.

La fonction temporelle décrit à quelle vitesse se déroule une animation dans le temps.

- L’axe `x` est le temps: `0` -- le début, `1` -- la fin.
- L’axe `y` indique la progression du processus : `0` -- la valeur de départ de la propriété, `1` -- la valeur finale.

La version la plus simple est quand l’animation se déroule uniformément, avec la même vitesse linéaire. Ce qui peut être fait avec la courbe `cubic-bezier(0, 0, 1, 1)`.

Voici à quoi ressemble cette courbe :

![](bezier-linear.svg)

… Comme il est possible de voir, c’est une simple ligne droite. Lorsque le temps (`x`) passe, l’animation se complète de manière régulière de `0` à `1`.

Le train illustré dans l’exemple ci-dessous va de gauche à droite à une vitesse constante (cliquez le) :

```css
.train {
  left: 0;
  transition: left 5s cubic-bezier(0, 0, 1, 1);
  /* JavaScript met la valeur de left à 450px */
}
```

… Et comment peut-on ralentir un train ?

The graph:

![](train-curve.svg)

Comme nous pouvons le constater, le déroulement est rapide : l’animation s’affole rapidement et va de plus en plus doucement.

Voici la fonction temporelle en action (cliquez sur le train) :

[codetabs src="train"]

CSS:
```css
.train {
  left: 0;
  transition: left 5s cubic-bezier(0, .5, .5, 1);
  /* JavaScript met la valeur de left à 450px */
}
```

Il y a plusieurs courbes par défaut : `linear`, `ease`, `ease-in`, `ease-out` et `ease-in-out`.

`linear` est un raccourcis pour `bezier-curve(0, 0, 1, 1)` -- une ligne droite, comme nous l’avons déjà vu.

Les autres valeurs sont des raccourcis pour les `cubic-bezier` suivantes :


| <code>ease</code><sup>*</sup> | <code>ease-in</code> | <code>ease-out</code> | <code>ease-in-out</code> |
|-------------------------------|----------------------|-----------------------|--------------------------|
| <code>(0.25, 0.1, 0.25, 1.0)</code> | <code>(0.42, 0, 1.0, 1.0)</code> | <code>(0, 0, 0.58, 1.0)</code> | <code>(0.42, 0, 0.58, 1.0)</code> |
| ![ease, figure](ease.svg) | ![ease-in, figure](ease-in.svg) | ![ease-out, figure](ease-out.svg) | ![ease-in-out, figure](ease-in-out.svg) |

`*` -- valeur par défaut, s’il n’y a pas de fonction temporelle c’est `ease` qui est utilisé.

Nous pourrions donc utiliser `ease-out` pour faire ralentir notre train.

