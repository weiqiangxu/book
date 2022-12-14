# middlare如何实现next然后回来时候统计执行时长的

> 责任链模式将handler func注入router对象之中某一个route对象的handler func chain之中

```
func (c *Context) Next() {
	c.index++
	for c.index < int8(len(c.handlers)) {
		c.handlers[c.index](c)
		c.index++
	}
}
```