# RUBY学习笔记
- - - -
## nil?,empty?,blank?区别
- nil? : 判断变量是否nil(没有引用对象) 
- empty? : 用于判断(string, array or hash)是否为空
- blank? : nil? || empty? || false || " " 

PS: 付上一张很有用的[值表][reflink_1]
![值表插图][pic_1]

- - - -
## #{var}
把变量var的值嵌入到字符串中的写法

- - - -
## 注释
- 行注释: 行里面**#**符号开始到行末都是注释内容
- 多行注释: **=begin**注释开始行， **=end**注释结束行

- - - -
## 条件语句
### 条件真假值
条件|对象
:--|:--
真|false、nil以外的所有对象
假|false、nil

**注意 :0值为true!!!**

### if语句
<pre>
if 条件1 [then]
    处理1
[elsif] 条件2 [then]
    处理2
[elsif] 条件3 [then]
    处理3
[else]
    处理4
end
PS: []的内容可以省略
</pre>

### unless语句
unless与if相反
<pre>
unless 条件 then
    条件不满足时执行
end
```
unless也可以有else
```
unless 条件 then
    处理1 #条件不满足执行
else
    处理2 #条件满足执行
end
</pre>

### case语句
<pre>
case 比较对象
when 值1 then
    处理1
when 值2 then
    处理2
when 值3 then
    处理3
else
    处理4
end
</pre>

**PS: when语句后面可以一次性指定多个值, 以逗号分隔。**

#### ===与case语句
case语句中when判断值是否相等是用`===`操作符号
1. 比较对象是数值或字符串时，`===`与`==`的效果一样;
2. `===`可以用来判断正则表达式是否匹配，与`=~`类似, 只是前者返回true/false,后者返回匹配的位置或nil;
3. 判断右边的对象是否等于左边的类;

**注意: when指定的对象是在左边的，所以可以上面的第3项;**

### if unless 修饰符
if与unless可以写在希望执行的代码后面
<pre>
puts "a > b" if a > b
puts "a > b" unless a <= b
</pre>

- - - -
## 循环语句
- while语句
<pre>
while 循环条件 do
    希望循环的处理
end
</pre>
- times方法
times是方法！
<pre>
循环次数.times do
    希望循环的处理
end
#如果需要在循环体里面获知当前的循环次数
循环次数.times do |i|
    希望循环的处理 #i值就是当前的循环次数
end
</pre>
- for 语句
<pre>
for 变量 in 开始时的数值..结束时的数值 do
    希望循环的处理
end
</pre>
- until 语句:  与while相反
<pre>
until 条件 do
    条件不成立时执行的处理
end
#相当于条件成立之后才会跑到这里
</pre>
- each方法: 將对象集合里面的随想逐个取出，执行循环体
<pre>
对象集合.each do |变量|
    希望循环的处理
end
</pre>
- loop方法: 没有循环条件，需要靠循环体里面的逻辑控制跳出
<pre>
loop do
    循环体
end
</pre>

### 循环控制命令

命令|用途
:-:|:--
break|跳出循环
next|跳到下一次循环,类似continue
redo|不修改循环条件再重复刚执行的处理

- - - -
## 数组
### 定义数组
<pre>
#空数组
a = [] 

#动态数组 b = [nil, nil, nil, 123]
b = []
b[3] = 123 
</pre>

### 数组的循环
ruby为数组对象提供了each的迭代，类似数字的times
<pre>
数组.each do |变量|
    希望循环的处理
end
</pre>

***each,times***这种后面接 do ~ end 的方法成为***带块的方法***

- - - -
## 符号(symbol)
Ruby的1种对象，可以认为是轻量级的字符串，可以作为hash的key值
<pre>
#创建符号
sym = :foo
sym3 = :"foo" #意思同上
</pre>

### 符号与字符串互相转换
<pre>
:sym.to_s #->"sym"
"str".to_sym #-> :str
</pre>

- - - -
## 散列(hash)
### 散列创建
<pre>
address = {:name => "高桥", :postal => "123456" } #用符号作为key
address = {name => "高桥", postal => "123456" }   #用字符串作为key
</pre>

### 散列的循环
<pre>
散列.each do |键变量, 值变量|
    希望循环的处理
end
</pre>

- - - -
## 正则表达式
### 模式的表示
`/模式/`

### 模式的使用
***=~***符号，模式出现在左右边都可以<br>
/模式/ =~ 希望匹配的字符串
<pre>
mymode = /abc/
result = mymode =~ "123abc" #-> result = 3
result = mymode =~ "123ab" #-> result = nil
</pre>

### 模式选项
- 忽略大小写 在右斜杠后面加上***i*** `/模式/i`

- - - -
## 命令行参数
ruby使用预订好的`ARGV`数组作为从命令行传的参数,这个参数不包括脚本命令。

- - - -
## 字符串操作
- 字符串转符号(symbol): `str.to_sym`
- 字符串转整型: `str.to_i`                

- - - -
## 文件操作
File对象

### 一次性读取
<pre>
file = File.open([filepath]) #得到File对象
content1 = file.read #得到文件内容
file.close
# 或者
content2 = File.read([filepath]) #直接得到文件内容
</pre>

### 逐行读取
使用`each_line`迭代方法
<pre>
file = File.open([filepath])
file.each_line do |line|
    print line
end
file.close
</pre>

