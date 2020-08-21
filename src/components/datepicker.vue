<template>
  <div style="width: 400px">
    <date-picker  v-show='true'
        v-model='selectedDate.date' 
        @input="onDateChanged"
    >
    </date-picker>

    <div style='width: 700px' class="wrap_member_list">
        <grid
          ref="tuiGrid"
          :data="customerList"
          :columns="gridProps.columns"
          :rowHeaders="gridProps.rowHeaders"
          :columnOptions="gridProps.columnOptions"
          :bodyHeight="gridProps.bodyHeight"
          :scrollX="gridProps.scrollX"
          :scrollY="gridProps.scrollY"
          :theme="gridProps.theme"
          :header="gridProps.header"
          :summary="gridProps.summary"
          @click="onGridClick"
          @focusChange="onGridFocusChange"
        ></grid>
    </div>
    <div>
        E-mail : 
    <input type='text' v-model="info.email">
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import Calendar from 'v-calendar/lib/components/calendar.umd'
import DatePicker from 'v-calendar/lib/components/date-picker.umd'

import 'tui-grid/dist/tui-grid.css'
import { Grid } from '@toast-ui/vue-grid'

// Register components in your 'main.js'
Vue.component('calendar', Calendar)
Vue.component('date-picker', DatePicker)

import deepEqual  from 'deep-equal'

// Or just use in separate component
export default {
    name: 'MyDatePicker',
    components: {
        // eslint-disable-next-line vue/no-unused-components
        Calendar,
        DatePicker,
        grid: Grid
    },
    created () {

      const self = this

      self.gridProps = {
        bodyHeight: 500,
        scrollX: true,
        scrollY: true,
        data: [],
        selectionUnit: 'row',
        columnOptions: {
            minWidth: 80,
            resizable: true
        },
        rowHeaders: [
            { type: 'rowNum', width: 100, align: 'center' }
        ],
        columns: [
            { header: '고객명', name: 'custName', width: 30, align: 'center', sortable: true },
            { header: '생일', name: 'birthDateStr', width: 30, align: 'center', sortable: true },
            { header: '차수', name: 'custLevelDiv', width: 30, align: 'center', sortable: true },
            { header: '인정잔여기간', name: 'manageStatus', width: 120, align: 'center', sortable: true,
                formatter: (item) => {
                    return '인정유효기간이 ' + item.value + '일이 남았습니다'
                },
                editor: {
                    type: 'text',
                    options: {
                        language: 'ko'
                    }
                }
            },
            { header: '급여종류', name: 'payName', align: 'center' }
        ],
        theme: {
            name: 'default',
            value: {
            cell: {
                normal: {
                showHorizontalBorder: false
                },
                header: {
                background: '#FFF',
                text: '#323861'
                },
                oddRow: {
                background: '#fff'
                },
                evenRow: {
                background: '#f6f6f6'
                },
                selectedHeader: {
                background: '#FFF'
                },
                selectedRowHeader: {
                background: '#e8e8e8'
                }
            },
            row: {
                hover: {
                background: '#e8e8e8'
                }
            }
            }
        }
      }

      self.customerList = [
          {
            custName: '이지훈',
            birdateStr: '1975-03-10',
            custLevelDiv: 1,
            manageStatus: 100,
            payName: '주야간보호'
          },
          {
            custName: '박지혜',
            birdateStr: '1975-05-08',
            custLevelDiv: 1,
            manageStatus: 200,
            payName: '방문요양'
          },
          {
            custName: '이주원',
            birdateStr: '2004-07-19',
            custLevelDiv: 1,
            manageStatus: 200,
            payName: '방문요양'
          },
          {
            custName: '이예준',
            birdateStr: '2008-08-29',
            custLevelDiv: 1,
            manageStatus: 200,
            payName: '주야간보호'
          }
      ]
  },
  data () {
      return {
          selectedDate: {
              date: null
          },
          // email: '',
          info: {
              name: '',
              email: ''
          },
          customerList: {
              type: Array,
              required: true
          },
          dontMove: false
      }
  },
  mounted() {
      // eslint-disable-next-line no-unused-vars
      const self = this
        // self.selectedDate = new Date()

        var x = { a: 1, b: 2}
        var y = { b: 2, a: 1}

        if( deepEqual(x, y) ) {
            console.log('x == y')
        }
        else {
            console.log('x != y')
        }

        self.$nextTick(() => {
            self.$refs.tuiGrid.invoke('resetData', self.customerList)
        })
  },
  watch: {
      selectedDate: function () {
          // alert(this.selectedDate)
      },
      info: {
          deep: true,

          handler(newVal, oldVal) {
            console.log(oldVal.email + '=>' + newVal.email)
        }
      }
  },
  methods: {
      // eslint-disable-next-line no-unused-vars
      onDateChanged: function (newDate) {

          const self = this
          //let strDate = newDate.getFullYear() + '-' + (newDate.getMonth()+1) + '-' + newDate.getDate()

            
          let strDate = self.selectedDate.date.getFullYear() + '-' + (self.selectedDate.date.getMonth()+1) + '-' + self.selectedDate.date.getDate()
        alert('onDateChanged. today=' + strDate)


      },
     onGridClick: function (event) {
          const self = this
        console.log('onClickEvent: event=' + event)

        var result = new Promise((resolve) => self.$emit('callback', event, resolve))

        result.then((value) => console.log(value))

        return result

        //var resolve = new Promise.resolve
        // await self.$emit('callback', event)

        // console.log('onGridClick done')
    },
    onGridFocusChange: function (event) {
        // eslint-disable-next-line no-unused-vars
        const self = this
        console.log( 'onGridFocusChange: ' + event.prevRowKey + ' => ' + event.rowKey)

        // if (self.dontMove) {
        //     self.dontMove = false
        //     return
        // }

        // if (confirm('움직이지 말까 ?')) {
        //     self.dontMove = true
        //     // self.$refs.tuiGrid.invoke('focus', { rowKey: event.prevRowKey, 
        //     //                                     columnName: event.prevColumnName, 
        //     //                                     setScroll: true } )

        //     self.$refs.tuiGrid.invoke('blur')
        //     self.$refs.tuiGrid.invoke('click', {
        //         rowKey: event.prevRowKey,
        //         columnName: event.prevColumnName
        //     })
        // }
    }
  }
}
</script>