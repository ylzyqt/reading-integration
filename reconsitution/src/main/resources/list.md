## 重构列表
   
#### 提炼函数(Extract Method)   
      
     1. 如果每个函数的粒度都很小，那么被复用的可能性就很高
     2. 高层函数读起来像是一系列的注释
     3. 合适的函数命名
     4. 类型
        - 无局部变量
            直接把需要提炼的部分提出来
        - 有局部变量
            当成变量传递给目标函数
                
####  内联函数(Inner Method)
     1. 如果内部代码和函数名称同样清晰易懂，并且不具备可重用性，那么可以去掉这个函数   
     
####  内联临时变量(Inner Temp)
     1. 将对该变量的引用动作，替换为它赋值的那个表达式自身 
        double basePrice = anOrder.basePrice();
        return basePrice>1000
        -->
        return anOrder.basePrice > 1000
        
#### 以查询取代临时变量(Replace Temp With Query)   
     1. 将表达式提炼到独立的函数中去      

#### 引入解释性变量(Introduce Explaining Variable)
     1. 表达式可能非常复杂而难以阅读，这种情况下，临时变量可以帮助表达式分解为比较容易管理的形式
     2. 即使不用注释，也可以看清楚具体要表达什么
     3. 更多的是使用提炼函数，因为可能被重用.
     
#### 分解临时变量(Split Temporary Variable)
     1. 针对每次赋值，创造一个独立的，对应的临时变量
         
#### 移除对参数的赋值(Remove Assignments to Parameters)      
     1. 不要对参数的值进行变更,这让人迷惑   
                        
#### 以函数对象取代函数(Replace Method with Method Object)       
     1. 把功能移动到具体的类里面去，减少外部代码的复杂性         

#### 替换算法(Substitute Algorithm)
     1. 将函数本体替换为另一个算法