### setState

>setState是一个异步的操作 

> 参考https://www.jianshu.com/p/7ab07f8c954c
```
执行了setState后不会立刻打印出state操作后的值，如果一次性调用多个setState，打印后的结果还会使原来的值
这就是因为它是异步的，会在所有的同步的代码执行完之后才会执行
```
> 原因在于setState之后会触发render，重绘DOM，多次调用就会造成不必要的性能损失，设计成异步的话，将多次调用放入一个队列中，统一进行更新DOM的过程

```
如何在调用完之后立即生效呢
- 使用setState的回掉方法可以实现，
this.setState((prevState) => ({ count: prevState.count + 1 }), () => {
    console.log(this.state)
})
```

#### 看下setState的源码
```
/**
 * Sets a subset of the state. Always use this to mutate
 * state. You should treat `this.state` as immutable.
 *
 * There is no guarantee that `this.state` will be immediately updated, so
 * accessing `this.state` after calling this method may return the old value.
 *
 * There is no guarantee that calls to `setState` will run synchronously,
 * as they may eventually be batched together.  You can provide an optional
 * callback that will be executed when the call to setState is actually
 * completed.
 *
 * When a function is provided to setState, it will be called at some point in
 * the future (not synchronously). It will be called with the up to date
 * component arguments (state, props, context). These values can be different
 * from this.* because your function may be called after receiveProps but before
 * shouldComponentUpdate, and this new state, props, and context will not yet be
 * assigned to this.
 *
 * @param {object|function} partialState Next partial state or function to
 *        produce next partial state to be merged with current state.
 * @param {?function} callback Called after state is updated.
 * @final
 * @protected
 */

Component.prototype.setState = function (partialState, callback) {
  if (!(typeof partialState === 'object' || typeof partialState === 'function' || partialState == null)) {
    {
      throw Error("setState(...): takes an object of state variables to update or a function which returns an object of state variables.");
    }
  }

  this.updater.enqueueSetState(this, partialState, callback, 'setState');
};
```
```
可以看到setState有两个参数，partialState和callback
partialState必须是一个对象
然后执行了enqueueSetState方法
```

```
  /**
   * Sets a subset of the state. This only exists because _pendingState is
   * internal. This provides a merging strategy that is not available to deep
   * properties which is confusing. TODO: Expose pendingState or don't use it
   * during the merge.
   *设置状态的子集。这是因为_pendingState是内部的。这就提供了一种合并策略，而这种策略对于令人困惑的深层属性是不可用的。TODO:在合并过程中暴露pendingState或者不使用它。
   *
   * @param {ReactClass} publicInstance The instance that should rerender.
   * @param {object} partialState Next partial state to be merged with state.
   * @param {?function} callback Called after component is updated.
   * @param {?string} Name of the calling function in the public API.
   * @internal
   */
  enqueueSetState: function (publicInstance, partialState, callback, callerName) {
    warnNoop(publicInstance, 'setState');
  }
```

```
enqueueSetState是个队列机制，进行更新合并
继续走warnNoop方法，这里warnNoop只是一个警告方法，所以实际的方法并非走的这个，
```

```
function warnNoop(publicInstance, callerName) {
  {
    var _constructor = publicInstance.constructor;
    var componentName = _constructor && (_constructor.displayName || _constructor.name) || 'ReactClass';
    var warningKey = componentName + "." + callerName;

    if (didWarnStateUpdateForUnmountedComponent[warningKey]) {
      return;
    }

    warningWithoutStack$1(false, "Can't call %s on a component that is not yet mounted. " + 'This is a no-op, but it might indicate a bug in your application. ' + 'Instead, assign to `this.state` directly or define a `state = {};` ' + 'class property with the desired state in the %s component.', callerName, componentName);
    didWarnStateUpdateForUnmountedComponent[warningKey] = true;
  }
}
```

#### 实际上还是走的这里--ReactNoopUpdateQueue
```
function PureComponent(props, context, updater) {
  this.props = props;
  this.context = context; // If a component has string refs, we will assign a different object later.

  this.refs = emptyObject;
  this.updater = updater || ReactNoopUpdateQueue;
}
```
待更新...

> 参考 https://segmentfault.com/a/1190000016998246
