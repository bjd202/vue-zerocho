<template>
    <div>
        <div>{{turn}}님의 턴</div>
        <table-component :table-data="tableData" />
        <div v-if="winner">{{winner}} 님의 승리</div>
    </div>
    
</template>

<script>
import TableComponent from './TableComponent'
import EventBus from './EventBus'

export default {
    components: {
        TableComponent,
    },
    data(){
        return{
            tableData:[
                ['', '', ''],
                ['', '', ''],
                ['', '', '']
            ],
            turn: 'O',
            winner: ''
        }
    },
    methods:{
        onChangeData(){
            // this.tableData[1][0] = 'X' 작동 안함
            this.$set(this.tableData[1], 0, 'X') // Vue.set과 동일
        },
        onClickTd(rowIndex, cellIndex){
            if(this.cellData) return;

            const rootData = this.$root.$data
            console.log(this)
            this.$set(this.tableData[rowIndex], cellIndex, this.turn)

            let win = false
            if(this.tableData[rowIndex][0] === this.turn && this.tableData[rowIndex][1] === this.turn && this.tableData[rowIndex][2] === this.turn){
                win = true
            }
            if(this.tableData[0][cellIndex] === this.turn && this.tableData[1][cellIndex] === this.turn && this.tableData[2][cellIndex] === this.turn){
                win = true
            }
            if(this.tableData[0][0] === this.turn && this.tableData[1][1] === this.turn && this.tableData[2][2] === this.turn){
                win = true
            }
            if(this.tableData[0][2] === this.turn && this.tableData[1][1] === this.turn && this.tableData[2][0] === this.turn){
                win = true
            }

            if(win){
                this.winner = this.turn
                this.turn = 'O'
                this.tableData = [
                    ['', '', ''],
                    ['', '', ''],
                    ['', '', '']
                ]
            }else { // 지거나 무승부
                let all = true
                this.tableData.forEach(row => {
                    row.forEach(cell => {
                        if(!cell){
                            all = false
                        }
                    })
                });

                if(all){
                    this.turn = 'O'
                    this.winner = ''
                    this.tableData = [
                        ['', '', ''],
                        ['', '', ''],
                        ['', '', '']
                    ]
                }else{
                    this.turn = this.turn === 'O' ? 'X' : 'O'
                }
            }
        },
    },
    created(){
        EventBus.$on('clickTd', this.onClickTd)
    }
}
</script>

<style>
table{
    border-collapse: collapse;
}
td{
    border: 1px solid black;
    width: 40px;
    height: 40px;
    text-align: center;
}
</style>