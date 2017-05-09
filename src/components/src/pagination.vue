<template>
  <div v-if="total">
    <ul class="pagination" :class="paginationSize" @click="listClickHandler">
      <li v-if="first" :class="{disabled: current===1}"><a href="javascript:void(0);" data-index="first">首页</a>
      </li>
      <li v-if="previous" :class="{disabled: current===1}"><a href="javascript:void(0);"
                                                              data-index="prev">〈 上一页</a></li>
      <li v-for="item in list" :class="{active: current===item, disabled: item==='...'}"><a
        v-if="current!==item && item!=='...'" href="javascript:void(0);"
        v-bind:data-index="item">{{item}}</a><span v-else>{{item}}</span></li>
      <li v-if="next" :class="{disabled: current===total}"><a href="javascript:void(0);"
                                                              data-index="next">下一页 〉</a></li>
      <li v-if="last" :class="{disabled: current===total}"><a href="javascript:void(0);" data-index="last">尾页</a>
      </li>
    </ul>
  </div>
</template>
<script>
  export default{
    name: 'v-pagination',
    props: {
      current: {
        type: Number,
        default: 1,
      },
      total: {
        type: Number,
        default: 1,
      },
      size: {
        type: String,
        default: 'md',
      },
      first: {
        type: Boolean,
        default: true,
      },
      last: {
        type: Boolean,
        default: true,
      },
      previous: {
        type: Boolean,
        default: true,
      },
      next: {
        type: Boolean,
        default: true,
      },
    },
    data() {
      return {
        paginationSize: `pagination-${this.size}`,
      };
    },
    computed: {
      list() {
        let result = [];
        if (this.total <= 7) {
          for (let i = 1; i <= this.total; i += 1) {
            result.push(i);
          }
        } else {
          result.push(1);
          if (this.current > 3) {
            result.push('...');
          }
          if (this.current <= 3) {
            result = [1, 2, 3, 4, 5];
          } else if (this.current > 3 && this.current < this.total - 2) {
            result.push(this.current - 1);
            result.push(this.current);
            result.push(this.current + 1);
          } else {
            for (let i = this.total - 4; i <= this.total - 1; i += 1) {
              result.push(i);
            }
          }
          if (this.current < this.total - 2) {
            result.push('...');
          }
          result.push(this.total);
        }
        return result;
      },
    },
    methods: {
      listClickHandler(e) {
        e.preventDefault();
        const index = e.target.getAttribute('data-index');
        if (!index) {
          return false;
        }
        if (isNaN(index)) { // 功能按钮
          switch (index) {
            case 'first':
              if (this.current > 1) {
                this.emitMsg(1);
              }
              break;
            case 'last':
              if (this.current < this.total) {
                this.emitMsg(this.total);
              }
              break;
            case 'prev':
              if (this.current > 1) {
                this.emitMsg(Number(this.current) - 1);
              }
              break;
            case 'next':
              if (this.current < this.total) {
                this.emitMsg(Number(this.current) + 1);
              }
              break;
            default : // ellipsis
              break;
          }
        } else {
          this.emitMsg(Number(index));
        }
        return true;
      },
      emitMsg(value, type) {
        if (type === 'total') {
          this.$emit('current-change', {
            total: value,
            current: this.current,
          });
        } else {
          this.$emit('current-change', {
            current: value,
            total: this.total,
          });
        }
      },
    },
  };
</script>
