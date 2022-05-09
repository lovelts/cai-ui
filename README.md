
# WebComponents 组件库
## 在原生、Vue 和 React 中优雅的使用
## #在原生 HTML 中应用：
```js


 <script type="module">
    import 'xxx';
 </script>

<!--or-->
<script type="module" src="//cai-ui"></script>

<cai-button type="primary">点击</cai-button>
<cai-input id="caiIpt"></cai-button>
<script>
  const caiIpt = document.getElementById('caiIpt')
  /* 获取输入框的值有两种方法
   * 1. getAttribute
   * 2. change 事件
  */
  caiIpt.getAttribute('value') 
  caiIpt.addEventListener('change', function(e) {
    console.log(e); // e.detail 为表单的值
  }) 
</script>
```
### 在 Vue 2x 中的应用:
```js
// main.js
import 'cai-ui';

<template>
  <div id="app">
    <cai-button :type="type">
      <span slot="text">哈哈哈</span>
    </cai-button>
    <cai-button @click="changeType">
      <span slot="text">哈哈哈</span>
    </cai-button>
    <cai-input id="ipt" :value="data" @change="(e) => { data = e.detail }"></cai-input>
  </div>
</template>
<script>
export default {
  name: "App",
  components: {},
  data(){
    return {
      type: 'primary',
      data: '',
    }
  },
  methods: {
    changeType() {
      console.log(this.data);
      this.type = 'danger'
    }
  },
};
</script>
```
### 在 Vue 3x 中的差异:
在最近的 Vue3 中，Vue 对 WebComponents 有了更好的支持。Vue  在 Custom Elements Everywhere 测试中获得了 100% 的完美分数。但是还需要我们做出如下配置：
跳过 Vue 本身对组件的解析
custom Elements 的风格和 Vue 组件很像，导致 Vue 会把自定义（非原生的 HTML 标签）标签解析并注册为一个 Vue 组件，然后解析失败才会再解析为一个自定义组件，这样会消耗一定的性能并且会在控制台警告，因此我们需要在构建工具中跳过这个解析：

```js
// vite.config.js
import vue from '@vitejs/plugin-vue'

export default {
  plugins: [
    vue({
      template: {
        compilerOptions: {
          // 将所有包含短横线的标签作为自定义元素处理
          isCustomElement: tag => tag.includes('-')
        }
      }
    })
  ]
}
```
组件的具体使用方法和 Vue 2x 类似。
### 在 React 中的应用
```js
import React, { useEffect, useRef, useState } from 'react';
import 'cai-ui'

function App() {
  const [type, setType] = useState('primary');
  const [value, setValue] = useState();
  const iptRef = useRef(null)
  useEffect(() => {
    document.getElementById('ipt').addEventListener('change', function(e) {
      console.log(e);
    })
  }, [])
  const handleClick = () => {
    console.log(value);
    setType('danger')
  }
  return (
    <div className="App">
      <cai-button type={type}>
        <span slot="text">哈哈哈</span>
      </cai-button>
      <cai-button onClick={handleClick}>
        <span slot="text">点击</span>
      </cai-button>
      <cai-input id="ipt" ref={iptRef} value={value} ></cai-input>
    </div>
  );
}

export default App;
```

+ Web Components 触发的事件可能无法通过 React 渲染树正确的传递。 你需要在 React 组件中手动添加事件处理器来处理这些事件。
+ 在 React 使用有个点我们需要注意下，WebComponents 组件我们需要添加类时需要使用 claas 而不是 className