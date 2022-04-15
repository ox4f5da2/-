# display:none、visibility:hidden和 opacity:0区别

### 共同点

都可以隐藏元素，让元素不可见

### 区别

**display: none**

1. DOM 结构：浏览器不会渲染 display 属性为 none 的元素，不占据空间；
2. 事件监听：无法进行 DOM 事件监听；
3. 性能：动态改变此属性时会引起重排，性能较差；
4. 继承：不会被子元素继承，毕竟子类也不会被渲染；
5. transition：transition 不支持 display。

**visibility: hidden**

1. DOM 结构：元素被隐藏，但是会被渲染不会消失，占据空间；
2. 事件监听：无法进行 DOM 事件监听；
3. 性能：动态改变此属性时会引起重绘，性能较高；
4. 继承：会被子元素继承，子元素可以通过设置 visibility: visible; 来取消隐藏；
5. transition：transition 支持 visibility。

**opacity: 0**

1. DOM 结构：透明度为 100%，元素隐藏，占据空间；
2. 事件监听：可以进行 DOM 事件监听；
3. 性能：提升为合成层，不会触发重绘，性能较高；
4. 继承：会被子元素继承,且，子元素并不能通过 opacity: 1 来取消隐藏；
5. transition：transition 支持 opacity。



### 总结

|                    | display:none | visibility:hidden | opacity:0  |
| :----------------: | :----------: | :---------------: | :--------: |
|       页面中       |  **不存在**  |       存在        |    存在    |
|        重绘        |      会      |        会         | **不一定** |
|        重排        |    **会**    |       不会        |    不会    |
|    自身绑定事件    |    不触发    |      不触发       | **可触发** |
|     transition     |  **不支持**  |       支持        |    支持    |
|     子元素复原     |     不能     |      **能**       |    不能    |
| 被遮挡元素触发事件 |    不影响    |      不影响       |  **影响**  |

