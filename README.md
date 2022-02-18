# RussiaTravel.club.SO.widget
Проект предназначен для туристско-информационных центров и музеев, включенных в проект Серебряное ожерелье России.
Для использования данных через API, то есть без виджета, свяжитесь с нами по электронной почте <naito@naito-russia.ru>
Использование виджета и API для членов Партнерства ТИЦ бесплатно.

## Простая установка виджета

Чтобы использовать виджет на сайте, необходимо:
- Получить код сайта. ТИЦ могут получить код сайта бесплатно в Партнерстве ТИЦ.
- Выбрать страницу, на которой будет размещен виджет (страница Серебряного ожерелья) код виджета




Для установки модуля необходимо разместить на странице с размещаемым модулем следующий код:
```html
<div id="rtc-container" class="rtc-container"></div>
    <script src="https://russiatravel.club/apika/rtc-widjet.js?V1.01"></script>
    <script type="text/javascript">
      (function() {
          var init = function() {
              russiaTravelClubApi.init({
                container: '.rtc-container',
                v: 'V1',
                siteCode: '<my-site-code>'
              });
          };
          if (typeof russiaTravelClubApi !== 'undefined') {
              init();
          } else {
              (russiaTravelClubApiInitCallbacks = window.russiaTravelClubApiInitCallbacks || []).push(init);
          }
      })();
    </script>
```
my-site-code — параметр, выданный Партнерством ТИЦ

Скрипт может работать в 2х оформлениях:
- в квадратных блоках (s: s1)
- в виде списка размещенных блоков (s: s2)

Параметры вызова функции инициализации:
```javascript
 russiaTravelClubApi.init({
                  container: '.rtc-container', // обязательный параметр, контейнер, в котором размещаются данные виджета
                  v: 'V1', // обязательный парамерт, версия
                  siteCode: 'my-site-code' // обязательный параметр, ключ сайта, на котором установлен виджет. 
               
});
```
## Настройки вывода

Эта секция не обязательна и рассматривает варианты настройки виджета для пользователей

Параметры, используемые в функции инициализации russiaTravelClubApi.init

|параметр|обязательный|значение по умолчанию|описание|
|-----------|------------|------------|------------|
|container|да||Служит для определения контейнера, используемого для размещения данных|
|v|да||обязательный параметр, текущее значение V1|
|siteCode|да||ключ сайта, полученный от Партнерства ТИЦ|
|oneSpotFunction|нет||функция обработчик данных, полученных от сервера для создания собственных шаблонов вывода|



### Варианты кастомизации
возможно переопределение строки, выводящей информацию.
Для этого при вызове скрипта необходимо передать переопределяющую функцию
```
oneSpotFunction(item) {
}
```
Функция принимает один параметр item

Структура объекта item
|параметр|тип|описание|
|-----------|------------|------------|
|areaString|varchar(100)|федеральный округ|
|body|varchar(400)|текст спота|
|dt|date(yyyy-mm-dd hh:MM:ss)|дата создания / обновления спота|
|organisationName|varchar(150)|название организации, разместившей спот|
|placeType|museum или tic|тип организации, разместившей код|
|regionCode|varchar(36)|fias код региона|
|regionString|varchar(150)|название региона, в котором находится организация, разместившая спот|
|spotCode|varchar(60)|код спорта|
|title|varchar(150)|заголовок спота|
|url|varchar(1000)|ссылка на источник информации или более полную информацию|
|w400|varchar(100)|код файла с картинкой (картинка находится по адресу https://russiatravel.club/images/spots/<w400>|

Полный пример с использованием функции (обратите внимание, что скрипт использует свои стили, поэтому они определены отдельным файлом):
```html
<div id="rtc-container" class="rtc-container"></div>
    <script src="https://russiatravel.club/apika/rtc-widjet.js?V1.02"></script>
    <link rel="stylesheet" type="text/css" href="demo2.css">
    <script type="text/javascript">
        (function() {
            var init = function() {
                russiaTravelClubApi.init({
                  container: '.rtc-container',
                  v: 'V1',
                  siteCode: 'my-site-code',
                  'enableMasonry': false,
                  oneSpotFunction: (item, color) => {
                      // console.log(item);
                      var imgUrl = "https://russiatravel.club/images/spots/" + item["w400"] ;
                      return `<div class="list-post fl-wrap">
                                  <div class="list-post-media">
                                      <a href="post-single.html">
                                          <div class="bg-wrap">
                                              <div class="bg" data-bg="` + imgUrl + `"
                                                  style="background-image: url(&quot;` + imgUrl + `&quot;);"></div>
                                          </div>
                                      </a>
                                      <span class="post-media_title"></span>
                                  </div>
                                  <div class="list-post-content">
                                      <a class="post-category-marker" href="#">` + item['regionString'] + `</a>
                                      <h3><a href="post-single.html">` + item["title"] + `</a></h3>
                                      <span class="post-date"><i class="far fa-clock"></i>` + item['dt'] + `</span>
                                      <p>` + item['body'] + `</p>
                                      <ul class="post-opt">
                                          <li><i class="far fa-comments-alt"></i>  </li>
                                          <li><i class="fal fa-eye"></i>  </li>
                                      </ul>
                                      <div class="author-link"><a href="author-single.html"><img src="https://visit-museums.ru/assets/icons/musinfologo.png"
                                                  alt=""> <span>` + item['organisationName'] + `</span></a></div>
                                  </div>
                              </div>`;
                  }
                });
            };
            if (typeof russiaTravelClubApi !== 'undefined') {
                init();
            } else {
                (russiaTravelClubApiInitCallbacks = window.russiaTravelClubApiInitCallbacks || []).push(init);
            }
        })();
      </script>
```
