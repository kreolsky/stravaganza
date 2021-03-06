# Использование тайловых карт

> [http://docs.godotengine.org/en/3.0/tutorials/2d/using_tilemaps.html](http://docs.godotengine.org/en/3.0/tutorials/2d/using_tilemaps.html) 

### Введение 

Использование тайловых карт - это простой и быстрый способ создания 2D уровней. Все сводится к тому, что вы взаимодействуете с набором одинаковых плиток (тайлов), которые могут быть помещены на графическую сетку столько раз, сколько нужно - представьте, что работаете в редакторе карт с набором изразцов:

![tilemap](/godot/tilemap/docs/tilemap.png)

К тайлам могут быть добавлены коллизии, что позволит расширить взаимодействие объектов с тайлами в игровом 2D мире.

### Создание тайла

В первую очередь нужно создать набор тайлов (tileset). Ниже представлены изображения, которые мы будем использовать. По соображениям оптимизации все они одинаковы (но могут принимать разную форму: прямоугольники, шестигранники, параллелограммы - прим. пер.). Благодаря "упаковке" мы можем собрать воедино несколько спрайтов и получить палитру тайлов.

![tilemap](/godot/tilemap/docs/tileset.png)

Создайте новый проект в Godot и переместите подготовленное изображение (в формате PNG) в каталог проекта. Затем, в окне импорта изображения, в настройках импорта, отключите `Filter`, чтобы в последствии избежать проблем с "артефактами". `Mipmaps` также должен быть отключен, если нет, отключите его.

Создадим ресурс `TileSet`. Хотя этот ресурс изначально содержит все свойства текстуры, довольно сложно каждый раз редактировать их. Вот как это будет выглядеть при просмотре настроек ресурса:

![tilemap](/godot/tilemap/docs/tileset_edit_resource.png)

Достаточно много свойств. Приложив усилия, редактирование ресурса таким способом определенно даст результаты, но зато сколько времени будет потрачено. Самый простой способ использовать и настраивать набор тайлов - это экспортировать его из специально созданной сцены!

### Сцена TileSet

Создайте новую сцену с `Node` или `Node2D`. Для каждого тайла, который вы хотите создать, добавьте узел спрайта в качестве дочернего элемента. Так как плитки имеют размер 50x50, вы должны включить сетку (`View -> Show Grid` или клавиша `G`) и включить привязку (иконка `Use Snap` или клавиша `S`). Перемещение тайлов с помощью мыши может быть неточным, поэтому используйте клавиши направлений.

Если в исходном изображении несколько тайлов, обязательно используйте свойство `Region Rect`, чтобы настроить, какая часть текстуры должна использоваться.

Наконец, убедитесь, что вы задали имя для спрайта. Это гарантирует, что при последующих изменениях в наборе тайлов (например, при добавлении коллизии, изменении области и т. д.), Тайл будет по-прежнему правильно идентифицироваться и обновляться. Это имя должно быть уникальным.

Большая часть требований, отражено на скриншоте ниже:

![tile_example](/godot/tilemap/docs/tile_example.png)

Продолжайте добавлять все тайлы, при необходимости корректируя смещения (если у вас несколько фрагментов в одном исходном изображении). Опять же, помните, что их имена должны быть уникальными.

![tile_example2](/godot/tilemap/docs/tile_example2.png)

### Коллизии

Чтобы добавить коллизии к тайлам, создайте дочерний элемент `StaticBody2D` для каждого спрайта. Это статический узел столкновения. Затем создайте `CollisionShape2D` или `CollisionPolygon` в качестве дочернего элемента `StaticBody2D`. Рекомендуется использовать `CollisionPolygon`, потому что его легче редактировать.

![tile_example3](/godot/tilemap/docs/tile_example3.png)

Наконец, осталось отредактировать `CollisionPolygon`, что позволит использовать коллизии и исправит значок предупреждения рядом с этим узлом. Не забудьте использовать привязку к сетке! Используя `Snap`, убедитесь, что узлы полигона столкновения выровнены правильно, позволяя персонажу беспрепятственно пройти вдоль плитки. Также не масштабируйте и не перемещайте узлы столкновений многоугольников. Следите чтобы значения, относительно родительского спрайта, были соблюдены: `Offset - 0,0`, `scale - 1,1` и `rotation - 0`.

![tile_example4](/godot/tilemap/docs/tile_example4.png)

Продолжайте добавлять коллизии к тайлам. Обратите внимание, что `BG` - всего лишь фон, поэтому бессмысленно использовать для него коллизии.

![tile_example5](/godot/tilemap/docs/tile_example5.png)

ОК! Все сделано! Не забудьте сохранить эту сцену для возможности редактирования в будущем. Назовите его `tileset_edit.scn` или как-то в этом роде.

### Экспорт TileSet

В созданной и открытой сцене следующим шагом будет создание набора тайлов. В меню `Scene` выберите `Scene> Convert To> Tile Set`:

![tileset_export](/godot/tilemap/docs/tileset_export.png)

Затем выберите имя файла, например, `mytiles.tres`. Убедитесь, что включена опция `Merge With Existing`. Таким образом, каждый раз, когда файл ресурса тайлов перезаписывается, существующие фрагменты объединяются и обновляются (на них ссылается их уникальное имя, поэтому правильно называйте свои тайлы).

![tileset_merge](/godot/tilemap/docs/tileset_merge.png)

### Использование набора тайлов в тайловой карте

Создайте новую сцену, используя Node или Node2d, а затем создайте `TileMap` в качестве дочернего элемента.

![tilemap_scene](/godot/tilemap/docs/tilemap_scene.png)

Перейдите в свойство `TileSet` этого узла и укажите созданный ранее набор тайлов:

![tileset_property](/godot/tilemap/docs/tileset_property.png)

Также установите размер ячеек `50` (это размер тайлов). Quadrant (квадрант) - это настраиваемое значение, которое указывает сколько плиток должен рисовать движок и выводить на видимую область экрана, по умолчанию оно равно `16х16`. Это значение изначально настроено правильно и не нуждается в изменении, но в определенных случаях, для точной настройки производительности можно прибегнуть к редактированию этого параметра (если вы знаете, что делаете).

### Картина вашего мира

Убедитесь, что выбран узел `TileMap`. На экране появится красная сетка, позволяющая рисовать на ней выбранный тайл, который расположены на палитре слева.

![tile_example6](/godot/tilemap/docs/tile_example6.png)

Чтобы избежать случайного перемещения и выбора узла `tilemap` (учитывая, что это огромный узел), рекомендуется заблокировать его, используя кнопку блокировки:

![tile_lock](/godot/tilemap/docs/tile_lock.png)

Если вы случайно разместите плитку где-то, где вы этого не хотите, вы можете удалить ее с помощью правой кнопки мыши в редакторе `tilemap`.

Вы также можете перевернуть и повернуть спрайты в редакторе `TileMap` (примечание: отражение спрайтов в `TileSet` не будет иметь должного эффекта). Значки в углу редактора позволяют "отзеркалить" и повернуть выбранный в настоящий момент тайл - вы также можете использовать клавиши A и S, чтобы отразить спрайт по горизонтали и по вертикали. Но для некоторых видов плиток отражение может быть удобной и компактной функцией.

Смещение и масштабирование артефактов

При использовании одной текстуры для всех фрагментов тайлового набора, скорее всего, приведет к фильтрации артефактов:

![tileset_filter](/godot/tilemap/docs/tileset_filter.png)

Это неизбежно, т.к. обусловлено спецификой работы аппаратного билинейного фильтра. Итак, чтобы избежать этой ситуации, есть несколько обходных решений. Попробуйте тот, который вас устроит:

* Отключить `Filter` и `Mipmaps` для текстуры плитки или всех текстур в окне настроек `Импорта изображений`.

* Включить `Pixel Snap` (установите значение `true`). `Project > Project Settings > Rendering > Quality > 2d > Use Pixel Snap`

* Часто помогает масштабирование посредством уменьшения карты через `Viewport`. Хорошей отправной точкой может послужить простое добавление 2D камеры, и с помощью включения `Current` и игры с параметром `Zoom` добиться нужного результата.

* Вы можете использовать отдельно взятое изображение для каждой плитки. Это позволит удалить все артефакты, но данный способ очень затратный по времени и менее оптимизирован.
