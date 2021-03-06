.. sectionauthor:: Артём Светлов <@nextgis.ru>

.. sxf:

Работа стека NextGIS с форматом sxf
=====================================

Введение
----------------------------

В этой инструкции мы возьмём геоданные в формате sxf, загрузим их в NextGIS Web, 
настроим их стиль отображения и отобразим их на веб-карте. 
Для работы потребуется:

1. Набор данных в формате sxf - скачайте выгрузку из Openstreetmap на странице http://www.gisinfo.ru/price/price_map.htm
2. :program:`NextGIS QGIS`.
3. Доступ к инстансу :program:`NextGIS Web` - у вас должен быть его URL, логин и пароль.


Подготовка sxf к загрузке
----------------------------

Скачайте на странице http://www.gisinfo.ru/price/price_map.htm выгрузку данных OpenStreetMap 
на любой интересующий вас регион.

.. figure:: _static/sxfDownloadSample.png
   :name: howto_sxfDownloadSample
   :align: center
   :width: 15cm


Распакуйте их в каталог.
Поскольку :program:`NextGIS Web` сейчас поддерживает импорт только из форматов ESRI Shapefile и geojson, то нам нужно сконвертировать sxf в geojson. 

Запустите :program:`NextGIS QGIS`.

Нажмите :menuselection:`Слой --> Добавить слой --> Добавить векторный слой`.


.. figure:: _static/sxfQGISOpenLayerDialog.png
   :name: howto_sxfQGISOpenLayerDialog
   :align: center
   :width: 15cm

На экране появится диалог выбора слоёв из файла sxf. В этом примере мы загрузим слой железных дорог (Railway) и землепользования (landuse).

.. figure:: _static/sxfQGISOpenSXFLayers.png
   :name: howto_sxfQGISOpenSXFLayers
   :align: center
   :width: 15cm

Сохраните эти два слоя в формате geojson. Для этого, выделите слой landuse в списке слоёв и нажмите Нажмите :menuselection:`Слой --> Сохранить как.`

.. figure:: _static/sxfQGISSaveLayerMenu.png
   :name: howto_sxfQGISSaveLayerMenu
   :align: center
   :width: 15cm

В диалоге сохранения задайте следующие настройки:

1. Формат - geojson.
2. Система координат - EPSG:4326. Если её не будет в предложенном списке (такое бывает при первом запуске программы), то нажмите на кнопку рядом, и в окне поиска введите "4326".
3. Кодировка - если вы работаете под ОС :program:`Windows`, то выставьте UTF-8. Если вы работаете под ОС :program:`GNU/Linux`, то оставьте в нём System, по умолчанию.


.. figure:: _static/sxfQGISSaveDialog.png
   :name: howto_sxfQGISSaveDialog
   :align: center
   :width: 15cm

   Окно экспорта слоя.


.. figure:: _static/sxfQGISSearchProjection.png
   :name: howto_sxfQGISSearchProjection
   :align: center
   :width: 15cm

   Окно поиска проекции.

Сохраните слои как landuse.geojson и railway.geojson.


Особенность формата SXF - типы дорог там записаны числами. 

Эти числа - дробного типа. 

В картостиле дробные числа нельзя сравнивать оператором "=". 

Для того, чтобы написать логичный картостиль, нужно преобразовать числа в целочисленный тип.

Эту операцию можно сделать разными способами: 
в NextGIS QGIS используя калькулятор полей и создание новых слоёв, 
либо в текстовом редакторе удалить из geojson автозаменой все включения ".000000".


Загрузка в NextGIS Web
----------------------------------

Откройте в браузере имеющийся у вас адрес инстанса, введите логин и пароль. Вы попадёте в админку. При желании вы можете создать в ней каталог ("группу ресурсов"), чтобы работать с тестовыми данными в ней. Затем нажмите :guilabel:`Векторный слой`.

.. figure:: _static/sxfNGWMainPage.png
   :name: howto_sxfNGWMainPage
   :align: center
   :width: 15cm

   Главная страница админки.

Задайте название "Землепользование". Перейдите на вкладку :guilabel:`Векторный слой`, нажмите на кнопку :guilabel:`Выбрать`, и загрузите файл landuse.geojson, затем нажмите на кнопку :guilabel:`Создать`.


.. figure:: _static/sxfNGWLayerUpload1.png
   :name: howto_sxfNGWLayerUpload1
   :align: center
   :width: 15cm


.. figure:: _static/sxfNGWLayerUpload2.png
   :name: howto_sxfNGWLayerUpload2
   :align: center
   :width: 15cm


Стилизация слоя в NextGIS Web
----------------------------------

Сейчас вы загрузили слой в NGW. Для показа на веб-карте или раздачи по WMS к нему нужно добавить стиль. 

Зайдите в :program:`NextGIS QGIS`.

Зайдите в свойства слоя Landuse, который вы экспортировали. Настройте его стиль - с классификацией по полю SC_20013.

.. figure:: _static/sxfQGISLayerStyleSettings.png
   :name: howto_sxfQGISLayerStyleSettings
   :align: center
   :width: 15cm

Найдите снизу окна настроек слоя кнопку :menuselection:`Стиль --> Сохранить стиль --> Файлы стилей QGIS`. У вас сохранится стиль в формате qml.

.. figure:: _static/sxfQGISSaveQML.png
   :name: howto_sxfQGISSaveQML
   :align: center
   :width: 15cm

Вернитесь в браузер. 
Сейчас вы находитесь в админке внутри слоя (если занудно - в окне свойств слоя), поэтому нажмите :guilabel:`Стиль Mapserver`.

Задайте стилю имя "Землепользование". Перейдите на вкладку :guilabel:`Mapserver`.

Нажмите кнопку  :guilabel:`Import QGIS style` (над текстовым полем). Выберите файл qml. 
После подтверждения, нажмите на кнопку :guilabel:`Создать`.

Показ слоя на веб-карте в NextGIS Web
------------------------------------------

Перейдите в список ресурсов и нажмите :guilabel:`Веб-карта`. Задайте название и перейдите на вкладку :guilabel:`Слои`. Нажмите на кнопку :guilabel:`Добавить слой` и выберите в списке ресурсов 
слои тех слоёв, что вы создали.

Нажмите на кнопку :guilabel:`Создать`.

Нажмите на кнопку :guilabel:`Просмотр`.

После этого шага в экране браузера появится веб-карта. Включите слой. 


