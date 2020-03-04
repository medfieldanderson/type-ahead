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
                        :class="{ 'is-active': i === navigationIndex }" >
                    {{item.profile.first_name}} {{middleInitial(item.profile.middle_name)}} 
                    {{item.profile.last_name}}{{suffix(item.profile.title)}}
                    {{item.npi}} 
                </li>
            </template>
        </type-ahead-list>
		<type-ahead-summary ref="taSummary" :summary="summary"/>

    </div>
</template>

<script>
    import TypeAheadBase from "./TypeAheadBase.vue";

    import axios from 'axios';  // ajax

    export default {
        extends: TypeAheadBase,
        name: "ProviderTypeAhead",
        components: {  },
        props: {
            intervalInMS: { type: Number, default: 500 },
            isAsync: { type: Boolean, default: true },
            label: { type: String, required: true },
            minLength: { type: Number, default: 2 }
        },
        methods: { 
            test: function () {
                console.log("test: ProviderTypeAhead")
            },
            middleInitial: function (middle) {
                return (middle) ? middle.charAt(0) + "." : "";
            },
            suffix: function (title) {
                return (title) ? ", " + title + ". " : "";
            },
            // MUST BE EXTENDED (OVERRIDDEN)
            getData: function (value) {
                console.log("getData: ProviderTypeAhead")

                this.input = value;
                var userKey = "ff62eebdbd4aa19116b8edcd583e8035";
                var url = "https://api.betterdoctor.com/2016-03-01/doctors?location=ma&limit=50&user_key=" + userKey + 
                            "&query=&insurance_uid=cigna-cignahmo&name=" + this.input;
                
                // "this" is not scoped correctly within axios scope;  so save it
                var self = this;

                // ajax call
                axios.get(url)
                .then (function(res){

                    // list data is in TypeAheadBase 
                    self.list = res.data.data;
                    console.log('input:  ' + self.input + " (ProviderTypeAhead)");
                    console.log('Results found:  ' + self.list.length + " (ProviderTypeAhead)")

                }).catch(function(error){
                    console.log('error:  ' + error + " (ProviderTypeAhead)");
                })
            },
			setInputDisplay: function () {
                console.log("setInputDisplay:  ProviderTypeAhead ");
                var selected = this.list[this.selectedIndex];
                var value = selected.profile.first_name + " " + selected.profile.last_name  + " "
                        + selected.npi;
                this.input = value;
            },
			setSummaryDisplay: function () {
                console.log("setSummaryDisplay:  ProviderTypeAhead");
                this.summary = [];
                this.summary.push({"label": "ID/NPI", "value": this.selected.npi});
                this.summary.push({"label": "Name", "value": this.selected.profile.first_name + " " + this.selected.profile.last_name});
                this.summary.push({"label": "Bio", "value": this.selected.profile.bio});
                // this.summary
			},
			setControlValue: function () {
                console.log("setControlValue:  ProviderTypeAhead");
                
			}
        }
    }
</script>

<style>
    div#ta-base {
        position: relative;
        z-index: 0;
        background-color: rgba(237, 240, 246, 0.952);
        display: block;    
        float: left;
        width: 600px;
    }
</style>