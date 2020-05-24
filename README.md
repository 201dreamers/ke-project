# Проект "Мікро пінг-понг"

Монохромний o-led дисплей 128х32 точки, мікроконтролер, 2 кнопки, штекер для загрузки прошивки через ASP программатор, відсік для батарейки, літієва батарейки типу CA.

панель для мікроконтролера, макетна плата, программатор USB ASP.
Дисплей підключається до шини I2C мікроконтроллера, кнопки підключаємо до інших вільних пінів. Також підключаємо живлення та вимикач. Для завантаження прошивки, программатор підключаємо за такою схемою.

Розставимо компоненти на платі. Непотрібну частину плати можна відрізати.Закріпимо всі компоненти, запаявши піни до плати.
З'єднаємо всі виходи за схемою. Для цього використаємо тонкий монтажний провід.

Далі припаюємо батарейний відсік та вимикач. Відсік приклеїмо до макетної плати з допомогою термоклею.

### Завантаження прошивки

Щоб завантажити прошивку на мікроконтролер, підключимо программатор за допомогою навчальної макетної плати.
>  Прошивка на сайті

Встановити Arduino IDE, завантажити ядро для роботи з мікроконтролерами серії ATiny, вибрати плату та программатор.

Для коректної роботи программатора ASP потрібно встановити для нього драйвер, завантажений з одного із сайтів.

Вибираємо частоту 8 або 16 МГц та натискаємо "Записати загрузчик". Це допоможе налаштувати мікроконтролер на потірбну частоту.

Після цього загружаємо скетч в мікроконтролер, виймаємо його та вставляємо в припаяну на плату панель для мікроконтролера

> Мікроконтролер ATiny 85 512б оперативної пам'яті
> Дисплей OLDE SSD1306. Дисплей використовує буфер на стороні мікроконтролера, щоб коректно промальовувати картинку оскільки на цьому дисплеї немає можливості зчитати дані з його власного буфера. Буфер цього дисплею 128х32/8=512б якщо ми хочемо пам'ятати стан кожного пікселю. Оскільки нам потрібно зберігати і інші дані, такий варіант не вміщається в 512б оперативної пам'яті мікроконтролера тому було прийнято рішення знизити роздільну здатність екрану до 64х16 точок за допомогою буферизації квадратів які складаються з 4 пікселів.

### Логіка гри

За окремим таймером рухається кулька. Рух виконується за рахунок видалення точки з минулими координатами та створення нової точки з новими координатами.

Розрахунок координат це звичайне додавання величини швидкості кульки до величини координат в системі дисплею.

При дотиканні кульки до горизонтальних меж екрану, він просто від них відскакує, змінюючи вертикальну складову швидкості на таку ж з протилежним знаком.

Також програма виконує перевірки по вертикальних межах екрану. Якщо кулька дотикається до ракетки, то вона відскакує, при чому кут відскоку вибирається випадковий. Якщо кулька токається стінки за ракеткою, гравець програв раунд і супротивник отримує бал.

Рух ракеток в грі реалізовано як і рух кульки, а саме стирається стара ракетка і промальовується нова із новими координатами. Координати змінюються при натисканні кнопки. Такий спосіб виходить швидше, ніж стирання всього вмісту дисплею і промальовування всіх елементів заново.

Що стосується ракетки супротивника, нею керує невеликий блок коду. Ця ракетка рухається в ту сторону, куди летить кулька, стараючись співставивти свій центр із нею. Щоб такого супротивника можна було обіграти, додано таймер за яким він реагує на зміну координат кульки.

Також в грі присутня механіка збільшення складності. Кожні 10 балів на користь гравця швидкість кульки збільшується. Відповідно збільшується і швидкість реакції супротивника