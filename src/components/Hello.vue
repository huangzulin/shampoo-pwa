<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div>
      <button v-if="!shampooDate" @click="shampoo(true)">要</button>
      <button v-if="!shampooDate" @click="shampoo(false)">不要</button>
    </div>

  </div>
</template>

<script>
import localforage from 'localforage'
var moment = require('moment')

export default {
  name: 'hello',
  data() {
    return {
      msg: '今天要洗头',
      shampooDate: ''
    }
  },
  created() {
    var task = localforage.getItem('shampoo_date').then(value => {
      if (value) {
        this.shampooDate = moment(value)
      }

      if (this.shampooDate == '') {
      this.msg = '今天要洗头吗?'
    } else {
      var date = moment(this.shampooDate)
      if (date.isSame(moment(), 'day')) {
        this.msg = '今天洗过啦'
      } else {
        var days = date.diff(moment(), 'days')
        if (days == 1) {
          this.msg = '不！洗！头！'
        } else {
          this.msg = `${days}天没洗头了`
        }
      }
    }
    })
    
    this.msg = '啥情况？'
  },
  methods: {
    shampoo(need) {
      if (need) {
        this.shampooDate = moment()
      } else {
        this.shampooDate = moment().add(-1, 'days')
      }
      localforage.setItem('shampoo_date', this.shampooDate.toISOString())
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
h1,
h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #35495e;
}
</style>
