# vue-vuetify-cascade
## 一个基于 vuetify 的级联下拉选择框/a cascade select component base on vuetify

### 1. 前置/Dependencies：vue、vuetify、lodash

### 2. 特性/Features:
(1) 无限级数/Infinite cascade  
(2) 支持同步和异步数据/Support for synchronous and asynchronous data  
(3) 简短高效，只有84行代码/Short and efficient, only 84 lines of code  
![image](https://github.com/cyyssly/vue-vuetify-cascade/blob/master/1.JPG)

### 3. 使用方法/Instructions: 

#### 3.1. 复制 Cascade.vue 到项目目录下，个人建议在 src/components/ 下，以下代码以此为例。
#### 3.1. Copy Cascade.vue to the project directory, personally recommend under src/components/. The following code uses this as an example.

#### 3.2. 调用/Use：
  <v-menu v-model="item.menu" :close-on-content-click="false">
    <template v-slot:activator="{ on }">
      <v-text-field
        v-model="item.model"
        :label="item.label"
        readonly
        v-on="on"
      ></v-text-field>
    </template>
    <Cascade
      v-model="item.model"
      :Tabs="item.tabs"
      :Items="item.items"
      :AsyncMode="item.async"
      :ApiHref="item.api"
      @input="item.menu = !item.menu"
    ></Cascade>
  </v-menu>
  
  data: () => ({
    item: {
      label: "省市区",
      model: "",
      tabs: ['level1', 'level2', 'level3'],
      items: [],
      menu: false,
      async: true,
      api: "/console/checkarea"
    }
  })
  
### 4. 源码解析/Source code analysis
Cascade 接受5个传入的参数，并通过 input 事件传出用户选择结果。
Cascade accepts 5 incoming parameters and passes the user selection result via the input() event
#### 4.1. 入参/Incoming parameters
(1) Height: String
组件高度：当选项数量较多时，用于限制组件的高度，默认为300px。
(2) Tabs: Array
级别：格式为数组，例如：['省', '市', '区']，会生成一个三级的级联选择框。
(3) Items: Array 
Items是一个复杂的嵌套对象数组，格式类似于：
  [
    [ {id:1, text:'item1'}, {id:2, text:'item2'} ],
    [ {id:1, text:'item1', pid:1}, {id:2, text:'item2', pid:1}, {id:3, text:'item3', pid:2} ],
    [ {id:1, text:'item1', pid:1}, {id:2, text:'item2', pid:2}, {id:3, text:'item3', pid:3} ]
  ]
其中第一层的数组为级联下拉选择框每一级的可选项。
第二层的对象中 text 为该级选项显示内容，id 为选中值，pid 为父级节点 id。
(4) AsyncMode: Boolean; 
(5) ApiHref: String
如果数据量比较大，建议使用异步加载模式以改善性能。在异步加载模式下，组件初始化时 Items 属性只需要提供第一级选项的数据，
其他层级数据将在选中第一级选项的具体节点后，通过访问 API 接口从后台异步获取。
使用异步加载需要设置 AsyncMode 的值为 true，并提供获取后台数据的 API 地址
组件访问地址时会提供以下参数：1.level：数据层级，从0开始, 数值；2.pid：上级父节点的id, 字符串
API 返回数据的格式要求与 Items 属性相同，是一个对象数组，不过对象中的 pid 可以忽略，例如：
  [ {id:1, text:'item1'}, {id:2, text:'item2'}, {id:3, text:'item3'} ]
#### 4.1. 返回值/Return value
返回值为数组格式，例如：['node1','node2','node3']
    
    

