<!SLIDE subsection transition=uncover>

# Модульный подход #

<!SLIDE transition=uncover>

# Один файл - один внешний объект #

<!SLIDE transition=uncover>

# Один внешний объект - один модуль #

<!SLIDE transition=uncover>

# Один модуль - одно пространство имён #

<!SLIDE transition=uncover>

`maths.js`:

    @@@javascript
    var privateFunc = function(...) { ... };
    exports.state = { '...': ...,
                      '...': ... }; 

    exports.add = function(...) { ... };
    exports.mul = function(...) { ... };
    exports.sqrt = function(...) { ... };
    exports.max = function(...) { ... };

<!SLIDE transition=uncover>

`my.js`:

    @@@javascript
    var maths = require('maths');
    var res = maths.add(...);

`external.js`:
    
    @@@javascript    
    var state = require('maths').state;
    
<!SLIDE transition=uncover>

.notes нет require, не асинхронный путь

Эмуляция в браузере:

    @@@javascript
    var maths = {};
    . . .
    maths.add = function(...) {...};
    maths.mul = function(...) {...};

    if (typeof module === "object") {
        module.exports = maths;
    } else if (typeof window === "object") {
        window.maths = maths;
    } else {
        throw new Error("Can't export maths library (no \"module\" nor \"window\" object detected).");
    }

<!SLIDE transition=uncover>

## "Паттерн" из CommonJS, используется в node.js ##

Если много модулей – используйте Require.js

<!SLIDE transition=uncover>

Пространства имён

    @@@javascript
    var namespace = namespace || {};

    (function( o ){  
        o.foo = "foo";  
        o.bar = function(){  
            return "bar";  
        };
        
        var private = function() { ... };
    })(namespace);  
    console.log(namespace); 

