# PHP Cart

[![Build Status](https://img.shields.io/travis/riesenia/cart/master.svg?style=flat-square)](https://travis-ci.org/riesenia/cart)
[![Latest Version](https://img.shields.io/packagist/v/riesenia/cart.svg?style=flat-square)](https://packagist.org/packages/riesenia/cart)
[![Total Downloads](https://img.shields.io/packagist/dt/riesenia/cart.svg?style=flat-square)](https://packagist.org/packages/riesenia/cart)
[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE)

PHP library providing basic shopping cart functionality.

## Installation

Install the latest version using `composer require riesenia/cart`

Or add to your *composer.json* file as a requirement:

```json
{
    "require": {
        "riesenia/cart": "~1.0"
    }
}
```

## Usage

Constructor takes three configuration parameters:
 * context data that are passed to each added cart item (you can pass i.e. customer id to resolve custom price)
 * true when listing gross prices, false for net prices (see [nice explanation](http://makandracards.com/makandra/1505-invoices-how-to-properly-round-and-calculate-totals))
 * number of decimals for rounding

All of them can be set separately.

```php
use Riesenia\Cart\Cart; 

// default is (null, true, 2)
$cart = new Cart();

$cart->setContext(['customer_id' => $_SESSION['customer_id']]);
$cart->setPricesWithVat(false);
$cart->setRoundingDecimals(4);
```

### Manipulating cart items

Items can be accessed by their cart id (provided by *getCartId* method).

```php
// adding item to cart ($product class has to implement CartItemInterface)
$cart->addItem($product);

// set quantity of the item
$cart->addItem($anotherProduct, 3);

// when $product->getCartId() returns i.e. 'abc'
$cart->setItemQuantity('abc', 7);

// removing item
$cart->removeItem('abc');
```

### Getting totals

Cart works with *Decimal* class (see [litipk/php-bignumbers](https://github.com/Litipk/php-bignumbers/wiki/Decimal)).
You can access subtotal (without VAT), taxes (array of amounts for all rates) and total (subtotal + taxes).

```php
// item 1 [price: 1.00, tax rate: 10]
// item 2 [price: 2.00, tax rate: 20]

// 3.00
echo $cart->getSubtotal();

// 0.10
echo $cart->getTaxes()[10];

// 0.40
echo $cart->getTaxes()[20];

// 3.50
echo $cart->getTotal();
```

## Tests

You can run the unit tests with the following command:

    $ cd path/to/riesenia/cart
    $ composer install
    $ vendor/bin/phpspec run