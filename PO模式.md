## PO模式

### Page Object Model

```
	测试页面和测试脚本分离，即页面封装成类，供测试脚本进行调用。

```

### 优缺点

- 优点

```
	1.提高测试用例的可读性;
	2.减少了代码的重复;
	3.提高测试用例的可维护性，特别是针对UI频繁变动的项目;

```

- 缺点

```
	结构复杂: 基于流程做了模块化的拆分。

```

![po模式](/Users/Yoson/Desktop/MobileTestNote/笔记/移动端测试_image/po&%E9%9D%9E.png)



## 项目准备

### 需求

- 更多-移动网络-首选网络类型-点击2G
- 更多-移动网络-首选网络类型-点击3G
- 显示-搜索按钮-输入hello-点击返回

### 文件目录

```
PO模式
- scripts
- - __init__.py
- - test_settting.py
- pytest.ini
```

### 代码

test_setting.py

```
from appium import webdriver


class TestSetting:

    def setup(self):
        # server 启动参数
        desired_caps = dict()
        # 设备信息
        desired_caps['platformName'] = 'Android'
        desired_caps['platformVersion'] = '5.1'
        desired_caps['deviceName'] = '192.168.56.101:5555'
        # app的信息
        desired_caps['appPackage'] = 'com.android.settings'
        desired_caps['appActivity'] = '.Settings'
        # 解决输入中文
        desired_caps['unicodeKeyboard'] = True
        desired_caps['resetKeyboard'] = True

        # 声明我们的driver对象
        self.driver = webdriver.Remote('http://127.0.0.1:4723/wd/hub', desired_caps)

    def test_mobile_network_2g(self):
        self.driver.find_element_by_xpath("//*[contains(@text,'更多')]").click()
        self.driver.find_element_by_xpath("//*[contains(@text,'移动网络')]").click()
        self.driver.find_element_by_xpath("//*[contains(@text,'首选网络类型')]").click()
        self.driver.find_element_by_xpath("//*[contains(@text,'2G')]").click()

    def test_mobile_network_3g(self):
        self.driver.find_element_by_xpath("//*[contains(@text,'更多')]").click()
        self.driver.find_element_by_xpath("//*[contains(@text,'移动网络')]").click()
        self.driver.find_element_by_xpath("//*[contains(@text,'首选网络类型')]").click()
        self.driver.find_element_by_xpath("//*[contains(@text,'3G')]").click()

    def test_mobile_display_input(self):
        self.driver.find_element_by_xpath("//*[contains(@text,'显示')]").click()
        self.driver.find_element_by_id("com.android.settings:id/search").click()
        self.driver.find_element_by_id("android:id/search_src_text").send_keys("hello")
        self.driver.find_element_by_class_name("android.widget.ImageButton").click()

```

pytest.ini

```
[pytest]
# 添加行参数
addopts = -s --html=./report/report.html
# 文件搜索路径
testpaths = ./scripts
# 文件名称
python_files = test_*.py
# 类名称
python_classes = Test*
# 方法名称
python_functions = test_*
```

## 多文件区分测试用例

### 需求

- 使用多个文件来区分不同的测试页面

### 好处

- 修改不同的功能找对应的文件即可

### 步骤

1. 在scripts下新建test_network.py文件
2. 在scripts下新建test_dispaly.py文件
3. 移动不同的功能代码到对应的文件
4. 移除原有的test_setting.py文件

### 文件目录

```
PO模式
- scripts
- - __init__.py
- - test_network.py
- - test_dispaly.py
- pytest.ini

```

### 代码

test_network.py

```
from appium import webdriver


class TestNetwork:

    def setup(self):
        # server 启动参数
        desired_caps = dict()
        # 设备信息
        desired_caps['platformName'] = 'Android'
        desired_caps['platformVersion'] = '5.1'
        desired_caps['deviceName'] = '192.168.56.101:5555'
        # app的信息
        desired_caps['appPackage'] = 'com.android.settings'
        desired_caps['appActivity'] = '.Settings'
        # 解决输入中文
        desired_caps['unicodeKeyboard'] = True
        desired_caps['resetKeyboard'] = True

        # 声明我们的driver对象
        self.driver = webdriver.Remote('http://127.0.0.1:4723/wd/hub', desired_caps)

    def test_mobile_network_2g(self):
        self.driver.find_element_by_xpath("//*[contains(@text,'更多')]").click()
        self.driver.find_element_by_xpath("//*[contains(@text,'移动网络')]").click()
        self.driver.find_element_by_xpath("//*[contains(@text,'首选网络类型')]").click()
        self.driver.find_element_by_xpath("//*[contains(@text,'2G')]").click()

    def test_mobile_network_3g(self):
        self.driver.find_element_by_xpath("//*[contains(@text,'更多')]").click()
        self.driver.find_element_by_xpath("//*[contains(@text,'移动网络')]").click()
        self.driver.find_element_by_xpath("//*[contains(@text,'首选网络类型')]").click()
        self.driver.find_element_by_xpath("//*[contains(@text,'3G')]").click()
```

