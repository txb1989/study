### [学习博客    https://www.cnblogs.com/ryzen/p/12610249.html](https://www.cnblogs.com/ryzen/p/12610249.html)

#### Prism安装

* nuget安装Prism.Unity包，包含了Prism.WPF、Prism.Container和Prism.Unity.WPF
* 修改App.xaml.cs的基类，继承自PrismApplication，重写对应的RegisterTypes和CreateShell两个抽象方法，App.xaml增加prism的命名空间，然后修改标签未prism:PrismApplication，去掉StartupUri

#App启动类

* 前面修改了APP的基类未PrismApplication，PrismApplication基类包含了一堆虚方法和两个抽象方法

  > 抽象方法

  > > ` protected override Window CreateShell()`
  > >
  > > > 返回一个窗体类，通过IOC容器实例窗体对象并返回，程序启动时会直接显示该窗体

  > > ` protected override void RegisterTypes(IContainerRegistry containerRegistry)`
  >
  > > > 此方法主要是用于IOC容器注册
  >
  > 虚方法
  >
  > > `protected override void ConfigureViewModelLocator()`
  > >
  > > > 名字翻译配置ViewModel位置，其实就是配置ViewModel与View之间的映射，**Prism默认有个约束，就是视图放Views文件夹下，视图模型放在ViewModels文件夹下，然后ViewModel命名采用视图的名字后面跟上ViewModel的，这样Prism会自动映射两者的关系**
  > > >
  > > > 如果不想采用默认的约束，可以重写此方法，按照自己的约定来比如：
  > > >
  > > > ```  ConfigureViewModelLocator()重写
  > > >    protected override void ConfigureViewModelLocator()
  > > >         {
  > > >             base.ConfigureViewModelLocator();
  > > >             ViewModelLocationProvider.SetDefaultViewTypeToViewModelTypeResolver(viewType =>
  > > >             {
  > > >                 string typeName = viewType.FullName;
  > > >                 Assembly assembly = typeof(ViewModelBase).Assembly;
  > > >                 string viewModelName = $"{viewType.Name}ViewModel";
  > > >                 Type viewModelType = assembly.DefinedTypes.FirstOrDefault(t => t.Name == viewModelName);
  > > >                 return viewModelType;
  > > >             });
  > > >         }
  > > > ```

  > > `protected override void ConfigureModuleCatalog(IModuleCatalog moduleCatalog)`
  > >
  > > > Prism模块注册有三种方式：代码注册、扫描文件夹和配置文件
  > > >
  > > > ConfigureModuleCatalog方法是提供代码注册模块的入口方法，在此方法中可以进行模块的管理，但是需要添加项目的饮用
  > >
  > > `protected override IModuleCatalog CreateModuleCatalog()`
  > >
  > > > Prism模块注册有三种方式：代码注册、扫描文件夹和配置文件
  > > >
  > > > CreateModuleCatalog方法提供模块的配置，需要返回一个IModuleCatelog接口的实现类。如果使用目录扫描的方式，可以返回一个DirectoryModuleCatelog类，里面有个ModulePath属性确定模块存放的目录。然后Prism会自动扫描改目录，进行模块的初始化调用等等；如果使用配置文件的方式，则返回一个ConfigurationModuleCatelog类，默认读取App.Config

* 窗体类（MainWindow）

  > Code-Behind：没什么改变
  >
  > xaml：引入Prism名字控件，然后设置prism:ViewModelLocator.AutoWireViewModel="True"
  
* ViewModel

  > ViewModel继承自Prism.Mvvm.BindableBase
  >
  > 属性改变使用SetProperty通知，即在属性的Set方法使用SetProperty(ref _text, value);

#### Command

- 普通使用

> DelegateCommand包含两个版本，一个带泛型的，一个不带泛型的。不带泛型的绑定的是一个无参的方法。带泛型的绑定带一个参数的方法。如：
>
> ```c#
>  private DelegateCommand _clickCommnd;
>         public DelegateCommand ClickCommnd =>
>             _clickCommnd ?? (_clickCommnd = new DelegateCommand(()=> { }));
> 
>         private DelegateCommand<object> _inputTextChangeCommand;
> 
>         public DelegateCommand<object> InputTextChangeCommand => _inputTextChangeCommand ?? (_inputTextChangeCommand = new DelegateCommand<object>(content =>
>             {
>                 Text = content?.ToString() ?? "";
>             }));
> ```

- 基于Task的Command

  ```c#
          public DelegateCommand AsyncCommand => _asyncCommand ?? (_asyncCommand = new DelegateCommand(async () =>
          {
              await Task.FromResult(0);
          }));
  ```

- 复合命令

  content

  创建和使用一个prism标准复合命令的流程：

  - 创建一个全局的复合命令
  - 通过IOC容器注册其为单例
  - 给复合命令注册子命令
  - 绑定复合命令