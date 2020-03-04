<template>
    <div id="my-code">
        <type-ahead-input ref="taInput" :input="input"
                @input-action="processInputAction"
                @input-focus="processInputFocus"/>
        <hr>
        <type-ahead-list ref="taList" :list="list" :isOpen="isOpen">
        <!-- <type-ahead-list :list="list" :isOpen="isOpen"> -->
            <template #type-ahead-list-slot >
                <li v-for="(item, i) in list" :list="list" :key="i" class="type-ahead-list-item"
                        @click="setResult(i)" >
                    ({{i + 1}} of {{list.length}}) 
                </li>
            </template>
        </type-ahead-list>
    </div>
</template>

<script>
    import TypeAheadBase from "./TypeAheadBase.vue";

    // import axios from 'axios';  // ajax

    export default {
        extends: TypeAheadBase,
        name: "CodeTypeAhead",
        components: {  },
        props: {
            intervalInMS: { type: Number, default: 500 },
            isAsync: { type: Boolean, default: true },
            label: { type: String, required: true },
            minLength: { type: Number, default: 2 }
        },
        methods: { 
            test: function () {
                console.log("test: CodeTypeAhead")
            },
            getData: function (value) {
                console.log("getData: CodeTypeAhead | value:  "  + value)

            },
			setInputDisplay: function (index) {
                console.log("setInputDisplay:  CodeTypeAhead " +  index);
            }
        }
    }
</script>

<style>
    #my-code{
        background-color: rgba(237, 240, 246, 0.952);
        display: block;    
        width: 410px;
    }
</style>