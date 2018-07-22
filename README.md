# Selenium Documentation 中文翻译版
## 1.读者注意 - Selenium 2.0文档持续修订中！
## 2.简介
### 2.1 Web应用的自动化测试
### 2.2 自动化还是不自动化？
### 2.3 引入Selenium
### 2.4 Selenium项目的简史
### 2.5 Selenium的工具套件
### 2.6 选择你的Selenium工具
### 2.7 支持的浏览器和平台
### 2.8 灵活性和可扩展性
### 2.9 这本书里有什么？
### 2.10 文档团队 - 过去和现在的作者
## 3.Selenium IDE
### 3.1 简介
### 3.2 安装IDE
### 3.3 打开IDE
### 3.4 IDE功能
### 3.5 从旧版IDE迁移
### 3.6 构建测试用例
### 3.7 运行测试用例
### 3.8 在不同域中使用Base URL运行测试用例
### 3.9 Selenium命令 - “Selenese”
### 3.10 脚本语法
### 3.11 测试套件
### 3.12 常用的Selenium命令
### 3.13 验证页面元素
### 3.14 断言或验证？
### 3.15 定位元素
### 3.16 存储命令和Selenium变量
### 3.17 echo - Selenese打印命令
### 3.18 Alerts, 弹出窗口和多窗口
### 3.19 调试
## 4.Selenium WebDriver
### 4.1 WebDriver介绍
Selenium 2.0的主要新功能是集成了WebDriver API。 除了解决Selenium-RC API中的一些限制之外，WebDriver还旨在提供更简单，更简洁的编程接口。 Selenium-WebDriver的开发是为了更好地支持动态网页，页面元素可能会在不重新加载页面的情况下发生变化。 WebDriver的目标是提供精心设计的面向对象的API，为现代高级Web应用程序测试问题提供改进的支持。
### 4.2 与Selenium-RC相比，WebDriver如何“驱动”浏览器？
Selenium-WebDriver使用每个浏览器自动化的本地支持直接调用浏览器。这些直接调用如何进行，以及它们支持的功能取决于你使用的浏览器。有关每个“浏览器驱动程序”的信息将在本章后面提供。

对于熟悉Selenium-RC的人来说，这与你习惯的完全不同。 Selenium-RC以相同的方式为每个支持的浏览器工作。它在浏览器加载时将“javascript函数”注入浏览器，然后使用其javascript在浏览器中驱动AUT。WebDriver不使用此技术。同样，它使用浏览器内置的自动化支持直接驱动浏览器。
### 4.3 WebDriver和Selenium-Server
您可能需要也可能不需要Selenium Server，具体取决于您打算如何使用Selenium-WebDriver。如果您的浏览器和测试都在同一台机器上运行，并且您的测试只使用WebDriver API，那么您不需要运行Selenium-Server; WebDriver将直接运行浏览器。

有一些原因可以将Selenium-Server与Selenium-WebDriver一起使用。

- 您正在使用Selenium-Grid在多台计算机或虚拟机（VM）上分发测试。
- 您希望连接到具有特定浏览器版本但不在当前计算机上的远程计算机。
- 您没有使用Java绑定（即Python，C＃或Ruby），并且希望使用HtmlUnit Driver
### 4.4 设置Selenium-WebDriver项目
安装Selenium意味着在开发中设置项目，以便您可以使用Selenium编写程序。 如何执行此操作取决于您的编程语言和开发环境。
在此以Python为例。
如果您使用Python进行自动化测试，那么您可能已经熟悉使用Python进行开发。 要将Selenium添加到Python环境，请从命令行运行以下命令。
> pip install selenium
Pip需要安装pip，pip也依赖于setuptools。
教学Python开发本身超出了本文档的范围，但是Python上有许多资源，组织中的开发人员可能会帮助您加快速度。
### 4.5 从Selenium 1.0迁移(略)
### 4.6 通过示例介绍Selenium-WebDriver API
WebDriver是一种用于自动化Web应用程序测试的工具，尤其是用于验证它们是否按预期工作。它旨在提供一个易于探索和理解的友好API，比Selenium-RC（1.0）API更易于使用，这将有助于使您的测试更易于阅读和维护。它不依赖于任何特定的测试框架，因此它可以在单元测试项目中使用，也可以从普通的“main”方法中使用。本节介绍WebDriver的API，帮助您开始熟悉它。首先，如果您还没有设置WebDriver项目。这在上一节“设置Selenium-WebDriver项目”中有所描述。

