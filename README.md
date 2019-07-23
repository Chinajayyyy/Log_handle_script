# Log-capture-analysis-tool

阶段A
===

文件夹结构
---

> 根文件夹
>> <kbd>config.ini</kbd>

>> <kbd>file_path.ini</kbd>

>> <kbd>exe.bat</kbd>

>> <kbd>get_trace.py</kbd>

>> <kbd>handle_trace.py</kbd>

>> <kbd>get_processorID.py</kbd>

>> <kbd>get_single_trace2.py</kbd>

>> <kbd>download_trace_file.py</kbd>

>> <kbd>handle_ssh.py</kbd>

>> trace_file
>>> <kbd>Output.trace</kbd>

---
程序逻辑
---

1. <kbd>get_trace.py</kbd>通过读取<kbd>config.ini</kbd>中保存的服务器的IP地址、
账号密码登录到服务器上。并且读取<kbd>file_path.ini</kbd>中保存的文件路径以及
文件名，并且使用指定的文件名来下载对应的4个trace文件。

2. <kbd>handle_trace.py</kbd>将处理下载下来的4个trace文件，通过读取<kbd>config.ini</kbd>
中的规定的日志起止时间在原始的4个trace文件中截取对应时间的日志，并且按照时间戳
将指定时间段内的所有日志输出到<kbd>trace_file</kbd>文件夹中的<kbd>Output.trace</kbd>。

3. 运行<kbd>exe.bat</kbd>时要注意修改第一行的路径，否则会运行失败

---
下一阶段目标
---
1. <kbd>get_trace.py</kbd>和<kbd>handle.trace</kbd>中的代码逻辑有很多可以优化的地方
，暂时放到之后去优化

2. 现在暂时只是将trace文件下载到本地并做了简单的处理，下一阶段需要分析所截取的日志信息。

---
7月3日更新内容
---
1. 当按时间段获得所有的日志内容到<kbd>Output.trace</kbd>中后，需要完成输入给定的任务的关键信息
，将包含这些信息的日志内容输出到新的文件<kbd>Select.trace</kbd>中。其中关键信息需要在<kbd>config.ini</kbd>
中输入，目前只支持处理一个key word,即key1.

2. 这个功能由<kbd>get_info.py</kbd>完成，会将内容输出到<kbd>trace_file</kbd>中的<kbd>Select.trace</kbd>中
---
7月8日更新内容
---
1. 实现了通过给定的运行ProcessorID，去获取对应ProcessorID的trace文件，并保存到<kbd>single_trace_file</kbd>
中。但对于文件夹的格式有非常高的要求，之后会再改进

2. 新增了<kbd>get_processorID.py</kbd>文件和<kbd>get_single_trace.py</kbd>文件以及<kbd>single_trace_file</kbd>文件夹

---
下一阶段目标
---
1. 对于如何从数据库中获取ProcessorID仍存在问题，为解决这个问题留下了接口，等待mentor帮我找到解决方案。

2. 对于通过ProcessorID获得特定的trace文件对于原本服务器上的路径格式以及文件命名格式有非常高的要求，之后
需要找到一个普适的解决方法

3. 代码结构非常混乱，只是完成了功能实现，之后需要优化

---
7月8日下午更新内容
---
1. 新增了<kbd>handle_ssh.py</kbd>文件和<kbd>download_trace_file.py</kbd>文件。

2. <kbd>handle_ssh.py</kbd>实现了给定需要下载的文件名之后，去服务器上特定的目录遍历寻找需要下载的文件，并
最终将可以下载的文件的url写入到<kbd>config.ini</kbd>文件中

3. <kbd>download_trace_file</kbd>实现了从<kbd>config.ini</kbd>文件中读取所需要下载的文件url，通过ssh中的scp来
下载对应的文件到<kbd>single_trace_file</kbd>文件夹中。

4. 结合上午的更新，实现了以下逻辑：
>> 1. 当给定了主机IP地址、用户名、密码以及plan区名称后，通过IP地址、用户名以及密码登录服务器。
>> 2. 通过<kbd>get_processorID.py</kbd>实现从plan区地址到processorID的映射，即得出processorID
>> 3. 通过<kbd>get_single_trace2.py</kbd>实现从processorID得出需要下载的文件名称
>> 4. 通过<kbd>handle.ssh.py</kbd>获得文件绝对路径
>> 5. 通过<kbd>download_trace_file</kbd>通过获得的绝对路径下载trace文件
---
阶段B
===

文件夹结构
---

> 根文件夹
>> <kbd>config.ini</kbd>

>> <kbd>commonutils.py</kbd>

>> <kbd>download_trace_file.py</kbd>

>> <kbd>operate_trace_file.py</kbd>

