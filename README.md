HttpProxyMiddleware
-------
********

这是一个scrapy的中间件middleware，用户更改 HTTP代理。
在运行的过程中，HttpProxyMiddleware将会自动获取新的proxyes和删除无效的proxyes。

fetch_free_proxyes.py
---------------
*************

在各大免费代理网站上获取免费的proxyes。
### 数据来源 ###

[开心代理](www.kxdaili.com)
[米扑科技](mimvp.com)
[西刺免费代理](http://www.xicidaili.com/nn/)
[代理ip检测平台](http://www.ip181.com/)
[免费http代理IP](http://www.httpdaili.com/mfdl/)
[66免费代理](http://www.66ip.cn/)

Usage
---------------
***********
### settings.py ###
	DOWNLOADER_MIDDLEWARES = {
	    'scrapy.downloadermiddlewares.downloadtimeout.DownloadTimeoutMiddleware': 350,
	    'scrapy.contrib.downloadermiddleware.retry.RetryMiddleware': 351,
	    # put this middleware after RetryMiddleware
	    'crawler.middleware.HttpProxyMiddleware': 999,
	}
	
	DOWNLOAD_TIMEOUT = 10   
### 更改proxy ###
一般情况下，当我们的IP被禁，我们就必须使用新的代理去登陆，产生一个新的请求：

	request.meta["change_proxy"] = True
有时候我们使用代理也会返回一些无效HTML代码。所以在response解析过程中有任何异常也需要产生一个新的请求：

	request.meta["change_proxy"] = True
### spider.py ###
在spider爬取的时候，如果获取到的状态码不在200或者指定的任何状态码将被视为无效代理，代理将被丢弃。

	website_possible_httpstatus_list = [404]
	handle_httpstatus_list = [403]
spider爬取可能返回404响应，不丢弃代理。