<template name="ui-checkbox-table">
    <div class="ui-checkbox-table">
        <div class="table-header-wrap">
            <table class="table-header">
                <colgroup>
                    <col v-for="col in checkboxCols" :key="col.id" style="{width: col.width}"/>
                </colgroup>
            </table>
        </div>
    </div>
</template>

<script>
export default {
    props: ['checkboxCols'],
    data() {

    },
    methods: {
        
    },
    mounted() {
        
    }
}
</script>
