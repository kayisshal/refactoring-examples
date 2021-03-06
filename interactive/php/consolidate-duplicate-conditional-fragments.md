consolidate-duplicate-conditional-fragments:php

###

1.ru. Если дублирующие участки находятся вначале веток оператора, вынесите их перед условным оператором.
1.en. If the duplicated code is at the beginning of the conditional branches, move the code to a place before the conditional.
1.uk. Якщо дублюючі ділянки знаходяться на початку гілок оператора, винесіть їх перед умовним оператором.

2.ru. Если такой код выполняется в конце веток, поместите его после условного оператора.
2.en. If the code is executed at the end of the branches, place it after the conditional.
2.uk. Якщо такий код виконується у кінці гілок, помістить його після умовного оператора.

3.ru. Если дублирующий код расположен случайным образом внутри веток, вам нужно для начала попытаться передвинуть его в начало или в конец ветки, в зависимости от того, меняет ли он результат последующего кода.
3.en. If the duplicate code is randomly situated inside the branches, first try to move the code to the beginning or end of the branch, depending on whether it changes the result of the subsequent code.
3.uk. Якщо дублюючий код розташован випадковим чином усередині гілок, вам треба спробувати пересунути його в початок або в кінець гілки, залежно від того, чи міняє він результат подальшого коду.

4.ru. Дублирующий фрагмент кода более одной строки можно попытаться <a href="/extract-method">извлечь в новый метод</a>, если в этом есть смысл.
4.en. If appropriate and the duplicate code is longer than one line, try using <a href="/extract-method">Extract Method</a>.
4.uk. Дублюючий фрагмент коду більший за один рядок можна спробувати "відокремити в новий метод" (Extract method), якщо в цьому є сенс.



###

```
function sendEmailPromotion($email, $text) {
  // ...
  if (isSpecialDeal()) {
    $total = $price * 0.95;
    validateEmailAddress($email);
    sendEmail(formatEmail($text, $total));
  }
  else {
    validateEmailAddress($email);
    $total = $price * 0.98;
    sendEmail(formatEmail($text, $total));
  }
}
```

###

```
function sendEmailPromotion($email, $text) {
  // ...
  validateEmailAddress($email);
  if (isSpecialDeal()) {
    $total = $price * 0.95;
  }
  else {
    $total = $price * 0.98;
  }
  sendEmail(formatEmail($text, $total));
}
```

###

Set step 1

Select "sendEmail(formatEmail($text, $total));"
+ Select "validateEmailAddress($email);"

#|ru| Как видите, в нашем методе есть условный оператор с повторяющимся кодом в разных ветках.
#|en| As you see, our method has a conditional with repeating code in different branches.
#|uk| Як бачите, в нашому методі є умовний оператор з кодом, що повторюється у різних гілках.

#|ru| Попробуем вынести этот код за пределы условного оператора. Начнём с <code>sendEmail()</code>.
#|en| Let's move this code outside the conditional. We start with <code>sendEmail()</code>.
#|uk| Спробуємо винести цей код за межі умовного оператора. Почнемо з <code>sendEmail()</code>.

Select:
```
    sendEmail(formatEmail($text, $total));

```

#|ru| Вызов этой функции лежит в конце обеих веток условного оператора. Это значит, что мы можем перенести вызов после условия.
#|en| The call to this function is at the end of both branches of the conditional operator. That means that we can move the call after the condition.
#|uk| Виклик цієї функції лежить в кінці обох гілок умовного оператора. Це означає, що ми можемо перенести виклик після умови.

Remove selected

Go to the end of "sendEmailPromotion"

Print:
```

  sendEmail(formatEmail($text, $total));
```

#C|ru| Запускаем авто-тесты.
#S Пока что всё работает!

#C|en| Let's run the autotests.
#S So far so good!

#C|uk| Запускаємо авто-тести.
#S Поки що все працює.

Select:
```
    validateEmailAddress($email);

```

#|ru| Теперь попытаемся вынести <code>validateEmailAddress()</code>. Эти вызовы находятся в разных местах, поэтому нужно подумать, будет ли уместно вообще их двигать. В нашем случае, валидацию можно проводить где угодно, но лучше всего – ближе к началу метода. Поэтому и перемещаем её туда.
#|en| Now let's try to move <code>validateEmailAddress()</code>. These calls are in different places, so it is worth thinking about whether to move them at all. In our case, validation can be performed anywhere but preferably closer to the beginning of the method. So let's move it there.
#|uk| Тепер спробуємо винести <code>validateEmailAddress()</code>. Ці виклики знаходяться в різних місцях, тому потрібно подумати, чи буде доречно взагалі їх рухати. В нашому випадку, валідацію можна проводити де завгодно, але найкраще – ближче до початку методу. Тому і переміщаємо її туди.

Remove selected

Go to:
```
  // ...|||
  if
```

Print:
```

  validateEmailAddress($email);
```

#C|ru| Запускаем финальное тестирование.
#S Отлично, все работает!

#C|en| Let's start the final testing.
#S Wonderful, it's all working!

#C|uk| Запускаємо фінальне тестування.
#S Супер, все працює.

Set final step

#|ru|Q На этом рефакторинг можно считать оконченным. В завершение, можете посмотреть разницу между старым и новым кодом.
#|en|Q The refactoring is complete! You can compare the old and new code if you like.
#|uk|Q На цьому рефакторинг можна вважати закінченим. На завершення, можете подивитися різницю між старим та новим кодом.