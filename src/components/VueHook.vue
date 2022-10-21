<template>
    <div>
        <input type="text" v-model="Refvalue" @change="change">
        <p>500毫秒后变化：{{changeValue}}</p>
        <p>500毫秒变化一次:{{ThroValue}}</p>
        <hr>
        <button @click="()=>run()">点击我,只会在所有点击完成 1000ms 后执行一次相关函数：</button>
        <button @click="()=>run1()">点击我,只会每隔 1000ms 执行一次相关函数</button>
        <p>{{FnRef}}</p>
        <hr>
        <p>submit count:{{subValue}}</p>
        <p>开始：{{startValue}}</p>
        <p>结束：{{endValue}}</p>
        <button @click="()=>submit()">submit</button>
    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
//防抖
import {
    useDebounce, useThrottle, useCookieState,
    useDebounceFn, useThrottleFn, useLockFn, useTitle
} from 'vue-hooks-plus'
const Refvalue = ref('')
const changeValue = useDebounce(Refvalue, { wait: 500 })

//节流
const ThroValue = useThrottle(Refvalue, { wait: 500 })

//存储和设置Cookie
const [message, setMessage] = useCookieState('cookieName')
const change = (e: any) => {
    setMessage(e.target?.value)
}
//删除cookie=》setMessage()或者setMessage(undefined)


//函数防抖：登录发送信息计时器
const FnRef = ref(0)
const { run } = useDebounceFn(
    () => {
        FnRef.value += 1
    }, {
    wait: 1000
}
)
//函数节流：重新计算时间
const { run: run1 } = useThrottleFn(
    () => {
        FnRef.value += 1
    }, {
    wait: 1000
}
)
//Object.keys() 方法会返回一个由一个给定对象的自身可枚举属性组成的数组，数组中属性名的排列顺序和正常循环遍历该对象时返回的顺序一致 。
const subValue = ref(0)
const startValue = ref(0)
const endValue = ref(0)

function handleRequest() {
    return new Promise<void>(resolve => {
        setTimeout(() => {
            resolve()
        }, 2000)
    })
}

const submit = useLockFn(async () => {
    startValue.value += 1
    await handleRequest()
    subValue.value += 1
    endValue.value += 1
})

//设置标题
const title = ref('我是title')
useTitle(title)
</script>

<style scoped>
hr {
    margin: 20px 0;
}
</style>