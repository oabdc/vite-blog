<template>
  <div class="card">
    <div>{{person.name}}age：{{person.age}}</div>
    <div>{{per.name}}age：{{per.age}}</div>
    <p>{{count}},{{double}}</p>
    <div @click="updateObj2">{{obj2.a.b}}, {{obj2.c}}</div>
    <button type="button" @click="add">add</button>
  </div>
</template>

<script setup lang="ts">
  import { ref, reactive, watch, computed, shallowReactive, watchEffect } from 'vue'

  interface Per{
    name: string
    age: number
  }
  interface Person extends Per {
    id: number
  }
  // 1. ref 创建响应式对象 ,接收基础类型，通过 .value 属性进行访问和修改
  const count = ref<number>(0) 
  const double = computed(() => {count.value * 2})
  // ref 也可以创建响应式对象，内部会自动转为 reactive
  const per = ref<Per>({name: 'jerry', age: 1})
  // 2. reactive 用来创建响应式对象，它接收一个对象/数组参数
  const person = reactive<Person>({name: 'tom', age: 18, id: 1});
  const add = () => {
    count.value++
    person.age++;
    per.value.age++;
  }  
  // onBeforeMount() // 组件挂载前
  // onMounted() // 组件挂载后
  // onBeforeUpdate() // 组件更新之前
  // onUpdated() // 组件更新之后 ，例如：访问更新后的 DOM
  // onBeforeUnmount() // 组件卸载前
  // onUnmounted() // 组件卸载后
  // onActivated() // 组件从非激活状态切换到激活状态 组件的激活状态指的是组件被渲染到页面并且可见的状态
  // onDeactivated()// 组件从激活状态切换到非激活状态

  watch(count, (val) => {
    console.log('count', val);
  })

  watchEffect(() => {
    
  })

  
  // 高级功能
  
  // readonly 只读数据
  // const obj1 = readonly({ a: 1 })
  // obj1.a = 2; // error
  // 浅层响应式, 其内部的属性不会被转化为响应式的
  const obj2 = shallowReactive({ a: { b: 1 }, c: 3 })
  const updateObj2 = () => {
    // obj2.c = 4;
    obj2.a.b = 2; // obj2.a.b 的值虽然改变了，但是响应式不生效，页面没有重新渲染
    console.log(111, obj2); // 
  }
</script>

<!-- 
  新组件

  1. fragment  允许多个根结点
  2. Teleport 允许将组件渲染到任何位置
  <teleport to="#modal">
    <div>This will be rendered wherever the #modal element is.</div>
  </teleport>
  3.Suspense 组件允许你等待一个或多个异步组件，然后显示一些备用内容，直到所有的异步组件都被解析。
  <Suspense>
    <template #default>
      <AsyncComponent />
    </template>
    <template #fallback>
      <div>Loading...</div>
    </template>
  </Suspense>

  provide inject

  import { provide, inject } from 'vue'

  在父组件中
  provide('myValue', 123)

  在子组件中
  const myValue = inject('myValue') // myValue 现在是 123

  https://juejin.cn/post/7260070602614620221?searchId=20231027154823B6B090BECB7087B926FF#heading-41
 -->

