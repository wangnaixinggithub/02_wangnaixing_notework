# Parent-Child-View

# 1、MainContent

```vue
<template>
    <div>
        <el-container style="border: 1px solid #eee">
            <el-aside width="250px" style="background-color: rgb(238, 241, 246)">
                <el-menu :default-openeds="['1']" :router="true">
                    <el-submenu index="1">
                        <template slot="title"><i class="el-icon-folder-opened"></i>水库运行计划</template>
                        <el-menu-item index="/dc/mainContent/sdrjh"><i class="el-icon-document"></i>日发电计划</el-menu-item>
                        <el-menu-item index="/dc/mainContent/sdzjh"><i class="el-icon-document"></i>周发电计划</el-menu-item>
                        <el-menu-item index="/dc/mainContent/sdyjh"><i class="el-icon-document"></i>月发电计划</el-menu-item>
                        <el-menu-item index="/dc/mainContent/sdjjh"><i class="el-icon-document"></i>季发电计划</el-menu-item>
                        <el-menu-item index="/dc/mainContent/sdnjh"><i class="el-icon-document"></i>年发电计划</el-menu-item>
                    </el-submenu>
                    <el-submenu index="2">
                        <template slot="title"><i class="el-icon-folder-opened"></i>省级小水电发电管理</template>
                        <el-menu-item index="2-1"><i class="el-icon-document"></i>省级实际发电量</el-menu-item>
                        <el-menu-item index="2-2"><i class="el-icon-document"></i>省级计划发电量</el-menu-item>
                    </el-submenu>
                </el-menu>
            </el-aside>
            <el-container>
                <el-main>
                    <router-view></router-view>
                </el-main>
            </el-container>
        </el-container>
    </div>
</template>
<script>
export default {
    name: "MainContentViewByDc"
}
</script>

<style scoped>
.el-header {
    background-color: #B3C0D1;
    color: #333;
    line-height: 60px;
}

.el-aside {
    color: #333;
}
</style>

```

# 2、Chirld

```vue
<!--季发电计划视图-->
<template>
    <el-row>
        <el-col :span="24">
            <el-card>
                <div style="text-align: center;height: 30px;">
                    <h2>水库运行计划-季发电计划统计</h2>
                </div>
                <el-col :offset="2" :span="12">
                    <el-form ref="form" :inline="true" label-width="80px">
                        <el-form-item label="日期:">
                            <el-date-picker
                                v-model="currentDate"
                                placeholder="选择年"
                                size="mini"
                                type="year"
                                @change="date_picker_onChange">
                            </el-date-picker>
                        </el-form-item>
                        <el-form-item>
                            <el-button icon="el-icon-d-arrow-left" size="mini" type="info"
                                       @click="btn_toPrev_onClick"></el-button>
                        </el-form-item>
                        <el-form-item>
                            <el-button size="mini" type="info" @click="btn_toNow_onClick">本年</el-button>
                        </el-form-item>
                        <el-form-item>
                            <el-button icon="el-icon-d-arrow-right" size="mini" type="info"
                                       @click="btn_toNext_onClick"></el-button>
                        </el-form-item>
                    </el-form>
                </el-col>
            </el-card>
        </el-col>
        <el-col :span="24">
            <el-container style="height: 900px; border: 1px solid #eee">
                <el-main>
                    <el-row :gutter="20">
                        <el-col :span="2">&nbsp;</el-col>
                        <el-col v-for="(item,index) in dataList.slice(0,2)" :key="index" :span="10">
                            <el-card class="box-card">
                                <div style="padding: 10px;" @click="elCard_onClick(item)">
                                    <h3 style="text-align: center; color: #409EFF;">{{ item.JHZQ }}</h3>
                                    <div :class="item.SBZT === '已上报' ? 'colorBlue' : 'colorRed' ">{{ item.SBZT }}</div>
                                </div>
                            </el-card>
                        </el-col>
                        <el-col :span="2">&nbsp;</el-col>
                    </el-row>
                    <el-row :gutter="20" style="margin-top: 20px;">
                        <el-col :span="2">&nbsp;</el-col>
                        <el-col v-for="(item,index) in dataList.slice(2,4)" :key="index" :span="10">
                            <el-card class="box-card">
                                <div style="padding: 10px;" @click="elCard_onClick(item)">
                                    <h3  style="text-align: center; color: #409EFF;">{{ item.JHZQ }}</h3>
                                    <div :class="item.SBZT === '已上报' ? 'colorBlue' : 'colorRed' ">{{ item.SBZT }}</div>
                                </div>
                            </el-card>
                        </el-col>
                        <el-col :span="2">&nbsp;</el-col>
                    </el-row>
                </el-main>
            </el-container>
        </el-col>
    </el-row>
</template>

<script>

import dataApi from "../../../api/dataApi";
import DateUtils from "../../../utils/dateUtils";

export default {
    name: "SdnjhMainViewByDc",
    created() {
        this.getDataList()
    },
    data() {
        return {
            currentDate: new Date(),
            dataList: []
        }
    },
    methods: {
        //获取初始化数据集合
        getDataList() {
            dataApi.initDcSdjjhData({currentDate: ""}).then(res => this.dataList = res.data)
        },
        //处理日期组件被选中某个日期的函数
        date_picker_onChange(date) {
            dataApi.initDcSdjjhData({currentDate: DateUtils.formatDate(date, 'yyyy-MM-dd')}).then(res => this.dataList = res.data)
        },
        //处理点击本年按钮的函数
        btn_toNow_onClick() {
            this.currentDate = new Date();
            dataApi.initDcSdjjhData({currentDate: DateUtils.formatDate(this.currentDate, 'yyyy-MM-dd')}).then(res => this.dataList = res.data)
        },
        //处理点击上一年按钮的函数
        btn_toPrev_onClick() {
            this.currentDate = new Date(DateUtils.getPreYear(this.currentDate));
            dataApi.initDcSdjjhData({currentDate: DateUtils.formatDate(this.currentDate, 'yyyy-MM-dd')}).then(res => this.dataList = res.data)
        },
        //处理点击下一年按钮的函数
        btn_toNext_onClick() {
            this.currentDate = new Date(DateUtils.getNextYear(this.currentDate));
            dataApi.initDcSdjjhData({currentDate: DateUtils.formatDate(this.currentDate, 'yyyy-MM-dd')}).then(res => this.dataList = res.data)
        },
        //处理面板被点击
        elCard_onClick(item) {
            const loading = this.$loading({
                lock: true,
                text: "Loading",
                spinner: "el-icon-loading",
                background: "rgba(0, 0, 0, 0.7)",
            })
            setTimeout(() => {
                dataApi.createDcSdjjh(item).then(res => window.open(`skyxjh.html#/dc/sdjjh/detail/${res.data}`))
                loading.close();
            }, 300);

        },


    },

}
</script>

