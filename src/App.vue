<template>
    <div id="app">
        <home></home>
        <router-view></router-view>
        <main-tab-bar></main-tab-bar>

    </div>
</template>

<script>
import MainTabBar from './components/content/mainTabBar/MainTabBar'
import Home from './views/home/Home'
export default {
    name: 'app',
    components:{
        MainTabBar,
        Home
    }
}
</script>


<style>
    @import 'assets/css/base.css';
    
</style>

