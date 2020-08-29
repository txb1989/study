#### [学习博客https://blog.csdn.net/u011541946/category_8223796.html](https://blog.csdn.net/u011541946/category_8223796.html)

### CD开发流程

![CDPipeline流程图](https://img-blog.csdn.net/20181016170002335?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTE1NDE5NDY=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

1. 开发提交代码到项目仓库服务器
2. 开始执行Pipeline代码文件，开始从仓库check out代码
3. 启动Pipeline里面第一个stage，stage就是阶段的意思，后面会介绍语法
4. 图里面第一个Stage应该是代码打包构建（Build）
5. 然后进入测试的阶段，执行各种自动化测试验证
6. 然后测试结束，到运维的部署阶段。
7. 部署结束，输出报告，整个自动化流程工作完成，等待触发构建，开始重复下一轮1到7步骤。

#### Pipeline的一些概念

**pipeline**

​    这个单词是小写，可以看作是Pipeline语法中的一个关键字。以后一个groovy文件或者一个Jenkinsfile文件中不光只有Pipeline代码，例如还有其他的工具类方法等。通过pipeline { Pipeline代码}，这个关键字就是告诉Jenkins接下来{}中的代码就是pipeline代码，和普通的函数或者方法隔离出来。

**node**

​    关键字node就是用来区分，Jenkins环境中不同的节点环境。例如一个Jenkins环境包括master节点，也就是主节点，还包括N多个从节点，这些从节点在添加到主节点的向导页面中有一个参数，好像是label，就是给这个从节点取一个名称。在Pipeline代码中可以通过node这个关键字告诉Jenkins去用哪一台节点机器去执行代码。

**stage**

​    关键字stage，就是一段代码块，一般个stage包含一个业务场景的自动化，例如build是一个stage, test是第二个stage，deploy是第三个stage。通过stage隔离，让Pipeline代码读写非常直观。到后面你还会学习stages这个关键字，一个stages包含多个stage。

**step**

​    关键字step就是一个简单步骤，一般就是几行代码或者调用外部一个模块类的具体功能。这里step是写在stage的大括号里的。

​    以上就是Pipeline的基础语法内容，下一篇，介绍Declarative Pipeline 和 Scripted Pipeline在Jenkins环境上的实战练习，并解释没一行代码的含义，顺便复习一下Pipeline的基础语法，也就是pipeline, node, stage, step这几个关键字的使用。

#### Pipeline有两种模式，**Declarative**（声明模式）和**Scripted**（脚本）模式

> **Declarative**
>
> ```groovy
> pipeline {
>     agent any 
>     stages {
>         stage('Build') { 
>             steps {
>                 println "Build" 
>             }
>         }
>         stage('Test') { 
>             steps {
>                 println "Test" 
>             }
>         }
>         stage('Deploy') { 
>             steps {
>                 println "Deploy" 
>             }
>         }
>     }
> }
> ```
>
> 
>
> ##### 代码说明：
>
> 1. Declarative模式最外层是一定是pipeline，但是并不强制要求是第一行，前面可以包含其他代码，如导入语句或者其他功能代码。pipeline是一个执行pipeline代码的入口，jenkins可以根据这个入门开始执行里面不同stage
> 2. agent是代理的意思，这个和选择用jenkins平台上那一台机器去执行任务构建有关
> 3. 第三行的stages表示stage（场景）列表，一个Pipeline可以包含多个stage，如上面的代码包含了三个stage
> 4. stage("Build")：具体的场景，上面代码就表示了构建、测试和部署三个场景，
> 5. steps：stage的操作步骤，这里可以定义变量，调用模块。一般来说，一个steps{}里面就写几行代码，或者一个try catch语句。
>
> **Scripted模式**
>
> ```groovy
> node {  
>     stage('Build') { 
>         // 
>     }
>     stage('Test') { 
>         // 
>     }
>     stage('Deploy') { 
>         // 
>     }
> }
> ```
>
> 
>
> ##### 代码说明
>
> 1. script模式最外层是node
> 2. scripted模式下没有stages这个关键字或者指令，只有stage。上面其实可以node('Node name') {}来开头，Node name就是从节点或master节点的名称

#### agent指令

​      简单来说，agent部分主要作用就是告诉Jenkins，选择那台节点机器去执行Pipeline代码。取值：any、none、label、node、docker、dockerfile

> - any：在任何可用的代理上执行Pipeline或stage。这种是最简单的，如果你Jenkins平台环境只有一个master，那么这种写法就最省事情
>
> ```groovy
> pipeline {
>      agent any
> }
> ```
>
> 
>
> - none：当在`pipeline`块的顶层应用时，将不会为整个Pipeline运行分配全局代理，并且每个`stage`部分将需要包含其自己的`agent`部分。
>
> ```groovy
> pipeline {
>     agent none
>     stages {
>         stage(‘Build’){
> 	    agent {
>                label ‘具体的节点名称’
>             }
>         }
>     }
> }
> ```
>
> 
>
> - label：使用提供的标签在Jenkins环境中可用的代理机器上执行Pipeline或stage内执行。
>
> ```groovy
> pipeline {
>    agent {
>       label ‘具体一个节点label名称’
>    }
> }
> ```
>
> 
>
> - node：和上面label功能类似，但是node运行其他选项，例如customWorkspace
>
> ```示例代码```
>
> ```groovy
> pipeline {
>     agent {
>         node {
>             label ‘xxx-agent-机器’
>             customWorkspace "${env.JOB_NAME}/${env.BUILD_NUMBER}"
>         }
>     }
> }
> ```
>
> 

#### post指令

post部分定义将在Pipeline运行或阶段结束时运行的操作。post可以放在顶层，也就是和agent{…}同级，也可以放在stage里面。一般放顶层的比较多。而且pipeline代码中post代码块不是必须的，使用post的场景基本上执行完一个构建，进行发送消息通知，例如构建失败会发邮件通知。

pipeline取值：always，changed，failure，success，unstable，和aborted

```groovy
Pipeline {
    agent any
        stages {
            stage (‘Test’) {
        }
    }
    post {
          //写相关post部分代码
    }
}
```





- always：无论Pipeline运行的完成状态如何都会执行这段代码always场景，很容易想到的场景就是，事后清理环境。例如测试完了，对数据库进行恢复操作，恢复到测试之前的环境。

  > ```groovy
  > 
  > pipeline {
  >     agent {
  >         node {
  >             label ‘xxx-agent-机器’
  >             customWorkspace "${env.JOB_NAME}/${env.BUILD_NUMBER}"
  >         }
  >     }
  >     stages {
  >         stage (‘Build’) {
  >             bat “dir” // 如果jenkins安装在windows并执行这部分代码
  >             sh “pwd”  //这个是Linux的执行
  >         }
  >     }
  >     post {
  >         always {
  >             script {
  >                 //写相关清除/恢复环境等操作代码
  >             }
  >         }
  >     }
  > }
  > ```
  >
  > 
  >
  > 

- changed：只有当前Pipeline运行的状态与先前完成的Pipeline的状态不同时，才能触发运行。这个场景，大部分是写发邮件状态。例如，你最近几次构建都是成功，突然变成不是成功状态，里面就触发发邮件通知。当然，使用changed这个指令没success和failure要频率高。

  > ```示例代码```
  >
  > ```groovy
  > pipeline {
  >     agent {
  >         node {
  >             label ‘xxx-agent-机器’
  >             customWorkspace "${env.JOB_NAME}/${env.BUILD_NUMBER}"
  >         }
  >     }
  >     stages {
  >         stage (‘Build’) {
  >             bat “dir” // 如果jenkins安装在windows并执行这部分代码
  >             sh “pwd”  //这个是Linux的执行
  >         }
  >     }
  >     post {
  >         changed {
  >             script {
  >                 // 例如发邮件代码
  >             }
  >         }
  >     }
  > }
  > ```

- failure：只有当前Pipeline运行的状态与先前完成的Pipeline的状态不同时，才能触发运行。ailure条件一般来说，百分百会写到Pipeline代码中，内容无非就是发邮件通知，或者发微信群，钉钉机器人，还有国外的slack聊天群组等。

  ```groovy
  pipeline {
      agent {
          node {
              label ‘xxx-agent-机器’
              //customWorkspace "${env.JOB_NAME}/${env.BUILD_NUMBER}"
          }
      }
      stages {
          stage (‘Build’) {
              bat “dir” // 如果jenkins安装在windows并执行这部分代码
              sh “pwd”  //这个是Linux的执行
          }
      }
      post {
          failure {
              script {
                  // 例如发邮件代码
              }
          }
      }
  }
  ```

#### enviroment指令

该指令指定一系列键值队，这些键值队将被定义为所有步骤的环境变量或阶段特定步骤，具体取决于environment指令位于Pipeline中的位置。代码举例：

```groovy
pipeline {
    agent any
    environment {
        unit_test = true
    }
    stages {
        stage('Example') {
            steps {
                if(unit_test == true) {
                   // call run unit test methods
                }
            }
        }
    }
}
```

