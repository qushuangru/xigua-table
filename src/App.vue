<template>
  <div id="app">
    <img src="./assets/logo.png">
    <xigua-table v-model="listOpt">
      <div>
        {{listOpt.tableParams}}
        <table class="table table-bordered" v-if="listOpt.tableParams">
          <thead>
          <tr>
            <th>id</th>
            <th>姓名</th>
            <th>年龄</th>
          </tr>
          </thead>
          <tbody>
          <tr v-for="item in listOpt.$data">
            <td>{{item.id}}</td>
            <td>{{item.name}}</td>
            <td>{{item.age}}</td>
          </tr>
          </tbody>
        </table>
      </div>

    </xigua-table>
  </div>
</template>

<script>
  import xiguaTable from './components/table';

  const listData = [
    { name: '小红', age: 12, id: 1 },
    { name: '小橙', age: 6, id: 2 },
    { name: '小黄', age: 23, id: 3 },
    { name: '小绿', age: 15, id: 4 },
    { name: '小青', age: 8, id: 5 },
  ];

  function demo(data) {
    console.log(data);
  }

  export default {
    name: 'app',
    components: {
      xiguaTable,
    },
    data() {
      return {
        listOpt: {
          count: 5,
          getData: ($defer, $params) => {
            const params = $params;
            params.count = 5;
            const query = Object.assign({}, params, this.query);
            demo(query);
            params.total = 34;
            $defer.resolve(listData);
          },
        },
        model: {},
        query: {
          status: 1,
        },
      };
    },
  };
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