<style scoped>
.colorRed {
    color: #FF0000;
}

.colorBlue {
    color: #8CC5FF;
}

.colorRed, .colorBlue {
    text-align: center;
    margin-top: 5px;
    font-weight: bolder;
}
</style>

```



# 3、Router.js

```javascript
import Vue from "vue";
import Router from 'vue-router'
import MainContentViewByDc from "././views/dc/MainContentView"
import SdrjhMainViewByDc from "./views/dc/sdrjhb/SdrjhMainView"
import SdzjhMainViewByDc from "./views/dc/sdzjhb/SdzjhMainView"
import SdyjhMainViewByDc from "./views/dc/sdyjhb/SdyjhMainView"
import SdjjhMainViewByDc from "./views/dc/sdjjhb/SdjjhMainView"
import SdnjhMainView from "./views/dc/sdnjhb/SdnjhMainView"

import SdrjhDetailFromByDc from "./views/dc/sdrjhb/SdrjhDetailForm"
import SdzjhDetailFormByDc from "./views/dc/sdzjhb/SdzjhDetailForm"
import SdyjhDetailFormByDc from "./views/dc/sdyjhb/SdyjhDetailForm"
import SdjjhDetailFormByDc from "./views/dc/sdjjhb/SdjjhDetailForm"


import MainContentViewByZd from "./views/zd/MainContentView";
import SdrjhMainViewByZd from "./views/zd/sdrjhb/SdrjhMainView";
import SdzjhMainViewByZd from "./views/zd/sdzjhb/SdzjhMainView";
import SdyjhMainViewByZd from "./views/zd/sdyjhb/SdyjhMainView";
import SdjjhMainViewByZd from "./views/zd/sdjjhb/SdjjhMainView"

import SdrjhDetailFormByZd from "./views/zd/sdrjhb/SdrjhDetailForm"
import SdzjhDetailFormByZd from "./views/zd/sdzjhb/SdzjhDetailForm"
import SdyjhDetailFormByZd from "./views/zd/sdyjhb/SdyjhDetailForm"
import SdjjhDetailFormByZd from "./views/zd/sdjjhb/SdjjhDetailForm"



