<template>
    <div id="ta-base"> 
        <type-ahead-input ref="taInput" :input="input"
                @input-action="processInputAction"
                @input-focus="processInputFocus"
                @list-navigation="processListNavigation"
				@set-result="setResult"/>
        <type-ahead-list ref="taList" :list="list" :isOpen="isOpen">
            <template #type-ahead-list-slot >
                <li v-for="(item, i) in list" :id="name+'-item-'+i" :list="list" :key="i" class="type-ahead-list-item"
                        @click="clickItem(i)" 
						:class="{ 'is-active': i === navigationIndex }">
                    ({{i + 1}} of {{list.length}}) 
                </li>
            </template>
        </type-ahead-list>
		<type-ahead-summary ref="taSummary" :summary="summary"/>
		
    </div>
</template>
<script>
	import TypeAheadInput from "./TypeAheadInput.vue";
	import TypeAheadList from "./TypeAheadList.vue";
	import TypeAheadSummary from "./TypeAheadSummary.vue";

	import _ from 'lodash';

	export default {
		name: 'TypeAheadBase',
		components: { TypeAheadInput, TypeAheadList, TypeAheadSummary },
		props: {
            intervalInMS: { type: Number, required: true },
			isAsync: { type: Boolean, default: false },
			label: { type: String, required: true },
			name: { type: String, required: true }
		},
		data(){
			return {
				debounce: function(){},
				ignoreKeys: ["Alt", "ArrowDown", "ArrowLeft", "ArrowRight", "ArrowUp", "BackSpace", "Control", "End", "Enter", "Home", "Shift", "Tab"] ,
				isOpen: false,
				input: "",
				list: [],
				navigationIndex: -1,
				selectedIndex: -1,
				// summary: [{label: "ID", value: "value"},{label: "Name", value: "value"},{label: "Bio", value: "value"}]
				summary: []

				
			}
		},
		computed: {
			componentName: {
				get() {return this.$options.extends.name}
			},
			debounceInterval: {
				// getter that returns the wait interval between calls to a resource (in milliseconds)
				get() { return this.intervalInMS; }
			},
			hasResults: {
				get() { return (this.list && this.list.length > 0) ? true : false; }
			},
			query: {
				get() {
					return this.input;
				}
			},        
			selected: {
				get () {
					return (this.selectedIndex >= 0) ? this.list[this.selectedIndex] : null;
				}
			}
		},
		methods: {
			test: function () {
				console.log("test: TypeAheadBase")
			},
			doSomething: function () {
				console.log("doSomething: TypeAheadBase");
			},
			clickItem: function (index) {
				console.log("clickItem: TypeAheadBase", index);
				this.setResult(index); 

			},
			getData: function (query) {
				console.log("getData: " + query + " passed to TypeAheadBase" + " OVERRIDE ME!");
				// MUST BE OVERRIDDEN IN PARENT COMPONENT
			},			
			getThrottledData: function() {
				console.log("getThrottledData: TypeAheadBase" + " debounceInterval:  " + this.debounceInterval);
				this.debounce();
			},
			handleInputChange: function () {
				console.log("handleInputChange: TypeAheadBase");

				this.isLoading = true;
				this.getData(this.query);

				// show the list
				this.isOpen = true;
				// reset list navigation
				this.navigationIndex = -1;
			},
			handleClickOutside: function (event){
				if(this.isOpen == true){
					if(!this.$el.contains(event.target)){
						console.log("handleClickOutside: TypeAheadBase")
						this.isOpen = false;
						this.navigationIndex = -1;
					}
				}
			},
			processInputAction: function (event) {
				console.log("processInputAction: TypeAheadBase");
				// only interested in user input, not action keys
				// TODO: should test for minimun length (except for initial getData call)
				console.log("Event Key:  " + event.key);
				if(this.shouldProcessKey(event.key)){
					if(this.$refs.taInput){
						if(event.target.id == this.$refs.taInput.name){
							this.input = event.target.value;
							if(this.isAsync) {
								this.getThrottledData();
							}
							else{
								console.log("isAsync is false: TypeAheadBase")
							}
						}
					}
				}
			},
			processInputFocus: function () {
				console.log("processInputFocus:  TypeAheadBase");
				this.isOpen = true;
			},
			processListNavigation: function (event) {
				console.log("processListNavigation:  TypeAheadBase  " + event.key);
				this.isOpen = true;
				switch (event.key) {
					case "ArrowDown":
						if(this.navigationIndex === (this.list.length -1)) {
							// back to the beginning
							this.navigationIndex = 0;
						} 
						else if((this.list) && this.list.length > 0 && this.navigationIndex < (this.list.length - 1) ){
							this.navigationIndex += 1;
						}
						break;
					case "ArrowUp":
						if(this.navigationIndex === 0 ){
							this.navigationIndex = (this.list.length - 1)
						}
						else if((this.list) && this.list.length > 0 && this.navigationIndex > 0){
							this.navigationIndex -= 1;
						}
						break;
					default:
						break;
				}
				console.log("navigationIndex: " + this.navigationIndex);
				this.positionSelectionInList(event);
			},
			positionSelectionInList: function (event) {
				// TODO: Finish this.
				console.log("positionSelectionInList:  TypeAheadBase");
				console.log(event);
				console.log(this.$refs.taList);
				// var targetUl = document.getElementById(this.$refs.taList.id);
				// var list = this.$refs.taList;
				// TODO: this works... but it only after on the previously "is-active" item.
				this.$refs.taList.$vnode.elm.getElementsByClassName("type-ahead-list-item is-active")
				var li = this.$refs.taList.$vnode.elm.getElementsByClassName("is-active");
				this.$refs.taList.$vnode.elm.getElementsByClassName("type-ahead-list-item")
				console.log(li);
				

				

			},
			shouldProcessKey: function (key) {
				return (this.ignoreKeys.indexOf(key) === -1) ? true : false;	
			},
			setResult: function (index) {
				console.log("setResult:  TypeAheadBase");

				if(index >= 0){
					// index is only defined on click; set the navigationIndex
					this.navigationIndex = index;
				}
				
				this.selectedIndex = this.navigationIndex;
				this.setInputDisplay();
				this.setSummaryDisplay();
				this.setControlValue();
				this.isOpen = false;
			},
			setInputDisplay: function () {
				console.log("setInputDisplay:  TypeAheadBase");
				this.input = "Record " + this.selectedIndex + " selected"
			},
			setSummaryDisplay: function () {
				console.log("setSummaryDisplay:  TypeAheadBase");
			},
			setControlValue: function () {
				console.log("setControlValue:  TypeAheadBase");
			}
		},        
		mounted: function () {

			// function pointer that will spare too many calls to url
			this.debounce = _.debounce(function(){
				console.log('debounce:  TypeAheadBase');
				this.handleInputChange();
			}, this.debounceInterval )

			// Start listening for click outside type ahead control
			document.addEventListener('click', this.handleClickOutside, false);

			// get the initial list
			this.getData(this.query);
		},
		destroyed: function () {
			// Stop listening for click outside type ahead control
			document.removeEventListener('click', this.handleClickOutside, false)
		}
	}
</script>
