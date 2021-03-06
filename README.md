# 模式

> 经典后端模型MVC
> 演变的MVVM模型

## 1. MVC模型

> 后端经典模型

1. model 数据
2. view 视图层
3. control 控制层 通过不同的url+method获取不同的资源


## 2. MVVM

> 后端的MVC模式相对而言view层所要做的事比较单一,可以通过该模式进行管理
> 前端页面多处会触发数据改变(触发事件、ajax请求、延时器/定时器等),如此利用MVC模型将造成大量的代码冗余,复杂度高

1. MVVM框架主要包括三部分组成，其中有model,view,view-model
   - model表示数据，各种数据，后端，缓存
   - view表示用户所看到的页面
   - view-model处理业务和数据

           
   view  <——  双向数据绑定 ——>  view-model   <————> model
         



   view-model 通过双向数据绑定 实时的将view-model中的数据展现在view中。通过http请求将后端数据与view-model中的数据同步

  1. 缺点
     - bug难以定位，当页面展示出现问题时，bug的产生可能是view层自己代码错误，也有可能是view-model处理业务错误，或者是后端数据错误。
     - 构建大型项目，成本较高(不了解)

   **MVVM原理的框架的精髓在于将用户的行为和状态分离出一个抽象**
## 3. 关于VUE响应原理

> 观察者模式与发布订阅模式的一个结合应用。

Observer通过Obeject.defineProperty()进行数据劫持，即在数据发生读写的时候会执行相应的get/set方法，在生成Observe时会为每一个属性通过闭包绑定一个Dep，这个Dep就是订阅者，在get中进行收集Watcher，因为在Watcher出生是就把自己赋给Dep的静态属性上target，所以在收集时，只需要从自己的静态属性取即可，在set中去通知自己下面的所有Watcher执行更新方法，在Watcher执行更新时，它会采用异步更新策略，根据事件循环机制，将异步更新放置在微队列中将更加高效，但是在一些特殊时候，会出现混乱，因此也会放在宏队列中。Watcher将新的虚拟节点放入patch中通过diff算法进行对比并更新，最后更新视图。

执行视图更新

Event Loop:  宏队列 => 微队列 => 视图更新 => worker
一开始宏队列拥有一个script脚本 被推入到执行栈中执行，执行过程中产生异步事件，分别放入宏队列与微队列中，同步代码执行完成后，script脚本被移除宏队列，然后开始执行微队列，微队列出队方式为一队一队的出，与宏队列一个一个的出栈方式有所不同。 **区别微队列优先于宏队列，只是这种机制造成这种现象**

Setter ==> Dep ==> Watcher ----异步更新(nextTick)---- ==>patch ==> 更新视图

## 关于Vuex状态管理原理

在vue中引入vuex是通过Vue.use(),使用该方法需要一个install()方法，在install中通过Vue.mixin({beforCreate:vuexInit})将vuexInit混入到钩子函数中，在初始化时，每个实例根据自身情况获得store，根节点直接获取，非根节点通过父节点的$store获取

store数据实现响应，通过Vue实现数据响应

接下来实现相应的commit，dispatch，

   
