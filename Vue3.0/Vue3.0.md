# Vue 3.0

## Vue3.0六大亮点

- Performance：性能比Vue2.x快1.2~2倍
- Tree shaking support：按需编译，体积比Vue2.x更小
- Composition API ：组合API（类似React Hooks）
- Better TyperScript support：更好的Ts支持
- Fragemnt，Teleport（Protal），Suspense：更先进的组件

## Vue3.0是如何变快的

- diff方法优化

  - Vue2.0的虚拟dom是进项全量的对比
  - Vue3.0新增了静态标记
    - 在于上一次虚拟节点进行对比时候，只对比带有patch flag的节点
    - 并且可以通过flag的信息得知当前节点要对比的具体内容
  - ![](C:\Users\LENOVO\AppData\Roaming\Typora\typora-user-images\image-20201018093317002.png)

- hoisStatic 静态提升

  - Vue2中无论元素是否参与更新，每次都会重新创建

  - Vue3对于不参与更新的元素，只会被创建一次，之后会在每次渲染时候被不停的复用

    ```html
    <div>
      <div>我是div</div>
      <div>我是div</div>
      <div>我是div</div>
      <div>{{msg}}</div>
    </div>
    ```

    - 静态提升之前

    ```js
    import { createVNode as _createVNode, toDisplayString as _toDisplayString, openBlock as _openBlock, createBlock as _createBlock } from "vue"
    
    export function render(_ctx, _cache, $props, $setup, $data, $options) {
      return (_openBlock(), _createBlock("div", null, [
        _createVNode("div", null, "我是div"),
        _createVNode("div", null, "我是div"),
        _createVNode("div", null, "我是div"),
        _createVNode("div", null, _toDisplayString(_ctx.msg), 1 /* TEXT */)
      ]))
    }
    ```

    - 静态提升之后

    ```js
    import { createVNode as _createVNode, toDisplayString as _toDisplayString, openBlock as _openBlock, createBlock as _createBlock } from "vue"
    
    const _hoisted_1 = /*#__PURE__*/_createVNode("div", null, "我是div", -1 /* HOISTED */)
    const _hoisted_2 = /*#__PURE__*/_createVNode("div", null, "我是div", -1 /* HOISTED */)
    const _hoisted_3 = /*#__PURE__*/_createVNode("div", null, "我是div", -1 /* HOISTED */)
    
    export function render(_ctx, _cache, $props, $setup, $data, $options) {
      return (_openBlock(), _createBlock("div", null, [
        _hoisted_1,
        _hoisted_2,
        _hoisted_3,
        _createVNode("div", null, _toDisplayString(_ctx.msg), 1 /* TEXT */)
      ]))
    }
    ```

    

- cacheHandlers 事件侦听器缓存

  - 默认情况下onClick会被视为动态绑定，所以每次都会去追踪它的变化，但是因为是同一个函数，所以没有追踪变化，直接缓存起来复用即可

    ```html
    <div>
        <button @click="onClick">按钮</button>
    </div>
    ```

    - 开启事件监听缓存之前

    ```js
    import { createVNode as _createVNode, openBlock as _openBlock, createBlock as _createBlock } from "vue"
    
    export function render(_ctx, _cache, $props, $setup, $data, $options) {
      return (_openBlock(), _createBlock("div", null, [
        _createVNode("button", { onClick: _ctx.onClick }, "按钮", 8 /* PROPS */, ["onClick"])
      ]))
    }
    
  // 按钮 对应的 8 就是一个静态标记
    ```

    - 开启事件监听缓存之后
    
    ```js
    import { createVNode as _createVNode, openBlock as _openBlock, createBlock as _createBlock } from "vue"
    
    export function render(_ctx, _cache, $props, $setup, $data, $options) {
      return (_openBlock(), _createBlock("div", null, [
        _createVNode("button", {
          onClick: _cache[1] || (_cache[1] = (...args) => (_ctx.onClick(...args)))
        }, "按钮")
      ]))
  }
    
    // 开启缓存后没有静态标记，没有静态标记不会进行比较，不会追踪
    ```
    
    

- ssr 渲染

  - 当有大量静态的内容时候，这些内容会被当做纯字符串推进一个buffer里面，即使存在动态的绑定，会通过模板插值嵌入进去，这样会比通过虚拟dom来渲染的快上很多很多
  - 当静态内容大到一定量级的时候，会用_createStaticVNode方法在客户端去生成一个static node，这些静态node，会被直接innerHTML，就不需要创建对象，然后根据对象渲染



## 创建Vue3的三种方式

- Vue-cli

  - ```shell
    npm install -g @vue/cli
    ```

  - ```shell
    vue create 项目名称
    ```

  - ```shell
    cd 项目名称
    ```

  - ``` shell
    vue add vue-next
    ```

  - ``` shell
    npm run serve
    ```

  

- Webpack

  - ```shell
    git clone https://github.com/vuejs/vue-next-webpack-preview.git 项目名称
    ```

  - ```shell
    cd 项目名称
    ```

  - ```shell
    npm install
    ```

  - ```shell
    npm run dev
    ```

    

- Vite

  - Vite是vue作者开发的一款意图取代webpack的工具

  - 其实现原理是利用ES6的import会发送请求去加载文件的特性，拦截这些请求，做一些预编译，省去webpack漫长的打包时间

  - 安装Vite

    ``` shell
    npm install -g create-vite-app
    ```

  - 利用Vite创建Vue3项目

    ```shell
    create-vite-app 项目名称
    ```

  - 安装依赖运行项目

    ``` shell
    cd 项目名称
    ```

    ``` shell
    npm install
    ```

    ``` shell
    npm run dev
    ```

    







# 附录

## PatchFlags

```js
export const enum PatchFlags{
	TEXT = 1, // 动态文本节点
    CLASS = 1 << 1, // 2 动态class 
    STYLE = 1 << 2, // 4 动态style
    PROPS = 1 << 3, // 8 动态属性，但不包含类名和样式
    FILL_PROPS = 1 << 4, // 16 具有动态key属性，当key改变时，需要进行完整的diff比较
    HYDRATE_EVENTS = 1 << 5, // 32 带有监听事件的节点
    STABLE_FRAGMENRT = 1 << 6, // 64 一个不会改变子节点顺序的fragment
    KEYED_FRAGMENT = 1 << 7, // 128 带有key属性的fragment或部分字节有key
    UNKEYED_FRAGEMNT = 1 << 8, // 256 子节点没有key的fragment
    NEED_PATCH = 1 << 9, // 512 一个节点只会进行非props比较
    DYNAMIC_SLOTS = 1 << 10, // 1024 动态slot
    HOISTED = -1, // 静态节点 
    // 指示在diff过程应该要退出优化模式
    BAIL = -2
}
```

