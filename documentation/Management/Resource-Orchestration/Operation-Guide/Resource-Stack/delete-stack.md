# 删除资源栈


## 操作步骤：

一、选中需要删除的资源栈，点击“删除”按钮，弹出删除资源栈的确认页面，提示：“确定删除资源栈？”。如下图所示：

![](https://raw.githubusercontent.com/jdclouddocs/cn/resource-orchestration/image/resource/delete%20stack001.png)

二、点击“确定”，完成删除操作，进入资源栈列表页面，删除的资源栈的状态显示“删除中”

![](https://raw.githubusercontent.com/jdclouddocs/cn/resource-orchestration/image/resource/delete%20stack002.png)


## 注意事项

- 删除资源栈会同时删除资源栈下的资源，若想保留栈中的部分资源请在模板中自行定义删除策略。
  
  相关内容参见 编排模板基本语法说明-资源（Resources）- 删除策略（DeletionPolicy）
  
  
  删除策略（DeletionPolicy）

  在模板中，设置 DeletionPolicy 属性，可以声明在资源栈被删除时保留某个资源。可选值为Delete（删除）、Retain（保留）。

  例如，设置在资源栈删除时，保留 vm 实例。可按照如下代码段进行声明：
     
     ```
      "Resources" : {
        "MyInstance" : {
          "Type" : "JDCLOUD::VM::Instance",
          "Properties" : {
            "ImageId" : "img-wcewkxc5c1"
          },
          "DeletionPolicy" : "Retain"
        }
      }
      ```
