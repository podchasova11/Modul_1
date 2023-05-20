# Modul_1

<details>
  <summary>Пред- и постусловия</summary>
  
## Пред- и постусловия:

Я не буду заострять внимание на этих понятиях, так как они уже нам знакомы из теории тестирования, лишь вкратце скажу:


- Предусловие - то, что будет выполнено перед запуском теста.
- Постусловие - то, что будет выполнено после прохождения теста.

Самый простой пример использования пред и постусловий - это вынос инициализации драйвера в предусловие и закрытие браузера в постусловие

Но перед реализацией примера давайте посмотрим, какие методы отвечают за `пред` и `пост-условия`:

- `setup()` - все, что находится внутри метода, будет выполнено перед запуском каждого теста внутри класса.
- `teardown()` - все, что находится внутри метода, будет выполнено после завершение каждого теста внутри класса

Структура будет выглядеть следующим образом:


```

class TestLogin: # Название тестового класса
		
     def setup(): # Пред-условие для тестов внутри класса
	  pass

     # Тут пишутся тесты

     def teardown(): # Пост-условие для тестов внутри класса
	  pass

```
Т.е по сути своей это обертка для наших тестов.
Давайте теперь реализуем вынос инициализации драйвера в `setup()`, а закрытие браузера вынесем в `teardown()`


```

class TestLogin: # Название тестового класса

    def setup(self): # Пред-условие для тестов внутри класса
        self.service = Service(ChromeDriverManager().install())
        self.driver = webdriver.Chrome(service=self.service)

    # Тут пишутся тесты

    def teardown(self): # Пост-условие для тестов внутри класса
        self.driver.close() # Закрытие браузера


```       
        
Ну а теперь добавим простейший тест на открытие страницы


```

class TestLogin: # Название тестового класса

    def setup(self):
	print("Выполняюсь до теста")
        self.service = Service(ChromeDriverManager().install())
        self.driver = webdriver.Chrome(service=self.service)

    def test_open_login_page(self):
        self.driver.get("https://demoqa.com/login")
        assert self.driver.current_url == "https://demoqa.com/login", "Ошибка"

    def teardown(self):
        self.driver.close()
	print("Выполняюсь после теста")
	
	
```

Принты добавлены, чтобы в терминале было явно видно, что до и после теста все работает, теперь просто запустим его нажав на кнопку `play` напротив теста. Все работает)

Но в случае, если мы запустим тест через терминал командой `pytest test_login.py`, то увидим, что наши принты не печатаются в терминале. Как этого избежать рассматривается в следующей главе.

</details>

<details>
<summary>Базовые опции запуска тестов</summary>	
	
## Базовые опции запуска тестов:
	
У pytest существует множество параметров для запуска, и они очень полезны, но в этой главе мы изучим 2 базовых.
- `-s` - данный параметр как раз будет отображать принты, которые прописаны в коде
- `-v` - данный параметр будет предоставлять расширенный лог запуска тестов
  
```

platform darwin -- Python 3.10.5, pytest - 7.2.0, pluggy-1.0.0
rootdir: /User/Mila/PytestClasses/1_Module_Pytest/lesson_2
collected 1 item

______________________________________
test_login.py Выполняюсь до теста    !
                                     !
Выполняюсь после теста               !
______________________________________

	
	
```
  
 <details>
  <summary>Пред- и постусловия</summary>
  
## Пред- и постусловия: 
