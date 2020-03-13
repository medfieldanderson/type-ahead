<template>
    <div id="ta-summary" v-show="showSummary">
        <slot name="type-ahead-summary-slot">
            <div v-for="(item, i) in summary" :key="i">
                <label :for="name+i" class="type-ahead-summary-label">{{item.label}}:</label>
                <span :name="name+i" class="type-ahead-summary-value truncate" :title="item.value">{{item.value}}</span>
                <br>
            </div>
        </slot>
    </div>
</template>

<script>
    export default {
        name: "TypeAheadSummary",
        computed: {
            name: {
                get() { return this.$parent.name + "-summary"; }
            },
            showSummary: {
                get () {
                    return ((this.$parent.selected)) ? true : false;
                }
            },
            summary: {
                get () {
                    return this.$parent.summary;
                }
            }
        },
        methods: {
            onMouseOver: function (){
                console.log("onMouseOver");
            }
        }
    }
</script>

<style scoped>
    div#ta-summary{ 
        position: relative;
        padding: 1px;
        z-index: 0;
        width: 600px;
        height: 90px;
        overflow-y: auto;
        scroll-behavior: smooth;
    }
    .type-ahead-summary-label, 
    .type-ahead-summary-value{
        color: black;
        float: left;
        font-size: 12px;
    }
    /* div#ta-summary div:first-of-type{
        opacity: 100%;
        position: fixed;
        width: 500px;
        
    }
    div#ta-summary div:not(:first-of-type){
        position: 20px;        
    } */
    .type-ahead-summary-label{
        width: 25%;
        font-weight: bold;
        text-align: right;
        /* overflow-y: auto; */
    }
    .type-ahead-summary-value{
        width: 70%;
        margin-left: 1%;
        text-align: left;
        /* overflow-y: auto; */
    }
    .truncate{
        font-style: initial;
    }
</style>