- - - -
## 定义方法
<pre>
def fun_name
   fun_body 
end
</pre>

- - - -
## 库的引用
包含其他ruby文件，相当于C/C++的include
<pre>
# PS: rb后缀可省略
require 希望使用的库名
</pre>

- - - -
## 类
常用对象的类型:

对象|类
:-|:-
数值|Numeric
字符串|String
数组|Array
散列|Hash
正则表达式|Regexp
文件|File
符号|Symbol

- - - -
## 变量类型
- 局部变量(local variable): 以英文字母或\_开头
- 全局变量(global variable): 以\$开头
- 实例变量(instance variable): 以@开头 
- 类变量(class variable): 以@@开头
- 伪变量(pseudo variable): Ruby预定义好的某些特定值的特殊变量, nil, true, false, self等都是伪变量

- - - -
## 常量
Ruby预定义好的常量: Ruby的运行版本(`RUBY_VERSION`)、运行平台(`RUBY_PLATFORM`)、命令行参数(ARGV)

- - - -
## 多重赋值

### 合并执行多个赋值操作
<pre>
a, b, c = 1, 2, 3  # 相当于 a=1 b=2 c=3

a, b, c = 1, 2, 3, 4, 5 # 等号右边多余的参数会被忽略
a, b, c, d, e = 1, 2, 3 # 等号左边多余的参数会设置为nil

a, b, * c = 1, 2, 3, 4, 5 # a=1 b=2 c=[3,4,5] 用 * 号收集多余的参数

ary = [1, 2, 3]
a, b = arr #-> a=1, b=2
a, = arr #-> a=1
</pre>

### 置换变量
<pre>
a, b = 0, 1
a, b = b, a  #置换a与b的值
</pre>

- - - -
## 判断对象的同一性
Ruby中所有对象都是唯一的，通过`object_id`(或`__id__`)来区别, 可以通过比较`object_id`或者直接使用方法`obj1.equal/eql?(obj2)`来判断两个变量是否指向同个对象.

- - - -
## 代码块 do ~ end 与 { ~ }
执行效果两种写法是相同的，但是有以下约定俗成的编码规则:
- 程序跨行写的时候使用 do~end
- 程序写在1行的时候用{~}
<pre>
10.times do |i|
    puts i
end
#等同
10.times { |i| puts i }
</pre>

- - - -
## 带块的方法

### 调用带块的方法
<pre>
对象.方法名(参数, ...) do | 变量1, 变量2, ... | 
    块内容
end
对象.方法名(参数, ...) { | 变量1, 变量2, ... | 块内容 }
</pre>

方法本身与块一起被执行，块里面的变量由方法返回

### 定义带块的方法
yield 关键字, 方法里面的yield关键字地方会装去执行块的代码，yield后面的参数就会传给块代码开头的变量; 块代码执行后，就回到方法里面的yield，块代码最后的表达式就是yield的返回值，继续执行函数体。

另外，可以使用**block\_given?**方法判断方法调用是否有块传递进来！

<pre>
def myloop
    while true 
        blockRlt = yield 
        puts "In myloop blockRlt = #{blockRlt}"
    end
end
num = 0
myloop do
    break if num >= 3
    puts "num=#{num}"
    num += 1
end
</pre>

输出结果:
<pre>
num=0
In myloop blockRlt = 1
num=1
In myloop blockRlt = 2
num=2
In myloop blockRlt = 3
</pre>

### block中的控制语句break next redo
- break 块中使用block，方法会立即返回，返回值为break的参数，省略的话为nil;
- next 中断块中当前的操作，返回值为next的参数值，省略的话为nil;
- redo 重新回到块头重新操作;

<pre>
def total(from, to)
  result = 0
  from.upto(to) do |num|
    result += block_given? ? yield(num) : num
  end
  result
end

p total(1,10) #=> 55  1+...+10

p total(1, 2) { |num| num  * num } #=> 5  1 * 1+ 2 * 2

p (total(1,10) do |num| 
  if num == 5
    break
  end
  num
end) #=> nil

p (total(1,10) do |num| 
  if num == 5
    break "break result"
  end
  num
end) #=> "break result"

p (total(1,10) do |num| 
  if num % 2 != 0
    next 0
  end
  num
end) #=> 30  2+4+6+8+10 = 30

p (total(1,10) do |num| 
  if num == 1 
    num += 1
    redo
  end
  num
end) #=> 56  1+1+...10 = 56

</pre>

### Proc对象 - block的封装
重用块的内容，就可以使用Proc.new创建Proc对象，参数就是块的定义！在调用Proc对象的call方法之前，块的代码不会被执行

<pre>
hello = Proc.new do |name|
  puts "Hello, #{name}."
end

hello.call("World") #=> Hello, World.
hello.call("Ruby") #=> Hello, Ruby.
</pre>

### 块参数 &block
方法定义末尾的参数如果使用&参数名，Ruby会自动把调用方法穿进来的块封装成Proc对象

**注意: &block一定要是方法的最后一个参数**

<pre>
def total2(from, to, &block)
  result = 0
  from.upto(to) do |num|
    result += block ? block.call(num) : num
  end
  result
end

proc_square = Proc.new do |num|
  num ** 2
end

p total2(1, 2, proc_square) # (ArgumentError) &参数只能接受block

p total2(1, 2){|num| num ** 2 } #=> 5

</pre>