>> <kbd>search_trace_file.py</kbd>

>> <kbd>search_trace_file_version2.py</kbd>

>> <kbd>README.md</kbd>

>> <kbd>trace_file</kbd>
>>> trace files

>> <kbd>tmp_file</kbd>
>>> tmp files

>> <kbd>collect</kbd>
>>> <kbd>collect_by_svr_name.trace</kbd>
---
7月10日上午更新内容
---
1. 重新写了<kbd>download_trace_file.py</kbd>和<kbd>operate_trace_file.py</kbd>
以及<kbd>commonutils.py</kbd>文件，将整体分工更加明确，实现了以下逻辑：
>> 1. 给定IP地址、用户名、密码以及特定的文件下载绝对路径，登录到服务器上下载对应trace文件。
下载到<kbd>trace_file</kbd>文件夹。由<kbd>download_trace_file.py</kbd>文件实现
>> 2. 操作下载下来的文件，通过给定的起止时间以及事务名称，在下载文件中查询对应的日志信息并导出
到特定文件。由<kbd>operate_trace_file.py</kbd>文件实现
>> 3. 整体逻辑是已知特定文件名到特定服务器路径上去下载特定的文件并筛选出特定时间段的特定事务的日志信息。
2. 将原有的<kbd>file_path.ini</kbd>中的内容整合到了<kbd>config_ini</kbd>中
---
7月10日下午更新内容
---
1. 新增了<kbd>search_trace_file.py</kbd>文件，实现了以下逻辑：
    1. 输入的信息是主机IP地址、用户名、密码、指定的时间范围以及特定的事务名称。通过输入信息在整个服务器上
的所有trace文件筛选出对应时间内的特定事物的日志信息并保存到本地。
    2. 通过IP地址、用户名、密码登录到服务器。
    3. 遍历<kbd>/export/home/osslog/U2020</kbd>文件夹下所有Service文件夹，将trace文件保存到本地。
    4. 在本地遍历所有下载下来的trace文件，筛选出对应时间的对应事务的日志信息保存到一个新的文件中，并将原来的
文件删除。

2. 实现了输入任意时间段和任意事务名称，在整个服务器上找到对应日志信息并保存到本地。
---
需要改进的内容
---
1. 现阶段使用的IP地址还不是主机地址，而是特定的服务器的IP地址，后续需要实现从主机IP获得分服务器IP的方法
2. 在<kbd>/export/home/osslog/U2020</kbd>目录下，有些Service文件夹没有log文件夹，trace文件直接保存在Service
路径下，这一部分的trace文件没有下载，后续需要处理这部分特殊情况
3. 有一些trace文件满之后，会压缩之后转存到<kbd>tracebak</kbd>文件夹，对于这部分trace文件还没有实现下载，后续
需要处理这个情况

---
7月11日更新内容
---
1. 新增了<kbd>search_trace_file_version2.py</kbd>文件
2. 将7月10日所留存的需要改进的几点内容进行了改进
3. 即通过给定的主机IP地址，去对应路径获取<kbd>nodelist.json</kbd>文件，从而获取整个网络节点的配置情况，
从而获得需要遍历的服务器的IP地址
4. 针对一部分trace文件转存到<kbd>tracebak</kbd>文件夹中，并转成了<kbd>.gz</kbd>结尾的压缩文件，采取的办法是
将压缩文件从服务器下载到本地之后解压成trace文件之后，再通过已经写好的方法对这个trace文件进行操作，筛选出其中的信息
5. 因为在程序运行的时候下载了很多新的文件，因此在程序执行完成之后会自动删除下载的trace文件
---
需要改进的内容
---
1. 7月10日的要改进的第二点经过考虑之后认为暂时不用实现
2. <kbd>search_trace_file_verison2.py</kbd>写的非常烂，只是实现了功能，代码的结构和可读性有很大的提高空间，
需要之后去改进
3. 搜索一次需要大约30秒钟，主要的时间开销在从服务器下载文件到本地上

---
7月12日更新内容
---
1. 新增了<kbd>search_trace_file_version3.py</kbd>文件
2. 对于7月11日的<kbd>search_trace_file_version2.py</kbd>文件进行了结构优化，对于代码的逻辑没有什么改变，运行时间
仍然是25秒到30秒左右，服务器上下载文件的开销暂时无法减少
3. 整个功能差不多实现了，主要逻辑如下：
>> 1. 配置文件<kbd>config.ini</kbd>中保存了所有需要的配置信息，同时当需要改变查找内容时也只要修改配置文件中的信息即可。
>> 2. 给定查询起止时间，事务内容之后，只需要点击<kbd>run.bat</kbd>文件即可自动运行所有内容。
