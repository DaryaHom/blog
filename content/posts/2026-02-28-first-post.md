+++
date = '2026-02-28T23:42:06+03:00'
draft = false
tags = ['hugo', 'blog']
title = 'Пошаговое создание блога'
+++

Я пишу на go без малого 3 года, что не мешает мне уверенно наступать на одни и те же грабли и искренне удивляться, перечитывая документацию языка.  
Большинство моих коллег имеет обширные чертоги памяти и не испытывает подобных проблем.  
Но если вы лох дырявый, как я, то лучше, конечно, всё записывать. Причём сразу в несколько мест, на случай, если одно сломается, а другое потеряется.  
В общем, долго не думая, я решила завести блог с заметками.  


Основные требования, которым должен отвечать мой блог: 
- он должен не требовать финансовых вложений (вот такая я жадина)
- быть простым для создания и поддержки
- иметь симпатичный дизайн или возможность быстро его настроить


Оказалось, для создания блога достаточно определиться с трёмя компонентами: 
1. **Генератор сайта.** Я выбрала [hugo](https://github.com/gohugoio/hugo), по нескольким причинам:
	- о нём пишут из каждого утюга, стоит только набрать *"static site generator"*;
	- один уважаемый коллега настроил на нём сборку своего весьма [симпатичного блога](https://borodyadka.wtf/about/);
	- hugo написан на go и имеет открытый исходный код с 86к звёзд. С большой вероятностью это значит, что он современный и широко поддерживаемый.  
2. **Формат контента.** Я уже пользуюсь Obsidian, поэтому Markdown — привычный вариант для ведения заметок. Кроме того, он простой, как карандаш, для меня это чуть ли не самый важный аргумент. 
3. **Хостинг.** Тут тоже всё просто — у github есть бесплатный [GitHub Pages](https://docs.github.com/en/pages/quickstart) для демонстрации работы проектов, ведения блогов и размещения резюме.  


Сразу скажу, у меня не всё получалось с первого раза, ибо криворукость никто не отменял. Но неправильные команды и настройки постараюсь не прилагать =)

Итак, приступим.  
Для начала я установила расширенную версии hugo на мою безыдейную Ubuntu Jammy Jellyfish.
```sh
CGO_ENABLED=1 go install -tags extended,withdeploy github.com/gohugoio/hugo@latest
```

> *На самом деле, изначально я подумала, что самая умная, и установила обычную версию. Но нужна именно расширенная, почему — будет понятно ниже `-_-`*

Затем создала директорию для блога:
```sh
hugo new site blog
```

А потом уже сам hugo посоветовал, что надо сделать: 
```txt
Congratulations! Your new Hugo site was created in /home/daria/blog.

Just a few more steps...

1. Change the current directory to /home/daria/blog.
2. Create or install a theme:
   - Create a new theme with the command "hugo new theme <THEMENAME>"
   - Or, install a theme from https://themes.gohugo.io/
3. Edit hugo.toml, setting the "theme" property to the theme name.
4. Create new content with the command "hugo new content <SECTIONNAME>/<FILENAME>.<FORMAT>".
5. Start the embedded web server with the command "hugo server --buildDrafts".

See documentation at https://gohugo.io/.
```

Значит, нужно перейти в текущую директорию блога и выбрать тему оформления.  


У hugo большое количество [тем](https://themes.gohugo.io/), даже слишком большое. На выбор из этого многообразия у меня ушло довольно много времени.  
И хотя мимо темы с названием [Anatole](https://github.com/lxndrblz/anatole) сложно было пройти, в итоге всё равно победила [m10c](https://github.com/vaga/hugo-theme-m10c) со своей лаконичностью и приятной палитрой из коробки. 
```sh
cd blog
git submodule add https://github.com/vaga/hugo-theme-m10c.git themes/m10c # добавляем тему, как сабмодуль
```
Я впервые в своей жизни увидела сабмодуль. Если коротко: он позволяет хранить ссылку на чужой репозиторий внутри твоего, не копируя весь его код к себе. Это удобно для обновлений темы.

> *Можно, конечно, просто склонировать репозиторий темы (именно так я сначала и сделала, я же самая умная, помните, да? `-_-`), но при его добавлении в индекс гит всё равно поругается на вложенный репозиторий и попросит удалить его либо создать сабмодуль.* 

Потом я создала свой первый пост:
```sh
hugo new content content/posts/about.md
```
Получила радостное `Content "/home/daria/blog/content/posts/about.md" created`.  
Уря, мой первый пост!  


Внутри он выглядел так: 
```txt
+++
date = '2026-02-14T19:29:57+03:00'
draft = true
title = 'About'
+++
```
Приемлемо.   {{< img src="/images/2026-02-08-first-post/5.png" width="56" height="57">}}  
Чтобы самим не запутаться в постах, когда их станет слишком много, лучше включать в наименование файла поста нумерацию или дату. 
```sh
hugo new content content/posts/2026-02-14-about.md
```
Вообще hugo сам берет дату из Front Matter (`date = ...`), поэтому имя файла с датой не обязательно, но удобно для сортировки в редакторе.  
Мой первый пост, так и быть, останется таким, как есть, но следующие буду датировать, честно-честно.  
Чтобы пост отобразился на сайте, нужно поменять флаг draft на false.  
Дальше я добавила текст своей первой заметки и переименовала её.  
Ещё добавила тегов, вдруг потом пригодятся.  
Итоговый заголовок получился таким: 
```txt
+++
date = '2026-02-14T19:29:57+03:00'
draft = false
tags = ['about', 'автор', 'блог']
title = 'Об авторе и блоге'
+++
```

Запустила сервер с флагом отображения черновиков:
```sh
hugo server --buildDrafts
```

> *Вот тут при запуске получила ошибку, из чего стало понятно, что для поддержки моей темы всё-таки нужна расширенная версия hugo:*
> ```sh
> ERROR error building site: render: [en v1.0.0 guest] failed to render pages: render of "/404" failed: "/home/daria/blog/themes/m10c/layouts/_default/baseof.html:12:42": execute of template failed: template: 404.html:12:42: executing "404.html" at <$style.RelPermalink>: error calling RelPermalink: TOCSS: failed to transform "css/main.scss" (text/x-scss). Check your Hugo installation; you need the extended version to build SCSS/SASS with transpiler set to 'libsass'.: this feature is not available in your current Hugo version, see https://goo.gl/YMrWcn for more information
> ```
> *Установила расширенную версию, и сервер заработал - дважды уря!*

Первая страница доступна на `localhost:1313`:  
{{< img src="images/2026-02-08-first-post/1.png" width="970" height="711">}}

Пришло время настроек конфига. Я оставила такой:
```toml
baseURL = 'https://daryahom.github.io/'
languageCode = 'ru-ru'
title = 'Технические заметки'
theme = 'm10c'

[pagination]
  pagerSize = 10

[params]
  author = ''
  description = 'О технологиях, коде и космических хомяках'
  menu_item_separator = " • "

[[params.social]]
  icon = "github"
  name = "My Github"
  url = "https://github.com/DaryaHom"
[[params.social]]
  icon = "telegram"
  name = "telegram"
  url = "https://t.me/daryahom"

[menu]
  [[menu.main]]
    identifier = "home"
    name = "Home"
    url = "/"
    weight = 1
  [[menu.main]]
    identifier = "posts"
    name = "Posts"
    url = "/posts/"
    weight = 2
  [[menu.main]]
    identifier = "about"
    name = "About"
    url = "/posts/about/"
    weight = 3

```
После заполнения конфига на сайте появилась менюшка, название и описание блога.  
Но вот пустой аватар остался. Мне он был не нужен, поэтому можно его убрать.  
Настройки дефолтного аватара хранятся в [baseof.html](https://github.com/vaga/hugo-theme-m10c/blob/862c6e941be9bc46ce8adc6a2fa9e984ba647d6f/layouts/_default/baseof.html#L24) темы. Однако, тему я загрузила как сабмодуль, поэтому нельзя просто так взять и отредактировать её. Только через изменения в оригинальном репозитории.  
Зато можно переопределить некоторые файлы, и тогда локальная версия получит приоритет перед источником темы. 
Я скопировала `baseof.html` в `blog/layouts/_default` и убрала из соответствующей строки указание на `default "avatar.jpg"` и дефолтного автора.
Было:
```html
<a href="{{ "" | relURL }}"><img class="app-header-avatar" src="{{ .Site.Params.avatar | default "avatar.jpg" | relURL }}" alt="{{ .Site.Params.author | default "John Doe" }}" /></a>
```

Стало:
```html
<a href="{{ "" | relURL }}"><img class="app-header-avatar" src="{{ .Site.Params.avatar | relURL }}" alt="{{ .Site.Params.author }}" /></a>
```

Помимо самого аватара нужно отредактировать стили, иначе его симпатичная зелёная обводка так и останется одиноко висеть в воздухе.  
Алгоритм тот же. Копируем `/assets/css/components`, находим блок `.app-header-avatar` и переопределяем его вот так:
```scss
.app-header-avatar {
	width: none !important;
	height: none !important;
	border-radius: none !important;
	border: none !important;
}
```
Я вошла во вкус и ещё немножко поправила цвета текста и отступы.

В моей первой заметке есть картинка, которую я положила в `static/images` и встроила в текст заметки с помощью ссылки...  
```html
 {{ < img src="/images/hamster.png" width="56" height="57"> }}
```

... и файла настроек `layouts/shortcodes/img.html`:
```html
{{- $src := .Get "src" -}}
{{- $alt := .Get "alt" | default "" -}}
{{- $width := .Get "width" -}}

<img 
  src="{{ $src | relURL }}" 
  alt="{{ $alt }}"
  {{ with $width }}width="{{ . }}"{{ end }}
>
```

> *Изначально была попытка добавить картинку через стандартный функционал md, но мне понадобилась возможность задать размеры, которую базовый синтаксис не предоставляет.  
> Тогда я вставила её через html, чтобы можно было отрегулировать размер вот так:*
> ```html
> <img src="../../static/images/hamster.png" width="56" height="57">
> ```
> *Но hugo поругался на меня за небезопасное использование =/*
> ```error
> WARN  Raw HTML omitted while rendering "/home/daria/blog/content/posts/about.md"; see https://gohugo.io/getting-started/configuration-markup/#rendererunsafe
> ```
> *Пришлось переписать.*

Теперь локально всё выглядит великолепно:
{{< img src="images/2026-02-08-first-post/2.png" width="970" height="711">}}

Время выкладывать на github!  


Нормальные люди инициализировали бы репозиторий прямо из папки с блогом, но таким оладушкам, как я, советую воспользоваться волшебной кнопкой "New" на вкладке Repositories  в github: 

{{< img src="images/2026-02-08-first-post/3.png" width="970" height="711">}}
А потом склонировать уже проинициализированный репозиторий и перетащить туда всё содержимое папочки блога:
```sh
git clone https://github.com/DaryaHom/daryahom.github.io.git
```
> *Забавная проблема (которая, может, и не проблема вовсе, но я задушню):  
> Изначально я создала github-репозиторий с логичным названием blog. Но после деплоя при попытке перейти в блог по адресу https://daryahom.github.io я получала 404 и уходила полная недоумения.  
> Блог был доступен только по адресу https://daryahom.github.io/blog, хотя в конфиге hugo baseURL был записан безо всяких blog-приписок.  
> Оказалось вот что: github лучше знает, как должен выглядеть базовый url, и если репозиторий назван blog, то он автоматически дописывает название после baseURL.  
> Пришлось переименовать репозиторий с блогом в `daryahom.github.io` — не очень красиво, зато блог сразу стал доступен по основному адресу.*

Далее в корне blog создала файл `.gitignore` с таким содержанием, чтобы не заливать в удалённый репозиторий генерируемые файлы:
```txt
/.hugo_build.lock
/public
/resources/_gen
```
И отправила всё содержимое папки в удалённый репозиторий.
```sh
git add .
git commit -m "Initial commit"
git push --set-upstream origin master
```

Осталось настроить деплой 🫠  


Документация hugo великолепна, в ней очень хорошо расписана последовательность действий для этого:  
1. В настройках репозитория **Settings** > **Pages** в разделе `Build and deployment` нужно поменять `Source` c `deploy from a branch` на `GitHub Actions`
2. Настроить кеширование в конфигурации сайта (этот шаг я пропустила😇)
3. Создать конфиг hugo в директории `.github/workflows` и заполнить его [готовым содержимым](https://gohugo.io/host-and-deploy/host-on-github-pages/#step-4).  
В содержимом конфига даже менять ничего не пришлось, вот такие hugo умнички.
```sh
mkdir -p .github/workflows
touch .github/workflows/hugo.yaml
# и скопировать содержимое в созданный файл
```
Снова закоммитить изменения: 
```sh
git add .
git commit -m "Add deploy config"
git push 
```

Далее меню репозитория находим вкладку **Actions**, и во вкладке `Build and deploy` жмав кнопку `Run workflow`  
После деплоя сайт будет доступен по адресу, указанному в  `Build and deploy`, у меня это https://daryahom.github.io/  
{{< img src="images/2026-02-08-first-post/4.png" width="970" height="711">}}
Что ж, на этом всё, пойду вознагражу себя за труды пачкой чокопаев, чего и вам желаю, если вы по какой-то причине дочитали до конца =)  

**Источники:**
- https://habr.com/ru/articles/589457/
- https://habr.com/ru/articles/840218/
- https://habr.com/ru/articles/778900/
- https://gohugo.io/host-and-deploy/host-on-github-pages/
- https://gohugo.io/getting-started/quick-start/
- https://medium.com/@jh.baek.sd/how-i-built-my-perfect-blogging-setup-with-obsidian-hugo-github-cloudflare-a4f68964fa87
- https://github.com/gohugoio/hugo
- https://gohugo.io/host-and-deploy/host-on-github-pages/
- https://docs.github.com/en/pages/quickstart