- - - -
## 局部变量与块变量
块内部的命名空间与块外部是共享的！在块外部定义的局部变量，在块中可以继续使用！
<pre>
x = 1
y = 1
ary = [1,2,3]

ary.each do |x|
  y = x     #=> 这里修改的y值是外部的局部变量y
end

p [x, y] #=>[1, 3] 
</pre>

问题：如何在块内部定义局部变量？
解决：在块变量后使用;之后定义块局部变量

<pre>
x = y = z = 1
ary = [1,2,3]

ary.each do |x; y|
  y = x
  z = x
  p [x, y, z]
end

p [x, y, z]  #=> [1,1,3]

#所有输出
#[1, 1, 1]
#[2, 2, 2]
#[3, 3, 3]
#[1, 1, 3]
</pre>

- - - 
## 方法的定义
<pre>
def 方法名(参数1, 参数2, ...) #方法名可有英文字母、数字、下划线组成，但不能以数字开头。
    希望执行的处理
end
</pre>

- - - -
## 方法的返回值
- 可以通过return返回
- 省略return 方法体里面最后一个表达式的结果就是方法的返回值
<pre>
def max(a, b)
    if a > b
        a
    else
        b
    end
end
</pre>
- 省略return的参数 ruby返回nil对象

- - - -
## 参数个数不确定的方法
通过`*参数名`的形式来定义不确定个数的方法, 参数名会把不确定的参数收集成数组的形式
<pre>
def foo(\*args)
    args
end

p foo(1,2,3) # => [1,2,3]
</pre>

- - - -
## 关键字参数
使用关键字参数，可以將参数名与参数值成对的传给方法使用

定义如下:
<pre>
def 方法名(参数1: 默认值, 参数2: 默认值, ... )
    Body
end
</pre>

调用如下:
<pre>
方法名(参数1:值1, 参数2:值2, ...)
</pre>
    
- - - - 
## 接收未定义的关键字参数
可以使用`**参数名`来收集方法调用中传入的未定义过的关键字参数, 所有未定义的参数会收集成散列的形式。
<pre>
def meth(x:0, y:0, z:0, ** args)
    [x, y, z, args]
end
p meth(z:4, y:3, x:2, w:5, v:4) #=> [2, 3, 4, {:w=>5, :v=>4}]
</pre>

- - - -
## 用散列传递关键字参数
调用关键字参数定义的方法, 可以使用以**符号**作为键的散列传递参数,ruby会把符合方法关键字参数的散列项作为参数使用

<pre>
def meth(x:0, y:0)
    [x, y]
end
args = {x:1, y:2, z:3}
p meth(args) #=> [1,2]
</pre>

- - - -
## 分解数组作为参数
调用方法使，以`*数组`的形式传递给方法，数组的元素就会按照顺序依次传递给方法的各个参数。
<pre>
def foo(a, b, c)
    [a, b, c]
end
p foo( * [1,2,3]) #=> [1, 2, 3]
</pre>

- - - -
## 判断对象的具体类型
- class方法 
<pre>
p [].class #=> Array
p 'abc'.class #=> String
</pre>

- instance\_of?方法
<pre>
p [].instance_of?(Array) #=> true
p ''.instance_of?(String) #=> true
</pre>

- - - -
## 判断类的继承关系
- is\_a?方法 判断对象的类型(包括基类) 或者 类型A是否为类型B的子类
<pre>
p ''.is_a?(String) #=> true
p ''.is_a?(BasicObject) #=> true
p String.is_a?(BasicObject) #=> true
</pre>

- - - -
## 类的定义
**类名的首字母必须大写！**
<pre>
def 类名
    类的内容
end
</pre>

- - - -
## 构造函数
initialize方法
<pre>
class HelloWorld
   def initialize(myname = "Ruby")
     @name = myname
   end
end
</pre>

- - - -
## 实例变量的存取
`@name`以@开头的变量就是实例变量, 在Ruby,从对象外部不能直接访问实例变量或赋值, 需要通过定义方法。

### var 与 var= 方法
<pre>
def name    # 获取@name
    @name
end

def name=(value)
    @name = value
end

调用如下:

p bob.name
bob.name = "Robet"
</pre>

### attr\_reader、attr\_writer、attr\_accessor

定义|意义
:--|:--
attr\_reader :name|只读(定义name方法)
attr\_writer :name|只写(定义name=方法)
attr\_accessor :name|读写(定义以上两个方法)

- - - -
## self变量
ruby的保留字之一
- 在实例方法里self用来表示实例;
- 在类上下文中使用时，引用的对象是该类本身;

- - - -
## 类方法
- class << 类名 ~ end  PS: ~ 中可以定义多个方法
<pre>
class HelloWorld
    class << HelloWorld  #定义类方法hello
        def hello(name)
            puts "#{name} said hello."
        end
    end
end
</pre>

- class << self ~ end  PS: ~ 中可以定义多个方法
<pre>
class HelloWorld
    class << self
        def hello(name)
            puts "#{name} said hello."
        end
    end
end
</pre>

- def 类名.方法名 ~ end
<pre>
class HelloWorld
    def HelloWorld.hello(name)
        puts "#{name} said hello."
    end
end
# OR
class HelloWorld
    def self.hello(name)
        puts "#{name} said hello."
    end
end
</pre>

- - - -
## 常量
class上下文可以定义常量, 外部可以通过类名访问。
<pre>
def HelloWorld
    Version = "1.0"
