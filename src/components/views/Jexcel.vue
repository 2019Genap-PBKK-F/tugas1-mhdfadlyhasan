<template>
<div>
  <div id="spreadsheet"></div>
  <div class="row n-5">
    <div><input type="button" class="col btn btn-primary btn-lg p-3" value="Add new row" @click="logdata" />
    <input type="button" value="Delete row ini" class=" col btn btn-danger btn-lg"  @click="deletedata" /></div>
  </div>
</div>

</template>
<script>
import axios from 'axios'
import jexcel from 'jexcel'
import 'jexcel/dist/jexcel.css'

export default {
  name: 'App',
  data () {
    return {
      listData: [],
      form: {
        id: '',
        nrp: '',
        nama: ''
      }
    }
  },
  mounted: function() {
    let spreadsheet = jexcel(this.$el, this.options)
    Object.assign(this, {spreadsheet})
  },
  methods: {
    New() {
      axios.post('http://localhost:3000/data/', this.form).then(res => {
        console.log(res.data)
      })
    },
    Update(instance, cell, columns, row, value) {
      console.log('jalan')
      axios.get('http://localhost:3000/data/').then(res => {
        var index = Object.values(res.data[row])
        index[columns] = value
        console.log(index)
        axios.put('http://localhost:3000/data/' + index[0], {
          id: index[0],
          nrp: index[1],
          nama: index[2]
        }).then(res => {
          console.log(res.data)
        })
      })
    },
    Delete(instance, row) {
      axios.get('http://localhost:3000/data/').then(res => {
        var index = Object.values(res.data[row])
        // console.log(index)
        console.log(row)
        axios.delete('http://localhost:3000/data/' + index[0])
      })
    },
    logdata: function() {
      console.log('data ditambahkan')
      this.spreadsheet.insertRow()
    },
    deletedata: function() {
      console.log('data dihapus')
      this.spreadsheet.deleteRow()
    }
  },
  computed: {
    options() {
      return {
        allowToolbar: true,
        data: this.listData,
        url: ('http://localhost:3000/data/'),
        columns: [
          { type: 'hidden', title: 'id', width: '120px' },
          { type: 'text', title: 'Nama', width: '120px' },
          { type: 'text', title: 'NRP', width: '120' }
        ],
        onchange: this.Update,
        oninsertrow: this.New,
        ondeleterow: this.Delete,
        mounted: function() {
        }
      }
    }
  }
}
</script>
