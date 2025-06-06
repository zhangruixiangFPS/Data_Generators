# 洛谷数据生成器
### Some Tools for Data_Create

[CYaRon教程](https://www.luogu.me/article/6uiax10m) by [RainCQwQ](https://www.luogu.me/user/952635)


> ## CYaRon 教程  
> ### 输入输出 IO  
> IO 库可以方便的帮您建立一组测试数据。构造函数的调用方法有以下几种：  
> ```python  
> IO("test1.in", "test1.out") # test1.in, test1.out  
> IO(file_prefix="test") # test.in, test.out  
> IO(file_prefix="test", data_id=3) # test3.in, test3.out  
> IO(file_prefix="test", data_id=6, input_suffix=".input", output_suffix=".answer") # test6.input, test6.answer  
> IO("test2.in") # test2.in, .out文件生成为临时文件  
> IO(file_prefix="test", data_id=5, disable_output=True) # test5.in, 不建立.out  
> IO() # .in, .out文件均生成为临时文件，一般配合对拍器使用  
> ```  
> 以上方法可以帮您创建一组测试数据的文件。其中使用 `file_prefix` 和 `data_id` 配合 `for` 循环可以较为方便地批量生成多个数据点。  
> IO 库的方法主要有以下几种：  
> ```python  
> io = IO("test1.in", "test1.out") # 先新建一组数据  
> io.input_write(1, 2, 3) # 写入1 2 3到输入文件  
> io.input_writeln(4, 5, 6) # 写入4 5 6到输入文件并换行  
> io.output_write(1, 2, 3) # 写入1 2 3到输出文件  
> io.output_writeln(4, 5, 6) # 写入4 5 6到输出文件并换行  
> io.input_write([1, 2, 3]) # 写入1 2 3到输入文件  
> io.output_write(1, 2, [1, 2, 3], [4]) # 写入1 2 1 2 3 4到输出文件  
> io.input_write(1, 2, 3, separator=',') # 写入1,2,3,到输入文件，目前版本尾部会多一个逗号，之后可能修改行为  
> io.output_gen("~/Documents/std") # 执行shell命令或二进制文件，把输入文件的内容通过stdin送入，获得stdout的内容生成输出  
> io.output_gen("C:\\Users\\Aqours\\std.exe") # 当然Windows也可以  
> ```  
> ### 图 Graph  
> Graph 库可以用来生成各种各样的树、图、链等结构。  
> 这个库中本身已经配备了各种常用的图的模板，不过我们先来看看手动建立一个图的方法。  
> ```python  
> graph = Graph(10) # 建立一个10个节点的无向图  
> graph = Graph(10, directed=True) # 建立一个10个节点的有向图  
> # 这两个图的节点编号范围都为1到10  
>  
> graph.add_edge(1, 5) # 建立一条从1到5，权值为1的边，若是无向图，还会建立从5到1的边  
> graph.add_edge(1, 6, weight=3) # 建立一条从1到6，权值为3的边，若是无向图，还会建立从6到1的边  
>  
> graph.edges # 一个邻接表数组，每一维度i保存的是i点出发的所有边，以Edge对象存储  
> for edge in graph.iterate_edges(): # 遍历所有边，其中edge内保存的也是Edge对象  
>     edge.start # 获取这条边的起点  
>     edge.end # 获取这条边的终点  
>     edge.weight # 获取这条边的边权  
>     io.input_writeln(edge) # 输出这条边，以u v w的形式  
>  
> io.input_writeln(graph) # 输出这个图，以每条边u v w一行的格式  
> io.input_writeln(graph.to_str(shuffle=True)) # 打乱边的顺序并输出这个图  
> io.input_writeln(graph.to_str(output=my_func)) # 使用my_func函数替代默认的输出函数，请查看源代码以理解使用方法  
> io.input_writeln(graph.to_str(output=Edge.unweighted_edge)) # 输出无权图，以每条边u v一行的格式  
> ```  
> 不过在大多数情况下您不需要手动建图，我们为您准备了大量模板，用法如下：  
> ```python  
> graph = Graph.graph(n, m) # 生成一个n点，m边的无向图，边权均为1  
> graph = Graph.graph(n, m, directed=True, weight_limit=(5, 300)) # 生成一个n点，m边的有向图，边权范围是5到300  
> graph = Graph.graph(n, m, weight_limit=20) # 生成一个n点，m边的无向图，边权范围是1到20  
> graph = Graph.graph(n, m, weight_gen=my_func) # 生成一个n点，m边的无向图，使用自定义随机函数my_func的返回值作为边权  
> graph = Graph.graph(n, m, self_loop=False, repeated_edges=False) # 生成一个n点，m边的无向图，禁止重边和自环  
> # 以上的directed, weight_limit, weight_gen参数，对如下的所有函数都有效。  
>  
> chain = Graph.chain(n) # 生成一条n个节点的链，是Graph.tree(n, 1, 0)的别名  
> flower = Graph.flower(n) # 生成一朵n个节点的菊花图，是Graph.tree(n, 0, 1)的别名  
> tree = Graph.tree(n) # 生成一棵n个节点的随机树  
> tree = Graph.tree(n, 0.4, 0.35) # 生成一棵n个节点的树，其中40%的节点呈现链状，35%的节点呈现菊花图状，剩余25%的节点随机加入  
> binary_tree = Graph.binary_tree(n) # 生成一棵n个节点的随机二叉树  
> binary_tree = Graph.binary_tree(n, 0.4, 0.35) # 生成一棵n个节点的二叉树，其中节点有40%的概率是左儿子，35%的概率是右儿子，25%的概率被随机选择  
> graph = Graph.hack_spfa(n) # 生成一个n点，1.5*n(下取整)边的图，具有卡SPFA的特点  
> graph = Graph.hack_spfa(n, extra_edge=m) # 生成一个n点，1.5*n+m(下取整)边的图，具有卡SPFA的特点  
> # 下列方法生成的图保证连通  
> # 支持 self_loop, repeated_edges, weight_limit, weight_gen 参数，但不支持 directed，DAG 的 self_loop 默认为 False  
> graph = Graph.DAG(n, m) # 生成一个 n 点，m 边的有向无环图  
> graph = Graph.DAG(n, m, loop=True) # 生成一个 n 点，m 边的有向有环图  
> graph = Graph.UDAG(n, m) # 生成一个 n 点，m 边的无向联通图  
> ```  
> ### 多边形 Polygon  
> 使用 Polygon 库您可以输入、生成多边形，并对其进行一些简单的操作。  
> ```python  
> p = Polygon([(0,0), (0,4), (4,4), (4,0)]) # 以这四个点生成四边形，注意点需要按照连线顺序  
> p.perimeter() # 周长  
> p.area() # 面积  
> io.input_writeln(p)  
>  
> # 您也可以使用以下的模板生成随机的多边形  
> p = Polygon.convex_hull(n) # 生成一个N个点的凸包  
> p = Polygon.simple_polygon(n) # 生成一个N个点的简单多边型  
> ```  
> 有关于 Polygon 库的更多高级用法，请参阅源代码。  
> ### 向量 Vector  
> CYaRon 的向量功能可以帮助您生成一个 list，包括若干个向量。在算法竞赛中，需要生成互不相同的坐标集，或者一组不重复的数列是很有用的。  
> 用法：  
> ```python  
> Vector.random(num=5, position_range=[10], mode=0)  
> ```  
> * 参数 `num`：生成的向量个数。  
> * 参数 `position_range`：一个 `list`。内有几个元素那么就是输出几维向量。每个元素可以是一个二维整数（或实数）元组 $(min,max)$ 表示每一维的取值是 $[min,max]$，也可以是可以是一个整数（或实数）$k$，则范围是 $[0,k]$。当该参数只有一个元素时，则生成的是一组数列而不是向量。  
> * 参数 `mode`：模式选择。0 为互相不重复的整数向量，1 为允许出现重复的整数向量（各维完全独立随机），2 为实数向量。  
> 一些范例：  
> ```python  
> output = Vector.random()  
> #默认值，随机生成5个[0,10]的不重复数字的数列。  
>  
> output = Vector.random(10, [(10,50)])  
> #生成10个范围在[10,50]之间的不重复数字数列。  
>  
> output = Vector.random(30, [(10,50), 20])  
> #生成30个第一维范围[10,50]之间、第二维范围在[0,20]之间的不重复的二维向量。  
>  
> output = Vector.random(30, [(1,10), (1,10), (1,10)], 2)  
> #生成30个每一维范围[1,10]之间的三维实数向量。  
>  
> output = Vector.random(30, [10], 1)  
> #生成30个[0,10]之间的随机数，当然肯定会有重复咯。  
> ```  
> 在不使用 pypy 的情况下，生成一组 $1e5$ 个 unique 的二维向量，坐标值不超过 $1e9$，大约需要 $10$ 秒钟。生成向量的算法均摊复杂度大于 $O(num)$，小于 $O(num*log(num))$。  
> 默认情况下，即使是一维数列，每一项数字也是一个列表。例如 `[[7], [110], [230], [415]]`，如果需要展平成一个一维 list，可以使用 `sum(output,[])`。  
> ### 字符串 String  
> String 库可以帮您生成各种随机字符串、单词、句子、段落等。  
> 使用方法如下：  
> ```python  
> str = String.random(5) # 生成一个5个字母的单词，从小写字母中随机选择  
> str = String.random((10, 20), charset="abcd1234") # 生成一个10到20个字母之间的单词，从abcd1234共8个字符中随机选择  
> str = String.random(10, charset="#######...") # 生成一个10个字母的只有'#'和'.'组成的字符串，'#'的可能性是70%，'.'可能30%。  
> str = String.random(None, charset=["foo", "bar"]) # 从foo、bar两个单词中随机选择一个返回  
> # charset参数对于以下所有指令也有效。  
>  
> str = String.random_sentence(5) # 生成一个5个单词的句子，以空格分割，第一个单词首字母自动大写，结尾有句号或感叹号，每个单词3到8个字母长  
> str = String.random_sentence((10, 20), word_separators=",;", sentence_terminators=None, first_letter_uppercase=False, word_length_range=(2, 10), charset="abcdefg") # 生成一个10到20个单词的句子，以逗号或分号随机分割，第一个单词首字母不大写，结尾没有任何符号，每个单词2到10字母长，从abcdefg共7个字符中随机选择  
> # 以上所有参数，对于以下所有指令也有效  
>  
> str = String.random_paragraph((3, 10)) # 生成一个3到10个句子的段落，句子之间以句号或感叹号分割，小句之间以逗号或分号分割，句子和小句结束后均接有一个空格，句子开头首字母大写而小句开头首字母不大写。生成句子的可能性为30%而小句的可能性为70%。  
> str = String.random_paragraph(6, sentence_joiners="|", sentence_separators=",", sentence_terminators=".?", termination_percentage=0.1) # 生成一个6个句子的段落，句子之间以句号或问号号分割，小句之间以逗号分割，句子和小句结束后均接有一个"|"号，句子开头首字母大写而小句开头首字母不大写。生成句子的可能性为10%而小句的可能性为90%。  
>  
> # 注意：如果您需要以两个空格分割单词，应该使用如下写法：  
> str = String.random_sentence(5, word_separators=["  "]) # 以两个空格分割单词  
> # 而不是：  
> str = String.random_sentence(5, word_separators="  ") # 这会导致从两个空格中随机选择一个，也就是只有一个空格  
> ```  
> ### 序列 Sequence  
> Sequence 是一个可以用来通过一个函数或者一个表达式，制造各种序列的东西。  
> 使用方法如下面代码所示：  
> ```python  
> Sequence(lambda i, f: 2*i+1) # f(i)=2*i+1  
> Sequence(lambda i, f: f(i-1) + 1, [0, 1]) # f(i)=f(i-1)+1, f(0)=0, f(1)=1  
> Sequence(lambda i, f: f(i-1) + 1, {100: 101, 102: 103}) # f(i)=f(i-1)+1, f(100)=101, f(102)=103  
> ```  
> 其第一个参数为一个 lambda 函数，该 lambda 函数的第一个参数 $i$ 代表这是序列的第几项，而第二个参数 $f$ 则是一个可以获取该数列任意一项的函数。  
> 第二个参数则是一个数组或 `dict`，默认为空，是该序列的初始值列表。当这个序列的表达式中需要使用到 $f$（即，需要递归进去获取函数值）的时候，必须提供第二个参数，否则找不到初始值会陷入死循环。  
> 我们可以对其做如下操作：  
> ```python  
> seq = Sequence(lambda i, f: f(i-1) + 2, [0, 2, 4])  
> seq.get(3) # 6  
> seq.get(4, 6) # [8, 10, 12]  
> io.input_write(seq.get(7, 10)) # 可以直接传递给IO库，写入14 16 18 20  
> ```  
> ### 对拍器 Compare  
> 将对拍器与您的数据生成器结合使用，您可以方便地检测您的程序的正确性。  
> 1. 对拍输出文件  
> ```python  
> # 默认比较器为NOIP风格，忽略最后空行和行尾空格  
> Compare.output("1.out", "2.out", std="std.out")  
> # 以std.out为标准，对比1.out和2.out的正确性  
>  
> std_io = IO()  
> std_io.output_writeln(1, 2, 3) # 往std_io的output里写入一些东西  
> Compare.output("1.out", "2.out", std=std_io)  
> # 以std_io这个IO对象中的output为标准，对拍1.out和2.out，  
> ```  
> 2. 对拍程序  
> ```python  
> input_io = IO()  
> input_io.input_write("1111\n")  
>  
> Compare.program("a.exe", input=input_io, std_program="std.exe")  
> # 以input_io这个IO对象中的input为stdin输入。  
> # std.exe的输出为标准输出，以此为标准对拍a.exe的输出。  
>  
> Compare.program("a.exe", "b.exe", input=input_io, std_program="std.exe")  
> # 和上面的方法类似，但是你可以以std.exe为标准对拍多个程序输出。  
>  
> Compare.program("a.exe", "b.exe", "c.exe", input="data.in", std="std.out")  
> # 当然input也可以简单地是文件，并以std.out这个输出文件的内容来对a.exe, b.exe, c.exe对拍。  
> # 这里std也可以是IO对象。  
>  
> while True:  
>     input_io = IO()  
>     input_io.input_writeln(randint(1,100))  
>     Compare.program("a.exe", "b.exe", input=input_io, std_program="std.exe")  
> # 不断地生成测试数据（这里是1到100的随机数），然后放到a.exe，b.exe中，分别以std.exe为标准进行对拍比较  
> # CYaRon 现在使用多线程比较器，原 stop_on_incorrect 参数现已 deprecated 且无实际作用。  
> # 并在工作目录下输出a.exe.out, std.out, error_input.in三个文件方便您进一步调试。