end
p HelloWorld.Version 
</pre>

- - - -
## 类变量
@@开头的变量称为类变量。与实例变量一样，从类外部也需要存取器，但只能通过定义(var var=)这一种存取器。

- - - -
## 访问修饰符
修饰方法:
- public/private/protect  :方法1, :方法2 ;
- public/private/protect 不指定参数，修饰符以下定义的方法都自动被修饰;

- - - -
## 扩展类
### 在原有类的基础上添加方法
给String类添加count\_word方法

<pre>
class String
    def count_word
        ary = self.split(/\s+/) #空格分割
        ary.size
    end
end
</pre>

### 继承
<pre>
class 类名 < 父类
    类定义
end
</pre>
Ruby定义的类默认继承Object类(Object类继承BasicObject类)

- - - -
## alias
为已经存在的方法设置别名
<pre>
alias 别名 原名
alias :别名 :原名 #使用符号名
</pre>

用途:
- 为同一个功能设置多种名称;
- 重定义已经存在的方法是，为了用别名调用原来的方法;

<pre>
class C1
    def hello
        "Hello"
    end
end

class C2 < C1
    alias old_hello hello
    def hello
        "#{old_hello}, again"
    end
end

obj = C2.new
p obj.old_hello
p obj.hello
</pre>

- - - -
## undef 
用于删除已有方法的定义
<pre>
undef 方法名
undef :方法名
</pre>

- - - -
## 模块Module
创建模块
<pre>
module 模块名
    模块定义
end

module HelloModule
  Version = '1.0'               #定义常量

  def hello(name)               #定义方法
    puts "hello, #{name}."
  end

  module_function :hello
end

p HelloModule::Version          #=> "1.0" 注意：不能通过HelloModule.Version来访问滴！
HelloModule.hello("Alice")      #=> Hello, Alice.

include HelloModule             #包含模块
p Version
hello("Alice")
</pre>

### 模块函数
module 上下文定义的方法默认只能在模块上下问使用，外部无法通过模块.方法来调用。需要调用module\_function方法，把方法名的符号作为参数。
<pre>
module_function :hello   #把hello作为模块函数
</pre>

### 模块中的self
- 一般情况下，模块方法上下文中的self是指该模块的对象
- 在类Mix-in模块的情况下，模块方法上下文的self则是指Mix-in的类的对象

- - - -
## Mix-in
Mix-in就是將模块混合到类中，在类定义中include共同的模块，就可以共用模块里面的方法。通过Mix-in可以实现类似C/C++多重继承的效果。**PS:Enumerable模块就是一个典型例子，Array,Hash,IO类等都包含了Enumerable模块**

<pre>
module MyModule
    #共通的方法
end

class MyClass1
    include MyModule
    #MyClass1 独有的方法
end

class MyClass2
    include MyModule
    #MyClass1 独有的方法
end
</pre>

### inlude? 判断类是否Mix-in了某个模块
<pre>
MyClass1.include?(MyModule)
</pre>

### ancestors与superclass 查看类的继承关系
- ancestors查看类的继承关系的列表，Mix-in的模块也算是类的祖先
- superclass查看类的父类

<pre>
module M
  def meth
    "meth"
  end
end

class C
  include M
end

c = C.new
p c.meth          #=> "meth"

p C.ancestors     #=> [C, M, Object, Kernel, BasicObject]   PS:Kernel是Ruby内部的一个核心模块
p C.superclass    #=> Object
</pre>

- - - -
## 方法查找规则
类查找方法的顺序： 类本身->包含的模块->父类

- 在同一个类中包含多个模块时，优先使用最后一个包含的模块
- 嵌套include时，查找顺序是线性
- 相同模块被重复包含两次以上时，第2次以后的会被忽略

<pre>
module M1
end

module M2
end

module M3
  include M2
end

class C
  include M1
  include M3
end

p C.ancestors     #=> [C, M3, M2, M1, Object, Kernel, BasicObject] #acestors里面的顺序就是方法查找对象或模块的顺序
</pre>

- - - -
## 单例类
***突破类的限制，扩展对象的功能***

### 定义用于特定实例的专属实例方法
<pre>
str1 = "ruby"
str2 = "ruby"
class << str1
    def hello
        "hello, #{self}"
    end
end
p str1.hello #=> "hello, ruby"
p str2.hello #=> 错误{NoMethodError}
</pre>

### extend方法 使单例类包含模块
<pre>
module Edition
  def edition(n)
    "#{self} 第#{n}版"
  end
end

str = "Ruby"

str.extend(Edition)

p str.edition(4) #=>"Ruby 第4版"
</pre>

- - - -
## 通过模块扩展类的类方法与实例方法
- include 扩展实例方法
- extend 扩展类方法

<pre>
module ClassMethods   #定义类方法的模块
  def cmethod
    "class method"
  end
end

module InstanceMethods  #定义实例方法的模块
  def imethod
    "instance method"
  end
end

class MyClass
  # extend定义类方法
  extend ClassMethods

  #include定义实例方法
  include InstanceMethods
end

p MyClass.cmethod   #=> "class method"
p MyClass.new.imethod #=> "instance method"
</pre>

