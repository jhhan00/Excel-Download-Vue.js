<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png"><br/>
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
  </div>
</template>

<script>
import axios from 'axios'

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
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
