---
title: "Python网络爬虫学习笔记"
date: 2025-06-15  # 可选，手动指定日期
layout: default      # 可选，指定布局（如 `post` 或 `default`）
categories: [Spider, Python]  # 可选，添加分类
---
# Python网络爬虫学习笔记

## 一、网络数据采集概述

### 1. 爬虫基本概念
- **定义**：爬虫（crawler）也叫网络蜘蛛（spider），是按一定规则自动浏览网站并获取信息的机器人程序。
- **工作原理**：通过网页超链接不断获取其他页面地址，实现数据采集。

### 2. 应用领域
- 搜索引擎、新闻聚合、社交应用、舆情监控、行业数据等。

### 3. 合法性探讨
- **Robots协议**：网站定义的`robots.txt`文件，是君子协议，非强制。
- **合法要点**：
  - 只爬取前端公开数据，不碰后台敏感信息。
  - 遵守Robots协议，控制抓取速度。
  - 尊重网站知识产权，不破坏服务器。
  - 隐匿身份，不公开爬虫代码。

### 4. HTTP协议基础
- **请求组成**：请求行、请求头、空行、消息体。
- **响应组成**：响应行、响应头、空行、消息体。
- **常用状态码**：
  - 200 OK：请求成功。
  - 404 Not Found：页面未找到。
  - 500 Internal Server Error：服务器内部错误。

### 5. 开发工具
- **Chrome Developer Tools**：查看页面元素、网络请求等。
- **Postman**：模拟HTTP请求，调试接口。
- **HTTPie**：命令行HTTP客户端。
- **builtwith**：识别网站技术栈。
- **python-whois**：查询网站所有者信息。

### 6. 爬虫基本流程
1. 设定抓取目标并获取网页。
2. 处理服务器无法访问的情况。
3. 设置用户代理或隐藏IP。
4. 解码页面并提取信息。
5. 提取页面链接。
6. 处理链接并重复抓取。
7. 持久化有用信息。

## 二、用Python获取网络资源

### 1. requests库
- **基本用法**：
  ```python
  import requests
  resp = requests.get('https://www.sohu.com/')
  if resp.status_code == 200:
      print(resp.text)  # 获取HTML文本
  ```
- **获取二进制资源**：
  ```python
  resp = requests.get('图片URL')
  with open('filename.png', 'wb') as f:
      f.write(resp.content)
  ```
- **设置请求头**：
  ```python
  headers = {'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_6)...'}
  resp = requests.get(url, headers=headers)
  ```

### 2. 编写爬虫示例（豆瓣电影Top250）
```python
import requests
import re
import time
import random

for page in range(1, 11):
    resp = requests.get(
        url=f'https://movie.douban.com/top250?start={(page-1)*25}',
        headers={'User-Agent': 'BaiduSpider'}
    )
    # 提取电影标题和评分
    titles = re.findall(r'<span class="title">([^&]*?)</span>', resp.text)
    ranks = re.findall(r'<span class="rating_num".*?>(.*?)</span>', resp.text)
    for title, rank in zip(titles, ranks):
        print(title, rank)
    time.sleep(random.random() * 4 + 1)  # 随机休眠，避免频繁请求
```

### 3. 使用IP代理
- **作用**：隐匿爬虫身份，避免IP被封。
- **示例（蘑菇代理）**：
  ```python
  import requests
  APP_KEY = '你的AppKey'
  PROXY_HOST = 'secondtransfer.moguproxy.com:9001'
  
  resp = requests.get(
      url,
      headers={
          'Proxy-Authorization': f'Basic {APP_KEY}',
          'User-Agent': 'Mozilla/5.0...'
      },
      proxies={
          'http': f'http://{PROXY_HOST}',
          'https': f'https://{PROXY_HOST}'
      },
      verify=False
  )
  ```

## 三、用Python解析HTML页面

### 1. HTML页面结构
- **基本结构**：
  ```html
  <!doctype html>
  <html>
      <head>
          <!-- 元信息 -->
      </head>
      <body>
          <!-- 页面主体 -->
      </body>
  </html>
  ```

### 2. 解析方法对比

| 解析方式   | 模块     | 速度 | 使用难度 |
|------------|----------|------|----------|
| 正则表达式 | re       | 快   | 困难     |
| XPath      | lxml     | 快   | 一般     |
| CSS选择器  | bs4/pyquery | 不确定 | 简单   |

### 3. XPath解析
- **安装**：`pip install lxml`
- **示例**：
  ```python
  from lxml import etree
  tree = etree.HTML(html_text)
  # 提取电影标题
  titles = tree.xpath('//*[@id="content"]/div/div[1]/ol/li/div/div[2]/div[1]/a/span[1]/text()')
  ```

### 4. CSS选择器解析（BeautifulSoup）
- **安装**：`pip install beautifulsoup4`
- **示例**：
  ```python
  import bs4
  soup = bs4.BeautifulSoup(html_text, 'lxml')
  # 提取电影标题
  titles = soup.select('div.info > div.hd > a > span:nth-child(1)')
  for title in titles:
      print(title.text)
  ```

## 四、Python并发编程在爬虫中的应用

