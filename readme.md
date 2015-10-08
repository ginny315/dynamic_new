1、在html结构中写好ID、tag，当展示详情页时直接append进去。
 结果：前面结构中有，当下拉加载之后没有。
 原因：下拉之后实例化对象时，不能保证页面已经渲染好前面的dom了，所以append时候会出现没有ID、tag。

2、瀑布流性能问题：采取方式，先计算列数，为了简便，定死了3列，采取遍历dom 结构，获取dom结构的高度，放入只有3个元素的数组中，当小于3时直接放入，大于3时，获取数组中的最小值，将此元素的高度加上这个最小值，作为这个数组当前的高度，然后每渲染一个元素获取一次数组最小值，效率为3*n。
将每个dom结构作为一个组件，初始化是通过ajax请求获取其ID和详情，实例化对象插入dom树中，position:absolute,计算left,top，结果是下拉加载时卡顿，FPS降低至40左右，影响人眼的正常视觉范围，同时点击对象时能最较快响应，但是渲染出遮罩详情页时卡顿很久，猜测原因：从dom树中取出元素获取详情要重新渲染计算，解决方式：观察network，查看渲染情况，暂时没有取得答案。
![](http://i.imgur.com/irnYKGb.png)

3、关于请求的优化：首先发送请求，从请求的回调函数中进行筛选
缺点：返回结果数量有限，不能保证在每次搜索之后每个页面展现数据量相同，用户体验不好
修改：先做好筛选条件，在请求时加入进去，使返回时数据基本一致（在数据库中有足够数据的情况下）

4、报FFF内部错

5、监听器的位置  index还是picture中写更好  
6、点击返回按钮  浏览器卡住,点击详情的时候就传递数据，在外面渲染？？？得出结论 DOM过多，在很多中难以找到
想法：jquery从根元素开始寻找，将详情页放在靠近根元素的地方，结果并没有改善，猜测：还在计算DOM？

7、absolute会不会新建一个层，覆盖层改变原来dom？没有，用的translate，但是渲染了
8、translate相对的父元素
left、right相对于上一个非absolute元素，translate相对于自己原来的位置
9、详情页html css js重点橙色下线随机出现，猜想:dom渲染的先后，首先渲染的默认为第一个