---
layout: chapter
title: Повторное использование
section: Background
permalink: /chapters/reuse/
description: Почему отказ от повторного использования и приемлемость повторений делает работу с CSS проще.
---

**Summary:** Не пытайтесь повторно использовать стили. Воспользуйтесь duplication-first подходом.

> &ldquo;Принцип DRY часто неверно понимается как отказ от повторения одного и того же дважды [...]. Это непрактично и обычно конрпродуктивно. Это может привести к вынужденным абстракциям, к замудренному и насыщенному конструкциями коду.&ldquo;
<br>&mdash; <cite>Harry Roberts, CSS Wizardy</cite>

Поймите правильно,&mdash;MaintainableCSS включает различные подходы для повторного использования, о которых речь пойдет позже. Проблемой же является повторное использование элементов, заключенных *внутри* фигурных скобок. И вот почему:

## Поскольку изменение стилей основывается на контрольных точках.

Создание адаптивных сайтов подразумевает задание стилей элемементов в зависимости от размера экрана. Предположим, нам нужно создать такую таблицу из двух колонок, чтобы:

-  свойство padding составляло 50px и 20px на больших и малых экранах соответственно.
- размер шрифта составлял 3em и 2em на больших и малых экранах соответственно.
- на малых экранах колонки располагались одна под другой. Заметим, что понятие колонка здесь становится довольно условным.

Отталкиваясь от этого, как бы вы воспользовались следующими элементарными именами классов: `.grid`, `.col`, `.pd50`, `.pd20`, `.fs2` and `fs3`, - чтобы удовлетворить требованиям задания?

	<div class="grid">
	  <div class="col pd50 pd20 fs3 fs2">Column 1</div>
	  <div class="col pd50 pd20 fs3 fs2">Column 2</div>
	</div>

Как видите, это просто не будет работать. Вам теперь понадобятся такие странные имена классов, как `fs3large`. И это только вершина айсберга, когда дело доходит до проблем реализации.

В качестве альтернативы, рассмотрим следующую семантическую разметку, которая не пытается использовать стили повторно:

	<div class="someModule">
	  <div class="someModule-someComponent"></div>
	  <div class="someModule-someOtherComponent"></div>
	</div>

Благодаря чему, реализация стилей, описанных ранее, является теперь простой задачей, требующей всего шесть CSS-деклараций, три из которых содержатся в медиа-запросах.

## Поскольку изменение стилей зависит от состояния.

Как бы вы сделали, чтобы класс `<a class="padding-left-20 red" href="#"></a>` имел padding 18px, тонкую границу и текст более темного оттенка на сером фоне, когда его состояние меняется на hovered, focused или active? `:hover`,`:focus`, `:active` и прочее?

Если коротко, то никак. Не создавайте себе проблемы.

## Поскольку повторное использование усложняет отладку.

Несколько CSS селекторов, примененных к элементу, добавляют помех в его отладку.

## Поскольку не стоит возиться с детализированными стилями.

Если вы собираетесь использовать `<div class="red">`, можно также сделать `<div style="color: red">`, что уж точно будет более понятным вариантом. Но мы не хотим это делать, чтобы не смешивать понятия.

## Поскольку имена класса, связанные с отображением, порой довольно неопределенны.

Например, имя `red`. Означает ли оно цвет фона или тескта или градиента? Какой оттенок красного оно обозначает?

## Поскольку изменения "служебного" класса влияют на все случаи его применения.

Звучит заманчиво, но на деле это не так. Вы столкнетесь с тем, что сделанные изменения проявятся не по месту предназначения. Возвращемся к исходному состоянию. Как вариант, вы начнете бояться прикасаться к этому классу общего пользования и создадите `.red2`. Так вы приходите к избыточному коду, поддерживать который, конечно, неприятно.

## Поскольку несемантические имена классов усложняют поиск.

Если элемент имеет классы, базирующиеся на том, как он выглядит, такие как `.red`, `.col-lg-4` и `.large`, тогда такие классы будут встречаться по всему коду. Результаты поиска для "red" будут многочисленны и разбросаны по шаблонам HTML, что усложняет поиск интересующего элемента.

В случае семантических имен класса поиск должен выдать единственный результат. А если результатов больше одного, тогда это сигнализирует о проблеме, требующей внимания.

Примечание: если у вас есть повторяющийся *component* внутри модуля, тогда поиск может дать несколько результатов в пределах одного файла. То есть, модуль, как правило, живет в одном шаблоне.

## Поскольку повторное использование вызывает увеличение объема кода.

Попытавшись повторно использовать каждое отдельное *правило*, вы прийдете к таким классами, как `red`, `clearfix`, `pull-left`, `grid`, что приведет к раздуванию HTML-кода:

	<div class="clearfix pull-left red etc">

Раздутый код труднее поддерживать и у него снижается производительность (хотя и незначительно).

## Поскольку повторное использование нарушает семантику.

Если вы стремитесь повторно использовать элементы внутри фигурных скобок, чтобы создать "елементарные" имена классов, то вы столкнетесь со всеми проблемами, изложенными в главе [Semantics](/chapters/semantics/). Прочитайте эту главу сейчас, если еще не успели.

## What if I really want to reuse a style?

It is extremely rare, but there are times when it really does make sense to reuse a style. In this case use the comma in your selectors and place it in a well named file.

For example let's say you wanted a bunch of different modules or components to have red text you might do this:

	/* colours.css */

	/* red text */
	.someSelector,
	.someOtherSelector {
		color: red;
	}

Remember though that if any selector deviates, even a little bit, then remove it from the common list and duplicate. You must be very careful with something like this. Do it for convenience, not for performance. Your mileage may vary.

## What about mixins?

CSS preprocessors allow you to create mixins which can be really helpful because they provide the best of both worlds but they should be designed with caution.

You have to be very careful to update a mixin because it propagates in all instances just like utility classes. It can be problematic to edit and instead you create new mixins that are slightly different causing redundancy and maintenance problems.

Also, mixins can become very complicated with lots of params, conditionality and large declarations of styles. All of this makes it complicated and complicated is hard to maintain.

To mitigate, you can make mixins really granular. For example you could have a "red text" mixin which is certainly better. But then again, the declaration of that mixin is basically the same as a declaration of red text&mdash;might as well just declare that instead.

If you need to update it in multiple places, then a search and replace might just do it, depending on your context. And even if you did use a mixin, when red changes to orange, you will have to do a search and replace anyway, because the mixin name will otherwise be misleading.

This does not mean mixins are "bad"&mdash;in fact they can be very helpful. For example, being able to apply *clearfix* rules across different elements at different breakpoints is probably a very helpful thing to do for maintainability. Just make sure to use them with care.

## What about performance?

I don't have stats to hand, but I can very confidently say that it's not wise to practice premature optimisation. Let's say you have a very large CSS codebase (100KB or more).

Firstly, I *guess* you *might* save yourself a few KB. Secondly, there are alternative paths to improving performance and thirdly, you probably have redundant code due to a lack of maintainability.

Also consider that one or two images are likely to be far larger than the entire CSS so exerting energy here is probably of little value.

## Is this violating DRY principles?

Attempting to reuse `float: left` as an example, is akin to trying to reuse variable names in different Javascript objects. It's simply not in violation of DRY.

## Final thoughts on reuse

Reuse and DRY are such important principles in software engineering but when it comes to CSS, striving to reuse too much ironically makes maintainance *harder*. However, there are times when reuse and abstraction makes sense which is discussed in later chapters.
