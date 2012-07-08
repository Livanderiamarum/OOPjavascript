OOPjavascript
=============

javascript oop 

vae Cat = new Class(function(){
  this.name = 'cat';
  this.age = 12;
});

var cat = new Cat;






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