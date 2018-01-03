> 虚拟dom框架

### dva
> 基于 redux、redux-saga 和 react-router 的轻量级前端框架

### react
* 无状态组件
    ```javascript
    export default props => <div>Hello {props.name}</div>
    ```
    优点：
    1. 去除结构体和继承关系，纯函数更合理
    2. 代码更简洁，便于理解、测试。
    3. 后期的渲染性能更优 -- `react团队正在优化`
    4. 复用性更容易 -- 结合高阶组件，可以复用出不同的状态组件，且

* 高阶组件

* 路由写法


### vue
* 路由写法 -- 按需加载 `需要简化`

    ```
    import VueRouter from 'vue-router'
    const routes = [
        require('../routers/indexRouter').default,
    ]
    return new VueRouter(router)
    ```

    ```
    exports.default = {
      path     : '/',
      component: resolve => require(['../pages/index.vue'], resolve)
    }
    ```


### inferno
* webpack 建构注意点
    1. `.babelrc`文件 plugin中增加: `babel-plugin-inferno`
    2. inferno源框架不支持自定义事件。需要修改inferno模块中的`delegatedEvents`对象 然后修改webpack的`alias`
* 路由写法  -- 按需加载

    ```javascript
    import Inferno from 'inferno'
    import { Route, createRoutes } from 'inferno-router'

    const index     = {
      path: '/',                //路径定义
      getComponent(n, cb) {     //获取模块回调
        require.ensure([], () => {
          cb(null, require('../pages/index').default)
        }, 'index')
      }
    }
    // 定义路由配置数据
    const rootRoute = [ index ]
    // 1.
    export default rootRoute.map(item => {
      item.onEnter = function () {}
      item.onLeave = function () {}
      item.path    = process.env.PATH + item.path;
      return <Route { ...item }/>;
    })
    // 2.
    export default createRoutes(
        rootRoute.map(item => {
          item.onEnter = function () {}
          item.onLeave = function () {}
          item.path    = process.env.PATH + item.path;
          return item;
        })
    );
    ```

* 事件绑定 -- `源码`
    ```javascript
    function patchEvent(name, lastValue, nextValue, dom) {
      if (lastValue !== nextValue) {
        // 事件委托
        if (delegatedEvents.has(name)) {
          handleEvent(name, lastValue, nextValue, dom);
        }
        else {
          var nameLowerCase = name.toLowerCase();
          var domEvent      = dom[nameLowerCase];
          // if the function is wrapped, that means it's been controlled by a wrapper
          if (domEvent && domEvent.wrapped) {
            return;
          }
          if (!isFunction(nextValue) && !isNullOrUndef(nextValue)) {
            var linkEvent = nextValue.event;
            if (linkEvent && isFunction(linkEvent)) {
              dom[nameLowerCase] = function (e) {
                linkEvent(nextValue.data, e);
              };
            }
            else {
              throwError();
            }
          }
          else {
            dom[nameLowerCase] = nextValue;
          }
        }
      }
    }
    ...
    ...
    function handleEvent(name, lastEvent, nextEvent, dom) {
      var delegatedRoots = delegatedEvents$1.get(name);
      if (nextEvent) {
        if (!delegatedRoots) {
          delegatedRoots          = { items: new Map(), docEvent: null };
          delegatedRoots.docEvent = attachEventToDocument(name, delegatedRoots);
          delegatedEvents$1.set(name, delegatedRoots);
        }
        if (!lastEvent) {
          if (isiOS && name === "onClick") {
            trapClickOnNonInteractiveElement(dom);
          }
        }
        delegatedRoots.items.set(dom, nextEvent);
      }
      else if (delegatedRoots) {
        var items = delegatedRoots.items;
        if (items.delete(dom)) {
          // If any items were deleted, check if listener need to be removed
          if (items.size === 0) {
            document.removeEventListener(normalizeEventName(name), delegatedRoots.docEvent);
            delegatedEvents$1.delete(name);
          }
        }
      }
    }
    ```

### preact