设置项目后，您可以看到WebDriver的行为与任何普通库一样：它完全是自包含的，您通常不需要记住在使用它之前启动任何其他进程或运行任何安装程序，而不是使用Selenium-RC到代理服务器。

注意：使用ChromeDriver，Opera Driver，Android Driver和iOS Driver需要执行其他步骤

你现在准备写一些代码了。一个简单的入门方法就是这个例子，它在Google上搜索“Cheese”，然后将结果页面的标题输出到控制台。
```python
from selenium import webdriver
from selenium.common.exceptions import TimeoutException
from selenium.webdriver.support.ui import WebDriverWait # 从2.4.0版本开始可以使用
from selenium.webdriver.support import expected_conditions as EC # 从2.26.0版本可以使用

# 创建一个新的Firefox driver实例
driver = webdriver.Firefox()

# 到达google主页
driver.get("http://www.google.com")

# 页面采用Ajax，所以页面原始title为以下：
print driver.title

# 找到name属性为q的元素(google搜索框)
inputElement = driver.find_element_by_name("q")

# 在搜索框输入
inputElement.send_keys("cheese!")

# 提交表单(尽管现在google在不提交时自动搜索)
inputElement.submit()

try:
    # 我们要等待也页面刷新，看起来最后更新的东西应该是title
    WebDriverWait(driver, 10).until(EC.title_contains("cheese!"))

    # 你应该看到了"cheese! - Google Search"
    print driver.title

finally:
    driver.quit()
```
在接下来的部分中，您将了解有关如何将WebDriver用于浏览器历史记录中向前和向后导航以及如何测试使用框架和窗口的网站等内容的更多信息。我们还提供了更全面的讨论和示例。
### 4.7 Selenium-WebDriver API命令和操作
- 获取一个页面
您可能想要使用WebDriver做的第一件事是导航到页面。 执行此操作的常规方法是调用“get”：
```Python
driver.get("http://www.google.com")
```
根据几个因素，包括操作系统/浏览器组合，WebDriver可能会也可能不会等待页面加载。在某些情况下，WebDriver可能会在页面加载完成之前，甚至开始加载之前返回控件。为确保稳健性，您需要使用显式和隐式等待等待页面中存在的元素。
- 定位UI元素（Web元素）
  1. 通过ID

        这是定位元素的最有效和首选方法。UI开发人员常见的陷阱是在页面上具有非唯一ID或自动生成id，两者都应该避免。html元素上的类比自动生成的id更合适。

        如何查找如下所示元素的示例：
        ```html
        <div id="coolestWidgetEvah">...</div>
        ```
        ```python
        element = driver.find_element_by_id("coolestWidgetEvah")

        or

        from selenium.webdriver.common.by import By
        element = driver.find_element(by=By.ID, value="coolestWidgetEvah")
        ```
  2. 通过Class Name

        在这种情况下，“Class”是指DOM元素上的属性。通常在实际使用中有许多具有相同类名的DOM元素，因此找到多个元素变得比找到第一个元素更实用。

        如何查找如下所示元素的示例：
        ```html
        <div class="cheese">
        <span>Cheddar</span>
        </div>
        <div class="cheese">
        <span>Gouda</span>
        </div>
        ```
        ```python
        cheeses = driver.find_elements_by_class_name("cheese")

        or

        from selenium.webdriver.common.by import By
        cheeses = driver.find_elements(By.CLASS_NAME, "cheese")
        ```
  3. 通过Tag Name

        DOM标签元素的名称。

        如何查找如下所示元素的示例：
        ```html
        <iframe src="..."></iframe>
        ```
        ```python
        frame = driver.find_element_by_tag_name("iframe")

        or

        from selenium.webdriver.common.by import By
        frame = driver.find_element(By.TAG_NAME, "iframe")
        ```
  4. 通过Name

        找到具有匹配name属性的input元素。

        如何查找如下所示元素的示例：
        ```html
        <input name="cheese" type="text"/>
        ```
        ```python
        cheese = driver.find_element_by_name("cheese")

        or

        from selenium.webdriver.common.by import By
        cheese = driver.find_element(By.NAME, "cheese")
        ```
  5. 通过Link Text

        找到具有匹配可见文本的链接元素。

        如何查找如下所示元素的示例：
        ```html
        <a href="http://www.google.com/search?q=cheese">cheese</a>>
        ```
        ```python
        cheese = driver.find_element_by_link_text("cheese")

        or

        from selenium.webdriver.common.by import By
        cheese = driver.find_element(By.LINK_TEXT, "cheese")
        ```
  6. 通过Partial Link
        
        找到部分匹配可见文本的链接元素。

        如何查找如下所示元素的示例：
        ```html
        <a href="http://www.google.com/search?q=cheese">search for cheese</a>>
        ```
        ```python
        cheese = driver.find_element_by_partial_link_text("cheese")

        or

        from selenium.webdriver.common.by import By
        cheese = driver.find_element(By.PARTIAL_LINK_TEXT, "cheese")
        ```
  7. 通过CSS

        就像名字所示它是css的定位策略。默认情况下使用本机浏览器支持，因此请参阅[w3c css选择器](http://www.w3.org/TR/CSS/#selectors)以获取通常可用的css选择器列表。如果浏览器没有css查询的本机支持，则使用Sizzle。 IE 6,7和FF3.0目前使用Sizzle作为css查询引擎。

        请注意，并非所有浏览器都是相同的，一些可能在一个版本中工作的CSS可能在另一个版本中不起作用。

        找到下面的cheese的例子：
        ```html
        <div id="food"><span class="dairy">milk</span><span class="dairy aged">cheese</span></div>
        ```
        ```python
        cheese = driver.find_element_by_css_selector("#food span.dairy.aged")

        or

        from selenium.webdriver.common.by import By
        cheese = driver.find_element(By.CSS_SELECTOR, "#food span.dairy.aged")
        ```
  8. 通过XPath

        在较高的层次上，WebDriver尽可能使用浏览器的本机XPath功能。在那些没有本机XPath支持的浏览器上，我们提供了自己的实现。除非您了解各种XPath引擎中的差异，否则这可能会导致一些意外行为。

        |Driver|标签和属性名称|属性值|本地XPath支持|
        |-|-|-|-|
        |HtmlUnitDriver|小写字母|与HTML出现的一致|是|
        |IE Driver|小写字母|与HTML出现的一致|否|
        |Firefox Driver|大小写不敏感|与HTML出现的一致|是|
        这有点抽象，请看下列HTML：
        ```html
        <input type="text" name="example" />
        <INPUT type="text" name="other" />
        ```
        ```python
        inputs = driver.find_elements_by_xpath("//input")

        or

        from selenium.webdriver.common.by import By
        inputs = driver.find_elements(By.XPATH, "//input")
        ```
        我们将会找到以下匹配数字：
        |XPath表达式|HtmlUnitDriver|Firefox Driver|IE Driver|
        |-|-|-|-|
        |//input|1("example")|2|2|
        |//INPUT|0|2|0|
        有时HTML元素不需要显式声明属性，因为它们将默认为已知值。 例如，“input”标记不需要“type”属性，因为它默认为“text”。 在WebDriver中使用xpath时的经验法则是，您不应期望能够匹配这些隐式属性。
  9. 使用JavaScript
        您可以执行任意javascript来查找元素，只要返回DOM元素，它就会自动转换为WebElement对象。

        加载jQuery的页面上的简单示例：
        ```python
        element = driver.execute_script("return $('.cheese')[0]")
        ```
        查找页面上每个标签的所有输入元素：
        ```python
        labels = driver.find_elements_by_tag_name("label")
        inputs = driver.execute_script(
            "var labels = arguments[0], inputs = []; for (var i=0; i < labels.length; i++){" +            "inputs.push(document.getElementById(labels[i].getAttribute('for'))); } return inputs;", labels)
        ```
- 获得文本值

人们通常希望检索元素中包含的innerText值。这将返回单个字符串值。请注意，这只会返回页面上显示的可见文本。

```python
element = driver.find_element_by_id("element_id")
element.text
```
- 用户输入-填充表单

我们已经看过如何在textarea或text字段中输入文本，但其他元素呢？您可以“切换”复选框的状态，也可以使用“单击”设置选择的OPTION标记。处理SELECT标签也不错：
```python
select = driver.find_element_by_tag_name("select")
allOptions = select.find_elements_by_tag_name("option")
for option in allOptions:
    print "Value is: " + option.get_attribute("value")
    option.click()
```
这将在页面上找到第一个“SELECT”元素，并依次遍历每个OPTION，打印出它们的值，然后依次选择每个OPTION。 正如您将注意到的，这不是处理SELECT元素的最有效方法。 WebDriver的支持类包括一个名为“Select”的支持类，它提供了与这些类进行交互的有用方法。
```python
# available since 2.12
from selenium.webdriver.support.ui import Select
select = Select(driver.find_element_by_tag_name("select"))
select.deselect_all()
select.select_by_visible_text("Edam")
```
这将从页面上的第一个SELECT中取消选择所有OPTION，然后选择显示“Edam”文本的OPTION。

填写完表单后，您可能想要提交表单。一种方法是找到“提交”按钮并单击它：
```python
driver.find_element_by_id("submit").click()
```
或者，WebDriver在每个元素上都有“提交”的便捷方法。如果在表单中的元素上调用它，WebDriver将向上走DOM，直到找到封闭的表单，然后调用submit。如果元素不在表单中，则抛出NoSuchElementException：
```python
element.submit()
```
- 在窗口和框架之间移动

某些Web应用程序具有许多帧或多个窗口。WebDriver支持使用“switchTo”方法在命名窗口之间移动：
```python
driver.switch_to.window("windowName")
```
现在，对驱动程序的所有调用都将被解释为定向到特定窗口。但你怎么知道窗口的名字？ 看一下打开它的javascript或链接：
```html
<a href="somewhere.html" target="windowName">Click here to open a new window</a>
```

153/5000
或者，您可以将“窗口句柄”传递给“switchTo().window()”方法。知道这一点，就可以迭代每个打开的窗口，如下所示：
```python
for handle in driver.window_handles:
    driver.switch_to.window(handle)
```
您还可以从frame切换到另一frame（或切换到iframe）：
```python
driver.switch_to.frame("frameName")
```
- 弹窗

从Selenium 2.0 beta 1开始，内置支持处理弹出对话框。触发打开弹出窗口的操作后，您可以使用以下命令访问警报：
```python
alert = driver.switch_to.alert
# usage: alert.dismiss(), etc.
```
这将返回当前打开的alert对象。使用此对象，您现在可以接受、关闭、读取其内容，甚至可以键入提示。此界面在alert,comfirm和prompts方面同样有效。有关更多信息，请参阅JavaDocs或RubyDocs。
- 导航：历史和定位

之前，我们介绍了在C＃中使用“get”命令`（driver.get（“http://www.example.com”）`或`driver.Url =“http://www.example.com”`导航到页面）。正如您所见，WebDriver具有许多较小的、以任务为中心的界面，导航是一项有用的任务。因为加载页面是一个基本要求，所以这样做的方法存在于主WebDriver接口上，但它只是一个同义词：
```python
driver.get("http://www.example.com")  # python doesn't have driver.navigate
```
重申：“navigate().to()”和“get()”完全相同。区别只是其中一个从键盘输入更快！
```python
driver.forward()
driver.back()
```
请注意，此功能完全取决于底层浏览器。如果您习惯于一个浏览器的行为而不是另一个浏览器，那么当您调用这些方法时，可能会发生意外情况。

“导航”界面还提供了在浏览器历史记录中前后移动的功能。
- Cookies

在我们离开这些后续步骤之前，您可能有兴趣了解如何使用cookie。首先，您需要在cookie有效的域上。如果您在开始与网站交互之前尝试预设Cookie并且您的主页很大/需要一段时间来加载，替代方法是在网站上找到一个较小的网页（通常404页面很小，例如`http://example.com/some404page`）。
```python
# Go to the correct domain
driver.get("http://www.example.com")

# Now set the cookie. Here's one for the entire domain
# the cookie name here is 'key' and its value is 'value'
driver.add_cookie({'name':'key', 'value':'value', 'path':'/'})
# additional keys that can be passed in are:
# 'domain' -> String,
# 'secure' -> Boolean,
# 'expiry' -> Milliseconds since the Epoch it should expire.

# And now output all the available cookies for the current URL
for cookie in driver.get_cookies():
    print "%s -> %s" % (cookie['name'], cookie['value'])

# You can delete cookies in 2 # Go to the correct domain
driver.get("http://www.example.com")

# Now set the cookie. Here's one for the entire domain
# the cookie name here is 'key' and its value is 'value'
driver.add_cookie({'name':'key', 'value':'value', 'path':'/'})
# additional keys that can be passed in are:
# 'domain' -> String,
# 'secure' -> Boolean,
# 'expiry' -> Milliseconds since the Epoch it should expire.

# And now output all the available cookies for the current URL
for cookie in driver.get_cookies():
    print "%s -> %s" % (cookie['name'], cookie['value'])

# You can delete cookies in 2 ways
# By name
driver.delete_cookie("CookieName")
# Or all of them
driver.delete_all_cookies()ways
# By name
driver.delete_cookie("CookieName")
# Or all of them
driver.delete_all_cookies()
```
- 改变用户代理

用Firefox Driver实现很简单：
```python
profile = webdriver.FirefoxProfile()
profile.set_preference("general.useragent.override", "some UA string")
driver = webdriver.Firefox(profile)
```
- 拖拽操作

这是使用Actions类执行拖放的示例。 需要启用本机事件。
```python
from selenium.webdriver.common.action_chains import ActionChains
element = driver.find_element_by_name("source")
target =  driver.find_element_by_name("target")

ActionChains(driver).drag_and_drop(element, target).perform()
```
### 4.8 Driver细节和权衡
### 4.9 Selenium-WebDriver的驱动程序
- HTML单元驱动
  1. 使用
  2. 优点
  3. 缺点
  4. HTML单元驱动中的JavaScript
  5. 赋能JavaScript 
- Firefox驱动(略)
- IE驱动（略）
- Chrome驱动
  1. 使用
  2. 优点
  3. 缺点
  4. 信息
  5. 开始使用ChromeDriver
- Opera驱动（略）
- iOS驱动
- Android驱动
### 4.10 备选后端：混合WebDriver和RC技术
- 优点
- 缺点
### 4.11 运行Standalone Selenium Server以与RemoteDrivers一起使用
### 4.12 其他资源
### 4.13 后续步骤
## 5.WebDriver：高级用法
### 5.1 显式和隐式等待
### 5.2 RemoteWebDriver
### 5.3 高级用户交互
### 5.4 浏览器启动操作
### 5.5 HTML5
### 5.6 并行化您的测试运行
## 6.Selenium 1（Selenium RC）
### 6.1 简介
### 6.2 Selenium RC的工作原理
### 6.3 安装
### 6.4 从Selenese到程序
### 6.5 编程测试
### 6.6 学习API
### 6.7 报告结果
### 6.8 为您的测试添加一些料
### 6.9 服务器选项
### 6.10 指定特定浏览器的路径
### 6.11 Selenium RC架构
### 6.12 处理HTTPS和安全弹出窗口
### 6.13 支持其他浏览器和浏览器配置
### 6.14 排除常见问题
## 7.测试设计注意事项
### 7.1 介绍测试设计
### 7.2 测试类型
### 7.3 验证结果
### 7.4 位置策略
### 7.5 包装Selenium请求
### 7.6 用户界面映射
### 7.7 页面对象设计模式
### 7.8 数据驱动测试
### 7.9 数据库验证
## 8.Selenium-Grid
### 8.1 快速入门
### 8.2 什么是Selenium-Grid？
### 8.3 何时使用它
### 8.4 Selenium-Grid 2.0
### 8.5 Selenium-Grid如何工作 - 使用Hub和节点
### 8.6 安装
### 8.7 启动Selenium-Grid
### 8.8 配置Selenium-Grid
### 8.9 Hub配置
### 8.10 节点配置
### 8.11 时序参数
### 8.12 自定义Grid
### 8.13 获取命令行帮助
### 8.14 常见错误
### 8.15 故障排除
## 9.用户扩展
### 9.1 简介
### 9.2 操作
### 9.3 访问器/断言
### 9.4 定位策略
### 9.5 在Selenium-IDE中使用User-Extensions
### 9.6 在Selenium RC中使用User-Extensions
## 10.旧版Selenium IDE
### 10.1 简介
### 10.2 安装IDE
### 10.3 打开IDE
### 10.4 IDE功能
### 10.5 构建测试用例
### 10.6 运行测试用例
### 10.7 使用Base URL在不同域中运行测试用例
### 10.8 Selenium命令 - “Selenese”
### 10.9 脚本语法
### 10.10 测试套件
### 10.11 常用的Selenium命令
### 10.12 验证页面元素
### 10.13 断言或验证？
### 10.14 定位元素
### 10.15 匹配文本模式
### 10.16 “AndWait”命令
### 10.17 AJAX应用程序中的waitFor命令
### 10.18 评估和流程控制的顺序
### 10.19 存储命令和Selenium变量
### 10.20 JavaScript和Selenese参数
### 10.21 echo - Selenese打印命令
### 10.22 Alert, 弹出窗口和多窗口
### 10.23 调试
### 10.24 编写测试套件
### 10.25 用户扩展
### 10.26 格式
### 10.27 在不同的浏览器上执行Selenium-IDE测试
### 10.28 故障排除
## 附录：
## 11 .NET客户端驱动程序配置
## 12.使用Maven将Sel2.0项目导入Eclipse
## 13.使用Maven将Sel2.0项目导入IntelliJ
## 14.从Selenium RC迁移到Selenium WebDriver
### 14.1 如何迁移到Selenium WebDriver
### 14.2 为什么要迁移到WebDriver
### 14.3 开始之前
### 14.4 入门
### 14.5 后续步骤
### 14.6 常见问题
