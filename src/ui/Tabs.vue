<template>
    <div class="tab">
        <div class="tab-bar">
            <div
              :class="getTabNameCls(tab.key)"
              v-for="tab in tabs"
              :key="tab.key"
              @click="clickTab(tab.key)"
            >
                {{ tab.name }}
            </div>
        </div>
        <div class="tab-content">
            <div
              :class="tab.key"
              v-for="tab in tabs"
              :key="tab.key"
              v-show="tabShouldShow(tab.key)"
            >
                <slot :name="tab.key"></slot>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: ['initTab', 'tabs'],

    mounted() {

        // 默认的Tab，没有传进来的话就第一个
        this.curTab = this.initTab || this.tabs[0].key
    },

    data() {
        return {
            curTab: null,
        }
    },

    computed: {},

    methods: {
        getTabNameCls(tabKey) {
            return {
                'tab-name': true,
                active: this.curTab === tabKey
            }
        },

        tabShouldShow(tabKey) {
            return this.curTab === tabKey
        },

        clickTab(tabKey) {
            this.curTab = tabKey
        },
    }
}
</script>

<style scoped lang="less">

    .tab-bar {
        display: flex;
    }

    .tab-name {
        flex: 1;
        height: 30px;
        line-height: 30px;

        &.active {
            color: #fff;
            background-color: #2196F3;
        }

        &:hover {
            cursor: pointer;
        }
    }

    .tab-content {
        > div {
            text-align: left;
        }
    }
</style>
