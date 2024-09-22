[![Typing SVG](https://readme-typing-svg.herokuapp.com?color=%2336BCF7&lines=Экологическая+ОС)](https://git.io/typing-svg)

Снизу вы можете видеть как выглядит наша "Экологическая ОС"

![Screenshot_62](https://github.com/user-attachments/assets/013694a8-b970-49f1-ad1f-afb3749d5a56)

Разбор функциональности :

- Снизу в левом углу вы найдете кнопку пуск , при нажатии на неё у вас высветятся варианты : Чистая энергия , Как работает чистая энергия , Как мы можем помочь природе. Если нажать на варианты то высветятся окна которые можно перетаскивать зажимая на синюю полоску сверху окна.
- Снизу в правом углу имеется текущее время пк
- Нажимая на приложение "Настройки" вы можете изменить цвет рабочего стола , а также цвет таск-бара , результаты сохраняться в локальное хранилище.
- Нажимая на приложение "Изобретения предотвращающие ГП" у вас откроется окно с выбором двух методов выработки электричества , эти окна также можно перетаскивать.
- Окна на рабочем столе можно перетаскивать зажимая ПКМ.



**Разбор Самой ОС**

Впрочем , я считаю что мой сайт с интерфейсом ос - получился , мне нравиться то как я выполонил работу , потрачено много сил на создание . При выполнении бета-тестирования с моими друзьями - багов не обнаружено . 
На сайте есть интерактивные среды повествующие о чистой энергии , как её использовать , зачем она нужна , а также почему она чистая.

Желая отлично провести время , почитать информацию на моем сайте , и задуматься о проблеме ГП.


Расписание кода :

- Плавное изменение цвета рабочего стола :

  body {
    margin: 0;
    font-family: Arial, sans-serif;
    background-color: #f0f0f0;
    transition: background-color 0.5s;
}

также

.taskbar {
    position: fixed;
    bottom: 0;
    width: 100%;
    background-color: #0078d7;
    display: flex;
    align-items: center;
    padding: 10px;
    transition: background-color 0.5s;
}

- Перетаскивание объектов

В своем коде я использую "MakeDraggable" , если же разбирать код более подробно , ему присвоен следующий код :

function makeDraggable(headerElement) {
    const windowElement = headerElement.closest('.window');

    headerElement.addEventListener('mousedown', function(e) {
        let shiftX = e.clientX - windowElement.getBoundingClientRect().left;
        let shiftY = e.clientY - windowElement.getBoundingClientRect().top;

        function moveAt(pageX, pageY) {
            windowElement.style.left = pageX - shiftX + 'px';
            windowElement.style.top = pageY - shiftY + 'px';
        }

        function onMouseMove(event) {
            moveAt(event.pageX, event.pageY);
        }

        document.addEventListener('mousemove', onMouseMove);

        document.addEventListener('mouseup', function() {
            document.removeEventListener('mousemove', onMouseMove);
            document.removeEventListener('mouseup', arguments.callee);

            if (windowElement.id === 'app1' || windowElement.id === 'app2') {
                localStorage.setItem(windowElement.id + '-left', windowElement.style.left);
                localStorage.setItem(windowElement.id + '-top', windowElement.style.top);
            }
        }, {once: true});
    });

    headerElement.ondragstart = function() {
        return false;
    };
}

Также , есть еще один подобный код , но уже на изменение расположения иконок на рабочем столе , вот код :

function makeIconDraggable(icon) {
    icon.addEventListener('mousedown', function(e) {
        if (e.button === 2) {
            let shiftX = e.clientX - icon.getBoundingClientRect().left;
            let shiftY = e.clientY - icon.getBoundingClientRect().top;

            function moveAt(pageX, pageY) {
                icon.style.left = pageX - shiftX + 'px';
                icon.style.top = pageY - shiftY + 'px';
            }

            function onMouseMove(event) {
                moveAt(event.pageX, event.pageY);
            }

            document.addEventListener('mousemove', onMouseMove);

            document.addEventListener('mouseup', function() {
                document.removeEventListener('mousemove', onMouseMove);
                document.removeEventListener('mouseup', arguments.callee);

                localStorage.setItem(icon.id + '-left', icon.style.left);
                localStorage.setItem(icon.id + '-top', icon.style.top);
            }, {once: true});
        }
    });

    icon.ondragstart = function() {
        return false;
    };
}

- Сохранение в локальное хранилище

  В моем сайте я использую сохранение в локальное хранилище (LocalStorage) , для сохранения позиции иконок рабочего стола и его цвета , код я уже описывал выше , но сейчас я это объясню:

  Для сохранения позиции окон , мы используем                 localStorage.setItem(windowElement.id + '-left', windowElement.style.left);
                localStorage.setItem(windowElement.id + '-top', windowElement.style.top);
в функции makeDraggable , то же самое в makeIconDraggable.

Вот для позиций :

function loadIconPositions() {
    const icons = ['app1', 'app2'];
    icons.forEach(iconId => {
        const icon = document.getElementById(iconId);
        const left = localStorage.getItem(iconId + '-left');
        const top = localStorage.getItem(iconId + '-top');
        if (left && top) {
            icon.style.left = left;
            icon.style.top = top;
        } else {
            // Снизу если че , начальные значения (иконок на рабочем столе)
            if (iconId === 'app1') {
                icon.style.left = '50px';
                icon.style.top = '50px';
            } else if (iconId === 'app2') {
                icon.style.left = '250px';
                icon.style.top = '50px';
            }
        }
    });
}

У нас отображены в :

            if (iconId === 'app1') {
                icon.style.left = '50px';
                icon.style.top = '50px';
            } else if (iconId === 'app2') {
                icon.style.left = '250px';
                icon.style.top = '50px';
Это базовые значения положения наших иконок на рабочем столе . То есть если готовых позиций не найдено => применить изначальные настройки.

Ниже отображены настройки сохранения цветов рабочего стола :

function grape() {
    const color = document.getElementById('background-color').value;
    document.body.style.backgroundColor = color;
    localStorage.setItem('backgroundColor', color);
}

function watermelon() {
    const color = document.getElementById('system-color').value;
    document.querySelector('.taskbar').style.backgroundColor = color;
    document.getElementById('start-button').style.backgroundColor = color;
    localStorage.setItem('systemColor', color);
}

И еще одни для загрузки элементов во время перезагрузки/загрузки страницы :


        document.addEventListener('DOMContentLoaded', function() {
            const backgroundColor = localStorage.getItem('backgroundColor') || '#f0f0f0';
            const systemColor = localStorage.getItem('systemColor') || '#0078d7';

            document.body.style.backgroundColor = backgroundColor;
            document.querySelector('.taskbar').style.backgroundColor = systemColor;
            document.getElementById('start-button').style.backgroundColor = systemColor;

            const app1 = document.getElementById('app1');
            makeIconDraggable(app1);

            const app2 = document.getElementById('app2');
            makeIconDraggable(app2);

            loadIconPositions();

            updateTime();
            setInterval(updateTime, 1000);

            setTimeout(hideLoadingScreen, 10000);
        });
