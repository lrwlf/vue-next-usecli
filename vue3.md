# Vue3.0

## Performance
1. 编译模板优化
   
  - Vue3.0模板编译网站：https://vue-next-template-explorer.netlify.app/

  - Vue2.X模板编译网站：https://codesandbox.io/s/github/vuejs/vuejs.org/tree/master/src/v2/examples/vue-20-template-compilation?from-embed
  - patchflag

  - 静态结点会被缓存，而不是每次重新创建
  

      ```html代码```
      ```html
      <div id="app">
        <span>这些结点</span>
        <span>都是静态结点</span>
        <span>而这个不是{{vue}}</span>
      </div>
      ```
      ```编译为render函数```
      ```js
      import { createVNode as _createVNode, toDisplayString as _toDisplayString, openBlock as _openBlock, createBlock as _createBlock } from "vue"

      const _hoisted_1 = { id: "app" }
      const _hoisted_2 = /*#__PURE__*/_createVNode("span", null, "这些结点", -1 /* HOISTED */)
      const _hoisted_3 = /*#__PURE__*/_createVNode("span", null, "都是静态结点", -1 /* HOISTED */)

      export function render(_ctx, _cache) {
        return (_openBlock(), _createBlock("div", _hoisted_1, [
          _hoisted_2,
          _hoisted_3,
          _createVNode("span", null, "而这个不是" + _toDisplayString(_ctx.vue), 1 /* TEXT */)
        ]))
      }
      ```
      ```js
      function anonymous() {
        with(this) {
          return _c('div', {
            attrs: {
              "id": "app"
            }
          }, [_c('span', [_v("这些结点")]), _c('span', [_v("都是静态结点")]), _c('span', [_v("而这个不是" + _s(
            vue))])])
        }
      }
      ```
  - 事件监听缓存: cacheHandlers

    在vue2中，针对节点绑定的事件，每次触发都要重新生成全新的function去更新，在vue3中，当cacheHandlers开启的时候，编译会自动生成一个内联函数，将其变成一个静态节点。
    
    
    ```html```

    ```html
    <div id="app">
      <button @click="confirmHandler">确认</button>
      <span>{{ vue3 }}</span>
    </div>
    ```
    ```开启cacheHandlers```
    ```js
    import { createVNode as _createVNode, toDisplayString as _toDisplayString, openBlock as _openBlock, createBlock as _createBlock } from "vue"

    const _hoisted_1 = { id: "app" }

    export function render(_ctx, _cache) {
      return (_openBlock(), _createBlock("div", _hoisted_1, [
        _createVNode("button", {
          onClick: _cache[1] || (_cache[1] = $event => (_ctx.confirmHandler($event)))
        }, "确认"),
        _createVNode("span", null, _toDisplayString(_ctx.vue3), 1 /* TEXT */)
      ]))
    }
    ```
    ```关闭cacheHandlers```
    ```js
    import { createVNode as _createVNode, toDisplayString as _toDisplayString, openBlock as _openBlock, createBlock as _createBlock } from "vue"

    const _hoisted_1 = { id: "app" }

    export function render(_ctx, _cache) {
      return (_openBlock(), _createBlock("div", _hoisted_1, [
        _createVNode("button", { onClick: _ctx.confirmHandler }, "确认", 8 /* PROPS */, ["onClick"]),
        _createVNode("span", null, _toDisplayString(_ctx.vue3), 1 /* TEXT */)
      ]))
    }
    ```
2. diff算法优化
    ```js  
    // old arr
    ["a", "b", "c", "d", "e", "f", "g", "h"]
    // new arr
    ["a", "b", "d", "f", "c", "e", "x", "y", "g", "h"]
    ```
    1. 从左往右，相同结点进入patch，从右往左比较，相同结点patch
    2. 比较完成若旧数组有剩余，则是被删除的结点进入unmounted，若新数组有剩余结点，则是新增的结点。
    3. 如果还有剩余结点，说明是乱序的。建立新数据的index的map。
       ```js
        {
          d=>2,
          f=>3,
          c=>4,
          e=>5,
          x=>6,
          y=>7
        }
        ```
        初始化一个长度为新数组中剩余数据的数组，初始值为0。```[0,0,0,0,0,0]```

        遍历旧数组，填充坐标。```[4,6,3,5,0,0]```








## Tree shaking support

## Composition API
1. setup函数
2. Composition API可以实现更灵活且无副作用的复用代码

## Fragments
不再限制组件中必须提供根节点。
1. &lt;Teleport&gt;
   https://juejin.im/post/5ee8301851882543423445ed
2. &lt;Suspense&gt;
   https://juejin.im/post/5ee8301851882543423445ed

## TypeScript支持

## Custom Render API

## Vite

Vite，一个**基于浏览器原生 ES imports** 的开发服务器。利用浏览器去解析 imports，在服务器端按需编译返回，完全跳过了打包这个概念，服务器随起随用。同时不仅有**Vue 文件支持**，还搞定了**热更新**，而且热更新的速度不会随着模块增多而变慢。针对生产环境则可以把同一份代码用 rollup 打包。虽然现在还比较粗糙，但这个方向我觉得是有潜力的，做得好可以彻底解决改一行代码等半天热更新的问题。

一个简易的http服务器，无需webpack编译打包，根据请求的Vue文件，直接发回渲染，且支持热更新（非常快）