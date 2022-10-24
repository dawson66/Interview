## 一、明显的新特性

* Composition API

* SFC Component API Syntax Sugar (<script setup>)

* Teleport

* Fragments

* Emits Component Option

* `createRenderer` API from `@vue/runtime-core` to create custom renderers

* SFC State-driven CSS Variables (v-bind in <style>)  (单文件组件状态驱动的CSS变量（<style>中的v-bind） )

* SFC <stype scoped> can now include global rules that target only slotted content  (SFC <style scoped>现在可以包含全局规则或只针对插槽内容的规则)

* Suspense

  

---

## 二、重大变化

### Global API

* Global Vue API is changed to use an appplication instance
* Global and internal APIs have been restructured to be tree-shakable

### Template Directives

* `v-model` usage on components has been reworked, replacing `v-bind.sync`
* `key` usage on `<template v-for>` and non-`v-for` nodes has changed
* `v-if` and `v-for` precedence when used on the same element has changed
* `v-bind ="object"` is now order-sensitive
* `v-on:event.native` modifier has been removed

### Components

* Functional components can only be created using a plain function
* `functional` attribute on single-file component (SFC) `<template>` and `functional` component option are deprecated
* Async components now require `defineAsyncComponent` method to be created
* Component events should now be declared with the `emits` option

### Render Function

* Render function API changed
* `$scopedSlots` property is removed and all slots are exposed via `$slots` as functions
* `$listeners` has been removed / merged into `$attrs`
* `$attrs` now includes `class` ans `style` attributes

### Custom Elements

* Custom elements checks are now performed during template compilation
* Special `is` attribute usage is restricted to the reserved `<component>` tag only

### Other Minor Changes

* The `destroyed` lifecycle option has been renamed to `unmounted`
* The `beforeDestroy` lifecycle option has been renamed to `beforeUnmount`



### Removed APIs

