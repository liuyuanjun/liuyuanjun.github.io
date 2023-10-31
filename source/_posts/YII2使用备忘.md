---
title: YII2使用备忘
date: 2023-03-17 12:03:10
updated: 2023-03-17 12:03:35
permalink: '/yii2-memo/'
tags: ['Yii2']
---

### Yii2 踩坑记录

#### 问题：Yii2 Model中的 `findOne` 和 `findAll`方法，当传入条件类似于 `['AND', ['id' => 1], ['>', 'num', 3]]` 这样时，会导致结果与预期不符，且不会报错

查找原因，这两个方法的 `$condition` 参数不可以与 `where` 方法的 `$condition` 视为等同，因为中间做了一层处理，代码如下：

```php
if (!ArrayHelper::isAssociative($condition) && !$condition instanceof ExpressionInterface) {
    // query by primary key
    $primaryKey = static::primaryKey();
    if (isset($primaryKey[0])) {
        // if condition is scalar, search for a single primary key, if it is array, search for multiple primary key values
        $condition = [$primaryKey[0] => is_array($condition) ? array_values($condition) : $condition];
    } else {
        throw new InvalidConfigException('"' . get_called_class() . '" must have a primary key.');
    }
}
```

其中 `ArrayHelper::isAssociative($condition)` 这句就是判断 `$condition` 是否是关联数组(associative array，相当于其它语言中的Map)，如果不是关联数组，会再做处理，问题就出在这里。
由于条件中的 `AND` 是没有String Key的，`ArrayHelper::isAssociative` 判断为 `false`，所以会把条件处理成 `['id' => ['AND', ['id' => 1], ['>', 'num', 3]]]` ，最终导致结果与预期不符。
