<!SLIDE subsection transition=uncover>

# Функциональный стиль #

<!SLIDE bullets incremental transition=uncover>

.notes Вы спросите почему?.. Редко нужно создавать/копировать сложные объекты

Польза отказа от методов в пользу функций:

* Проще оперировать экземплярами
* Функция будет работать с любыми типами 
* И требовать от них только то, что нужно ей
* Можно вообще не наследовать, а создавать объекты на месте, копировать или оборачивать
* Вообще, можно относиться проще к объектам

<!SLIDE transition=uncover>

# Пример из Python #

    @@@python
    class SomeClass:

        def method(self, param1, param2):
            . . .

(обратите внимание на *self*)            

<!SLIDE transition=uncover>

.notes Без IDE можно забыть интерфейс, нужно их все держать в памяти, лишние классы, делать интерфейс для каждого отдельного метода довольно странно, обычно это наборы методов 



    @@@java
    public interface CanMeow { // МожетМяукать
    	void meow(); // мяукнуть
    }	

    public interface IsCat extends CanMeow { 
	                            // ЯвляетсяКошкой
        void pullTail(); // дёрнутьХвост
    }

    public void pullTail(IsCat cat) { ... }
    public void doMeow(CanMeow meowable) { ... }

Осьмикошка может мяукать, но у неё нет хвоста!

<!-- (It have no tail and it cannot be pulled) -->

<!SLIDE transition=uncover>

.notes Можно даже не проверять тип: при вызове выбросится исключение. Кстати, хотел использовать собоутку для утиной типизации, но забыл

# JavaScript #

    @@@javascript
    function doMeow(mayBeCanMeow) { // можетУмеетМяукать
    	if (!mayBeCanMeow.meow) { 
	    	// mayBeCanMeow.meow !== undefined
    		throw new Error('Не умеет мяукать :(');
    	}
    	mayBeCanMeow.meow();
    }

    doMeow({}); // Не умеет мяукать!
    doMeow(petya); // Не умеет мяукать!
    doMeow({'meow': function() 
                    { console.log('Мяу!'); } }); // Мяу!
    doMeow(octocat); // Мяу!
    doMeow(elephantWhoCanMeow); 
                     // слонУмеющийМяукать: Мяу!

<!SLIDE transition=uncover>

# Без методов #

    @@@javascript
    function pullTail(possiblyCat) { // можетКошка
        if (!possiblyCat.tail) { 
	        // possiblyCat.tail !== undefined
	        throw new Error('Нет хвоста :(');
	    }
        console.log((possiblyCat.name 
	                 ? withTail.name 
	                 : 'Неизвестный') +
                    ' кричит «Ай!»');
    }

    pullTail({}); // Нет хвоста!
    pullTail(petya); // Нет хвоста!
    pullTail(musya); // Муся кричит «Ай!»
    pullTail(elephant); // Слонишка кричит «Ай!»
    pullTail({'tail': 1 }); // Неизвестный кричит «Ай!»
    pullTail(octocat); // Нет хвоста!

<!SLIDE transition=uncover>

<!-- Египетская рука дёргает слона за хвост -->

![Слон говорит «Ай!»](elephant-says-ow.png)

<!SLIDE transition=uncover>

.notes Легко создать новый объект

# На практике: #
    
    @@@javascript
    var myChain = { head: null };
    var divOne = { type: 'div', 'next': null };
    var divTwo = { type: 'table', 'next': null };
    divOne.next = divTwo;
    myChain.head = divOne;

    function walk(chain, func) {
    	var cursor = chain.head;
    	while (cursor != null) {
    		func(cursor);
    		cursor = cursor.next;
    	}
    }

    walk(myChain, function(elm) 
    > 'div'               { console.log(elm.type) } );
    > 'table'

<!SLIDE transition=uncover>

.notes Легко создать новый объект

# На практике: #
    
    @@@javascript
    var divOne = { type: 'div', 'next': null };
    var divTwo = { type: 'table', 'next': null };
    divOne.next = divTwo;
    
    function walk(chain, func) {
    	var cursor = chain.head;
    	while (cursor != null) {
    		func(cursor);
    		cursor = cursor.next;
    	}
    }

    function init_chain(head) {
    	return { 'head': head };
    }

    walk(init_chain(divOne), function(elm) 
    > 'div'                  { console.log(elm.type) } );
    > 'table'    

<!SLIDE transition=uncover>

# Кратко! #

<!SLIDE bullets incremental transition=uncover>

# Создавать объекты на месте – нормальная практика #

<!SLIDE transition=uncover>

.notes По названиям параметров не всегда понятно, что от них ожидается

# Всегда документируйте функции! #

(помечайте описанные где-либо в документации типы или ожидания)

<!SLIDE transition=uncover>

.notes Про монады рассказывать не буду

## Если вы считаете, что пора сгруппировать функции для работы с объектами одного типа в одном месте, вы на верном пути! ##