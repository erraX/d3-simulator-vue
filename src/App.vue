<template>
    <div id="app">
        <Character
            :equiped="equiped"
            :activeArea="activeArea"
            :activeArea.sync="activeArea"
        >
        </Character>
        <Editor
            :equiped="equiped"
            :activeArea="activeArea"
            @equipChanged="onEquipChanged"
        >
        </Editor>
        <Monitor
            :attributes="attributes"
        >
        </Monitor>
    </div>
</template>

<script>
import Character from './components/Character'
import Editor from './components/Editor'
import Monitor from './components/Monitor'

export default {
    components: {
        Character,
        Editor,
        Monitor,
    },

    data() {
        return {

            /**
             * 所有部位已装备的数据
             *
             * @type {Object}
             */
            equiped: {

                // 头部
                head: {
                    id: null,
                    name: '',
                    attrs: {},
                },

                // 身体
                body: {
                    id: null,
                    name: '',
                    attrs: {},
                },

                // 脚部
                footer: {
                    id: null,
                    name: '',
                    attrs: {},
                },
            },

            /**
             * 当前选中编辑的部位
             *
             * @type {string}
             */
            activeArea: null,

            /**
             * 技能情况
             *
             * @type {string}
             */
            skills: {},
        }
    },

    computed: {
        attributes() {
            const areas = ['head', 'body', 'footer']
            const equiped = this.equiped

            const attack = areas.reduce((total, area) => {
                const attack = equiped[area].attrs.attack
                const magic = equiped[area].attrs.magic
                const magicChecked = equiped[area].attrs.magicChecked

                if (attack) {
                    total += parseInt(attack, 10)
                }

                if (magicChecked && magic) {
                    total += parseInt(magic, 10)
                }

                return total
            }, 0);

            const shield = areas.reduce((total, area) => {
                const shield = equiped[area].attrs.shield
                if (shield) {
                    total += parseInt(shield, 10)
                }

                return total
            }, 0);

            return {
                attack,
                shield,
            };
        }
    },

    methods: {
        onEquipChanged({ id, name, attrs }) {
            if (id) {
                this.equiped[this.activeArea].id = id
            }

            if (name) {
                this.equiped[this.activeArea].name = name
            }

            if (attrs) {
                this.equiped[this.activeArea].attrs = attrs
            }
        },
    },
}
</script>

<style lang="less">
body {
    width: 1300px;
    margin: 0 auto;
}

ul {
    list-style-type: none;
    padding: 0;
}

li {
    display: inline-block;
    margin: 0 10px;
}

p {
    margin: 0;
    padding: 0;
}

.panel-title {
    /* background-color: #000; */
    /* color: #fff; */
}

#app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
    display: flex;
}

.character-container,
.editor-container,
.monitor-container {
    border: 1px solid #eee;
    border-right: 0;
    position: relative;
    flex-grow: 1;
    width: 33%;
    height: 800px;
}

.monitor-container {
    border: 1px solid #eee;
}
</style>
