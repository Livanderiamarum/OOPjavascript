OOPjavascript
=============

javascript oop 

СОБСТВЕННО ЧУДО ФУНКЦИЯ:

```html
<script>
function Class( a, b ) {
 
    var description = a.isClass ? b : a;
    var superClass = a.isClass ? a : b;
    var constructor = function () {if ( this['__construct'] )this['__construct'].apply( this, arguments );};
    var proto = superClass ? superClass.prototype : Class.base ? Class.base.prototype : Object.prototype;
    description.prototype = Object.create( proto );
    var self = Object.create( description.prototype );
    description.call( self, description.prototype, self );
    constructor.prototype = self;
    constructor.isClass = true;
 
    return constructor;
}
</script>
```



Создание класса:
```html
<script>

vae Cat = new Class(function(){
	this.name = 'cat';
	this.age = 12;
});	

var cat = new Cat;

</script>
```


Конструктор описывается в свойстве   __construct   и вызывается каждый раз при создании экземпляров
```html
<script>

vae Cat = new Class(function(){
	this.name = '';
	this.__construct = function( name ){
		this.name = name;
	}
});	

var cat = new Cat('Барсик');

</script>
```

Наследование:
```html
<script>

vae Animal = new Class(function(){
	this.say = function(){ alert('im animal!'); };
});

// первым или вторым (как больше нравится) параметром передаем класс родитель
vae Cat = new Class( Animal, function(){ });	

var cat = new Cat;
cat.say() // im animal!

</script>
```


Перекрытие родительских СВОЙСТВ и доступ к ним:

```html
<script>

vae Animal = new Class(function(){
	this.name = 'animal';
});

// в функцию описатель передается ссылка на экземпляр родительского класса Animal, 
// который создается каждый раз при обьявлении дочернего класса и является общим 
// для всех сущностей этого дочернего класса
vae Cat = new Class(Animal, function( parent ){
	this.name = 'cat';
	this.say = function(){
		alert( this.name ); // cat
		alert( parent.name ); // animal
	}
});

</script>
```


Перекрытие родительских МЕТОДОВ и доступ к ним:

```html
<script>

vae Animal = new Class(function(){
	this.name = 'animal';
	this.say = function(){ alert( this.name ) };
});


vae Cat = new Class(Animal, function( parent ){
	this.name = 'cat';
	this.sayWithParent = function(){  parent.say()  };
	this.sayWithThis = function(){  parent.say.call(this)  };
});

</script>
```





И еще кое что, можно подключать для всех классов некий базовый класс
это некий аналог Object.prototype, только для классов,
BASE CLASS

```html
<script>

// подключаем базовый класс в Class.base
// include base class to Class.base
Class.base = new Class( function () {
	var self = this;

	this.isClass = true;
	this.toString = function () {
		return '[object Class]'
	};
	this.toLocaleString = function () {
		return '[object Class]'
	};
	this.hasOwnProperty = function ( key ) {
		var hasOwn = Object.prototype.hasOwnProperty;
		return  hasOwn.call( this ) || Object.getPrototypeOf( this );
	};
	this.forIn = function ( callback ) {
		for ( var key in this )
			if ( key.indexOf( '_' ) !== 0 && !Object.prototype.hasOwnProperty.call( self, key ) )
				if ( !callback.call( this, key ) === false )break

	};
	this.keysOf = function ( value ) {
		var findKeys = [];
		this.forIn( function ( key ) {
			if ( this[key] === value ) findKeys.push( key );
		} );
		return findKeys;
	};
} );

</script>
```




ENJOY