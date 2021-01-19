## 请简述 React 16 版本中初始渲染的流程

答：获取到container容器，为容器添加_react属性，注明是初始渲染还是更新渲染，然后将container内部元素循环删除，为FiberRoot和RootFiber建立关联，在currnt树渲染完毕后，就进行拷贝一份存储在 workInprohress Fiber树中，并进行属性的复用

## 为什么 React 16 版本中 render 阶段放弃了使用递归

答：因为递归调用在执行的时候是一层一层进入处理完后在一层一层出来会将主线程一直占用，如果页面的层级过深的情况下，就会出现页面的卡顿情况

## 请简述 React 16 版本中 commit 阶段的三个子阶段分别做了什么事情

答：

1.处理DOM节点渲染/删除后的 autoFocus、blur逻辑，调用getSnapshotBeforeUpdate生命周期钩子，调度useEffect。

2.如果该fiber类型是ClassComponent的话，执行getSnapshotBeforeUpdate生命周期api，将返回的值赋到fiber对象的__reactInternalSnapshotBeforeUpdate上，如果该fiber类型是FunctionComponent的话，执行hooks上的effect相关 API

3. 重置 nextEffect，useEffect是让FunctionComponent产生副作用的hooks，当使用useEffect后，会在fiber上的updateQueue.lastEffect生成effect链，具体请看ReactFiberHooks.js中的pushEffect()

## 请简述 workInProgress Fiber 树存在的意义是什么

答：主要意义是将Vdom的页面对比，差异校验，Vdom修改等操作交由内存来处理，因为在内存中的所有操作是可以暂停的，在内存中处理好Vdom树结构和更改后，直接将currnt树进行替换，拥有更高的响应速度，更好的提高了用户的体验