# Coding Rules

### 1\. Never pass Container into class constructors. Pass all dependencies separately.

```php
<?php
namespace Espo\Modules\MyModule;

use Espo\Core\{
    ORM\EntityManager
    Utils\Metadata,
};

class MyClass
{
    protected $entityManager;    
    protected $metadata;

    public function __constructor(EntityManager $entityManager, Metadata $metadata)
    {
        $this->entityManager = $entityManager;
        $this->metadata = $metadata;
    }
}
```

### 2\. Never write RAW SQL query. Use ORM instead.

```php
<?php

$account = $this->entityManager->getRepository('Account')
    ->select(['id', 'name'])
    ->where([
        'type' => 'Customer',
    ])
    ->order('createdAt')
    ->findOne();
```

### 3\. Specify type hinting for method parameters. Also specify a method return type unless it's supposed to be void.

Parameter types:

```php
<?php
class MyClass
{
    public function myMethod(Request $request, string $text)
    {
    }
}
```

Return type:

```php
<?php
class MyClass
{
    public function getAsString() : ?string
    {
        // ...
    }
}
```

### 4\. Never return TRUE or FALSE to indicate a success or failure. Throw an exception if failure, return NULL for empty result.

```php
<?php

use Espo\Core\Exceptions\Error;

class MyClass
{
    public function getAsString() : ?string
    {
        if ($this->isSomethingWrong()) {
            throw new Error("Something is wrong.");
        }
        
        if ($this->hasNoData()) {
            return null;
        }
        
        return $this->value;
    }
}
```

### 5\. Don't write comments, write self explanatory code.

In addition:

* Do not write *phpdoc* for non-public methods and properties. It's OK if you don't write *phpdoc* at all.
* Do not comment out a code. Remove it. It will still be available in *git*. 

Bad:

```php
    /**
     * Sanitize input.
     *
     * @param string $input Input string.
     * @return string Sanitized string.
     */
    protected function sanitizeInput(string $input) : string
    {
        // sanitize
        return Util::sanitize($input);
    }
```

Good:

```php
    protected function sanitizeInput(string $input) : string
    {
        return Util::sanitize($input);
    }
```

Bad:

```php
// start sanitizing
$string = filter_var($string, FILTER_SANITIZE_STRING, FILTER_FLAG_STRIP_HIGH);
// end sanitizing
```

Good:
```php
$string = $this->sanitizeString($string);
```
