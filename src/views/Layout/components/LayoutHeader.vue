<script setup>
import LayoutHeaderUl from './LayoutHeaderUl.vue'
import HeaderCart from './HeaderCart.vue'
import { onBeforeUnmount, ref, watch } from 'vue';
import { debounce } from 'lodash';
import { useCategory } from '../../Category/composables/useCategory'

const search = ref('');
const { categoryData } = useCategory()

const handleSearch = (val) => {
  console.log(search.value);
}

const debouncedSearch = debounce((val) => {
  handleSearch(val)
}, 500)

// 监听 search 输入
watch(search, (newVal) => {
  debouncedSearch(newVal)
})

//在 onBeforeUnmount 时取消防抖，以避免内存泄漏
onBeforeUnmount(() => {
  debouncedSearch.cancel();
})

</script>

<template>
  <header class='app-header'>
    <div class="container">
      <h1 class="logo">
        <RouterLink to="/">小兔鲜</RouterLink>
      </h1>

      <LayoutHeaderUl />
      <div class="search">
        <i class="iconfont icon-search"></i>
        <input type="text" placeholder="搜一搜" v-model="search">
      </div>
      <!-- 头部购物车 -->
      <HeaderCart />
    </div>
  </header>
</template>


<style scoped lang='scss'>
.app-header {
  background: #fff;

  .container {
    display: flex;
    align-items: center;
  }

  .logo {
    width: 200px;

    a {
      display: block;
      height: 132px;
      width: 100%;
      text-indent: -9999px;
      background: url('@/assets/images/logo.png') no-repeat center 18px / contain;
    }
  }


  .search {
    width: 170px;
    height: 32px;
    position: relative;
    border-bottom: 1px solid #e7e7e7;
    line-height: 32px;

    .icon-search {
      font-size: 18px;
      margin-left: 5px;
    }

    input {
      width: 140px;
      padding-left: 5px;
      color: #666;
    }
  }

  .cart {
    width: 50px;

    .curr {
      height: 32px;
      line-height: 32px;
      text-align: center;
      position: relative;
      display: block;

      .icon-cart {
        font-size: 22px;
      }

      em {
        font-style: normal;
        position: absolute;
        right: 0;
        top: 0;
        padding: 1px 6px;
        line-height: 1;
        background: $helpColor;
        color: #fff;
        font-size: 12px;
        border-radius: 10px;
        font-family: Arial;
      }
    }
  }
}
</style>