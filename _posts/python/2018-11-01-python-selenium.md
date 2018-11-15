---
layout: post
title: Python使用Selenium包做Web页面自动化测试
categories: python
description: 实现像用户一样的点击输入等操作
keywords: python, chrome, selenuim
---

Python使用Selenium包做Web页面自动化测试，像有人在操控一样自动化操控。       


1，前置条件
-----
* 安装python
* 安装Selenium
* 安装chromedriver或geckodriver，并将其所在文件路径添加到path上

2，打开指定页面
-----

    # 引入webdriver
    from selenium import webdriver

    # 目前支持的driver有 Firefox, Chrome, IE和Remote等
    driver = webdriver.Chrome()

    # 提前设置页面的宽高
    driver.set_window_size(1740,900)

    # 直到页面被加载完（onload事件被触发）才将控制权返回脚本
    driver.get('https://www.baidu.com')
    
    # 断言，验证一下页面的title是否为'百度一下，你就知道'
    assert '百度一下，你就知道' in driver.title

    # 控制台打印当前url
    print ("当前URL：", driver.current_url)

3，元素定位
-----
元素定位的方式有很多，单个元素定位推荐使用`find_element_by_xpath`     
（刚开始以为`find_element_by_css_selector`对于前端工作者而言更好用，结果在实际使用的时候不尽人意）

    find_element_by_id：通过id属性定位元素（返回第一个匹配的）。
    find_element_by_name：通过name属性定位元素（返回第一个匹配的）。
    find_element_by_xpath：通过xpath定位元素（返回第一个匹配的）。
    find_element_by_link_text：通过超链接文本定位超链接元素，必须是完全匹配（返回第一个匹配的）。
    find_element_by_partial_link_text：通过超链接文本定位超链接元素，可以是部分匹配（返回第一个匹配的）。
    find_element_by_tag_name：通过标签名字定位元素（返回第一个匹配的）。
    find_element_by_class_name：通过class属性定位元素（返回第一个匹配的）。
    find_element_by_css_selector：使用CSS选择器语法定位元素（返回第一个匹配的）。

上面的方法，对应的批量定位方法如下（返回对应网页元素的列表）：

    find_elements_by_name
    find_elements_by_xpath
    find_elements_by_link_text
    find_elements_by_partial_link_text
    find_elements_by_tag_name
    find_elements_by_class_name
    find_elements_by_css_selector

此外，webdriver还有find_element及find_elements方法定位元素：

    from selenium.webdriver.common.by import By

    driver.find_element(By.XPATH, '//button[text()="Sometext"]')
    driver.find_elements(By.XPATH, '//button')


规则：`find_element_by_xpath`会返回匹配的第一个元素

划重点：        
1，在定位元素时，在选取了第一个元素后，后面的选择只能一级一级往下定位，否则定位不到目标元素。       
**这一点在`find_element_by_css_selector`方法上显得尤为清晰**        
eg:

    driver.find_element_by_css_selector(".main>content>box>span")
    不能省略为:
    driver.find_element_by_css_selector(".main>box>span")

    driver.find_element_by_xpath("//div[@class='box']/div[@class='later']/div[@class='main']/span")
    不能省略为：
    driver.find_element_by_xpath("//div[@class='box']/div[@class='main']/span")

4，用户操作事件模拟
-----
等待

    import time
    // 强制等待
    time.sleep(3)  // 等待3秒钟
    // 隐式等待 ---- 3秒内页面加载完成了就结束等待
    driver.implicitly_wait(3)
    // 显式等待  ---- 每隔3秒检查一次
    WebDriverWait(driver, 3).until(...)
    

点击事件

    driver.find_element_by_xpath("//div[@class='box']/span").click()

文本框输入事件
    
    driver.find_element_by_xpath("//input[@class='input']").send_keys("admin")

清空文本框

    driver.find_element_by_xpath("//input[@class='input']").clear()

处理弹窗

点击alert框
    
    from selenium.webdriver.support.ui import WebDriverWait
    driver.find_element_by_css_selector("button[id='alert']").click()
    // 先出弹框，才有driver.switch_to.alert
    confirm = driver.switch_to.alert
    time.sleep( 1 )
    confirm.accept()

点击confirm框
    
    driver.find_element_by_css_selector(".box>button:nth-of-type(2)").click()
    time.sleep( 1 )
    # 点击弹出框的确定按钮
    confirm.accept()
    # 点击弹出框的取消按钮
    # alert.dismiss()

前进/后退

    # 后退
    driver.back()
    # 前进
    driver.forward()


5，页面操作
----
设置浏览器尺寸

    # 设置成指定尺寸
    driver.set_window_size(1740,900)
    # 设置成全屏
    driver.maximize_window()

设置/获取cookie

    driver.add_cookie({'name': 'username', 'value': 'marsloo'})
    print (driver.get_cookies())

获取元素的属性/批量操作

    li_list = driver.find_elements_by_xpath("//ul/li")
    // 可以是自定义属性，也可以是自定义属性
    for item in li_list:
        print ("class is: " , item.get_attribute("class"))
        print ("data-id is: " , item.get_attribute("data-id"))
        item.click()

滚动至屏幕底部

    driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")

其他操作

    # 表单元素
    form = driver.find_element_by_name('survey')
    form.submit()

6，关闭浏览器
----

    driver.quit()