# Git Style Guide

Это гид по стилю для Git, вдохновленный [*How to Get Your Change Into the Linux
Kernel*](https://www.kernel.org/doc/Documentation/SubmittingPatches), the [git man pages](http://git-scm.com/doc) и различными практиками популярными в сообществе.

Переводы доступны на следующих языках:

* [Английский (оригинал)](https://github.com/agis-/git-style-guide)
* [Греческий](https://github.com/grigoria/git-style-guide)
* [Китайский (Традиционный)](https://github.com/JuanitoFatas/git-style-guide)
* [Китайский (Упрощенный)](https://github.com/aseaday/git-style-guide)
* [Корейский](https://github.com/ikaruce/git-style-guide)
* [Португальский](https://github.com/guylhermetabosa/git-style-guide)
* [Тайский](https://github.com/zondezatera/git-style-guide)
* [Украиский](https://github.com/denysdovhan/git-style-guide)
* [Французский](https://github.com/pierreroth64/git-style-guide)
* [Японский](https://github.com/objectx/git-style-guide)

Если вы хотите помочь в распространении, пожайлуста! Форкните проект и сделайте pull request.

# Содержание

1. [Ветви](#branches)
2. [Коммиты](#commits)
  1. [Сообщение](#messages)
3. [Слияние](#merging)
4. [Разное](#misc)

## Branches

* Выбирайте *короткие* и *ёмкие* имена:

  ```shell
  # хорошее
  $ git checkout -b oauth-migration

  # плохое - слишком расплывчатое
  $ git checkout -b login_fix
  ```

* Идентификаторы из corresponding билетов во внешних сервисах (например GitHub issue, TFS bug) так же хорошие кандидаты для имени ветви. Например:

  ```shell
  # GitHub issue #15
  $ git checkout -b issue-15
  ```

* Используйте *дефисы* для разделения слов в именах веток.

* При работе нескольких людей над *одной* чертой программы (feature), удобно иметь *личную* и *командную* ветви для этой черты(feature). Используйте следующее соглашение по именованию:

  ```shell
  $ git checkout -b feature-a/master # Командная ветвь
  $ git checkout -b feature-a/maria  # Личная ветвь Маши
  $ git checkout -b feature-a/nick   # Личная ветвь Никиты
  ```
  
  Сливайте личные ветки с командной как угодно (смотри ["Слияние"](#merging)).
  В конечном итоге командная ветвь будет слита с "master".

* Удаляйте свои ветки из уделённого репозитория (upstream repository), после того как они были слиты воедино. Только при наличии особой причины, ветви можно оставить.

  Подсказка: Используйте следующую команду, когда находитесь в "master",
  что бы посмотреть слитые ветви:

  ```shell
  $ git branch --merged | grep -v "\*"
  ```

## Commits

* Каджый коммит должен быть единичным *логическим изменением*. Не делайте несколько *логических изменений* в одном коммите.
  Для примера, если патч исправляет ошибку и оптимизирует производительность черты программы, разделите его на два раздельных коммита.

  *Подсказка: Используйте `git add -p` для прятанья определенных частей измененных файлов, в интерактивном режиме.*
  
* Не стоит разделять одно *лоигческое изменение* на несколько коммитов. Например,
  реализация функции(feature) и сопутствующие тесты должны быть в одном коммите.

* Коммитьте *своевременно* и *часто*. Мелкие и независимые коммиты просты для понимания и отката,
  если что-то пойдет не так.

* Коммиты должны быть *логически* упорядочены. Например, если *коммит А*
  зависит от изменений сделанных в *коммите Б*, то *коммит Б* должен идти перед *коммитом А*.

Примечание: Когда вы единолично работаете над локальной ветвью, которая *еще не была отправлена на сервер*, удобно использовать коммиты как временные снапшоты вашей работы. Тем не менее необходимо придерживаться правил, изложенных выше, *перед тем как* фиксировать изменения.

### Messages

* Используйте текстовый редактор, а не терминал, для написания комментариев коммитов:

  ```shell
  # хорошо
  $ git commit

  # плохо
  $ git commit -m "Hot fix"
  ```
  
  Коммит из терминала подстёгивает мышление уместить всё сообщение в одну строку,
  обычно, результатом этого являются неинформативные, двусмысленные комментарии коммитов.

* Обобщающая строка(т.е. первая линия сообщения) должна быть 
  *описательной* и *ёмкой*. В идеале она должна быть не длиннее *50 символов*.
  Она должна начинаться с заглавной буквы и должна быть написана в настоящем времени повелительного наклонения.
  Так же она не должна заканчиваться запятой, стоит воспринимать её как *заголовок* коммита:

  ```shell
  # good - imperative present tense, capitalized, fewer than 50 characters
  Mark huge records as obsolete when clearing hinting faults

  # bad
  fixed ActiveModel::Errors deprecation messages failing when AR was used outside of Rails.
  ```
* После должна идти пустая строка, следом за ней идёт более детальное описание.
  Оно должно объяснять *почему* данное изменение было сделано,
  какую решает проблему и какие *побочные эффекты* может иметь.
  
  Так же сообщение должно включать ссылки на связанные ресурсы 
  (например ссылку на таск/баг/проблему в баг трекере:

  ```text
  Краткое (50 симв. или меньше) описание изменений
  
  Более подробное описание, если требуется. Установите обертку
  строки на 72 символа. В некоторых контекстах, первая линия
  воспринимается как тема e-mail, а всё остальное как тело письма.
  Пустая линия, разделяющая краткое описание от подробного, критична
  (если вы не опустите тело полностью); tools like rebase can get
  confused if you run the two together.

  Следующие параграфы разделяются пустыми строками.
  
  - Пункты списка тоже применимы
  
  - Используйте дефис или звездочки для отметок списка,
    разделяйте отметку и текст пробелом, а сами пункты пустыми строками.

  Источник http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
  ```
  
  В конченом счете, когда пишите сообщение для коммита, думайте о том что вам потребуется
  знать, если вы вернетесь к этому коммиту через год.

* If a *commit A* depends on *commit B*, the dependency should be
  stated in the message of *commit A*. Use the SHA1 when referring to
  commits.

  Similarly, if *commit A* solves a bug introduced by *commit B*, it should
  also be stated in the message of *commit A*.
  
* Если коммит должен быть сплющен(squash) в другой коммит, импользуйте флаг `--squash` или 
  `--fixup`, для ясности пример:

  ```shell
  $ git commit --squash f387cab2
  ```
  *(Подсказка: используйте флаг `--autosquash` при перемещении(rebase). Помеченный коммиты
  Будут сплющены автоматически.)*

## Merging

* **Не переписывайте опубликованную историю.** История репозитория ценна сама по себе
  и очень важна для того что бы понять *что происходило на самом деле*.
  Изменение опубликованной истории - частый источник проблем, для всех кто работает над проектом. 

* Однако, есть случаи когда переписывание истории обосновано. 
  Когда:
    * Вы единственный человек работающий над веткой,
      и она не будет просмотрена кем-то еще.
    * Вы хотите почистить вашу ветку (например сплющить коммиты) 
      и/или перебазировать(rebase) его на "master", что бы объединить его позже.

  Тем не менее, *никогда не переписывайте историю "master" ветки*,
  или любых других специальных веток (т.е. используемых производственными или CI серверами).

* Содержите историю *чистой* и *простой*. *Перед тем как слить(merge)* вашу ветку:

    1. Убедитесь что она соответсвует стайл гайду, выполните необходимые действия,
       если нет (сплющите(squash)/измените порядок коммитов, перепишите сообщение и т.д.)

    2. Перебазируйте её на ветку с которой она должна быть слита:

      ```shell
      [my-branch] $ git fetch
      [my-branch] $ git rebase origin/master
      # затем слияние
      ```

      Это привёдет к тому, что ветвь может быть применена непосредственно
      к концу "master" ветви, и как результат очень простая история.

      *(Заметка: Эта стратегия лучше подходит для проектов с короткоживущими ветками,
      иначе может быть лучше время от вермени объединять изменения с "master" веткой
      вместо того что бы перебазироваться на неё.

* Если ваша ветвь включает в себя больше одного коммита, не объединяйте с 
  быстрой перемоткой (fast-forward):

  ```shell
  # хорошо - убедитесь что будет создан коммит слияния
  $ git merge --no-ff my-branch

  # bad
  $ git merge my-branch
  ```

## Misc.

* There are various workflows and each one has its strengths and weaknesses.
  Whether a workflow fits your case, depends on the team, the project and your
  development procedures.

  That said, it is important to actually *choose* a workflow and stick with it.

* *Be consistent.* This is related to the workflow but also expands to things
  like commit messages, branch names and tags. Having a consistent style
  throughout the repository makes it easy to understand what is going on by
  looking at the log, a commit message etc.

* *Test before you push.* Do not push half-done work.

* Use [annotated tags](http://git-scm.com/book/en/v2/Git-Basics-Tagging#Annotated-Tags)
  for marking releases or other important points in the history. Prefer
  [lightweight tags](http://git-scm.com/book/en/v2/Git-Basics-Tagging#Lightweight-Tags)
  for personal use, such as to bookmark commits for future reference.

* Keep your repositories at a good shape by performing maintenance tasks
  occasionally:

  * [`git-gc(1)`](http://git-scm.com/docs/git-gc)
  * [`git-prune(1)`](http://git-scm.com/docs/git-prune)
  * [`git-fsck(1)`](http://git-scm.com/docs/git-fsck)

# License

![cc license](http://i.creativecommons.org/l/by/4.0/88x31.png)

This work is licensed under a [Creative Commons Attribution 4.0
International license](https://creativecommons.org/licenses/by/4.0/).

# Credits

Agis Anastasopoulos / [@agisanast](https://twitter.com/agisanast) / http://agis.io
... and [contributors](https://github.com/agis-/git-style-guide/graphs/contributors)!