test_dispaly.py

```
from appium import webdriver


class TestDisplay:

    def setup(self):
        # server 启动参数
        desired_caps = dict()
        # 设备信息
        desired_caps['platformName'] = 'Android'
        desired_caps['platformVersion'] = '5.1'
        desired_caps['deviceName'] = '192.168.56.101:5555'
        # app的信息
        desired_caps['appPackage'] = 'com.android.settings'
        desired_caps['appActivity'] = '.Settings'
        # 解决输入中文
        desired_caps['unicodeKeyboard'] = True
        desired_caps['resetKeyboard'] = True

        # 声明我们的driver对象
        self.driver = webdriver.Remote('http://127.0.0.1:4723/wd/hub', desired_caps)

    def test_mobile_display_input(self):
        self.driver.find_element_by_xpath("//*[contains(@text,'显示')]").click()
        self.driver.find_element_by_id("com.android.settings:id/search").click()
        self.driver.find_element_by_id("android:id/search_src_text").send_keys("hello")
        self.driver.find_element_by_class_name("android.widget.ImageButton").click()
```

## 封装前置代码

### 需求

- 将前置代码进行封装

### 好处

- 前置代码只需要写一份

### 步骤

1. 新建base文件夹
2. 新建base_driver.py文件
3. 新建函数init_driver
4. 写入前置代码并返回
5. 修改测试文件中的代码

### 文件目录

```
PO模式
- base
- - __init__.py
- - base_driver.py
- scripts
- - __init__.py
- - test_network.py
- - test_dispaly.py
- pytest.ini

```

### 代码

base_driver.py

```
from appium import webdriver


def init_driver():
    # server 启动参数
    desired_caps = dict()
    # 设备信息
    desired_caps['platformName'] = 'Android'
    desired_caps['platformVersion'] = '5.1'
    desired_caps['deviceName'] = '192.168.56.101:5555'
    # app的信息
    desired_caps['appPackage'] = 'com.android.settings'
    desired_caps['appActivity'] = '.Settings'
    # 解决输入中文
    desired_caps['unicodeKeyboard'] = True
    desired_caps['resetKeyboard'] = True

    # 声明我们的driver对象
    return webdriver.Remote('http://127.0.0.1:4723/wd/hub', desired_caps)

```

test_network.py

```
from base.base_driver import init_driver


class TestNetwork:

    def setup(self):
        self.driver = init_driver()

    def test_mobile_network_2g(self):
        self.driver.find_element_by_xpath("//*[contains(@text,'更多')]").click()
        self.driver.find_element_by_xpath("//*[contains(@text,'移动网络')]").click()
        self.driver.find_element_by_xpath("//*[contains(@text,'首选网络类型')]").click()
        self.driver.find_element_by_xpath("//*[contains(@text,'2G')]").click()

    def test_mobile_network_3g(self):
        self.driver.find_element_by_xpath("//*[contains(@text,'更多')]").click()
        self.driver.find_element_by_xpath("//*[contains(@text,'移动网络')]").click()
        self.driver.find_element_by_xpath("//*[contains(@text,'首选网络类型')]").click()
        self.driver.find_element_by_xpath("//*[contains(@text,'3G')]").click()
```

test_dispaly.py

```
from base.base_driver import init_driver


class TestDisplay:

    def setup(self):
        self.driver = init_driver()

    def test_mobile_display_input(self):
        self.driver.find_element_by_xpath("//*[contains(@text,'显示')]").click()
        self.driver.find_element_by_id("com.android.settings:id/search").click()
        self.driver.find_element_by_id("android:id/search_src_text").send_keys("hello")
        self.driver.find_element_by_class_name("android.widget.ImageButton").click()
```

## 前置代码导入模块问题

### 步骤

在base的 \__init__.py 中写入

```
from base.base_driver import init_driver
```

可将test脚本中的（test_network&test_display）

```
from base.base_driver import init_driver
```

修改为（test_network&test_display）

```
from base import init_driver
```

另外，在base的 \__init__.py 因为是导入自己的模块下的某个文件，可将前面的模块名省略

```
from .base_driver import init_driver
```

### 代码

base/\__init__.py

```
from .base_driver import init_driver
```

### 好处

系统内部都是这么写的，也更省事儿。

## 分离测试脚本和页面

### 需求

- 测试脚本只剩流程
- 其他的步骤放倒page中

### 好处

- 测试脚本只专注过程
- 过程改变只需要修改脚本

### 步骤

1. 新建page文件夹
2. 新建network_page.py文件
3. 新建display_page.py文件
4. init函数传入driver
5. init进入需要测试的页面
6. page中新建“小动作”函数
7. 移动代码
8. 修改测试文件中的代码

### 文件目录

