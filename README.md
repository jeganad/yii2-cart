# Yii2 shopping cart 

This extension adds shopping cart for Yii framework 2.0

## Installation

The preferred way to install this extension is through [Composer](https://getcomposer.org/download/)

Either run

```
php composer.phar require devanych/yii2-cart "*"
```

or add

```
devanych/yii2-cart: "*"
```

to the ```require``` section of your ```composer.json``` file.

## Configuration

Configure the ```cart``` component (default values are shown):

```php
return [
    //...
    'components' => [
        //...
        'cart' => [
            'class' => 'devanych\cart\Cart',
            'storageClass' => 'devanych\cart\storage\SessionStorage',
            'calculatorClass' => 'devanych\cart\calculators\SimpleCalculator',
            'params' => [
                'key' => 'cart',
                'expire' => 604800,
                'productClass' => 'app\model\Product',
                'productFieldId' => 'id',
                'productFieldPrice' => 'price',
            ],
        ],
    ]
    //...
];
```

In addition to ```devanych\cart\storage\SessionStorage```, there is also ```devanych\cart\storage\CookieStorage``` and ```devanych\cart\storage\DbSessionStorage```. It is possible to create your own storage, you need to implement the interface ```devanych\cart\storage\StorageInterface```.

```DbSessionStorage``` uses ```SessionStorage``` for unauthorized users and database for authorized.

> If you use the ```devanych\cart\storage\DbSessionStorage``` as ```storageClass``` then you need to apply the following migration:

```
php yii migrate --migrationPath=@vendor/devanych/yii2-cart/migrations
```

```devanych\cart\calculators\SimpleCalculator``` produces the usual calculation of the total cost and total quantity of items in the cart. If you need to make a calculation with discounts or something else, you can create your own calculator by implementing the interface ```devanych\cart\calculators\CalculatorInterface```.

Setting up the ```params``` array: 

* ```key``` - For Session and Cookie.

* ```expire``` - Cookie life time.

* ```productClass``` - Product class is an ActiveRecord model.

* ```productFieldId``` - Name of the product model ```id``` field.

* ```productFieldPrice``` - Name of the product model ```price``` field.

## Usage

You can get the shopping cart component anywhere in the app using ```Yii::$app->cart```.

Using cart:

```php
// Product is an AR model
$product = Product::findOne(1);

// Get component of the cart
$cart = \Yii::$app->cart;

// Add an item to the cart
$cart->add($product, $quantity);

// Adding item quantity in the cart
$cart->plus($product->id, $quantity);

// Change item quantity in the cart
$cart->change($product->id, $quantity);

// Removes an items from the cart
$cart->remove($product->id);

// Removes all items from the cart
$cart->clear();

// Get all items from the cart
$cart->getItems();

// Get an item from the cart
$cart->getItem($product->id);

// Get ids array all items from the cart
$cart->getItemIds();

// Get total cost all items from the cart
$cart->getTotalCost();

// Get total count all items from the cart
$cart->getTotalCount();
```

Using cart items:

```php
// Product is an AR model
$product = Product::findOne(1);

// Get component of the cart
$cart = \Yii::$app->cart;

// Get an item from the cart
$item = $cart->getItem($product->id);

// Get the id of the item
$item->getId();

// Get the price of the item
$item->getPrice();

// Get the product, AR model
$item->getProduct();

// Get the cost of the item
$item->getCost();

// Get the quantity of the item
$item->getQuantity();

// Set the quantity of the item
$item->setQuantity($quantity);
```

> By using method ```getProduct()```, you have access to all the properties and methods of the product.

```php
$product = $item->getProduct();

echo $product->name;
```

## Testing

For testing, the ```codeception``` included in the Yii2 framework is used. In the configuration file ```codeception.yml``` in the root folder of the extension ```@vendor/devanych/yii2-cart``` set path to the configuration file ```test.php```. By default, this is the path to the file ```/config/test.php``` [yii2-app-basic](https://github.com/yiisoft/yii2-app-basic).

Configure the ```cart``` component in the ```test.php```

```php
return [
    //...
    'components' => [
        //...
        'cart' => [
            'class' => 'devanych\cart\Cart',
            'storageClass' => 'devanych\cart\tests\dummy\DummyStorage',
            'params' => [
                'productClass' => 'devanych\cart\tests\dummy\DummyProduct',
            ],
        ],
    ]
    //...
];
```
## Useful links

Article with a detailed description in Russian language: <https://zyubin.ru/frameworks/yii/rasshirenie-korzina-dlya-yii2.html>