- - - -
## 范围运算符
### Ruby有表示数值范围的对象(range)
- 通过Range对象生成
<pre>
Range.new(1,10) #=> 1..10
</pre>
- 通过范围运算符 ../... 来生成
<pre>
1..10   # 1~10
1...10  # 1~9 
</pre>

### Range内部实现就是使用succ方法根据起点值逐个生成接下来的值
<pre>
1.succ      #=>2
'a'.succ    #=> "b"
</pre>

- - - -
## self.class 方法内创建当前类的对象
self.class指向的是调用当前方法的实际对象(有可能是子类调用父类的方法或者模块的方法)

- - - -
## 可重载的一元运算符

运算符|重载时的方法名
:--|:--
+ | +@
- | -@
~ | ~@
! | !@

- - - -
## 下标方法
obj[i] 与 obj[i] = x 这样的方法

运算符|重载时的方法名
:--|:--
obj[i]|def [](index)
obj[i]=|def []=(index, val)

- - - -
## 异常
Ruby用 begin~rescure~end 语句描述异常处理

<pre>
begin
    有可能发生异常的处理
rescue [ => 变量 ]
    发生异常后的处理
ensure
    不管是否发生异常都希望执行的处理
end
</pre>

PS: 如果 begin~rescue 的范围是整个方法体，可以省略begin语句！

### 异常变量
rescue语句中 => 变量用来获取异常对象。即使不指定变量，Ruby也会把异常对象赋值给\$!

变量|变量的意义
:--|:--
 \$!|最后发生的异常对象
 \$@|最后发生异常的位置信息

异常对象的方法

方法名|用途
:--|:--
class|异常的种类
message|异常的信息
backtrace|异常发生的位置信息($@与$!调用是等价的)

### 重试retry
rescue中使用retry后，会把begin以下的处理重做一遍。

### rescue修饰符
**表达式1 rescue 表达式2** 如果表达式1发生异常，表达式2的值就是整个表达式的值, 与下面的语句等同
<pre>
begin
    表达式1
rescue
    表达式2
end
</pre>

### rescue指定捕捉的异常类型
<pre>
begin
    可能发生异常的处理
rescue Exception1, Exception2 => 变量
    Exception1或Exception2异常的处理
rescue Exception3
    Exception3异常的处理
rescue 
    其他类型异常的处理
end
</pre>

- - - -
## 异常类
- ruby所有异常都是Exception类的子类
- rescue不指定异常类时，程序会默认捕捉StandardError类及其子类的异常, 所以自定义异常时，一般会先继承StandardError

<pre>
begin
  MyError = Class.new(StandardError) 
  MyError1 = Class.new(MyError)
  raise MyError1.new
rescue => exp
  p exp.message  #=> "MyError1"
end
</pre>

上面代码里面的Class.new(StandardError)与下面的定义是一样的
<pre>
class MyError < StandardError
end
</pre>

- - - -
## 主动抛出异常
- raise message  抛出RuntimeError异常，把message字符串设置为异常对象的信息
- raise 异常类 抛出指定的异常
- raise 异常类, message  抛出指定的异常，把message字符串设置为异常对象的信息
- raise 1,rescue外的话抛出RuntimeError异常; 2,rescue中调用的话会再次抛出最后一次发生的异常($!)

- - - -
## 块block的使用

### 通过块block来优雅打开文件
块变量直接接受了File.open的对象，省略了close的操作
<pre>
File.open([filename]) do |file|
    file.each_line do |line|
        print line
    end
end
</pre>

### 通过块block替换部分算法
Array#sort 方法没有指定块，会使用<=>运算符对各个元素进行比较。可以通过指定块自定义比较的顺序。

**运算符<=>**

<=>|比较结果
:--|:--
a < b|-1
a == b|0
a > b|1

<pre>
array = ['ruby', 'perl', 'PHP', 'Python']
sorted = array.sort #=> ["PHP", "Python", "perl", "ruby"]
p sorted
#逆序
sorted = array.sort{ |a, b| b <=> a } #=> ["ruby", "perl", "Python", "PHP"]
p sorted
</pre>

#### sort\_by方法
sort\_by方法会把每个元素在块中执行一次，然后根据这些结果进行排序处理
<pre>
array = ['ruby', 'perl', 'PHP', 'Python']
sorted = array.sort_by{|item| item.length} 
p sorted #=> ["PHP", "ruby", "perl", "Python"]
</pre>

- - - -
## Integer类的计数
- n.times{ |i| ... }  循环n次，0~n-1可以依次赋值给块变量
- from.upto(to) { |i| ... } i从from开始循环+1到to, 直到i等于to。 from比to大时不会执行循环
- from.downto(to) { |i| ... }  i从from开始循环-1到to, 直到i等于to。 from比to小时不会执行循环
- from.step(to, step) { |i| ... } i从from开始循环+step到to, 直到i等于to。

如果不对times upto downto step指定块，会返回Enumerator对象，可以通过Enumerator#collect方法获取遍历的值数组
<pre>
irb(main):006:0> 10.step(15).collect{|i| i * 2}
=> [20, 22, 24, 26, 28, 30]
</pre>

- - - -
## Comparable模块
Comparable模块封装了比较运算符，如果定义的类需要用到比较运算符，可以直接Mix-in Comparable模块，然后定义<=>运算符，因为Comparable的比较运算符都是使用<=>运算符进行比较的

Comparable封装的方法
- <
- <=
- == 
- \>=
- \>
- between?