```
PO模式
- base
- - __init__.py
- - base_driver.py
- page
- - __init__.py
- - network_page.py
- - display_page.py
- scripts
- - __init__.py
- - test_network.py
- - test_dispaly.py
- pytest.ini
```

### 代码

display_page.py

```
class DisplayPage:

    def __init__(self, driver):
        self.driver = driver

    def click_display(self):
        self.driver.find_element_by_xpath("//*[contains(@text,'显示')]").click()

    def click_search(self):
        self.driver.find_element_by_id("com.android.settings:id/search").click()

    def input_search_keyword(self, text):
        self.driver.find_element_by_id("android:id/search_src_text").send_keys(text)

    def click_back(self):
        self.driver.find_element_by_class_name("android.widget.ImageButton").click()
```



network_page.py

```
class NetworkPage:

    def __init__(self, driver):
        self.driver = driver

    def click_more(self):
        self.driver.find_element_by_xpath("//*[contains(@text,'更多')]").click()

    def click_mobile_network(self):
        self.driver.find_element_by_xpath("//*[contains(@text,'移动网络')]").click()

    def click_first_network(self, text):
        self.driver.find_element_by_xpath("//*[contains(@text,'首选网络类型')]").click()

    def click_2g_network(self):
        self.driver.find_element_by_xpath("//*[contains(@text,'2G')]").click()

    def click_3g_network(self):
        self.driver.find_element_by_xpath("//*[contains(@text,'3G')]").click()
```



test_display.py

```
from base import init_driver
from page.display_page import DisplayPage


class TestDisplay:

    def setup(self):
        self.driver = init_driver()
        self.display_page = DisplayPage(self.driver)

    def test_mobile_display_input(self):
        self.display_page.click_display()
        self.display_page.click_search()
        self.display_page.input_search_keyword("hello")
        self.display_page.click_back()
```



test_network.py

```
from base import init_driver
from page.network_page import NetworkPage


class TestNetwork:

    def setup(self):
        self.driver = init_driver()
        self.network_page = NetworkPage(self.driver)

    def test_mobile_network_2g(self):
        self.network_page.click_more()
        self.network_page.click_mobile_network()
        self.network_page.click_first_network()
        self.network_page.click_2g_network()

    def test_mobile_network_3g(self):
        self.network_page.click_more()
        self.network_page.click_mobile_network()
        self.network_page.click_first_network()
        self.network_page.click_3g_network()
```

## 输入文字由脚本传入

### 需求

将display配置中的hello移动到test脚本中。

input方法，需要传一个要输入文字的参数，并在方法中使用。

### 好处

- 方便脚本做参数化

### 步骤

### 文件目录

### 代码

## 实验 - find_element_by_xxx和find_element

```
self.driver.find_element_by_xpath("//*[contains(@text,'移动网络')]")
<==>
self.driver.find_element(By.XPATH, "//*[contains(@text,'移动网络')]")
```

class_name 和 id 一样。

可以将字符串部分，写在最上面。

当获取方式不变，获取的字符串的部分发生改变的时候，可以快速的修改。

此时会发现，如果有一个元素的获取方式发生变换后，需要上下改两个地方。先保留疑问。

## 实验 - 元组的写法

```
a = 1, 2
```

a 本质上是一个元组

## 实验 - 二次封装find_element

自己写一个find_element方法。

调用系统的find_element方法，并接受系统的返回值，系统需要传两个参数，我们的方法传一个参数，但是是一个元组，里面有两个元素。第一个元素表示系统方法的第一个参数，第二个元素表示系统方法的第二个参数。

```
# 元素的特征(元组)
mobile_network_button = By.XPATH, "//*[contains(@text,'移动网络')]"

#调用
find_element(self.mobile_network_button)

# 封装后的函数
def find_element(feature):
	# 调用系统的方法
	return self.driver.find_element(feature[0], feature[1])
```

## 抽取find_element和元素的特征到page

### 需求

- 将find_element_by_xxx改为driver中的find_element方法

### 好处

- 提高复用性

### 步骤

1. 观察find_element方法，需要两个参数，一个是找的方式（by），一个是找什么（字符串）。
2. 自己在page中单独写一个find_element方法，调用系统并使用元组的形式进行传参

### 文件目录

```
PO模式
- base
- - __init__.py
- - base_driver.py
- page
- - __init__.py
- - network_page.py
- - display_page.py
- scripts
- - __init__.py
- - test_network.py
- - test_dispaly.py
- pytest.ini
```



### 代码

## 抽取点击和输入到base_action

### 需求

### 好处

### 步骤

### 文件目录

### 代码

## 抽取find_element到base_action

### 需求

### 好处

### 步骤

### 文件目录

### 代码

## 增加WebDriverWait

### 需求

### 好处

### 步骤

### 文件目录

### 代码

## page的统一入口

### 需求

### 好处

### 步骤

### 文件目录

### 代码

## XPath特殊处理

### 需求

### 好处

### 步骤

### 文件目录

### 代码