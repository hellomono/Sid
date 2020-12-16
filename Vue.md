# Vue 3 簡介

---

## 雙向綁定使用的底層實作差異

Vue 3 使用 `Proxy API`
Vue 2 使用 `Object.defineProperty`

---

<!-- > 參考資料： https://vue-js.com/topic/5fd1aed496b2cb0032c3885a -->

```jsx=
Object.defineProperty(obj, prop, {
    get: function() {}
    set: function() { // ... }
})
```

---

```jsx=
  const target = { x: 1, y: 1, z: [] }
  const handler = {
    set: (obj, key, value) => {
      console.log('set')
      obj[key] = value
    },
    get: (obj, key) => {
      if (!obj[key]) throw "物件沒有這個屬性"
      return obj[key]
    },
  }
  const p = new Proxy(target, handler)

  console.log(p.a) // Uncaught 物件沒有這個屬性
```

```jsx=
  console.log(p.x) // 1
  p.z = [] // set
  p.z.push(1) //
  p.ccc = 1 // set
```

---

## 這兩者最主要的差異
1. 原生標準效能較好
2. Object.defineProperty 和 Proxy 本質差別是，defineProperty 只能對屬性進行劫持，新增屬性需要手動 Observe 的問題。
3. 目前並沒有一個完整支持 Proxy 所有攔截方法的 Polyfill 方案


---

CDN 引入的 JS 就已經不管 IE 了
![](https://i.imgur.com/XEEJvjw.png)

> IE 11 的支援目前被列為 Vue 3 中優先開發項目之一

---

## Vue 3 語法上的改變

不使用 `new` 來建立實體，而是使用 `createApp` 這個方法來建立

```jsx=
Vue.createApp(App).use(store).mount('#app');
```

---

> Vuex、Vue Router 的建立也有提供相對應的方法，如： `createStore`

---

## 寫法比較

新增了使用 Composition API 的寫法，使用 `setup` 這個 hook ，在其中完成所有邏輯撰寫

---

### Vue Option API 與 Composition API 在單一組件中只能擇一

---

### props、components 寫法與 Vue 2 一致

```jsx=
import Nav from '@/components/Nav'
export default {
  components: {
    Nav,
  },
  props: {
    itemList: {
      type: Array,
      default: () => [],
    },
  },
  setup() {
      // ...
  }
}
```

---

### 常用方法
- ref
    - 可定義任何型別
    - 不追蹤物件內變化
- reactive
    - 只可定義 Object、Array
    - 追蹤物件內變化

---

### Vue 2 與 Vue 3 差異比較

---

```jsx=
// Vue 2
<template>
  <div>
    <div>Count: {{ count }}</div>
    <button @click="add">Add</button>
  </div>
</template>

<script>
export default {
  data(){
    return {
      count: 3,
    }
  },
  methods: {
    add() {
      this.amount++
    }
  }
};
</script>
```

---

```jsx=
// Vue 3
<template>
  <div>
    <div>Count: {{ count }}</div>
    <button @click="add">Add</button>
  </div>
</template>

<script>
import { ref } from "vue";
export default {
  setup() {
    const count = ref(3);
    function add() {
      count.value++
    }
    return { count, add };
  }
};
</script>
```

---

## 常見生命週期

- beforeCreate
- created
- beforeMount
- mounted
- beforeUpdate
- updated
- beforeUnmount（beforeDestroy）
- unmounted（destroyed）

---

## 生命週期 hooks 的使用方式

```jsx=
  const { onBeforeMount, onMounted, onUpdated } = Vue;
  const App = {
    setup() {
      onBeforeMount(() => {
        // DOM 渲染前
      })
      onMounted(() => {
        // DOM 渲染完成後
      })
      onUpdated(() => {
        // 在資料更改導致virtual DOM重新渲染後調用
      })
      return {}
    },
  }
```

<!-- ![](https://i.imgur.com/N28chSo.png) -->

---


## 原本常用的 Option 依然健在
用法也幾乎一致

---

## mixin 淘汰

---

## Vue 2 專案升級為 Vue 3 的動作

1. 修改建立 Vue、Vue Router、Vuex 實體的方式
2. 重構使用 mixin 的組件
3. 調整生命週期函式

---

## Vue 3 的 Composition API 提供了哪些優勢?

---

- 減少 this 的指向問題
- 更清楚的追蹤共用邏輯還有資料來源
- 清楚區分哪些資料需要被 Vue 追蹤
- 可以只 import 需要使用的方法

---

近期計畫：
- Mock Service Worker
- Tailwind CSS
- Nuxt.js