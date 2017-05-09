<template>
  <div id="app">
    <img src="./assets/logo.png">
    <xigua-table v-model="listOpt">
      <div>
        {{listOpt.tableParams}}
        <table class="table table-bordered" v-if="listOpt.tableParams">
          <thead>
          <tr>
            <th></th>
            <th></th>
            <th></th>
          </tr>
          </thead>
          <tbody>
          <tr v-for="item in listOpt.$data">
            <td>{{item}}</td>
            <td>{{item}}</td>
            <td>{{item}}</td>
          </tr>
          </tbody>
        </table>
      </div>

    </xigua-table>
  </div>
</template>

<script>
  import xiguaTable from './components/table';

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
            const data = Object.assign({}, params, this.query);
            demo(data);
            params.total = 34;
            $defer.resolve([1, 2, 3, 4, 5]);
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
