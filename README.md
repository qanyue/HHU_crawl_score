# HHU-URP教务系统全自动爬虫
## 功能需求
全自动登录河海大学URP教务系统，查询各科成绩、每学期绩点、平均分、总绩点和总平均分。<br>
项目需求1：因为教务系统的学生端只能查询总绩点和平均分，无法查询每学期单独的绩点和平均分，本程序可分学期进行计算。<br>
项目需求2：因为教务系统在登录时需要人工输入验证码，本程序使用卷及神经网络对验证码进行破解，只需输入账号密码即可登录。<br>

## 设计思路
该程序使用Python编写，共分为三个模块：**（1）河海大学教务系统验证码识别模块；（2）河海大学教务系统登录模块；（3）获取成绩并计算绩点和平均分模块。**<br>
（1）利用get方法向验证码所在网页发送请求，得到jpg格式的图片。<br>![image](/assets/1-a.jpg)<br>
经过如下步骤的预处理：1）将图像转化为灰度图；2）利用threshold二值化；3）flood fill算法清除噪点。得到最终的预处理后的验证码图片。<br>![image](/assets/1-b.jpg)<br>
训练集全是人工打的标签，时间有限，只打了640张，能够meet系统要求。<br>
![image](/assets/2.jpg)<br>
将其输入如下结构的卷积神经网络进行预测，将结果返回给系统。<br>
![image](/assets/3.jpg)<br>
（2）在获取到用户输入的账号、密码和验证码识别模块返回的验证码信息后组成数据字典post_data = {'zjh':account, 'mm':password, 'v_yzm':yzm }。<br>
![image](/assets/4.jpg)<br>
通过post方式向登录页面发送请求，登录成功后保存当前的cookies。<br>
（3）利用得到的cookies和访问成绩页面必要的文件头信息（如下图所示）访问成绩页面，返回的html页面即包含所有成绩。使用BeautifulSoup将html页面转化为soup对象，方便对标签进行筛选。在筛选得到所需信息后，将每学期成绩传给calculate_GPA_and_avg_score()，返回每学期的绩点和平均分。<br>
![image](/assets/5.jpg)<br>

## 使用方法
