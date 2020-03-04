<template>
  <div id="type-ahead-list">
    <ul v-show="showList" :id="name" :name="name" class="type-ahead-list" >
        <slot name="type-ahead-list-slot">
            <li v-for="(item, i) in list" :key="i" class="type-ahead-list-item"
                    @click="setResult" >
                ({{i + 1}} of {{list.length}}) 
            </li>
        </slot>
        <li>FOO</li>
    </ul>
  </div>
</template>

<script>
import { TypeAheadList } from "./TypeAheadList.vue";
export default {
    extends: TypeAheadList,
    name: 'ProviderTypeAheadList',
    props: { },
    data(){
        return {
            componentName:  "TypeAheadList"
        }
    },
    computed: {
        name: {
            get() { return this.$parent.name + "-list"; },
            set(value) { return value; }
        },
        list: {
            get () { return this.$parent.list; }
        },
        showList: {
            get () { return (this.$attrs.isOpen && (this.list)) ? true : false; }
        }
    },
    methods: {
        // doSomething: function () {
        //     console.log("doSomething: " + this.$options.name)
        //     this.$emit('anger');
        // },
        clickItem: function () {
            console.log("clickItem: " + this.$options.name)
            this.$emit("click-item");
        },
        setResult: function () {
            console.log("setResult: " + this.$options.name)
        },
    }
}
</script>

<style scoped>
    .type-ahead-list{
        padding: 0;
        margin: 0;
        border: 1px solid #0a0909;
        height: 120px;
        width: 400px;
        overflow: auto;
        background-color: rgb(235, 235, 255);
    }
    .type-ahead-list-item {
        list-style: none;
        text-align: left;
        padding: 2px 2px;
        font-size: 12px;
        cursor: pointer;
    }
    .type-ahead-list-item:nth-of-type(odd) {
        background-color: rgb(187, 210, 233)
        /* color: white; */
    }
    .type-ahead-list-item:nth-of-type(even) {
        background-color: rgb(216, 225, 241);
    }
    .type-ahead-list-item.is-active,
    .type-ahead-list-item:hover{
        background-color: rgb(108, 153, 236);
        color: white;
    }
</style>
