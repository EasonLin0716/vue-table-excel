<template>
  <div class="hello">
    <h2>Input your new values here</h2>
    <label>first name</label>
    <input type="text" v-model="firstName" />
    <label>last name</label>
    <input type="text" v-model="lastName" />
    <label>age</label>
    <input type="number" v-model="age" />
    <button @click="insert">Insert</button>
    <br /><br /><br />

    <button @click="downloadExcel">Download as .xlsx file</button>
    <table style="width:100%">
      <tr>
        <th v-for="(value, index) in header">{{ value }}</th>
      </tr>
      <tr v-for="(item, index) in list" :key="index">
        <td v-for="(value, index) in item" :key="index">
          {{ value }}
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      firstName: '',
      lastName: '',
      age: '',

      header: ['Firstname', 'Lastname', 'Age'],
      list: [
        ['Jill', 'Smith', 50],
        ['Eve', 'Jackson', 94],
        ['Jane', 'Doe', 26]
      ]
    }
  },
  methods: {
    insert() {
      if (!this.firstName || !this.lastName || !this.age) {
        return alert('must input every column')
      }
      this.list.push([this.firstName, this.lastName, +this.age])
      this.firstName = ''
      this.lastName = ''
      this.age = ''
    },
    downloadExcel() {
      import('@/vendor/Export2Excel').then(excel => {
        const tHeader = this.header
        const data = this.list
        excel.export_json_to_excel({
          header: tHeader, //表头 必填
          data, //具体数据 必填
          filename: 'excel-list', //非必填
          autoWidth: true, //非必填
          bookType: 'xlsx' //非必填
        })
      })
    }
  }
}
</script>

<style scoped>
table * {
  border: 1px solid;
}
</style>
