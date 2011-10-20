<!SLIDE subsection transition=uncover>

# Миксыны #

<!SLIDE transition=uncover>

# Ещё один способ клонирования: смешивание #

<!SLIDE transition=uncover>

.notes Добавляет свойства кнопки к любому объекту

Кнопка:

    @@@javascript
    var asButton = function() {
      this.hover = function(bool) {
        bool ? this.classes.push('hover') : this.classes.remove('hover');
      };
      this.press = function(bool) {
        bool ? this.classes.push('pressed') : this.classes.remove('pressed');
      };
      this.fire = function() {
        return this.action();
      };
      return this;
    };

<!SLIDE transition=uncover>

.notes Добавляет свойства овала к любому объекту

Овал:

    @@@javascript
	var asOval = function(options) {
	  this.area = function() { 
	    return Math.PI * this.longRadius * this.shortRadius;
	  };
	  this.ratio = function() {
	    return this.longRadius/this.shortRadius;
	  };
	  this.grow = function() {
	    this.shortRadius += (options.growBy/this.ratio());
	    this.longRadius += options.growBy;
	  };
	  this.shrink = function() {
	    this.shortRadius -= (options.shrinkBy/this.ratio());
	    this.longRadius -= options.shrinkBy;
	  };
	  return this;
	}

<!SLIDE transition=uncover>

Овальная кнопка:

    @@@javascript
	var OvalButton = function(longRadius, shortRadius, label, action) {
	  this.longRadius = longRadius;
	  this.shortRadius = shortRadius;
	  this.label = label;
	  this.action = action;
	};

<!SLIDE transition=uncover>

.notes при вызове функций `as...` функции клонируются, это не страшно, их можно кэшировать, подробности в литературе

Смешиваем:

    @@@javascript
	asButton.call(OvalButton.prototype);
	asOval.call(OvalButton.prototype, {growBy: 2, shrinkBy: 2});

	var myButton = new OvalButton(3, 2, 'Нажми!', function() {alert('Он нажал!')});
	myButton.area(); //18.84955592153876
	myButton.grow();
	myButton.area(); //52.35987755982988
	myButton.fire(); //'Он нажал!'

<span class="legal-copy">http://javascriptweblog.wordpress.com/2011/05/31/a-fresh-look-at-javascript-mixins/</span>