- - - -
## %w %i创建字符串数组和符号数组
<pre>
irb(main):035:0> lang = %w(Ruby Perl Python Scheme)
=> ["Ruby", "Perl", "Python", "Scheme"]
irb(main):036:0> lang = %i(Ruby Perl Python Scheme)
=> [:Ruby, :Perl, :Python, :Scheme]
irb(main):037:0>
</pre>

- - - -
## 数组插入元素
利用array[n, len]表示法插入元素

<pre>
irb(main):038:0> alpha = ['a', 'b', 'c', 'd', 'e', 'f']
=> ["a", "b", "c", "d", "e", "f"]
irb(main):039:0> alpha[2,0] = ['x', 'y']
=> ["x", "y"]
irb(main):040:0> p alpha
["a", "b", "x", "y", "c", "d", "e", "f"]
=> ["a", "b", "x", "y", "c", "d", "e", "f"]
</pre>

- - - -
## 作为集合的数组
Ruby的数组可以当前集合使用，支持集合的操作：并集，交集，差集
- 交集: ary1 & ary2
- 并集: ary1 | ary2
- 差集: ary1 - ary2

<pre>
irb(main):043:0> ary1 = [1,2,3]
=> [1, 2, 3]
irb(main):044:0> ary2 = [2,3,4]
=> [2, 3, 4]
irb(main):045:0> p ary1 & ary2
[2, 3]
=> [2, 3]
irb(main):046:0> p ary1 | ary2
[1, 2, 3, 4]
=> [1, 2, 3, 4]
irb(main):047:0> p ary1 - ary2
[1]
=> [1]
irb(main):048:0> p ary1 + ary2
[1, 2, 3, 2, 3, 4]
=> [1, 2, 3, 2, 3, 4]
</pre>

- - - -
## 数组开头末尾元素的方法

操作|对数组开头的元素|对数组末尾的元素
:--|:--|:--
追加元素|unshift|push 或者 <<
删除元素|shift|pop
引用元素|first|last

- - - -
## 数组的 concat 与 +
concat与+都是对两个数组进行相加，前者把参数的数组加到调用的数组里面，会破坏调用concat的数组; 后者+是创建新的数组;

