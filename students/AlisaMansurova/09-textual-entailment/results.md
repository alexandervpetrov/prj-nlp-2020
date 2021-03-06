### Ітерації

#### 0. Бейзлайн

```
               precision    recall  f1-score   support

contradiction       0.42      0.51      0.46      3237
   entailment       0.43      0.62      0.51      3368
      neutral       0.35      0.11      0.17      3219

     accuracy                           0.42      9824
    macro avg       0.40      0.42      0.38      9824
 weighted avg       0.40      0.42      0.38      9824
```

Втупу використала функцію `similarity` зі `spacy` для пар речень

#### 1. Частка NER

```
               precision    recall  f1-score   support

contradiction       0.42      0.53      0.47      3237
   entailment       0.42      0.68      0.52      3368
      neutral       0.60      0.07      0.13      3219

     accuracy                           0.43      9824
    macro avg       0.48      0.43      0.37      9824
 weighted avg       0.48      0.43      0.38      9824
```

Результат погіршився, тому далі не використовую

#### 2. Частка лем усіх слів, а також іменників та дієслів

```
               precision    recall  f1-score   support

contradiction       0.48      0.57      0.52      3237
   entailment       0.55      0.66      0.60      3368
      neutral       0.45      0.27      0.34      3219

     accuracy                           0.50      9824
    macro avg       0.49      0.50      0.49      9824
 weighted avg       0.49      0.50      0.49      9824
```

Результат трошки покращився

#### 3. Частка н-грамів для лем, частин мови і типів зв'язку

```
               precision    recall  f1-score   support

contradiction       0.49      0.55      0.52      3237
   entailment       0.57      0.65      0.61      3368
      neutral       0.45      0.33      0.38      3219

     accuracy                           0.51      9824
    macro avg       0.50      0.51      0.50      9824
 weighted avg       0.51      0.51      0.50      9824
```

Використовувала бі- та три-грами. Результат - зовсім невеличке покращення

#### 4. Частка лем з препроцесингом заперечень

```
               precision    recall  f1-score   support

contradiction       0.49      0.55      0.52      3237
   entailment       0.57      0.65      0.61      3368
      neutral       0.45      0.33      0.38      3219

     accuracy                           0.51      9824
    macro avg       0.50      0.51      0.50      9824
 weighted avg       0.51      0.51      0.50      9824
```

Нічого не вийшло чомусь. Або я неправильно препроцесила, або лема для даної фічі не підходить, а треба було спробувати їнші ознаки токенів

#### 5. Робота з залежностями

```
               precision    recall  f1-score   support

contradiction       0.50      0.55      0.53      3237
   entailment       0.57      0.64      0.60      3368
      neutral       0.46      0.35      0.40      3219

     accuracy                           0.52      9824
    macro avg       0.51      0.51      0.51      9824
 weighted avg       0.51      0.52      0.51      9824
```

Каюсь, не приділила достатньо уваги і не розібралась, як правильно треба було їх використати. На даному етапі додала такі фічі: 1) тип залежності слова і тип залежності його батька, 2) тип залежності слова і частина мови батька, 3) частина мови слова і батька, 4) лемма слова і батька. Впевнена, тут є над чим попрацювати, обов'язково повернусь до цього на днях

#### 6. Частка стем

```
               precision    recall  f1-score   support

contradiction       0.50      0.55      0.53      3237
   entailment       0.57      0.64      0.60      3368
      neutral       0.46      0.35      0.40      3219

     accuracy                           0.52      9824
    macro avg       0.51      0.52      0.51      9824
 weighted avg       0.51      0.52      0.51      9824
```

Не допомогло

#### 7. Семантичні зв'язки

```
               precision    recall  f1-score   support

contradiction       0.50      0.55      0.53      3237
   entailment       0.57      0.65      0.60      3368
      neutral       0.46      0.35      0.40      3219

     accuracy                           0.52      9824
    macro avg       0.51      0.52      0.51      9824
 weighted avg       0.51      0.52      0.51      9824
```

Мій найбільший фейл, бо я в цей етап вклала досить багато часу, а отримала натомість... нічого.

Що було зроблено

-   Я зібрала невеличкий [датасет](./all_concepts.zip) з `ConceptNet` для всіх слів з тестових, тренувальних і дев даних, які є ROOT у реченнях (вийшло 5831 слів, вони [отут](./concs.txt)). Датасет і справді вийшов надто маленьким, бо через те, що воно відбувається досить нешвидко, я качала дані тільки з першої сторінки і з лімітом 100 😞 Більш ніж впевнена, через цю позорну річ і ніякого толку з цих даних не було
-   брала виключно ROOT-и речень, гралася з різними зв'язками, зупинилась на `synonyms`, `meanings`, `similarities`, `forms` і `antonyms`

Що варто було б зробити

-   зібрати набагато більший датасет
-   зібрати датасет не тільки для ROOT, але і для `nsubj` та `obj` залежностей

Обов'язково спробую попрацювати над цим (як буде час)

### 8. Схожість слів зі spacy

```
               precision    recall  f1-score   support

contradiction       0.51      0.55      0.53      3237
   entailment       0.58      0.63      0.60      3368
      neutral       0.47      0.38      0.42      3219

     accuracy                           0.52      9824
    macro avg       0.52      0.52      0.52      9824
 weighted avg       0.52      0.52      0.52      9824
```

Остання надія на розум штучний. Трошки допомогло, хоч і несуттєво

## Проблеми

Основна проблема, яку я побачила - результати на фінальному тестуванні відрізнялись від тих, які я отримувала на іграшковій вибірці (наприклад, робота з семантичними зв'язками на іграшковій вибірці давала покращення в той час як у фінальному прогоні - ні)
