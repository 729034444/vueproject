<template>
    <div id="home">
        <!-- 1.顶部导航栏 -->
        <nav-bar class="home-nav"><div slot="center">购物车</div></nav-bar>
        <!-- 2.轮播图 -->
        <home-swiper :banners="banners"/>
        <!-- 3.推荐 -->
        <recommend-view :recommends="recommends"/>
        <!-- 4.本周流行 -->
        <feature-view/>

        <!-- 5.内部分类 -->
        <tab-control class="tab-control" 
        :titles="['流行','新款','精选']" 
        @tabClick="tabClick"/>

        <!-- 6.商品展示 -->
        <goods-list :goods="showGoods"/>
    </div>
</template>

<script>
import NavBar from '../../components/common/navbar/NavBar'
import HomeSwiper from './childComps/HomeSwiper'
import RecommendView from './childComps/RecommendView'
import FeatureView from './childComps/FeatureView'

import TabControl from '../../components/content/tabControl/TabContol'
import GoodsList from '../../components/content/goods/GoodsList'

import {getHomeMultidata, getHomeGoods} from "network/home"


export default {
    name: "Home",
    components: {
        NavBar,
        HomeSwiper,
        RecommendView,
        FeatureView,
        TabControl,
        GoodsList
    },
    data(){
        return {
            banners:[],
            recommends:[],
            goods: {
                'pop': {page:0, list:[]},
                'new': {page:0, list:[]},
                'sell': {page:0, list:[]}
            },
            currentType: 'pop'
        }
    },
    computed:{
        showGoods(){
            return this.goods[this.currentType].list
        }
    },
    created(){
        // 1.请求多个数据
        this.getHomeMultidata()
        // 2.请求首页展示商品数据
        this.getHomeGoods('pop')
        this.getHomeGoods('new')
        this.getHomeGoods('sell')
    },
    methods: {
        /* 
         *事件监听相关的方法
        */
        tabClick(index) {
            switch (index) {
                case 0:
                    this.currentType = 'pop'
                    break
                case 1:
                    this.currentType = 'new'
                    break
                case 2:
                    this.currentType = 'sell'
                    break
            }
        },




        /* 
         *网络请求相关的方法
        */
        getHomeMultidata(){
            getHomeMultidata().then(res => {
            this.banners = res.data.banner.list;
            this.recommends = res.data.recommend.list;
            })
        },
        getHomeGoods(type) {
            const page = this.goods[type].page + 1
            getHomeGoods(type, page).then(res => {
                this.goods[type].list.push(...res.data.list);
                this.goods[type].page += 1
                // console.log(this.goods)
            })
        }
    }
}
</script>

<style scoped>
    .home-nav {
        background-color: #2F4F4F;
        color: #fff;
        z-index: 1;
    }

    .tab-control {
        position: sticky;
        top: 44px;
        z-index: 1;
    }
</style>