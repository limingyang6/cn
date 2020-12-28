# 常见问题

**Q：设备上报了一个事件，但是数据概览页的上报事件曲线仍显示为0，为什么？**

A：事件数据的统计有一定延迟

**Q：应用创建成功后，其对应的AppID和Secret是什么意思？**

A：用户创建的应用在智能生活物联网平台中被看做是一个上层应用，平台为所创建应用分配的AppID和Secret是用于上层应用与本平台通信时，接口请求中信息鉴权校验和参数签名

**Q：产品创建时选择了某个标准物模型，创建后还能修改吗？**

A：可以修改。标准物模型仅仅是提供了某类型产品的一个标准模板，在创建产品后可根据产品的实际功能进行编辑

**Q：已经创建的产品不想要了可以删除吗？**

A：已创建的产品已绑定设备后无法删除，未绑定设备可以删除

**Q：打开设备的属性tab页面， 能看到设备的属性值，但属性值对应的期望值列不允许填写，怎么办？**

A：查看该属性的权限值，如果权限是“r”,说明该属性值是只读的，不允许修改是正常的
对于想下发期望值的属性，需要在新建物模型的时候就将属性值设置为rw或w

**Q：新建规则引擎时的类SQL语句的表名是固定的吗？**

A：不是。如：select * from aaa where timestamp > 1592289113。其中表名“aaa”可以填写任意字符串，过滤时只参考where后的条件部分

**Q：创建的规则启动时提示无可用资源该怎么处理？**

A：建议停止不用的规则释放资源