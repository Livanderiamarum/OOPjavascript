OOPjavascript
=============

javascript oop 


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


Конструктор описывается в свойстве __construct и вызывается каждый раз при создании экземпляров
```html
<script>
vae Cat = new Class(function(){
	this.name = 'cat';
	this.age = 12;
	
	this.__construct = function( name, age ){
		this.name = name;
		this.age = age;
	}
});	

var cat = new Cat('Барсик', 12);
var cat2 = new Cat('Даша', 3);
</script>
```

Наследование:
```html
<script>
vae Animal = new Class(function(){
	this.say = function(){
		alert('im animal!');
	};
});


vae Cat = new Class(Animal, function(){
	this.name = 'cat';
	this.age = 12;
});	

var cat = new Cat('Барсик', 12);
cat.say() // im animal!
</script>
```

Перекрытие родительских СВОЙСТВ и доступ к ним:

```html
<script>
vae Animal = new Class(function(){
	this.name = 'animal';
});


vae Cat = new Class(Animal, function(parent){
	this.name = 'cat';
	
	alert( parent('name') );  // animal - parent property
});	
</script>
```


Перекрытие родительских МЕТОДОВ и доступ к ним:

```html
<script>
vae Animal = new Class(function(){
	this.name = 'animal';
	this.say = function(){
		alert( 'name' );
	};
});


vae Cat = new Class(Animal, function(parent){
	this.name = 'cat';
	
	parent('say')() // 'animal' -  with parent object
	parent('say', this)() // 'cat' -  with this object	
});	
</script>
```



Собственно сам pattern

```html
<script>
function Class( a, b ) {

	var description = a.isClass ? b : a;
	var superClass = a.isClass ? a : b;
	var constructor = function () {
		if ( this['__construct'] )this['__construct'].apply( this, arguments );
	};
	var create = Object.create || function ( proto ) {
		function Object() {
		}

		Object.prototype = proto;
		return new Object;
	};
	var proto = superClass ? superClass.prototype : Class.base ? Class.base.prototype : Object.prototype;
	description.prototype = create( proto );
	constructor.prototype = new description( parent );
	constructor.isClass = true;
	var prototype = description.prototype;

	function parent( key, context ) {
		if ( !key )return prototype;
		var value = prototype[key];
		if ( value instanceof Function ) return value.bind( context || prototype );
		return  value;
	}

	return constructor;
}	
</script>
```

ENJOY