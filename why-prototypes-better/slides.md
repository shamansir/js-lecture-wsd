<!SLIDE subsection transition=uncover>

# Прототипное наследование #

<!SLIDE transition=uncover>

Например, такое...

![Осьмикошка](octocat.png)

(okutokettu)

<span class="legal-copy">Octocat Logo is the property of GitHub Inc.</a>

<!SLIDE transition=uncover>

.notes В природе такое встречается редко, но вот когда мы разрабатываем сложную систему, у нас часто появляются сложные компоненты 

Или вот такое...

![Собоутка](doggyduck.png)

(dogudakku)

<!SLIDE bullets incremental transition=uncover>

* Куда же его в втиснуть в иерархии?
* ![Странные животные в иерархии](strange-animals-in-hierarchy.png)
* Может и не надо?

<!SLIDE bullets incremental transition=uncover>

* Может лучше не наследовать, а *клонировать*?
* ![Иллюстрация клонирования](cloning-illustration.png)
* Клонировать, а потом *настраивать*
* ![Иллюстрация настройки](tuning-illustration.png)

<!SLIDE transition=uncover>

# Классов нет, есть только экземпляры #

![Экземпляры](just-instances.png)

<!SLIDE bullets incremental transition=uncover>

* Нет никаких иерархий
* Всегда есть образец
* Взяли кошку у соседей, клонировали, добавили щупальца
* ![Осьмикошка](making-octocat.png)
* Взяли собаку у соседей, клонировали, добавили клюв
* ![Собоутка](making-doggyduck.png)

<!SLIDE transition=uncover>

* Вася – клон Пети, но с другим именем
* Тумбочка – клон шкафа, но меньше по высоте
* Шкаф – клон тумбочки, но выше и с вешалками
* Диван – клон кресла, но шире и раздвигается

<!SLIDE transition=uncover>

# TIMTOWTDY #

(Есть больше одного способа сделать это)

<!SLIDE transition=uncover>

.notes Намеренно использую маленькую букву - это экземпляры

    @@@javascript
    var petya = Object.create(null); // или Object.create();
    petya.name = 'Петя';
    petya.greet = function() { console.log('Я – ' + this.name); }

    var vasya = Object.create(petya);
    vasya.name = 'Вася';
    vasya.greet();
    > 'Я – Вася'
    
<!SLIDE transition=uncover>

# Fork and change #

(UNIX, GitHub, ...)

[Вилка и гнутая вилка](fork-and-modified-fork.png)    

<!SLIDE transition=uncover>

.notes Фабрика кошек (но не осьмикошек, мы не планируем их наследовать, не будем преумножать сущности).

    @@@javascript
    var cat = function(subtype) {
    	return {
    		type: subtype || 'кошка',
    		identify: function() { // создаётся для каждого клона 
	    		console.log('Я – ' + this.type); }
    	};
    }

    var octocat = new cat('осьмикошка');
    octocat.identify();
    > 'Я – осьмикошка'

<!SLIDE transition=uncover>

## Не надо преумножать сущности ##

<!SLIDE transition=uncover> 

.notes Если мы хотим что-то запретить

    @@@javascript
    var cat = function(subtype) {
    	return {
    		...
    		fear: function() {console.log('ШШШШШШ!');}
    	};
    }

    var octocat = new cat('осьмикошка');
    octocat.fear = function() { throw new Error('Я не просто кошка!'); };
    // или octocat.fear = undefined;
    octocat.fear();
    > Error

<!SLIDE transition=uncover>

.notes Если мы всё же решили наследовать осьмикошек. Кстати, так работает instanceof

    @@@javascript
    function cat(type) {
        this.type = 'cat';
    }
    cat.prototype = {
      react: function(who) { // одна и та же функция для всех клонов
	           console.log(this.type + ' reacts on ' + who); }
    };

    function octocat() {}
    octocat.prototype = new cat();
    octocat.prototype.constructor = octocat; // вызвать cat.call(this, args) при создании

    octocat.prototype.type = 'octocat'; // не атрибут конструктора
    octocat.prototype.tentacles = 6;    

    var superoctocat = new octocat();
    superoctocat.tentacles = 8;
    superoctocat instanceof cat;
    > true
    superoctocat.type

<!SLIDE transition=uncover>

    superoctocat [instance of octocat]
        octocat.prototype [instance of cat] 
               { type: 'octocat' }
            cat.prototype
                { react: ... }
                Object.prototype
                       { toString: ... /* и т.д. */ }

<span class="legal-copy">Modified from http://bonsaiden.github.com/JavaScript-Garden/#object.prototype</span>                

<!SLIDE bullets incremental transition=uncover>

   Для работы с прототипами нужны:

   * Функция, которая конструирует объект
   * Прототип (клонируемое), который содержится в сконструированном объекте (или наполняется позже)
   * Она же становится конструктором

<!SLIDE transition=uncover>

.notes Программисты очень не хотят использовать имя родительского класса и заводят свои обёртки

Сложности начинаются, когда оказывается нужен `super`:

    @@@javascript
    octocat.prototype.react=function(who){ 
	    cat.prototype.react.call(this, who); // вспомните self
	    console.log('Перехвачено в octocat');
    }

<!SLIDE transition=uncover>

Но `super` не имеет смысла, если мы *клонируем*

<!SLIDE bullets incremental transition=uncover>

.notes Именно прикручивание ООП внесло в JS много костылей. Не забывайте оператор `new` и вам не нужно будет костылей

Кстати...

* функция – тоже объект с прототипом
* не забывайте оператор `new`
* `this` указывает на функцию, если не указано явно
* в первых двух примерах `instanceof` тоже не работает
* но зачем нам он вообще, если есть *Duck Typing*