<template>
    <div id="ta-list">
        <ul v-show="showList" :id="name" :name="name" class="type-ahead-list" >
            <slot name="type-ahead-list-slot">
                <li v-for="(item, i) in list" :key="i" class="type-ahead-list-item"
                        @click="clickItem(i)" >
                    ({{i + 1}} of {{list.length}}) 
                </li>
            </slot>
        </ul>
    </div>
</template>

<script>
export default {
    name: 'TypeAheadList',
    props: { },
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
    div#ta-list {
        position: absolute;
        z-index: 3;
        font-family: Verdana, Geneva, Tahoma, sans-serif;
        width: 600px;
    }
    .type-ahead-list-spacer {
        width: 30%;
    }
    .type-ahead-list{
        padding: 0;
        margin: 0;
        border: 1px solid #0a0909;
        height: 110px;
        width: 70%;
        overflow-x: auto;
        background-color: rgb(235, 235, 255);
        scroll-behavior:smooth;
    }
    .type-ahead-list-item {
        list-style: none;
        text-align: left;
        padding: 4px 2px;
        font-size: 11px;
        cursor: pointer;
    }
    .type-ahead-list-item:nth-of-type(odd) {
        background-color:lightsteelblue;
    }
    .type-ahead-list-item:nth-of-type(even) {
        background-color: lightblue;
    }
    .type-ahead-list-item.is-active,
    .type-ahead-list-item:hover{
        background-color: rgb(108, 153, 236);
        color: white;
    }
</style>
