<!SLIDE subsection transition=uncover>

# Пояснения #

<!SLIDE bullets inclremental transition=uncover>

Что применять?

* В скриптах – функционально
* В системах / библиотеках – модульно и функционально
* В очень крупных приложениях – миксины или простейшая реализация ООП

<!SLIDE transition=uncover>

# Проще не значит путаннее #

(Если вы пишете документацию к функциям)

    @@@javascript
    // (other:Number[, yet_another:Number]) → Number
    // Returns the sum of the object's value with the given Number
    function add(other, yet_another) {
        return this.value + other + (yet_another || 0)
    }


<!SLIDE transition=uncover>

# Не плодите сущности #

(Библиотеки не стоят двух однострочных функций)

(ООП нужно вовсе не всегда)

<!SLIDE transition=uncover>

.notes позволяет не думать о непривычных вещах

# CoffeeScript - OK #

![CoffeeScript](coffeescript.png)

<!SLIDE transition=uncover>

# Dart - не попадёт! #

![Dart](dart.png)

<!SLIDE transition=uncover>

# Dart - не попадёт? #

![Dart](dart.png)

<!SLIDE transition=uncover>

## Спасибо, ваш shaman.sir ##

* http://shamansir.github.com
* shaman.sir@gmail.com
* Skype: shaman.sir
* twitter: @shaman_sir

<img src="../js-lecture-literature-qr.png" width="320px" height="320px" />

2011 ©
