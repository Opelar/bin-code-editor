## JsonEditor 编辑器

### 基础用法

::: demo
```html
<template>
<div>
  <b-code-editor v-model="jsonStr"/>
</div>
</template>
<script>
  const jsonData = `{"title":"测试json数据","children":[{"name":"子项名称", "desc":"子项说明" },{"name":"子项名称1", "desc":"子项说明1" }]}`

  export default {
    data() {
      return {
        jsonStr: jsonData
      }
    }
  }
</script>
```
:::


### 其他配置项

::: demo
```html
<template>
<div flex="box:mean">
  <b-code-editor v-model="jsonStr" :theme="theme"
      :show-number="showNumber" :readonly="readonly" :lint="lint"/>
  <div class="pl-15">
    <p>行号：<b-switch v-model="showNumber" /></p>
    <p>只读：<b-switch v-model="readonly" /></p>
    <p>检查：<b-switch v-model="lint" /></p>
    <p>皮肤：
        <b-radio-group v-model="theme">
           <b-radio label="idea"><span>idea</span></b-radio>
           <b-radio label="eclipse"><span>eclipse</span></b-radio>
           <b-radio label="rubyblue"><span>rubyblue</span></b-radio>
           <b-radio label="duotone-light"><span>duotone-light</span></b-radio>
           <b-radio label="monokai"><span>monokai</span></b-radio>
           <b-radio label="elegant"><span>elegant</span></b-radio>
           <b-radio label="mdn-like"><span>mdn-like</span></b-radio>
           <b-radio label="xq-light"><span>xq-light</span></b-radio>
           <b-radio label="neo"><span>neo</span></b-radio>
       </b-radio-group>
    </p>
  </div>
</div>
</template>
<script>
  const jsonData = `{"title":"测试json数据","children":[{"name":"子项名称", "desc":"子项说明" },{"name":"子项名称1", "desc":"子项说明1" }]}`

  export default {
    data() {
      return {
        jsonStr: jsonData,
        showNumber: true,
        lint: true,
        readonly: false,
        theme: 'idea'
      }
    }
  }
</script>
```
:::

### Attributes

| 参数      | 说明    | 类型      | 可选值       | 默认值   |
|---------- |-------- |---------- |-------------  |-------- |
| value    | 绑定数据，可用v-model   | String  |  —   |   0  |
| show-number   | 显示行号   | Boolean  |  —   |   true  |
| mode   | 模式   | String  |  'application/json','text/javascript'   |   'application/json'  |
| theme   | 提供若干个默认比较好看的皮肤   | String  | 可选值参考其他配置项中列出  |   idea  |
| lint   | 是否进行lint检查   | Boolean  | 暂时只支持json  |   true  |
| readonly   | 只读模式   | Boolean  | -  |   false  |
| readonly   | 只读模式   | Boolean  | -  |   false  |

### Events

| 事件名      | 说明    | 返回值      |
|---------- |-------- |---------- |
| on-change    | 输入项改变事件   | value  |
