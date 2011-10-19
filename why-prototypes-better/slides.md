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

(There Is More Than One Way To Do It)

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

.notes Фабрика кошек (но не осьмикошек, мы не планируем их наследовать, не будем преумножать сущности).

    @@@javascript
    var cat = function(subtype) {
    	return {
    		type: subtype || 'кошка',
    		identify: function() {
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
    function cat() {
        this.type = 'cat';
    }
    cat.prototype = {
      react: function(who) { 
	           console.log(this.type + ' reacts on ' + who); }
    };

    function octocat() {}

    octocat.prototype = new cat();
    octocat.prototype.type = 'octocat'; // не атрибут конструктора
    octocat.prototype.tentacles = 6;
    octocat.prototype.constructor = octocat;

    var superoctocat = new octocat();
    superoctocat.tentacles = 8;
    superoctocat instanceof cat
    > true

<!SLIDE bullets incremental transition=uncover>

Кстати...

* В первых двух примерах `instanceof` не работает
* но зачем он, если есть *Duck Typing*