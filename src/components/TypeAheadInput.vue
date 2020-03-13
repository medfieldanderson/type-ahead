<template>
	<div id="ta-input">
		<!-- <div class="type-ahead-label"> -->
			<label :for="name" class="type-ahead-label">{{label}}:</label>
		<!-- </div> -->
		<!-- <div class="type-ahead-input"> -->
			<input :id="name" :name="name" type="text" v-model="input"
					class="type-ahead-input" placeholder="Search"
					@keyup="onKeyUpEvent"
					@focus="onFocusEvent"
					@keydown.down="navigateList"
					@keydown.up="navigateList"
					@keydown.enter.prevent="onEnter">
		<!-- </div> -->
	</div>
</template>

<script>
export default {
    name: 'TypeAheadInput',
    props: { },
    computed: {
        input: {
            get () { return this.$parent.input; },
            set(value) { return value; }
        },      
        label: {
            get() { return this.$parent.label; },
            set(value) { return value; }
        },      
        name: {
            get() { return this.$parent.name + "-input"; }
        } 
    },
    methods: {
        doSomething: function () {
            console.log("doSomething: " + this.$options.name);
            this.$parent.doSomething();
		},
		navigateList: function (event) {
            console.log("navigateList: " + this.$options.name);
			this.$emit("list-navigation", event);
		},
		onEnter: function () {
            console.log("onEnter: " + this.$options.name);
			this.$emit("set-result");
		},
        onFocusEvent: function () {
            console.log("onFocusEvent: " + this.$options.name);
            this.$emit('input-focus');
        },
        onKeyUpEvent: function (event) {
            console.log("onKeyUpEvent: " + this.$options.name);
            // tell mom
            this.input = event.target.value;
            this.$emit("input-action", event)
        }
    },
    mounted: function () {
        this.label = this.$parent.label;
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
	div#ta-input {
		position: relative;
		z-index: 2;
        width: 600px;
        /* border: 1px dashed orangered */
        
	}
    label.type-ahead-label{
        width: 30%;
		font-size: 14px;
        font-weight: bold;
        color: darkblue;
        text-align: right;
        word-wrap: none;
        /* border: 1px solid purple; */
    }
    input.type-ahead-input{
        width: 70%;
        /* float: right; */
        
        border: 1ps solid darkblue;
    }
</style>