<template>
  <div id="app">
<!--    <img alt="Vue logo" src="./assets/logo.png"><br/>-->
    {{line}}<br/>
    <button @click="clickAction">Click</button>
    <br/>
    <div>
      <button @click="apiAction">GO</button>
    </div>
    <div>
      <button @click="makeExcelFile">Excel</button>
    </div>
    <div>
      <button @click="makeExcelFile2">Excel2</button>
    </div>
    <div>
      <button @click="makeExcelFile3">Excel3</button>
    </div>
    <div>
      <table id="table">
        <thead>
        <tr>
          <th>id</th>
          <th>name</th>
          <th>age</th>
        </tr>
        </thead>
        <tbody>
        <tr>
          <td>1</td>
          <td>aa</td>
          <td>24</td>
        </tr>
        <tr>
          <td>2</td>
          <td>bb</td>
          <td>46</td>
        </tr>
        <tr>
          <td>3</td>
          <td>cc</td>
          <td>84</td>
        </tr>
        </tbody>
      </table>
      <button @click="makeExcelFile4">Excel4</button>
    </div>
    <div>
      <button @click="makeExcelFile5">Excel5</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Xlsx from 'xlsx'

export default {
  name: 'App',
  data () {
    return {
      line: 'nothing',
      data1: [
        {
          mode: 1,
          affinity: 0.5,
          lb: 1.34,
          ub: 2.53,
          pUrl: "purl1",
          qUrl: "qurl1"
        },
        {
          mode: 2,
          affinity: 1.5,
          lb: 13.34,
          ub: 2.453,
          pUrl: "purl2",
          qUrl: "qurl2"
        }
      ]
    }
  },
  methods: {
    clickAction () {
      axios.get(process.env.VUE_APP_API_ENDPOINT + "/test1").then(response => {
        this.line = response.data
      })
    },
    apiAction () {
      console.log("action!")
      axios.post(process.env.VUE_APP_API_ENDPOINT + "/binding", this.data1).then()
    },
    makeExcelFile () {
      console.log("Excel!")
      axios.post(process.env.VUE_APP_API_ENDPOINT + "/excel", this.data1, {
        responseType: 'arraybuffer'
      })
          .then(result => {
            console.log(result)
            console.log(result.headers["content-type"])
            const url = window.URL.createObjectURL(new Blob([result.data], { type: result.headers["content-type"] }))
            const link = document.createElement("a")
            link.href = url
            link.download = "example.xlsx"
            link.click()
            window.URL.revokeObjectURL(url)
          })
          .catch(er => {
            console.log(er)
          })
    },
    makeExcelFile2 () {
      console.log("Excel-2!")
      axios.post(process.env.VUE_APP_API_ENDPOINT + "/excel2", this.data1, {
        responseType: 'arraybuffer'
      })
          .then(result => {
            console.log(result)
            const url = window.URL.createObjectURL(new Blob([result.data], { type: "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet" }))
            const link = document.createElement("a")
            link.href = url
            link.download = "example.xlsx"
            link.click()
            window.URL.revokeObjectURL(url)
          }).catch(er => {
            console.log(er)
          })
    },
    makeExcelFile3 () {
      console.log("Excel-3!")
      axios.post(process.env.VUE_APP_API_ENDPOINT + "/excel3", this.data1, {
        responseType: 'arraybuffer'
      })
          .then(result => {
            console.log(result)
            const url = window.URL.createObjectURL(new Blob([result.data], { type: result.headers["content-type"] }))
            const link = document.createElement("a")
            link.href = url
            link.download = "example.xlsx"
            link.click()
            window.URL.revokeObjectURL(url)
          }).catch(er => {
            console.log(er)
          })
    },
    makeExcelFile4 () {
      let tab_text = '<html xmlns:x="urn:schemas-microsoft-com:office:excel">'
      tab_text += '<head><meta http-equiv="content-type" content="application/vnd.ms-excel; charset=UTF-8">'
      tab_text += '<xml><x:ExcelWorkbook><x:ExcelWorksheets><x:ExcelWorksheet>'
      tab_text += '<x:Name>Test Sheet</x:Name>'
      tab_text += '<x:WorksheetOptions><x:Panes></x:Panes></x:WorksheetOptions></x:ExcelWorksheet>'
      tab_text += '</x:ExcelWorksheets></x:ExcelWorkbook></xml></head><body>'
      tab_text += "<table>"
      const temp = document.getElementById('table').innerHTML
      // console.log(temp)
      tab_text += temp
      tab_text += '</table></body></html>'
      console.log(tab_text)
      const fileName = 'exampleTable.xls'
      const a_tag = document.createElement('a')
      const blob = new Blob([tab_text], { type: 'application/vnd.ms-excel;charset=utf-8;' })
      a_tag.href = window.URL.createObjectURL(blob)
      a_tag.download = fileName
      a_tag.click()
    },
    makeExcelFile5 () {
      const workBook = Xlsx.utils.book_new()
      const workSheet = Xlsx.utils.json_to_sheet(this.data1)
      Xlsx.utils.book_append_sheet(workBook, workSheet, 'example')
      Xlsx.writeFile(workBook, 'example.xlsx')
    }
  }
}
</script>

<style>
</style>
