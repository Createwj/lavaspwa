<template>
    <div class="page-index">
        <h2>LAVAS</h2>
        <h4>[ˈlɑ:vəz]</h4>
        <router-link :to="{
        name:'detailId',
        params:{
            id:1
        }
        }">详情页面</router-link>
    </div>
</template>

<script>
function setState(store,route) {
    console.log('store 数据')
    console.log(store)
    console.log(route)
}

export default {
    name: 'index',
    /**
     * vue-mate的配置钩子
     * **/
    metaInfo: {
        title: 'Home',
        titleTemplate: '%s - Lavas',
        meta: [
            {name: 'keywords', content: 'lavas PWA'},
            {name: 'description', content: '基于 Vue 的 PWA 解决方案，帮助开发者快速搭建 PWA 应用，解决接入 PWA 的各种问题'}
        ]
    },
    async asyncData({store, route}) {
        setState(store,route);
    }
};
</script>

<style lang="stylus" scoped>

.page-index
    display flex
    justify-content center
    align-items center
    flex-direction column

    h2
        font-size 46px
        font-weight 500
        margin-bottom 0

</style>
