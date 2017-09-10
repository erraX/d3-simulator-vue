<template>
    <div class="monitor-container">
        <h1 class="panel-title">属性</h1>
        <p class="prop">
            <label class="prop-name">攻击:</label>
            <span class="prop-value">{{ attributes.attack }}</span>
        </p>
        <p class="prop">
            <label class="prop-name">防御:</label>
            <span class="prop-value">{{ attributes.shield }}</span>
        </p>
    </div>
</template>

<script>
    export default {
        props: ['attributes'],
    }
</script>

<style scoped lang="less">
    .prop {
        font-size: 20px;
        text-align: left;
        margin-bottom: 10px;
        padding-left: 50px;
    }

    .prop-name {
        display: inline-block;
        font-weight: 500;
        width: 60px;
    }
</style>
