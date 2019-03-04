<font color = red size = 15>工作记录</font>  

## 2018.11.7
-------------------------

## **通过Python筛选excel文件的内容**
* 1 安装配置Python3.7.1环境。
    * 1.1 直接官网下载，安装，工作目录添加到PATH路径。
* 2 下载安装相关库xlrd,xlwt。
    * 2.1 安装[xlrd库](https://pypi.org/project/xlrd/)，[xlwt库](https://pypi.org/project/xlwt/1.1.2/)。
    * 2.2 在黑框中输入“python setup.py install”安装


`读取一个该目录下名字为“test.xlsx”的excel文件：`
```
def open_excel(file= 'test.xlsx'):
    try:
        data = xlrd.open_workbook(file)
        return data
    except Exception as e:
        print (e)
```

`取数据：`
```
def excel_table_byindex(file= 'test.xlsx',colnameindex=0,by_index=0):
    data = open_excel(file) #打开excel文件
    table = data.sheets()[by_index] #根据sheet序号来获取excel中的sheet
    nrows = table.nrows #行数
    ncols = table.ncols #列数
    colnames =  table.row_values(colnameindex) #某一行数据 
    list =[] #装读取结果的序列
    for rownum in range(0,nrows): #遍历每一行的内容

         row = table.row_values(rownum) #根据行号获取行
         if row: #如果行存在
             app = [] #一行的内容
             for i in range(len(colnames)): #一列列地读取行的内容
                app.append(row[i])
             if app[0] == app[1] : #如果这一行的第一个和第二个数据相同才将其装载到最终的list中
                list.append(app)
    testXlwt('new.xls', list) #调用写函数，讲list内容写到一个新文件中
    return list
```


`python读取excel中单元格的内容返回的有5种类型:`  
ctype：0 empty,1 string, 2 number, 3 date, 4 boolean, 5 error

。。。。。。。。。。<font color = red size = 6>遇到的问题</font>。。。。。。。。。。。
1. **try,except问题。**
```
    try:
        data = xlrd.open_workbook(file)
        return data
    except Exception,e:
        print str(e)
```
以上是Python2中的语法格式，在Python3中无法调用，e为错误信息。应改为：
```
    try:
        data = xlrd.open_workbook(file)
        return data
    except Exception as e:
        print (e)
```

2. **对于数据的处理**
```
    ctype = row[i].ctype #表格数据类型
    cell = row[i]
    #数据类型处理ctype：0 empty,1 string, 2 number, 3 date, 4 boolean, 5 error
    if ctype == 2 and cell%1 == 0: #如果是整形
        cell = int(cell)
    elif ctype == 3:  #转化为datetime对象
        date = datetime(*xldate_as_tuple(cell,0))
        cell = date.strftime('%Y/%d/%m/ %H:%M/%S')
    elif ctype == 4:
        cell = True if cell == 1 else False
```

3. **时间放面的处理**  
    timestamp：时间戳，自1970年1月1日(00:00:00 GMT)以来的秒数  
    time tuple： time.struct_time对象类型  
    datetime tuple：datetime.datetime(2012, 12, 17, 21, 3, 44, 139715)  
    time string： 字符串类型

    [时间方面的相互转换](http://blog.sina.com.cn/s/blog_b09d460201018o0v.html)




## 2018.11.8
-------------------------

## **python创建，写入TXT文件**
```
def text_create(path = "E:\\PyFile\\Data\\",name = 'new',msg = []):
    desktop_path = path #文件存放路径
    full_path = desktop_path + name + '.txt' #创建txt文件
    file = open(full_path, 'w')
    for data in msg:
        file.write(str(data))
    file.close()
```

`需要注意的地方"E:\\PyFile\\Data\\":`  
1. 如果使用“E:\PyFile\Data”，那么在desktop_path + name + '.txt'语句中无法创建该目录下的文件（直接报错）；  
1. 如果使用“E:\PyFile\Data\”，将会报最后一个下划线错误。

## **对于时间的处理**  
`需要注意的是excel中存储的有两种时间类型：YYYY-MM-DD HH:MM:SS和YYYY/MM/DD HH:MM:SS`  
先把excel中的时间格式调为YYYY/MM/DD HH:MM:SS，接下来用下面的方式读取，返回datetime类型。
```
Time = xlrd.xldate.xldate_as_datetime(table.cell(rownum,0).value,0)
```
<font color = red size = 3>**可用于比较，例如：if ( (T1-T2).seconds > 20*60 )**</font>

### datetime 转化为 string类型：date_time.strftime('%Y-%m-%d')
### string类型 转化成 datetime类型： datetime.datetime.strptime(str,'%Y-%m-%d')

### datetime 转化为 时间戳: time.mktime(date_time.timetuple())
### datetime 转化为 string类型: date_time.strftime('%Y-%m-%d')
### 时间戳类型 转化为 string类型： time.strftime('%Y-%m-%d',time.localtime(time_time))

<font color = red size = 5>**PS:目录，文件名字最好以参数的形式传入，方便修改**</font>





## 2018.11.8
-------------------------

import numpy as np

a = np.array([1,2,3,4])

a = np([1,2,3,4],
        [5,6,7,8])

a = np.zero()

a = np.zeros( (3,4) )



## 2018.12.3
---

`打开trace目录：`
```
bws = []
files = os.listdir(PATH)

for file in files:
    with open("%s/%s" % (PATH, filename)) as f:
            lines = f.readlines()
            for line in lines:
                bws.append(float(line.split(" ")[-1]))
```


`如果没有该目录，创建目录:`
```
if not os.path.exists(filepath):
        os.makedirs(filepath)  
```



整理了一下mpc和bola的过程。在 https://docs.google.com/document/d/1ciY7fy3ta4Hnm3MztVvrvjTWm7skIDPID08wumuUAf4/edit 



----
如何检测分类结果的效果
```
看了下这个问题的回答，除了个别答案，其他都蛮扯淡的。正好今天在一个群里讨论了下这个问题，发表下我自己的一点看法：1. 聚类没有统一的评价指标因为不同聚类算法的目标函数相差很大，有些是基于距离的，比如kmeans，有些是假设先验分布的，比如GMM，LDA，有些是带有图聚类和谱分析性质的，比如谱聚类，还有些是基于密度的拿谱聚类距离，其中有一种叫 normalized 谱聚类，基于 normlaized Laplacian matrix，它的目标函数里面已经包含了对类簇间数目的约束( normalized cut )，所以聚类出来的结果天然地能使得 类间数目较平均。但你不能拿这个作为评估它结果的指标，否则就因果颠倒了2. 应该嵌入到问题中进行评价很多实际问题中，聚类仅仅是其中的一步，可以对比不聚类的情形（比如人为分割、随机分割数据集等等），所以这时候我们评价『聚类结果好坏』，其实是在评价『聚类是否能对最终结果有好的影响』

作者：知乎用户
链接：https://www.zhihu.com/question/19635522/answer/119880135
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```


---------
---------
运行abr_code 文件

先运行：python   bb.py   mpc.py   hls.py   bola_impl.py   sim_dp.py   
然后：gcc dp.cc -o app     ./app

最后：python plot_result.py