### 1. 多线程编程
- **优点**：适合I/O密集型任务，提升程序效率。
- **示例（下载文件）**：
  ```python
  from threading import Thread
  import time
  
  def download(filename):
      print(f'开始下载 {filename}')
      time.sleep(2)  # 模拟下载耗时
      print(f'{filename} 下载完成')
  
  # 创建并启动线程
  threads = [
      Thread(target=download, args=('文件1',)),
      Thread(target=download, args=('文件2',))
  ]
  for t in threads:
      t.start()
  for t in threads:
      t.join()  # 等待线程结束
  ```

### 2. 多进程编程
- **优点**：突破GIL限制，适合计算密集型任务。
- **示例（进程池）**：
  ```python
  from concurrent.futures import ProcessPoolExecutor
  
  def is_prime(n):
      # 判断质数的逻辑
      pass
  
  with ProcessPoolExecutor(max_workers=4) as pool:
      results = pool.map(is_prime, numbers)
  ```

### 3. 异步I/O编程
- **优点**：高效处理大量I/O操作，减少线程开销。
- **示例（aiohttp爬取网页）**：
  ```python
  import asyncio
  import aiohttp
  
  async def fetch(url):
      async with aiohttp.ClientSession() as session:
          async with session.get(url) as resp:
              if resp.status == 200:
                  return await resp.text()
  
  async def main():
      urls = ['https://python.org', 'https://jd.com', ...]
      tasks = [fetch(url) for url in urls]
      results = await asyncio.gather(*tasks)
      for result in results:
          print(result[:100])  # 打印前100字
  
  asyncio.run(main())
  ```

### 4. 三种方式对比
- **单线程**：简单，但效率低，适合小规模任务。
- **多线程**：适合I/O密集型，提升效率，但有线程切换开销。
- **异步I/O**：高效处理大量并发I/O，代码更简洁，推荐用于爬虫。

## 五、使用Selenium抓取动态内容

### 1. Selenium简介
- **作用**：驱动浏览器获取动态渲染的页面内容。
- **安装**：`pip install selenium`
- **驱动下载**：根据Chrome版本下载对应ChromeDriver。

### 2. 基本用法
```python
from selenium import webdriver

# 创建浏览器对象
browser = webdriver.Chrome()
browser.get('https://www.baidu.com')

# 查找元素并操作
input_elem = browser.find_element_by_id('kw')
input_elem.send_keys('Python')
button_elem = browser.find_element_by_id('su')
button_elem.click()

# 等待元素加载
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.common.by import By

wait = WebDriverWait(browser, 10)
element = wait.until(EC.presence_of_element_located((By.ID, 'content_left')))

# 执行JavaScript
browser.execute_script('window.scrollTo(0, document.body.scrollHeight)')

# 无头模式
options = webdriver.ChromeOptions()
options.add_argument('--headless')
browser = webdriver.Chrome(options=options)
```

## 六、爬虫框架Scrapy简介

### 1. Scrapy概述
- **优点**：模块化设计，提高爬虫开发效率。
- **核心组件**：
  - 引擎（Engine）：控制数据流程。
  - 调度器（Scheduler）：管理请求队列。
  - 下载器（Downloader）：下载网页。
  - 蜘蛛（Spiders）：解析页面，提取数据。
  - 数据管道（Item Pipeline）：处理和存储数据。
  - 中间件（Middlewares）：扩展框架功能。

### 2. 快速入门
- **创建项目**：`scrapy startproject demo`
- **创建蜘蛛**：`cd demo && scrapy genspider douban movie.douban.com`
- **定义Item**（`items.py`）：
  ```python
  import scrapy
  class DoubanItem(scrapy.Item):
      title = scrapy.Field()
      score = scrapy.Field()
      motto = scrapy.Field()
  ```
- **编写蜘蛛**（`spiders/douban.py`）：
  ```python
  import scrapy
  class DoubanSpider(scrapy.Spider):
      name = 'douban'
      allowed_domains = ['movie.douban.com']
      start_urls = ['https://movie.douban.com/top250']
      
      def parse(self, response):
          for movie in response.css('#content > div > div.article > ol > li'):
              item = {
                  'title': movie.css('.title::text').get(),
                  'score': movie.css('.rating_num::text').get(),
                  'motto': movie.css('.inq::text').get()
              }
              yield item
          # 处理翻页
          next_page = response.css('.paginator .next a::attr(href)').get()
          if next_page:
              yield response.follow(next_page, self.parse)
  ```
- **运行爬虫**：`scrapy crawl douban -o result.json`

### 3. 数据管道（`pipelines.py`）
```python
class DoubanPipeline:
    def process_item(self, item, spider):
        # 数据处理逻辑，如保存到数据库
        return item
```

### 4. 配置（`settings.py`）
```python
USER_AGENT = 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36'
CONCURRENT_REQUESTS = 16
DOWNLOAD_DELAY = 0.5
ITEM_PIPELINES = {'demo.pipelines.DoubanPipeline': 300}
```

## 七、总结
- **爬虫开发流程**：确定目标→分析网站→选择工具→编写代码→处理反爬→存储数据。
- **反爬应对**：设置请求头、控制抓取频率、使用IP代理、模拟浏览器行为。
- **框架选择**：简单任务用requests+解析库，复杂任务用Scrapy框架。