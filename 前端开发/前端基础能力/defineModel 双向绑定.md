来源：[v-model 淘汰！Vue3 新玩法代码量减少 60%](https://mp.weixin.qq.com/s/)

### 核心概念

`defineModel` 是 Vue 3.4 引入的编译期宏，让子组件像原生 `<input>` 一样直接支持 `v-model`。

- 编译阶段展开成 `props` + `emit`，运行时零额外开销。
- 无需 import，天生自带。
- 仅限 `<script setup>` 使用。

编译前后对比：

```javascript
// 你写的代码
const model = defineModel<string>({ default: 'hello' })

// 编译后等价于
const props = defineProps({
  modelValue: { type: String, default: 'hello' }
})
const emit = defineEmits(['update:modelValue'])
const model = computed({
  get: () => props.modelValue,
  set: val => emit('update:modelValue', val)
})
```

### 三种使用场景

#### 1. 单 v-model（90% 场景）

父组件：

```vue
<template>
  <UserName v-model="name" />
</template>

<script setup lang="ts">
import { ref } from 'vue'
const name = ref('张三')
</script>
```

子组件 UserName.vue：

```vue
<template>
  <input v-model="modelValue" />
</template>

<script setup lang="ts">
const modelValue = defineModel<string>()
</script>
```

#### 2. 多 v-model（表单组件刚需）

父组件：

```vue
<template>
  <UserForm v-model:name="form.name" v-model:age="form.age" v-model:phone="form.phone" />
</template>

<script setup lang="ts">
const form = reactive({ name: '张三', age: 18, phone: '13800138000' })
</script>
```

子组件 UserForm.vue：

```vue
<template>
  <input v-model="name" placeholder="姓名" />
  <input v-model="age" placeholder="年龄" />
  <input v-model="phone" placeholder="手机号" />
</template>

<script setup lang="ts">
const name = defineModel<string>('name')
const age = defineModel<number>('age')
const phone = defineModel<string>('phone')
</script>
```

#### 3. 修饰符 + 转换器（替代手动 .trim）

父组件：

```vue
<template>
  <TrimInput v-model.trim="keyword" />
</template>

<script setup lang="ts">
const keyword = ref('')
</script>
```

子组件 TrimInput.vue：

```vue
<template>
  <input v-model="modelValue" />
</template>

<script setup lang="ts">
const [modelValue, modifiers] = defineModel<string, 'trim'>({
  set(val) {
    return modifiers.trim ? val.trim() : val
  }
})
</script>
```

### TypeScript 用法速查

| 场景       | 写法                                                     |
| -------- | ------------------------------------------------------ |
| 必填项      | `defineModel<string>({ required: true })`              |
| 可选 + 默认值 | `defineModel<string>({ default: '张三' })`               |
| 联合类型     | `defineModel<'male' \| 'female'>()`                    |
| 复杂对象     | `defineModel<User>()`                                  |
| 对象/数组默认值 | `defineModel<string[]>({ default: () => ['A', 'B'] })` |

注意：对象/数组默认值必须用函数返回，避免引用共享。
