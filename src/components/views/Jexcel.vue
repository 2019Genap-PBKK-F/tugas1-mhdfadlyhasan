<template>
<div id="spreadsheet">
    <div id="tabel">
    <input type="button" @click="logdata" value="Send Data">
    </div>
</div>

</template>
<script>
import jexcel from 'jexcel'
import 'jexcel/dist/jexcel.css'
import Vue from 'vue'
// import axios from 'axios'

var data = [
  ['Jazz', 'Honda', '2019-02-12', '', true, '$ 2.000,00', '#777700'],
  ['Civic', 'Honda', '2018-07-11', '', true, '$ 4.000,01', '#007777']
]

var vm = new Vue(
  {
    el: '#inputer',
    data: data,
    mounted: function() {
      console.log('fetchdata disini')
    }
  })
console.log(vm)

var options = {
  data: data,
  allowToolbar: true,
  columns: [
    { type: 'text', title: 'Car', width: '120px' },
    { type: 'dropdown', title: 'Make', width: '250px', source: [ 'Alfa Romeo', 'Audi', 'Bmw' ] },
    { type: 'calendar', title: 'Available', width: '250px' },
    { type: 'image', title: 'Photo', width: '120px' },
    { type: 'checkbox', title: 'Stock', width: '80px' },
    { type: 'numeric', title: 'Price', width: '100px', mask: '$ #.##,00', decimal: ',' }
  ]
}
export default {
  name: 'tabel',
  data: data,
  mounted: function () {
    let spreadsheet = jexcel(this.$el, options)
    Object.assign(this, spreadsheet)
  },
  methods:
  {logdata: function() {
    console.log('data dikirim')
    console.log(data)
  }}
}
</script>
