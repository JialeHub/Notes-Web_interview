# MVVM、MVC、MVP

MVC、MVP 和 MVVM 是三种常见的软件架构设计模式，主要通过**分离关注点**的方式来**组织代码结构**，优化开发效率。

## MVC

MVC 通过分离 **Model**、**View** 和 **Controller** 的方式来组织代码结构。其中 **View** 负责页面的**显示**逻辑，**Model** 负责**存储**页面的**业务数据**，以及对相应**数据的操作**。并且 View 和 Model 应用了**观察者模式**，当 Model 层发生改变的时候它会通知有关 View 层更新页面。**Controller** 层是 View 层和 Model 层的**纽带**，它主要负责用户与应用的**响应操作**，当用户与页面产生**交互**的时候，Controller 中的**事件触发器**就开始工作了，通过**调用 Model** 层，来完成对 Model 的修改，然后 Model 层再去通知 View 层更新。 

- **优点**：
  - **耦合性低**：在开发界面显示部分时，你仅仅需要考虑的是如何布局一个好的用户界面；开发模型时，你仅仅要考虑的是业务逻辑和数据维护，这样能使开发者专注于某一方面的开发，提高开发效率。
  - **重用性高**：因为模型是独立于视图的，所以可以把一个模型独立地移植到新的平台工作。
  - **部署快**；
  - **可维护性高**；
  - **有利于软件工程化管理**。
- **缺点**：
  - **不适合小型中等规模**的应用程序；
  - 增加了系统结果和实现的**复杂性**；
  - View和Model之间不匹配，用户界面和流程要考虑易用性，用户体验优化同时考虑业务流程的精确和无错。
  - View的变化不能完全由Model控制，即Observer模式不足以支持复杂的用户交互。这其实要求VC之间要有依赖。牵一发而动全身，数据，显示不分离，Controller，Model联系过于紧密。
  - Controler和Model之间界线不清，什么样的逻辑是界面逻辑，什么样的逻辑是业务逻辑，很难定义清楚。没有明确的定义；

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a65e1b9145894647a25788caf12ddd26~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)



## MVVM

分为 Model、View、ViewModel：
**Model**：代表**数据模型**，**数据和业务逻辑**都在Model层中定义；（Dep）
**View**：代表UI视图，负责数据的**展示**；
**ViewModel**：负责**监听Model**中数据的改变并且控制**视图的更新**，处理用户**交互**操作；（Watcher）

Model和View并无直接关联，而是通过ViewModel来进行联系的，**Model和ViewModel**之间有着**双向数据绑定**的联系。因此当Model中的数据改变时会触发View层的刷新，View中由于用户**交互**操作而改变的数据也会在Model中同步。这种模式实现了 Model和View的数据自动同步，因此开发者只需要专注于数据的维护操作即可，而不需要自己操作DOM。

视图V上的交互导致VM触发相应逻辑，修改M模型；
当M模型数据发生变化，被VM监听到，VM触发相应回调通知V视图更新。

**优点**：

- **低耦合**：View可以独立于Model变化和修改，一个ViewModel可以绑定到不同的View上，当View变化的时候Model可以不变，当Model变化的时候View也可以不变。
- **可重用性**：可以把一些视图的逻辑放在ViewModel里面，让很多View重用这段视图逻辑。
- **独立开发**：开发人员可以专注与业务逻辑和数据的开发(ViewModel)。设计人员可以专注于界面(View)的设计。
- **可测试性**：可以针对ViewModel来对界面(View)进行测试
- **自动更新DOM**：利⽤双向绑定,数据更新后视图⾃动更新,让开发者从繁琐的⼿动dom中解放

**缺点**：

- **DEBUG困难**：Bug很难被调试，因为使⽤双向绑定的模式，当你看到界⾯异常了，有可能是你View的代码有Bug，也可能是Model的代码有问题。数据绑定使得⼀个位置的Bug被快速传递到别的位置，要定位原始出问题的地⽅就变得不那么容易了。另外，数据绑定的声明是指令式地写在View的模版当中的，这些内容是没办法去打断点debug的
- ⼀个⼤的模块中model也会很⼤，花费更多的**内存**
- 对于⼤型的图形应⽤程序，视图状态较多，ViewModel的**构建和维护的成本**都会⽐较⾼。

**应用场景**：

- 针对具有**复杂交互逻辑**的前端应用
- 提供**基础的架构抽象**
- 通过**Ajax数据持久化**，保证前端用户体验

**常见的MVVM框架**：Vue.js，AngularJs，ReactJs

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d5ce15b7b704483eb91ee1f5d1d64786~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)



## MVP

MVP 模式与 MVC 唯一不同的在于 **Presenter(发布层)** 和 Controller。在 MVC 模式中使用观察者模式，来实现当 Model 层数据发生变化的时候，通知 View 层的更新。这样 View 层和 Model 层耦合在一起，当项目逻辑变得复杂的时候，可能会造成代码的混乱，并且可能会对代码的复用性造成一些问题。MVP 的模式通过使用 **Presenter** 来实现对 View 层和 Model 层的解耦。MVC 中的Controller 只知道 Model 的接口，因此它没有办法控制 View 层的更新，MVP 模式中，View 层的接口暴露给了 Presenter 因此可以在 Presenter 中**将 Model 的变化和 View 的变化绑定**在一起，以此来实现 View 和 Model 的**同步更新**。这样就实现了对 View 和 Model 的解耦，Presenter 还包含了其他的响应逻辑。

**优点**：

- **低耦合**：模型与视图完全分离，我们可以**修改视图而不影响模型**；
- **可重用性**：我们可以将一个Presenter用于**多个视图**，而不需要改变Presenter的逻辑。这个特性非常的有用，因为视图的变化总是比模型的变化频繁；
- 可以更**高效**地**使用模型**，因为所有的**交互**都发生在一个地方——Presenter内部；
- **可测试性**：如果我们把逻辑放在Presenter中，那么我们就可以脱离用户接口来测试这些逻辑（**单元测试**）。

**缺点**：

- MVP主要在进行DOM操作，**视图和Presenter的交互会过于频繁**，使得他们的联系过于紧密。也就是说，一旦**视图变更**了，**presenter也要变更**。

![img](https://upload-images.jianshu.io/upload_images/1858247-57db1c3f3fa9380d.png?imageMogr2/auto-orient/strip|imageView2/2/w/822/format/webp)

