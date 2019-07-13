# Rest描述

学名 : **表现层状态转移**

解释 : **URL定位资源,用HTTP动词(GET,POST,DELETE,DETC)描述操作**





1. Rest是一种思想,描述,抽象的东西,不具体

   它用来描述网络应用中,Client和Server交互的**方式**

2. Restful是一种**技术**,(Rest风格的网络接口)

   如:

   - 增加一个朋友 uri: generalcode.cn/va/friends 接口类型：POST

   - 删除一个朋友 uri: generalcode.cn/va/friends 接口类型：DELETE

   - 修改一个朋友 uri: generalcode.cn/va/friends 接口类型：PUT

   - 查找朋友        uri:  generalcode.cn/va/friends 接口类型：GET

   反例:

   - 表示删除朋友 uri: generalcode.cn/va/deleteFriends 这样就不符合Rest风格





3. 总结而言,请求地址相同,请求方式不同,代表的增删改查类型不同

**用URL定位资源，用HTTP描述操作**



- 看**Url**就知道要什么
- 看**http method**就知道干什么
- 看**http status code**就知道结果如何