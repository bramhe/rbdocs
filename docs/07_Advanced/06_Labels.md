# Labels

A Label is a bean with just a name property. You can generate a batch of labels of a certain type
using:

```php
$labels = R::dispenseLabels('meals',
	array('pizza', 'pasta', 'hamburger')
);
```

This will create three meal objects. Each bean will have a name property
that corresponds to one of the strings mentioned in the comma separated list.

You can also collect the strings from label beans using:

```php
$array = R::gatherLabels($meals);
```

The gatherLabels() function returns an alphabetically sorted array of strings each
containing one name property of a bean in the bean list provided.

# Cooker

The cooker is a tool to turn arrays (forms, XML, JSON) into beans.
An array has to contain a key named 'type' containing the type of bean it represents.
Further more an array can contain own-lists and shared-lists as nested arrays.

```php
$bandMemberArr = array(
	'type' => 'bandmember',
	'name' => 'Duke',
	'ownInstrument' => array(
		'type'=>'instrument',
		'name'=>'kazoo'
	)
);

$bandMemberBean = R::graph($bandMemberArr);
```

In this example we convert the array 'bandMemberArr' to a bean of type
'bandmember'. The bean can now be stored in the database.

```php
R::store($bandMemberBean);
```

If an array has a field with key 'id', the Cooker will try to load
the bean instead of dispensing a fresh bean. This means you can also update parts of beans.

The fact that the Cooker also automatically loads beans can become a security issue if
you don't verify ID integrity. From RedBeanPHP 3.4 on the Cooker will have an extra
safety setting; if you want to enable bean loading you need to invoke:

RedBean_Plugin_Cooker::enableBeanLoading(true);

first. Otherwise the Cooker will not load and/or update existing beans.

R::graph($array,**TRUE**) will ignore all beans that appear to be empty (You can use this if you build
forms; it makes it possible to add an empty form entry to add a new entity of something).
