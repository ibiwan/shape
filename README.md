# shape [![CircleCI](https://circleci.com/gh/leocavalcante/shape.svg?style=svg)](https://circleci.com/gh/leocavalcante/shape)
Run-time type checks against plain old PHP arrays.

```bash
composer require leocavalcante/shape dev-master
```

## Why

### Validate basic data structures

```php
use function Shape\shape;
use const Shape\int;

$pointShape = shape([
    'x' => int,
    'y' => int,
]);

$validPoint = ['x' => 1, 'y' => 2];

$pointShape($validPoint); // Shape

$invalidPoint = ['x' => 1, 'y' => 'two'];

$pointShape($invalidPoint); // TypeError: Key y passed to shape() must be of the type integer, string given
```

### Mimic tuples

```php
use const Shape\string;
use const Shape\int;
use function Shape\shape;

function string_int_tuple(array $tuple)
{
    // assert it is a (String, Int) tuple
    shape([string, int])($tuple);

    return true;
}

var_dump(string_int_tuple(['one', 1])); // bool(true)

var_dump(string_int_tuple(['one', 'two'])); // TypeError: Key 1 passed to shape() must be of the type integer, string given
```

### `shape()` function returns a Closure, so you can easily use it to assert vector arrays

```php
use const Shape\int;
use function Shape\shape;

$points = [
    ['x' => 1, 'y' => 2],
    ['x' => 3, 'y' => 4],
];

array_map(shape(['x' => int, 'y' => int]), $points);
```

## Inspiration

* [Hack / HHVM Shapes](https://docs.hhvm.com/hack/shapes/introduction)
* [Extremely Defensive PHP - Marco Pivetta](https://www.youtube.com/watch?v=8d2AtAGJPno)
