<template>
  <main>
    <h1>{{title}}</h1>
  </main>
</template>
<script>
import moment from 'moment'
const DateNow = moment().format('YYYY-MM-DD')
export default{
  data(){
    return{
      title: "Hello"
    }
  },
  methods:{
    generateRandomNumberWithCharactersandNumbers(length) {
      let result           = '';
      const characters       = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
      const charactersLength = characters.length;
      for ( let i = 0; i < length; i++ ) {
        result += characters.charAt(Math.floor(Math.random() * charactersLength));
      }
      return result;
    },
    async someRequest(){
      const res = await fetch('http://localhost:3000/api/v1/getInfo',{
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify([{ "Date": `${DateNow}`, "Month": `${moment().format('MMMM')}`, "物品": '卸妆水', "销售": '123.00', "赠送": '12.00', "InvoiceNo": `${this.generateRandomNumberWithCharactersandNumbers(10)}`,"type":"sales" }])
      })
      if(res.ok){
        const data = await res.json()
        console.log(data.data.msg)
        if(data.data.msg == "Success"){
          this.title = data.data.data.info
        }else{
          this.title = data.data.msg
        }
      }
    }
  },
  mounted() {
    this.someRequest()
  },
}
</script>