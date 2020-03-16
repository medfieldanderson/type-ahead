<template>
    <div class="type-ahead-label">
        <label :for="name" class="type-ahead-label" v-if="showLabel">{{label}}:</label>
    </div>
</template>

<script>
    export default {
        name: 'TypeAheadLabel',
        computed: {
            name: {
                get () { return this.$parent.name + "-input"; }
            },
            label: {
                get () { return this.$parent.label; }
            },
			showLabel: { 
                get() {return (this.label) ? true : false;}
			}        }
    }
</script>

<style scoped>
    label.type-ahead-label{
		font-size: 80%;
        font-weight: bold;
        color: darkblue;
        text-align: right;
        word-wrap: none;
        /* border: 1px dotted purple; */
        
    }
</style>