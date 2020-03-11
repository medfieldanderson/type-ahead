<template>
    <div id="ta-summary" v-show="showSummary">
        <slot name="type-ahead-summary-slot">
            <div v-for="(item, i) in summary" :key="i">
                <label :for="name+i" class="type-ahead-summary-label">{{item.label}}:</label>
                <span :name="name+i" class="type-ahead-summary-value">{{item.value}}</span>
                <br>
            </div>
        </slot>
    </div>
</template>

<script>
    export default {
        name: "TypeAheadSummary",
        props: {
        },
        data () {
            return {
                // summary: [{label: "ID", value: "value"},{label: "Name", value: "value"},{label: "Bio", value: "value"}]
                // summary: []
            }
        },
        computed: {
            name: {
                get() { return this.$parent.name + "-summary"; }
            },
            showSummary: {
                get () {
                    return ((this.$parent.summary)) ? true : false;
                }
            },
            summary: {
                get () {
                    return this.$parent.summary;
                }
            }
        }
    }
</script>

<style scoped>
    div#ta-summary { 
        position: absolute;
        z-index: 1;
    }
    .type-ahead-summary-label, .type-ahead-summary-value{
        color: black;
        float: left;
        font-size: 12px;
    }
    .type-ahead-summary-label{
        width: 15%;
        font-weight: bold;
        text-align: right;
    }
    .type-ahead-summary-value{
        width: 80%;
        margin-left: 1%;
        text-align: left;
    }
</style>