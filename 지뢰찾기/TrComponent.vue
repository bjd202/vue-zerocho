<template>
    <tr>
        <td-component v-for="(cellData, index) in rowData" :key="index" :cell-data="cellData" :cell-index="index" :row-index="rowIndex"></td-component>
    </tr>
</template>

<script>
import TdComponent from './TdComponent'

export default {
    props:{
        rowData: Array,
        rowIndex: Number,
    },
    components: {
        TdComponent
    },
    data(){
        return {
            parent: 'parent'
        }
    },
    computed: {
        rowData() {
            return this.$store.state.tableData[this.rowIndex]
        }
    },
    methods: {
        
    }
}
</script>
