<h1 align="center">
 babyagi

</h1>


# Мета
Цей Python-скрипт є прикладом системи управління задачами на основі штучного інтелекту. Система використовує API OpenAI та Pinecone для створення, пріоритезації та виконання задач. Основна ідея цієї системи полягає в тому, що вона створює задачі на основі результату попередніх задач та попередньо визначеної мети. Після цього скрипт використовує можливості обробки природньої мови (NLP) OpenAI для створення нових задач на основі мети та Pinecone для збереження та отримання результатів задач для контексту. Це спрощена версія оригінального [Task-Driven Autonomous Agent](https://twitter.com/yoheinakajima/status/1640934493489070080?s=20) (28 березня 2023 року).

This README will cover the following:

* [Як працює скрипт](#how-it-works)

* [Як використовувати скрипт](#how-to-use)

* [Підтримувані моделі](#supported-models)

* [Попередження про постійну роботу скрипту](#continous-script-warning)
# Як це працює<a name="how-it-works"></a>
Скрипт працює за допомогою безкінечного циклу, який виконує наступні кроки:

1. Витягує першу задачу зі списку задач.
2. Надсилає задачу до виконавчого агента, який використовує API OpenAI для виконання задачі на основі контексту.
3. Збагачує результат та зберігає його в Pinecone.
4. Створює нові задачі та змінює пріоритети списку задач на основі мети та результату попередньої задачі.
Функція execution_agent() є місцем, де використовується API OpenAI. Вона приймає два параметри: мету та задачу. Далі вона відправляє запит до API OpenAI, який повертає результат задачі. Запит складається з опису задачі системи штучного інтелекту, мети та самої задачі. Результат повертається у вигляді строки(string).

Функція task_creation_agent() - це місце, де використовується API OpenAI для створення нових завдань на основі цілей та результатів попереднього завдання. Функція приймає чотири параметри: мету, результат попереднього завдання, опис завдання та поточний список завдань. Після цього вона надсилає запит до API OpenAI, який повертає список нових завдань у вигляді строк(strings). Функція повертає нові завдання у вигляді списку словників(dictionary), де кожен словник містить назву завдання.

Функція prioritization_agent() - це місце, де використовується API OpenAI для перепріоритизації списку завдань. Функція приймає один параметр - ID поточного завдання. Вона надсилає запит до API OpenAI, який повертає перепріоритизований список завдань у вигляді нумерованого списку.

Наостанку, скрипт використовує Pinecone для зберігання та отримання результатів завдань для контексту. Скрипт створює індекс Pinecone на основі імені таблиці, яке вказано у змінній YOUR_TABLE_NAME. Pinecone використовується для зберігання результатів завдання в індексі, разом з назвою завдання та будь-якою додатковою метаданою.

# Як використовувати скрипт<a name="how-to-use"></a>
Для використання скрипту вам потрібно виконати наступні кроки:

1. Зклонуйте репозиторій через `git clone https://github.com/yoheinakajima/babyagi.git` та перейдіть до клонованого репозиторію за допомогою команди `cd`. 
2. Встановіть необхідні пакети: `pip install -r requirements.txt`
3. Скопіюйте файл .env.example в .env: `cp .env.example .env`. Тут треба встановити наступні змінні.
4. Встановіть свої ключі OpenAI та Pinecone API в змінні OPENAI_API_KEY, OPENAPI_API_MODEL та PINECONE_API_KEY.
5. Встановіть середовище Pinecone у змінну PINECONE_ENVIRONMENT.
6. Встановіть ім'я таблиці, де будуть зберігатись результати задач, у змінну TABLE_NAME.
7. (Опціонально) Встановіть мету системи управління завданнями в змінну OBJECTIVE.
8. (Опціонально) Встановіть перше завдання системи у змінну INITIAL_TASK.
9. Запустіть скрипт.

Всі необов'язкові значення вище також можна вказати через командний рядок.

# Підтримувані моделі<a name="supported-models"></a>

Цей скрипт працює з усіма моделями OpenAI, а також Llama через Llama.cpp. Модель за замовчуванням - **gpt-3.5-turbo**. Щоб використовувати іншу модель, вкажіть її через OPENAI_API_MODEL або використайте командний рядок.


## Llama

Завантажте останню версію [Llama.cpp](https://github.com/ggerganov/llama.cpp) та дотримуйтеся інструкцій для його компіляції. Вам також знадобляться вагові коефіцієнти моделі Llama.

 - **Ні за яких обставин не діліться IPFS, magnet-посиланнями або будь-якими іншими посиланнями на завантаження моделі ніде в цьому репозиторії, включаючи issues, discussions або pull requests. Вони будуть негайно видалені.**

Після цього прив'яжіть `llama/main` до `llama.cpp/main` та `models` до папки, де ви маєте вагові коефіцієнти моделі Llama. Потім запустіть скрипт з аргументом `OPENAI_API_MODEL=llama` або `-l`.

# Попередження<a name="continous-script-warning"></a>
Цей скрипт призначений для постійної роботи в рамках системи управління завданнями. Постійний запуск цього скрипту може призвести до високого використання API, тому використовуйте його з обережністю. Крім того, для правильної роботи скрипту необхідно налаштувати API OpenAI та Pinecone, тому переконайтеся, що ви налаштували API перед запуском скрипту.

# Контрибуція
Звичайно, BabyAGI ще на початковому етапі, тому ми все ще визначаємо його напрямок та кроки для досягнення цілей. На даний момент ключовою ціллю дизайну BabyAGI є те, щоб він був *простим*, щоб його було легко зрозуміти та розширювати. Щоб зберегти цю простоту, ми люб'язно просимо дотримуватися наступних принципів надсилаючи PR:
* Фокусуйтеся на невеликих, модульних змінах, а не на обширних рефакторингах.
* При введенні нових функцій(features) надавайте детальний опис конкретного випадку використання(use case), який ви розглядаєте.

Примітка від @yoheinakajima (5 квітня 2023 року):

> Я знаю, що кількість запитів на злиття зростає, і дякую вам за терпіння - я ще новачок у GitHub/OpenSource, і не вдалося правильно розпланувати свій час на цей тиждень. Щодо напрямку, я роздираюся між збереженням простоти та розширенням - на даний момент нахил полягає на збереженні основної простої Baby AGI та використанні її як платформи для підтримки та просування різних підходів до розширення (наприклад, BabyAGIxLangchain як один з напрямків). Я вважаю, що є різні підходи, які варто досліджувати, і бачу цінність в тому, щоб мати центральне місце для порівняння та обговорення. Незабаром очікуються більш докладні оновлення.

Я новачок на GitHub і у відкритому програмному забезпеченні, тому будь ласка, будьте терплячими, поки я вивчу правильне управління цим проектом. Я працюю в венчурній фірмі вдень, тому, як правило, перевірятиму PR та проблеми ввечері після того, як я вкладу спати своїх дітей - що може не бути кожної ночі. Готовий до ідеї залучення підтримки, скоро оновлю цей розділ (очікування, бачення і т.д.). Говорю з багатьма людьми та навчаюся - чекайте оновлень!

# Передісторія
BabyAGI - це спрощена версія оригінального [Task-Driven Autonomous Agent](https://twitter.com/yoheinakajima/status/1640934493489070080?s=20) (28 березня 2023 року) опублікованого на Twitter. Ця версія містить 140 рядків: 13 коментарів, 22 порожніх та 105 коду. Назва репозиторію виникла в реакції на оригінальний автономний агент - автор не має на меті натякати на AGI.

Створено з любов'ю [@yoheinakajima](https://twitter.com/yoheinakajima), який виявилось є VC (було б цікаво побачити, що ви будуєте!)