# sdlc-lab-Faizulina
Software Development Life Cycle (SDLC) — Життєвий цикл програмного забезпечення.

18.	Лічильник кроків (імітація) — ручний ввід кількості кроків для симуляції прогресу.

1) Планування 
Мета продукту:
Користувач повинен мати можливість вручну вводити кількість кроків за день, задавати денну ціль і бачити прогрес у вигляді відсотків/індикатора.

2) Аналіз вимог — User Stories (15 хв)
✅ (Must have) Як користувач, я хочу ввести кількість кроків за день, щоб фіксувати свій прогрес.
✅ (Must have) Як користувач, я хочу бачити прогрес відносно денної цілі, щоб розуміти, скільки лишилось пройти.
Як користувач, я хочу задати/змінити денну ціль (наприклад, 8000–12000), щоб підлаштувати додаток під себе.
Як користувач, я хочу переглядати історію за останні 7 днів, щоб відстежувати динаміку.
Як користувач, я хочу виправити помилковий ввід, щоб редагувати значення за конкретну дату.

3) Дизайн — Прототип.
   prototype.png
   
5) Реалізація.
```
   state:
  dailyGoal := 10000
  stepsByDate := map<date, integer> // { "2025-09-30": 7200, ... }

function addSteps(date, stepsInput):
  if stepsInput is null OR not integer(stepsInput):
      return "Error: Некоректний формат числа"
  if stepsInput <= 0:
      return "Error: Кроки мають бути > 0"
  current := stepsByDate.getOrDefault(date, 0)
  stepsByDate[date] := current + stepsInput
  return "Success: Додано " + stepsInput

function setDailyGoal(newGoal):
  if not integer(newGoal) OR newGoal < 1000 OR newGoal > 50000:
      return "Error: Ціль має бути від 1000 до 50000"
  dailyGoal := newGoal
  return "Success: Ціль оновлено"

function getTodayProgress(today):
  todaySteps := stepsByDate.getOrDefault(today, 0)
  percent := clamp( round( (todaySteps / dailyGoal) * 100 ), 0, 100 )
  return { todaySteps, dailyGoal, percent }

function editSteps(date, newValue):
  if not integer(newValue) OR newValue < 0:
      return "Error: Значення має бути ≥ 0"
  stepsByDate[date] := newValue
  return "Success: Оновлено"

function getHistory(lastNDays):
  return list of (date, steps) for lastNDays sorted by date desc
```

6) Тестування.
   
Додавання кроків — валідне
Дія: addSteps(сьогодні, 1500)
Очікувано: сума за сьогодні зростає на 1500; повідомлення Success ("Додано 1500"); у getProgress() todaySteps відображає нове значення.

Додавання кроків — порожній/некоректний ввід
Дія: addSteps(сьогодні, "") або addSteps(сьогодні, "abc")
Очікувано: помилка Error("Некоректний формат числа"); стан не змінюється.

Додавання кроків — нуль/від’ємне
Дія: addSteps(сьогодні, 0) або addSteps(сьогодні, -200)
Очікувано: помилка Error("Кроки мають бути > 0"); стан не змінюється.

7. Висновки.
Для цього навчального застосунку оптимально підходить Agile-підхід із короткими ітераціями.
Це дає можливість швидко розширювати функціонал (наприклад, історія кроків, графік прогресу, інтеграція з реальними сенсорами) та гнучко реагувати на зміни вимог чи відгуки користувачів.
Waterfall тут менш доречний, оскільки кожна зміна вимагала б проходження всіх етапів заново, а Spiral надто складний і громіздкий для маленького проєкту без значних ризиків.
