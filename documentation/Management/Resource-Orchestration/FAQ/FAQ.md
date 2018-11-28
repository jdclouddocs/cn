#### 1.什么是资源编排

资源编排是一项简化云计算资源管理和运维的服务。用户通过模板描述多个京东云资源的配置信息和依赖关系，通过模板创建资源栈，自动完成所有资源的创建和配置，以实现资源的统一管理和自动化运维等目的。

#### 2.什么是编排模板

编排模板是一种标准化的资源和应用交付方式。在模板中定义所需的云资源、资源间的依赖关系、资源配置属性以及自动化部署特定应用程序所必需的参数等。

#### 3.什么是示例模板

示例模板是京东云官方提供的标准化的资源和应用交付方式的参考文本文件，覆盖多个应用场景，帮助降低用户的使用成本。

#### 3.什么是资源栈

资源栈一种逻辑集合，该集合用来统一管理一组云资源。对于云资源的创建、更新、删除等操作，都以资源栈为单位来完成。

#### 4.编辑编排文件，提示解析内容失败时怎么办？

请检查编排文件的格式是否书写错误，如：是否有多余或缺失的空格、是否缺少必要的字段、是否包含非法字符等。

#### 5.如何在编排中建立服务之间（比如web服务与数据库）的联系？

通过配置编排文件的服务的环境变量中添加关联服务信息。
示例:web服务wordpress链接mysql,请在wordpress服务处添加环境变量：

> environment:
>   WORDPRESS_DB_HOST: ${name}-mysql


#### 6. 资源栈创建失败的原因？

> 资源栈重名

同一用户创建的资源栈名不能重复。

> 超出可创建资源栈数量限制

目前资源编排服务限制每个用户最多可创建 50 个资源栈。

> 资源创建失败

由于某个资源创建失败，也会导致资源栈创建失败。如XXXX

#### 7. 删除资源栈失败的原因？

如果不能删除一个资源栈，原因为该资源栈正在操作中。

如果资源栈正在创建中、更新中、或者其它操作中，只有等到操作成功或者失败后，才能够删除该资源栈。


#### 8. 给VM资源指定镜像的方式？