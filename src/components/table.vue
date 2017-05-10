<template>
  <div>
    <slot></slot>

    <xigua-pagination :current='listOpt.tableParams.page' :total='totalPage' :size="'md'" :first="false" :last="false"
                      @current-change="changePage($event)"></xigua-pagination>
  </div>
</template>
<script>
  import Vue from 'vue';
  import xiguaPagination from './src/pagination';


  export default{
    data() {
      return {
        defer: {},
      };
    },
    components: {
      xiguaPagination,
    },
    props: {
      value: Object,
      default() {
        return {};
      },
    },
    computed: {
      listOpt() {
        return this.value;
      },
      totalPage() {
        return Math.ceil(this.listOpt.tableParams.total / this.listOpt.count);
      },
    },
    created() {
      this.getData = this.listOpt.getData;
      const reload = () => {
        this.getData(this.defer, this.listOpt.tableParams);
      };
      const resolve = (data) => {
        this.listOpt.$data = data;
      };

      Vue.set(this.defer, 'resolve', resolve);
      Vue.set(this.listOpt, 'reload', reload);
      Vue.set(this.listOpt, 'tableParams', {});
      Vue.set(this.listOpt, '$data', []);
      Vue.set(this.listOpt.tableParams, 'page', 1);
      Vue.set(this.listOpt.tableParams, 'total', 0);
      Vue.set(this.listOpt.tableParams, 'count', this.listOpt.count);
      Vue.delete(this.listOpt, 'getData');
      this.$emit('input', this.listOpt);
      this.getData(this.defer, this.listOpt.tableParams);
    },
    methods: {
      changePage($event) {
        this.listOpt.tableParams.page = $event.current;
        this.getData(this.defer, this.listOpt.tableParams);
      },
    },
  };
</script>
