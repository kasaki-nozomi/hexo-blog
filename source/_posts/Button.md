---
title: VUE 简单封装一个 Button 组件
date: 2022-09-01 11:20:22
description: 使用 Vue2 + Typescript + Stylus + 类组件
categories:
- VUE
tags:
- VUE
- TypeScript
- STYLUS
---

**<font color="#dd00dd" size=6>VUE 简单封装一个 Button 组件</font>**

***

**使用** **`Vue2 + Typescript + Stylus`**，**类组件**写法

```vue
<template>
    <Button
        class="ko-button normal-ko-button"
        :class="{
        'xsmall-ko-button': xsmall,
        'small-ko-button': small,
        'large-ko-button': large,
        'xlarge-ko-button': xlarge,
        'tile-ko-button': tile,
        'rounded-ko-button': rounded,
        'circle-ko-button': circle,
        'disabled-ko-button': disabled,
        }"
        :style="`--color-tint: ${TintColor}`"
        @click="KoButtonClick">
        <slot />
    </Button>
</template>

<script lang="ts">
import { Component, Emit, Prop, Vue } from "vue-property-decorator"
@Component
export default class KoButton extends Vue {
    @Prop(Boolean) public xsmall: boolean | undefined
    @Prop(Boolean) public small: boolean | undefined
    @Prop(Boolean) public large: boolean | undefined
    @Prop(Boolean) public xlarge: boolean | undefined
    @Prop(Boolean) public tile: boolean | undefined
    @Prop(Boolean) public rounded: boolean | undefined
    @Prop(Boolean) public circle: boolean | undefined
    @Prop(Boolean) public disabled: boolean | undefined
    @Prop(String) public color: string | undefined
    public get TintColor() {
        return this.color ? this.color : 'royalblue'
    }
    public KoButtonClick(event: MouseEvent) {
        if (!this.disabled) {
            console.log('default click event')
            this.EmitKoClick(event)
        }
    }
    @Emit("click") private EmitKoClick(event: MouseEvent) {}
}
</script>

<style lang="stylus" scope>
resize(minWidth, height, paddingLR, fontSize)
    min-width minWidth
    height height
    padding 0 paddingLR
    font-size fontSize
    &.tile-ko-button
    	border-radius 0
    &.rounded-ko-button
    	border-radius (@height / 2)
    &.circle-ko-button
    	width @height
    	min-width 0
    	border-radius (@height / 2)
    	padding 0

.ko-button 
    resize(56px, 34px, 14px, 0.9rem)
    border 0 solid black
    border-radius 4px
    color white
    background-color var(--color-tint, royalblue)
    font-weight 500
    letter-spacing 0.08em
    user-select none
    cursor pointer
    &:hover
    	filter brightness(120%)
    &:active
    	filter brightness(80%)
    &.xsmall-ko-button 
    	resize(36px, 26px, 10px, 0.7rem)
    &.small-ko-button
    	resize(46px, 30px, 12px, 0.8rem)
    &.large-ko-button
    	resize(66px, 38px, 16px, 1.0rem)
    &.xlarge-ko-button
    	resize(76px, 42px, 18px, 1.1rem)
    &.disabled-ko-button
    	color white
    	cursor not-allowed
    	background-color gray
        filter brightness(100%)
</style>
```
**实现的功能：**<br />![image.png](https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/image-03.png)<br />![image.png](https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/image-04.png)<br />![image.png](https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/image-05.png)<br />![image.png](https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/image-06.png)<br />**调用：**

```vue
<template>
    <div>
        <ko-button xlarge color="green" rounded  @click="buttonClick"> OK </ko-button>
    </div>
</template>

<script lang="ts">
import KoButton from '@/components/KoButton.vue'
import { Component, Vue } from 'vue-property-decorator'
@Component({
    components: {
        KoButton
    }
})
export default class TestPage extends Vue {
    public buttonClick() {
        console.log('ko-button click event')
    }
}
</script>
```
