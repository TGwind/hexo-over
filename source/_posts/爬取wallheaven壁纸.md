---
title: 爬取wallheaven壁纸
cover: http://myblog.over2022.top/image-20230220220838683.png
tags:
- 爬虫
- Python
categories:
- 爬虫
---
### 爬取wallheaven壁纸

爬取的网址为 https://wallhaven.cc/toplist

![image-20230220220838683](http://myblog.over2022.top/image-20230220220838683.png)

爬取流程：

1. 在首页中F12检查html

2. ![image-20221222151129807](http://myblog.over2022.top/image-20231011165229687.png)

3. 在详情页中检查html

   ![image-20231011165237859](http://myblog.over2022.top/image-20231011165237859.png)

4. 编码

   ```python
   # -- coding:UTF-8 --
   import requests
   from bs4 import BeautifulSoup
   import os
   
   '''
   思路：获取网址
         获取图片地址
         爬取图片并保存
   '''
   
   
   # 获取网址
   def getUrl(url):
       # 请求头
       headers = {
           "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) "
                         "Chrome/108.0.0.0 Safari/537.36 "
       }
       try:
           read = requests.get(url, headers=headers)  # 获取url
           read.raise_for_status()  # 状态响应 返回200连接成功
           read.encoding = read.apparent_encoding  # 从内容中分析出响应内容编码方式
           return read.text  # Http响应内容的字符串，即url对应的页面内容
       except:
           return "连接" + url + "失败！"
   
   
   # 获取图片地址并保存下载
   def getPic(html):
       soup1 = BeautifulSoup(html, "html.parser")
       # 通过分析网页内容，查找img的统一父类及属性
       all_a = soup1.find_all(name='a', attrs={"class": "preview"})  # a为图片的标签
       for img in all_a:
           href = img['href']  # 获取a标签里的href内容，是该图片的详情页
           img_href = href
           second_html = getUrl(img_href)
           soup2 = BeautifulSoup(second_html, "html.parser")
           # 通过分析网页内容，查找img的统一父类及属性
           img = soup2.find(name='img', attrs={'id': "wallpaper"})  # img为图片的标签,id属性值为wallpaper
           download_link = img['src']  # 获取img的src属性值
           print("图片下载地址："+download_link)
           root = "F:/Pic/"  # 保存的路径，可以自定义
           path = root + download_link.split('/')[-1]  # 获取img的文件名
           print("图片保存路径："+path)
           try:
               if not os.path.exists(root):  # 判断是否存在文件并下载img
                   os.mkdir(root)
               if not os.path.exists(path):
                   read = requests.get(download_link)
                   with open(path, "wb") as f:
                       f.write(read.content)
                       f.close()
                       print("文件保存成功！")
               else:
                   print("文件已存在！")
           except:
               print("文件爬取失败！")
   
   
   # 主函数
   if __name__ == '__main__':
       init_page = int(input("请输入读取的初始页码："))
       max_page = int(input("请输入读取的最大页码："))
       # 分页读取图片
       for page in range(init_page, max_page):  # 此为1~5页，自行设定
           html_url = getUrl("https://wallhaven.cc/toplist?page=" + str(page))
           getPic(html_url)
   
   ```

   