Vue.use(Router)
const router = new Router({
    routes: [
        {
            path: '/dc/mainContent',
            name: 'MainContentViewByDc',
            component: MainContentViewByDc,
            meta: {
                title: '水库运行计划主页面（电厂方）'
            },
            children:[
                {
                    path: 'sdrjh',
                    name: 'SdrjhMainViewByDc',
                    component: SdrjhMainViewByDc,
                    meta: {
                        title: '日计划主页面（电厂方）'
                    },
                    props: (route) => ({ params: route.query })
                },
                {
                    path: 'sdzjh',
                    name: 'SdzjhMainViewByDc',
                    component: SdzjhMainViewByDc,
                    meta: {
                        title: '周计划主页面（电厂方)'
                    },
                    props: (route) => ({ params: route.query })
                },
                {
                    path: 'sdyjh',
                    name: 'SdyjhMainViewByDc',
                    component: SdyjhMainViewByDc,
                    meta: {
                        title: '月计划主页面（电厂方）'
                    },
                    props: (route) => ({ params: route.query })
                },
                {
                    path: 'sdjjh',
                    name: 'SdjjhMainViewByDc',
                    component: SdjjhMainViewByDc,
                    meta: {
                        title: '季计划主页面（电厂方）'
                    },
                    props: (route) => ({ params: route.query })
                },
                {
                    path: 'sdnjh',
                    name: 'SdnjhMainView',
                    component: SdnjhMainView,
                    meta: {
                        title: '年计划主页面（电厂方）'
                    },
                    props: (route) => ({ params: route.query })
                },
            ]
        },
        {
            path: '/zd/mainContent',
            name: 'MainContentViewByZd',
            component: MainContentViewByZd,
            meta: {
                title: '水库运行计划主页面（中调方）'
            },
            children:[
                {
                    path: 'sdrjh',
                    name: 'SdrjhMainViewByZd',
                    component: SdrjhMainViewByZd,
                    meta: {
                        title: '日计划主页面（中调方）'
                    },
                    props: (route) => ({ params: route.query })
                },
                {
                    path: 'sdzjh',
                    name: 'SdzjhMainViewByZd',
                    component: SdzjhMainViewByZd,
                    meta: {
                        title: '周计划主页面（中调方)'
                    },
                    props: (route) => ({ params: route.query })
                },
                {
                    path: 'sdyjh',
                    name: 'SdyjhMainViewByZd',
                    component: SdyjhMainViewByZd,
                    meta: {
                        title: '月计划主页面（中调方)'
                    },
                    props: (route) => ({ params: route.query })
                },
                {
                    path: 'sdjjh',
                    name: 'SdjjhMainViewByZd',
                    component: SdjjhMainViewByZd,
                    meta: {
                        title: '季计划主页面（中调方)'
                    },
                    props: (route) => ({ params: route.query })
                },
            ]
        },
        {
            path: '/dc/sdrjh/detail/:id',
            name: 'SdrjhDetailFormByDc',
            component: SdrjhDetailFromByDc,
            meta: {
                title: '日计划详情页（电厂方）'
            },
            props: (route) => ({ params: route.query })
        },
        {
            path: '/zd/sdrjh/detail/:id',
            name: 'SdrjhDetailFromByZd',
            component: SdrjhDetailFormByZd,
            meta: {
                title: '日计划详情页（中调方）'
            },
            props: (route) => ({ params: route.query })
        },
        {
            path: '/dc/sdzjh/detail/:id',
            name: 'SdzjhDetailFormByDc',
            component: SdzjhDetailFormByDc,
            meta: {
                title: '周计划详情页（电厂方）'
            },
            props: (route) => ({ params: route.query })
        },
        {
            path: '/zd/sdzjh/detail/:id',
            name: 'SdzjhDetailFormByZd',
            component: SdzjhDetailFormByZd,
            meta: {
                title: '周计划详情页（中调方）'
            },
            props: (route) => ({ params: route.query })
        },
        {
            path: '/dc/sdyjh/detail/:id',
            name: 'SdyjhDetailFormByDc',
            component: SdyjhDetailFormByDc,
            meta: {
                title: '月计划详情页（电厂方）'
            },
            props: (route) => ({ params: route.query })
        },
        {
            path: '/zd/sdyjh/detail/:id',
            name: 'SdyjhDetailFormByZd',
            component: SdyjhDetailFormByZd,
            meta: {
                title: '月计划详情页（中调方）'
            },
            props: (route) => ({ params: route.query })
        },
        {
            path: '/dc/sdjjh/detail/:ids',
            name: 'SdjjhDetailFormByDc',
            component: SdjjhDetailFormByDc,
            meta: {
                title: '季计划详情页（电厂方）'
            },
            props: (route) => ({ params: route.query })
        },
        {
            path: '/zd/sdjjh/detail/:ids',
            name: 'SdjjhDetailFormByZd',
            component: SdjjhDetailFormByZd,
            meta: {
                title: '季计划详情页（中调方）'
            },
            props: (route) => ({ params: route.query })
        },

    ]
})


router.beforeEach((to, from, next) => {
    if (to.meta.title) {
        document.title = to.meta.title
    }
    next()
})

export default router

```

