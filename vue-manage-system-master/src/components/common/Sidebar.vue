<template>
    <div class="sidebar">
        <el-menu
            class="sidebar-el-menu"
            :default-active="onRoutes"
            :collapse="collapse"
            background-color="#324157"
            text-color="#bfcbd9"
            active-text-color="#20a0ff"
            unique-opened
            router
        >
            <template v-for="item in items">
                <template v-if="item.subs">
                    <el-submenu :index="item.index" :key="item.index">
                        <template slot="title">
                            <i :class="item.icon"></i>
                            <span slot="title">{{ item.title }}</span>
                        </template>
                        <template v-for="subItem in item.subs">
                            <el-submenu
                                v-if="subItem.subs"
                                :index="subItem.index"
                                :key="subItem.index"
                            >
                                <template slot="title">{{ subItem.title }}</template>
                                <el-menu-item
                                    v-for="(threeItem,i) in subItem.subs"
                                    :key="i"
                                    :index="threeItem.index"
                                >{{ threeItem.title }}</el-menu-item>
                            </el-submenu>
                            <el-menu-item
                                v-else
                                :index="subItem.index"
                                :key="subItem.index"
                            >{{ subItem.title }}</el-menu-item>
                        </template>
                    </el-submenu>
                </template>
                <template v-else>
                    <el-menu-item :index="item.index" :key="item.index">
                        <i :class="item.icon"></i>
                        <span slot="title">{{ item.title }}</span>
                    </el-menu-item>
                </template>
            </template>
        </el-menu>
    </div>
</template>

<script>
import bus from '../common/bus';
export default {
    data() {
        return {
            collapse: false,
            items: [
                {
                    icon: 'el-icon-lx-home',
                    index: 'dashboard',
                    title: '系统首页'
                },
              /*  {
                    icon: 'el-icon-lx-cascades',
                    index: 'table',
                    title: '基础表格'
                },*/
                {
                    icon: 'el-icon-lx-calendar',
                    index: '3',
                    title: '库存管理',
                    subs: [
                        {
                            index: 'form',
                            title: '商品上传'
                        },
                        {
                            index: 'upload',
                            title: '文件上传'
                        },
                        {
                            index:'inventory',
                            title: '库存情况'
                        }
                    ]
                },
                {
                    icon: 'el-icon-lx-cascades',
                    index: '6',
                    title: '订单管理',
                    subs: [
                        {
                            index: 'orderTable',
                            title: '订单表'
                        },
                        {
                            index: 'orderDetailAll',
                            title: '所有订单'
                        },
                        {
                            index: 'orderDetail',
                            title: '订单详情'
                        }
                    ]
                },
                {
                    icon: 'el-icon-lx-cascades',
                    index: '8',
                    title: '用户管理',
                    subs: [
                        {
                            index: 'RegisterForm',
                            title: '用户注册'
                        },
                        {
                            index: 'UserTable',
                            title: '用户管理'
                        }
                    ]

                },
                {
                    icon: 'el-icon-lx-cascades',
                    index: 'profitTable',
                    title: '销售情况'
                },
                {
                    icon: 'el-icon-pie-chart',
                    index: 'charts',
                    title: 'schart图表'
                },

                {
                    icon: 'el-icon-lx-warn',
                    index: '7',
                    title: '错误处理',
                    subs: [
                        {
                            index: '404',
                            title: '404页面'
                        }
                    ]
                },

            ]
        };
    },
    computed: {
        onRoutes() {
            return this.$route.path.replace('/', '');
        }
    },
    created() {
        // 通过 Event Bus 进行组件间通信，来折叠侧边栏
        bus.$on('collapse', msg => {
            this.collapse = msg;
            bus.$emit('collapse-content', msg);
        });
    }
};
</script>

<style scoped>
.sidebar {
    display: block;
    position: absolute;
    left: 0;
    top: 70px;
    bottom: 0;
    overflow-y: scroll;
}
.sidebar::-webkit-scrollbar {
    width: 0;
}
.sidebar-el-menu:not(.el-menu--collapse) {
    width: 250px;
}
.sidebar > ul {
    height: 100%;
}
</style>
