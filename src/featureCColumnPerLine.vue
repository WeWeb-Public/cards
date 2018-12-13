<template>
    <div class="feature-c-column-per-line">
        <input type="number" v-model="columnPerLine" v-on:change="updateResult">
    </div>
</template>

<script> 

export default {
    name: "featureCColumnPerLine",
    props: {
        options: {
            type: Object,
            default: function () {
                return {}
            }
        },
    },
    data() {
        return {
            columnPerLine: 2
        };
    },
    computed: {
    },
    watch: {
    },
    methods: {
        init() {
            this.columnPerLine = this.options.data.columnPerLine;

            this.options.result.columnPerLine = this.columnPerLine;
        },
        updateResult() {
            this.options.result.columnPerLine = this.columnPerLine
        }
    },
    mounted: function () {
        this.init()
    },
    beforeDestroy() {

    }
};
</script>

<style scoped >
</style>
