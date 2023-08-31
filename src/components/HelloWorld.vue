<script setup lang="ts">
//https://blog.csdn.net/dreamer_smith/article/details/130010392
import { ref, reactive, toRef, toRefs, watchEffect, watch } from 'vue';
import { useMouse } from './mouse.js';

defineProps<{ msg: string; title: string }>();

//当 ref 作为响应式数组或像 Map 这种原生集合类型的元素被访问时，不会进行解包
const count = ref(0);
const age = ref(1);
const map = reactive(new Map([['count', ref(0)]]));
console.log(map.get('count').value);

//获得age的value的值
//const value = unRef(age);

const emit = defineEmits(['response']);

emit('response', 'hi');

const info = reactive({ name: 'zhou' });

//转reactive to 普通引用
const infoRef = toRefs(info);
//对象和key
const nameRef = toRef(info, 'name');
const change = (name: string, newAge: number) => {
  info.name = name;
  age.value = newAge;
};
const { x, y } = useMouse();

let data1 = ref('');
let data2 = ref('');
// 假设 这是从后端取到的数据
const data = {
  data1: new Date().getTime()
  data2: '这是渲染内容2',
};
data1.value = data.data1;
//利用requestAnimationFrame 在空闲的时候当前渲染之后在渲染剩余内容
requestIdleCallback(() => {

  data2.value = new Date().getTime()
 // alert('requestIdleCallback');
});

watchEffect(() => {
  console.log(`watchEffect ${info.name}`);
});
watch(info, () => {
  console.log(`watch ${info.name}`);
});
</script>

<template>
  <div>
    {{ data1 }}
  </div>
  <div v-if="data1">
    {{ data2 }}
  </div>
  <h1>{{ msg }} {{ title }} x: {{ x }} y: {{ y }}</h1>

  <div class="card">
    <button type="button" @click="count++">
      count is {{ count }} age {{ age }} {{ info.name }}
    </button>
    <p>
      <button type="button" @click="change('haha', 10)">change value</button>
    </p>
    <p>
      Edit
      <code>components/HelloWorld.vue</code> to test HMR
    </p>
  </div>

  <p>
    Check out
    <a href="https://vuejs.org/guide/quick-start.html#local" target="_blank"
      >create-vue</a
    >, the official Vue + Vite starter
  </p>
  <p>
    Install
    <a href="https://github.com/vuejs/language-tools" target="_blank">Volar</a>
    in your IDE for a better DX
  </p>
  <p class="read-the-docs">Click on the Vite and Vue logos to learn more</p>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
