# Python-Login_zhihu
基于python3.6实现模拟登录知乎（验证码为点击倒立中文）
# 实现登录的大致流程
* 确定请求地址
* 确定发送的参数
* 识别验证码（坐标）
* 伪装浏览器访问
### 一、确定请求地址、参数（以手机号码形式登录为例子）
* 推荐使用谷歌浏览器
![登录首页](https://github.com/legendheng/Python-Login_zhihu/blob/master/index.png)
* 打开F12开发人员，选择Network(可以看到所有的网页请求)
![查看请求信息](https://github.com/legendheng/Python-Login_zhihu/blob/master/getinfo.png)
* 然后随便填写手机号码和密码，即可看到有一个phone_num请求，点击查看可以发现需要请求的地址(Request URL)、需要提交的参数(Form Data)【注意这里的_xsrf时一串随机数，可以在请求之前用正则获取到，captcha是验证码(可以构造坐标列表得到)】

### 二、分析页面获取验证码（人工识别发送）
* 查看html元素可以获取验证码图片的请求地址
* 把图片保存到本地并自动打开
* 输入验证码拼接参数(根据观察发现验证码每次都是由7个中文组成，并且有一到两个字是倒立的，这里发送的参数就是倒立中文的坐标，用的是列表格式)
  * 总结得七个中文的坐标[17.29688,24],[40.2969,25],[66.2969,24],[89.2969,24],[114.297,25],[138.297,26],[162.297,24](该例子若需输入多个验证码时用空格隔开)
### 三、伪装浏览器访问
* User-Agent(可以通过浏览器开发人员工具得到)，这个必须要带上，不然知乎服务器会识别出你是爬虫而并非正常浏览(这里会出现500，直接访问不了主页)
* Referer（同上），告诉服务器我是从哪个页面链接过来的
### 四、使用http.cookiejar库保存cookies
* 作用是把cookies保存到本地文件cookies.txt，下次再访问时不需要再请求登陆页面和识别验证码
