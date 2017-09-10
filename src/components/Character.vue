<template>
  <div class="character-container">
    <h1 class="panel-title">人物</h1>
    <div
      :class="headCls"
      @click="selectArea('head')"
    >
      {{ equiped.head.name || '未装备' }}
    </div>
    <div
      :class="bodyCls"
      @click="selectArea('body')"
    >
      {{ equiped.body.name || '未装备' }}
    </div>
    <div
      :class="footerCls" 
      @click="selectArea('footer')"
    >
      {{ equiped.footer.name || '未装备' }}
    </div>
  </div>
</template>

<script>
export default {
  props: ['equiped', 'activeArea'],

  data () {
    return {}
  },

  computed: {
    headCls() {
      return {
        equip: true,
        head: true,
        checked: this.activeArea === 'head',
      }
    },

    bodyCls() {
      return {
        equip: true,
        body: true,
        checked: this.activeArea === 'body',
      }
    },

    footerCls() {
      return {
        equip: true,
        footer: true,
        checked: this.activeArea === 'footer',
      }
    },
  },

  methods: {
    selectArea(area) {
      this.$emit(
        'update:activeArea',
        this.activeArea === area ? null : area
      )
    },
  }
}
</script>

<style scoped lang="less">
  .equip {
    border: 1px solid #eee;
    position: absolute;
    font-weight: 500;
    font-size: 21px;
    top: 0;
    left: 168px;
    width: 100px;
    height: 100px;
    line-height: 100px;
    text-align: center;
    z-index: 2;

    &:hover {
      cursor: pointer;
      background-color: #fafafa;
    }

    &.checked {
      background-color: #fafafa;
    }
  }

  .head {
    top: 120px;
  }

  .body {
    top: 270px;
  }

  .footer {
    top: 420px;
  }
</style>
