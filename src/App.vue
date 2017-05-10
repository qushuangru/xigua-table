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

  const listData = {
    data: [[
        { name: '小红', age: 12, id: 1 },
        { name: '小橙', age: 6, id: 2 },
        { name: '小黄', age: 23, id: 3 },
        { name: '小绿', age: 15, id: 4 },
        { name: '小青', age: 8, id: 5 },
    ], [
        { name: '老赵', age: 66, id: 6 },
        { name: '老钱', age: 87, id: 7 },
        { name: '老孙', age: 72, id: 8 },
        { name: '老李', age: 59, id: 9 },
        { name: '老王', age: 108, id: 10 },
    ], [
        { name: '大仁', age: 12, id: 1 },
        { name: '大义', age: 6, id: 2 },
        { name: '大礼', age: 23, id: 3 },
        { name: '大智', age: 15, id: 4 },
        { name: '大信', age: 8, id: 5 },
    ]],
    totalSize: 15,
  };


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
            const query = Object.assign({}, params, this.query);

            // ajax
            $defer.resolve(listData.data[query.page - 1]);
            params.total = listData.totalSize;
            // ajax
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
