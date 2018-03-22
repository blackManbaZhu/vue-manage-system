<template>
    <div class="sidebar">
        <el-menu :default-active="onRoutes" class="el-menu-vertical-demo" theme="dark" unique-opened router 
        background-color="#242f42"
        text-color="#fff"
        active-text-color="#ffd04b">
            <template v-for="item in items">
                <template v-if="item.subs">
                    <el-submenu :index="item.index" :key="item.index">
                        <template slot="title"><i :class="item.icon"></i>{{ item.title }}</template>
                        <el-menu-item 
                        v-for="(subItem,i) in item.subs" 
                        :key="i" 
                        :index="subItem.index"
                        @click="clkTab(item,subItem.title)"
                        >
                        {{ subItem.title }}
                        </el-menu-item>
                    </el-submenu>
                </template>
                <template v-else>
                    <el-menu-item :index="item.index" :key="item.index" @click="clkTab(item,item.title)">
                        <i :class="item.icon"></i>{{ item.title }}
                    </el-menu-item>
                </template>
            </template>
        </el-menu>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                items: [
                    {
                        icon: 'el-icon-setting',
                        index: 'home',
                        title: '首页'
                    },
                    {
                        icon: 'el-icon-menu',
                        index: '2',
                        title: '表格',
                        subs: [
                            {
                                index: 'basetable',
                                title: '基础表格'
                            },
                            {
                                index: 'map',
                                title: '地图展示'
                            },
                            {
                                index: 'vuetable',
                                title: 'Vue表格组件'
                            }
                        ]
                    },
                    {
                        icon: 'el-icon-date',
                        index: '3',
                        title: '表单',
                        subs: [
                            {
                                index: 'baseform',
                                title: '基本表单'
                            },
                            {
                                index: 'vueeditor',
                                title: '编辑器'
                            },
                            {
                                index: 'markdown',
                                title: 'markdown'
                            },
                            {
                                index: 'upload',
                                title: '文件上传'
                            }
                        ]
                    },
                    {
                        icon: 'el-icon-star-on',
                        index: 'basecharts',
                        title: '图表'
                    },
                    {
                        icon: 'el-icon-upload2',
                        index: 'drag',
                        title: '拖拽'
                    }
                ]
            }
        },
        computed:{
            onRoutes(){
                return this.$route.path.replace('/','');
            }
        },
        methods:{
            clkTab(item,title){
                this.$emit('clickMenu',item,title,'item','title');
            },
            initMenu() {
                let path  = this.$route.path.replace('/','');
                let title = '';
                let arr   = this.items.filter(function (d) {
                    if( d.index != path && d.subs != undefined && d.subs ) {
                        for(let i=0;i<d.subs.length;i++){
                            if(d.subs[i].index == path){
                                title = d.subs[i].title;
                                return d.subs[i].title;;
                            }
                        }
                    } else 
                    if( d.index == path ){
                        title = d.title;
                        return d;
                    }
                });
                this.$emit('initMenu',arr,title,'item','title');
            }
        },
        mounted(){
           this.initMenu(); //初始化内容区头部导航
        }
    }
</script>

<style scoped>
    .sidebar{
        display: block;
        position: absolute;
        width: 250px;
        left: 0;
        top: 70px;
        bottom:0;
        background: #2E363F;
    }
    .sidebar > ul {
        height:100%;
    }
</style>
