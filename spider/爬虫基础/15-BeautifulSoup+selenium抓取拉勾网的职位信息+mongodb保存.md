BeautifulSoup+selenium抓取拉勾网的职位信息



```python

"""

使用自动化测试工具 selenium 和 BeautifulSoup 抓取 拉钩网的职位信息

"""


import time
import pymongo

from selenium import webdriver
from bs4 import BeautifulSoup


def get_html(url, keywords):
    """
     获取 页面 返回获取的页面列表
    :param url: 目标网站 这是是 拉钩网
    :param keywords: 搜索的关键字
    :return: 获取的页面列表
    """
    # 存放 获取的页面的容器， 最后返回此容器
    page_html_list = []
    chromeDriver = 'D:/00venv/soft/chromedriver_win32/chromedriver.exe'
    browser = webdriver.Chrome(chromeDriver)
    
    # 后台运行 使用 phantomjs   下载：http://phantomjs.org/download.html
    # chromeDriver = r"D:\00venv\soft\phantomjs-2.1.1-windows\bin\phantomjs.exe"
    # browser = webdriver.PhantomJS(chromeDriver)
    
    browser.get(url)  # 获取页面首页
    time.sleep(3)
    # 首页 弹框 需要选择城市 这里选择的是成都
    browser.find_element_by_xpath('//*[@id="changeCityBox"]/ul/li[7]/a').click()
    time.sleep(2)
    # 全国
    all_in_china = browser.find_element_by_xpath('//*[@id="filterCollapse"]/div[1]/div[2]/li/div[1]/a')
    #  切换到 全国进行查找
    all_in_china.click()
    time.sleep(2)
    
    # 其他城市 a[1] - a[13]  更多需要切换页面 暂时就这么多
    # 可以通过循环来 获取 这里暂时不写
    # city = browser.find_element_by_xpath('//*[@id="filterCollapse"]/div[1]/div[2]/li/div[2]/div/a[4]')

    # 进入页面后 搜索的  元素框是不变的， 所有可以放在外面， 只需要在循环中添加关键字就行
    search = browser.find_element_by_xpath('//*[@id="search_input"]')
    for keyword in keywords:
        # 将关键字写入到搜索框中
        search.send_keys(keyword)
        # 点击搜索
        browser.find_element_by_xpath('//*[@id="search_button"]').click()
        # 点击事件后 休眠 2 秒 等待页面全部加载出来
        time.sleep(2)
        #  第一次获取失败后 尝试的 次数， 这里设置的是三次，三次还获取不到，进入下一页
        retry_time = 0
        # 默认是第一页， 换下一页从 2 开始
        page_num = 2
        # 设置标志为， 循环终止条件
        flag = True
        while flag:
            # 下一页
            try:
                next_page = browser.find_element_by_xpath('//*[@id="s_position_list"]/div[2]/div/span[@page="%s"]' % page_num)
                next_page.click()
                time.sleep(2)
                # 获取页面
                page_html = browser.page_source
                # 页面添加到列表中
                page_html_list.append(page_html)
                # 一次获取成功 页码加 1
                page_num += 1
                # 判断下一页的 下一页  因为最后有 next 这个按钮， 判断 next 后还有没有元素 来终止循环
                try:
                    browser.find_element_by_xpath('//*[@id="s_position_list"]/div[2]/div/span[@page="%s"]' % page_num + 1)
                except:
                    flag = False
                    pass
            except:
                retry_time += 1
                print('第 %s 页，第 %s 尝试抓取！' % (page_num, retry_time))
                if retry_time > 3:
                    print('结束获取页面')
                    page_num += 1
    # 关闭浏览器
    browser.quit()
    return page_html_list


def main():

    mongo = pymongo.MongoClient('mongodb://127.0.0.1:27017')
    db = mongo.spider

    url = 'https://www.lagou.com/'
    keywords = ['python']
    # keywords = ['python', '爬虫', '大数据']
    page_html_list = get_html(url, keywords)  # 获取所有的网页信息
    for page_html in page_html_list:
        page = BeautifulSoup(page_html, 'lxml')   # 初始化 bs 对象
        company_list = page.find_all('div', {'class', 'list_item_top'}) # 获取每页的公司列表
        for company in company_list:  # 遍历 获取需要的信息

            company_name = company.find("", {'class': "company_name"}).find('a').get_text()
            job = company.find('h3').get_text()
            salary = company.find('span', {'class': 'money'}).get_text()
            # 插入数据库
            db.lagou.insert({'公司：': company_name, '职位：': job, '工资：': salary})


if __name__ == '__main__':
    main()


```

