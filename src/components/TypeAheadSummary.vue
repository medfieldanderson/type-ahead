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
        padding: 4px 2px;
        z-index: 0;
        /* width: 600px; */
        height: auto;
        max-height: 90px;
        overflow-y: auto;
        scroll-behavior: smooth;

        /* border: 1px dotted red; */
    }
    .type-ahead-summary-label, 
    .type-ahead-summary-value{
        color: black;
        float: left;
        font-size: 12px;

        display: table-cell;
    }
    .type-ahead-summary-label{
        width: 15%;
        font-weight: bold;
        text-align: right;
        white-space: nowrap;
    }
    .type-ahead-summary-value{
        width: 75%;
        margin-left: 1%;
        text-align: left;
    }
    .truncate{
        font-style: initial;
    }
</style>