- - - -
## fun!与fun
Ruby的方法中，在相同方法名加上!的方法名，用于区分方法是否具有破坏性!
- arr.compact 与 arr.compact! 从数组arr中删除所有nil元素，前者返回新的数组，后者直接在arr数组上进行删除
- arr.reject{|item|...} 与 arr.reject!{|item|...} 从数组arr中删除特定的元素(块中返回true的元素)
<pre>
irb(main):058:0> ary1 = (1..10).to_a
=> [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
irb(main):059:0> ary1.reject{|item| item % 2 == 0}
=> [1, 3, 5, 7, 9]
irb(main):060:0> ary1
=> [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
irb(main):061:0> ary1.reject!{|item| item % 2 == 0}
=> [1, 3, 5, 7, 9]
irb(main):062:0> ary1
=> [1, 3, 5, 7, 9]
</pre>
- arr.slice 与 arr.slice! 删除数组arr中指定的部分,参数有三种类型 (n) (n..m) (n,len)
- arr.uniq 与 arr.uniq! 删除数组arr中重复的元素

- - - -
## 替换数组元素

- arr.collect\[!]{ |item| ... } <br>
  arr.map\[!]{ |item| ... } <br>
將数组arr中的元素传给块，用块的结果值创建新的数组

- a.fill 修改数组a的元素
<pre>
p [1,2,3,4,5].fill(0)         #=>[0, 0, 0, 0, 0] 替换所有元素为0
p [1,2,3,4,5].fill(0, 2)      #=>[1, 2, 0, 0, 0] 替换索引为2之后(包括2)的元素为0
p [1,2,3,4,5].fill(0, 2, 2)   #=>[1, 2, 0, 0, 5] 替换索引为2之后(包括2),2个元素为0
p [1,2,3,4,5].fill(0, 2..3)   #=>[1, 2, 0, 0, 5] 替换索引范围为2~3的元素为0
</pre>

- a.flatten\[!] 展开数组里面的嵌套数数组(这个展开过程是会“递归”进行的)
<pre>
irb(main):003:0> p [1,[2,3,[4, 5]],[6],7].flatten!
[1, 2, 3, 4, 5, 6, 7]
=> [1, 2, 3, 4, 5, 6, 7]
</pre>

- a.reverse\[!] 反转数组

- a.sort\[!]<br>
  a.sort\[!]{ |i, j| ...}
  对数组进行排序，排序方法可以通过块指定。没有块的时候用<=>运算符比较

- a.sort\_by{|i|...} 根据块的结果对数组进行排序

- - - -
## 数组的each each\_with\_index迭代器
其实迭代器就是带块的方法！Enumerable模块中定义了数组类常用的这些"带块的方法"
<pre>
arr.each do |item|
    ....
end

arr.each_with_index do |item, idx|
    ....
end
</pre>

### 自定义迭代器
<pre>
module MyModule
  def my_each_with_index
    index = 0
    self.each do |x|
      yield(index, x)
      index += 1
    end
  end
end

list = [1,2,3,4]
list.extend(MyModule)
list.my_each_with_index do |index, item|
  p [index,item]
end  
</pre>

- - - -
## %Q %q创建字符串
- %Q{可包含"'的字符串} == "可包含\"'的字符串"
- %q|可包含"'的字符串| == '可包含"\'的字符串'

- - - -
## Here Document
Unix Shell字符串的一种写法,注意，结束服标志所在行只能有结束符!
<pre>
<< "结束标识符"
字符串内容
结束标识符
</pre>

使用 <<- 代替 <<，ruby程序会忽略结束标识符前的空格和制表符。

- - - -
## \`Command\` 获取外部Command的标准输出
<pre>
irb(main):018:0> print `ls -l ~`
total 76
-rw-rw-r--  1 lsff lsff    0 4月   8 12:14 1.dump
-rw-r--r--  1 root root 7487 4月   8 12:15 abc.dump
drwxrwxr-x 12 lsff lsff 4096 3月  12 14:52 bookmarks
drwxrwxr-x  3 lsff lsff 4096 3月   2 23:09 config
</pre>
      
- - - -
## 删除最后一个字符与删除最后一个换行符
- chop[!] 删除最后一个字符
- chomp[!] 行末为换行符才將其删除

- - - -
## index, rindex, include?
- str.index(sub) 从左到右查找sub的位置，找不到返回nil
- str.rindes(sub) 从右到左查找sub的位置，找不到返回nil
- str.include?(sub) 判断字符串中是否存在字串匹配

- - - -
## 散列的创建
1. {键 => 值} 或 {符号 : 值}
2. Hash.new

- - - -
## fetch与[]的区别
1. 通过fetch方法获取散列的索引值时，如果索引不存在会产生异常;而后者在索引值不存在则返回nil;
2. fetch可以通过第二个参数或者块作为在索引值不存在时的返回值;

- - - -
## 设置散列的默认值
1. 创建散列时指定 Hash.new(defaultValue) 如果不指定则默认为nil;
2. 通过块指定默认值;
<pre>
h = Hash.new do | hash, key |
  hash[key] = key.upcase
end
h["a"] = "b"
p h["a"], h["x"], h["y"] #=> "b" "X" "Y"
</pre>
3. 通过fetch获取，前面已经讲过;

- - - -
## 查看散列是否存在特定的key或者value
- h.key?(key) <br/>
h.has\_key?(key) <br/>
include?(key) <br/>
member?(key)
- h.value?(value) h.has\_value?(value)

- - - -
## 删除键值
- h.delete(key)
- h.delete\_if{|key, value|...}<br/>
h.reject!{|key, value|...}

- - - -
## 正则表达式对象的创建
- /模式/ 表达式
- Regexp.new(str)创建对象
- %语法 在正则表达式包含`/`字符时使用这种比较方便
<pre>
%r\(模式\)
%r<模式\>
%r\|模式\|
%r\!模式\!
</pre>

- - - -
## 行首行末 VS 串首串末
由于字符串中可以有换行符，也就是包含多行的内容，因此行首行末不等于字符串的首字符与末尾字符。

元字符|含义
:-:|:-:
^|行首
$|行末
\A|字符串的开头 
\z|字符串的末尾

<pre>
p "123\nabc" =~ /^abc$/     #-> 4 能匹配
p "123\nabc" =~ /\Aabc\z/   #-> nil 匹配不到
p 'abc' =~ /\Aabc\z/        #-> 0 能匹配
</pre>

- - - -
## 使用反斜杠的模式

模式|意义
:-:|:-:
\s|空白符，匹配空格(0x20)、制表符、换行符、换页符
\d|0到9的数字
\w|英文字符和数字
\A|字符串的开头
\z|字符串的末尾

- - - -
## 贪婪匹配与懒惰匹配
表数量的符号`*`与`+`默认情况下是会匹配尽可能多的字符，这种行为成为贪婪匹配;相反，匹配尽可能少的字符称为懒惰匹配。

- \*? 0次以上的重复中最短的部分
- +? 1次以上的重复中最短的部分

- - - -
## 多个候选模式
`|`符号可以在几个候选模式中匹配任意一个

- - - -
## Regexp.quote
如果模式中存在较高需要转义的元字符，可以使用Regexp.quote进行转义，再把返回值传给Regexp.new创建模式。

<pre>
reg1 = Regexp.new("abc+def")
reg2 = Regexp.new(Regexp.quote("abc+def")) #+表示匹配+

p reg1 =~ "abc+def" #-> nil
p reg2 =~ "abc+def" #-> 0
</pre>

- - - -
## 正则表达式的选项

选项|选项常量|意义
:--|:--|:--
i|Regexp::IGNORECASE|忽略大小写
x|Regexp::EXTENDED|忽略模式中的空白字符
m|Regexp::MULTILINE|匹配多行 `.`可以匹配换行符
o|(无)|只使用一次内嵌表达式

- - - -
## 分组()
使用括号可以匹配多个不同的字符，而且可以把匹配的结果进行分组，分组有序号，根据左括号出现的次序进行编号。可以使用(?:)过滤不需要捕获的模式，即(?:)这样的分组也不会参与编号。

### 分组的捕获
1. \$数字
<pre>
/(.)(.)(.)/ =~ "abc"
p $1  #=> "a"
p $2  #=> "b"
p $3  #=> "C"
</pre>

2. \$\`、\$&、\$' 分别表示匹配部分前的字符、匹配部分的字符串、匹配部分后的字符串
<pre>
/C./ =~ "ABCDEF"
p $\` #=> "AB"
p $& #=> "CD"
p $' #=> "EF"
</pre>

- - - -
## 正则表达式的应用
- str.sub(/模式/, 替换的内容) 只置换首次匹配的部分
- str.gsub(/模式/, 替换的内容) 置换所有匹配的部分
- str.scan(/模式/) do |mached| ~ end 处理所有匹配的部分，如果模式有多个分组，则块变量是个分组

- - - -
## 标准输入/输出
- 标准输入 $stdin 默认就是键盘输入
- 标准输出 $stdout 默认就是屏幕
- 标准错误输出 $stderr 默认就是屏幕

### 重定向默认输入输出
执行命令是， 命令后加上\>文件名，就是重定向默认输出; 加上\<文件名，就是重定向默认输入;

### 判断标准输入输出是否被重定向
\$stdin.tty?  \$stdout.tty? 如果返回true，则没有被重定向.

- - - -
## 输入操作自定义换行符
使用参数rs对读入的字符串进行分行:
- io.gets(rs)<br/>
io.each(rs)<br/>
io.each\_line(rs)<br/>
io.readlines(rs)

- - - -
## 文件指针

### io.pos io.pos=(positin)
通过pos获取文件指针当前位置; pos=改变文件指针的位置

### io.seek(offset, whence)
whence|意义
:--|:--
IO::SEEK\_SET|移动到offset指定的位置
IO::SEEK\_CUR|移动到相对于当前位置offset的位置
IO::SEEK\_END|移动到相对于文件末尾offset的位置

### io.rewind 
文件指针重新回到文件开头

### io.truncate(size)
按照size的大小对文件进行截断

- - - -
## io.binmode
默认情况下，文件输入输出会对\n符号正对不同的OS进行转换，设置了binmode之后，就不进行这个操作。

- - - -
## 与命令行进行交互
IO.popen(command, mode) open("/command", mode)<br/>
作用: IO对象的输出会作为命令的输入，命令的输出会作为IO对象的输出

<pre>
file = IO.popen("ls -l ~")
file.each\_line do |line|
  p line
end

➜  ruby\_programming ruby helloruby.rb
"total 76\n"
"-rw-rw-r--  1 lsff lsff    0 4月   8 12:14 1.dump\n"
"-rw-r--r--  1 root root 7487 4月   8 12:15 abc.dump\n"
"drwxrwxr-x 12 lsff lsff 4096 3月  12 14:52 bookmarks\n"
. . . .
</pre>

- - - -
## stringio库
StringIO对象用于模拟IO对象的对象，可以用来操作字符串

- - - -
## Dir.glob
可以通过模式匹配特定的文件名，然后以数组的形式返回

- - - -
## find库
find库中的find模块用于对指定的目录下的目录或文件做**递归**处理

- Find.find(dir) {|path| ...} 將目录dir下的所有文件路径逐个传给path
- Find.prune 在Find.find调用中调用prune，程序会跳过当前查找目录下的所有路径
- Find.next 与prune类似，next指示跳过当前目录，子目录的内容还是会继续查找

- - - -
## Proc对象的创建

- Proc.new(...)
<pre>
hello1 = Proc.new() do |name|
  puts "hello #{name}"
end
</pre>
- proc{...}
<pre>
hello2 = proc do |name|
  puts "hello #{name}"
end
</pre>
- lambda 这种创建方式对参数的检查更加严密
<pre>
hello3 = lambda do |name|
  puts "hello #{name}"
end
or
hello3 = -> (name) { puts("hello #{name}") }
</pre>

- - - -
## to\_proc
调用方法中指定块时，如果以`&对象`的形式传递参数，`对象.to_proc`就会被自动调用，从而生成Proc对象。Symbol#to\_proc比较经典,对符号:to\_i使用Symbol#to\_proc方法，生成的Proc对象为`Proc.new {|arg| arg.to_i}`

<pre>
p %w(1 2 3)                 #=> ["1", "2", "3"]

p %w(1 2 3).map {|i| i.to_i}#=> [1,2,3]

p %w(1 2 3).map(&:to_i)     #=> [1,2,3]
</pre>

- - - -
## Proc类的实例方法
### 调用Proc对象prc
- proc.call(args, ...) 
- proc[args, ...]
- proc.yield(args, ...)
- proc.(args, ...)
- proc === arg 通过===指定的参数只能1个

#### proc===在case语句中的使用

<pre>
fizz = proc{|n| n % 3 == 0}
buzz = proc{|n| n % 5 == 0}
fizzbuzz = proc{|n| n % 3 == 0 && n % 5 == 0}

(1..15).each do |i|
  case i
  when fizzbuzz then puts "fizz buzz"
  when fizz then puts "fizz"
  when buzz then puts "buzz"
  else puts i
  end
end

输出结果:
1
2
fizz
4
buzz
fizz
7
8
fizz
buzz
11
fizz
13
14
fizz buzz
</pre>

### proc.arity
返回块参数的个数，以|\*args|的形式指定块变量时，返回-1

### proc.parameters
返回块变量的详细信息

### proc.lambda?
判断proc是否通过lambda定义的方法。


[reflink_1]: http://stackoverflow.com/questions/885414/a-concise-explanation-of-nil-v-empty-v-blank-in-ruby-on-rails
[pic_1]: ./ruby_nil_empty_blank_table.png "Userful Value Table"
