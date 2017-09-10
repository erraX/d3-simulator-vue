<template>
    <div class="editor-container">
        <Tabs :tabs="tabs">
            <div slot="equip" v-if="activeArea && activeEquipLists.length">
                <div class="control-wrapper">
                    <label>装备名:</label>
                    <div class="control">
                        <select v-model="curEquipId">
                            <option disabled value="-1">请选择</option>
                            <option
                                v-for="e in activeEquipLists"
                                :key="e.id"
                                :ref="e.id"
                                :value="e.id"
                            >
                                {{e.name}}
                            </option>
                        </select>
                    </div>
                </div>
                <div
                    class="control-wrapper"
                    v-for="field in activeEquipData.fields"
                >
                    <label>{{field.name}}:</label>
                    <div class="control" v-if="field.type === 'number'">
                        <input
                            type="number"
                            v-model="draft[curEquipId][field.prop]"
                        />
                    </div>
                    <div class="control" v-else-if="field.type === 'number-toggle'">
                        <input
                            type="number"
                            :disabled="!draft[curEquipId][field.prop + 'Checked']"
                            v-model="draft[curEquipId][field.prop]"
                        />
                        <input
                            type="checkbox"
                            v-model="draft[curEquipId][field.prop + 'Checked']"
                        />
                    </div>
                </div>
            </div>
        </Tabs>
    </div>
</template>

<script>
import Tabs from '../ui/Tabs'
import headEquipsData from '../mocks/equips/head'
import bodyEquipsData from '../mocks/equips/body'

export default {
    props: ['activeArea', 'equiped'],

    components: {
        Tabs
    },

    data () {
        return {
            tabs: [
                {
                    key: 'equip',
                    name: '装备',
                },
                {
                    key: 'skill',
                    name: '技能',
                },
            ],

            draft: {},
        }
    },

    mounted() {
        this.initEquipData();
        this.initDraftData();
    },

    watch: {
        draft: {
            handler(val) {
                this.$emit('equipChanged', {
                    attrs: this.draft[this.curEquipId]
                })
            },
            deep: true
        }
    },

    computed: {
        activeEquipMaps() {
            if (this.activeArea === 'head') {
                return this.headEquipsMap
            }
            else if (this.activeArea === 'body') {
                return this.bodyEquipsMap
            }

            return {};
        },

        activeEquipLists() {
            if (this.activeArea === 'head') {
                return this.headEquipsList
            }
            else if (this.activeArea === 'body') {
                return this.bodyEquipsList
            }

            return [];
        },

        activeEquipData() {
            return this.activeEquipMaps[this.curEquipId] || {
                id: '-1',
                name: '',
                fields: []
            }
        },

        curEquipId: {
            get() {
                var data = this.equiped[this.activeArea]

                if (!data) {
                    return '-1'
                }

                return data.id || '-1'
            },

            set(value) {
                if (value == -1) {
                    return
                }

                this.$emit('equipChanged', {
                    id: value,
                    name: this.$refs[value][0].text,
                    attrs: this.draft[value]
                })
            }
        },
    },

    methods: {
        initEquipData() {

            // Head
            this.headEquipsMap = headEquipsData
            this.headEquipsList = Object.keys(headEquipsData).map(key => headEquipsData[key])

            // Body
            this.bodyEquipsMap = bodyEquipsData
            this.bodyEquipsList = Object.keys(bodyEquipsData).map(key => bodyEquipsData[key])
        },

        initDraftData() {
            Object.keys(this.headEquipsMap).forEach(key => this.$set(this.draft, key, {}))
            Object.keys(this.bodyEquipsMap).forEach(key => this.$set(this.draft, key, {}))
        }
    }
}
</script>

<style scoped lang="less">
.control-wrapper {
    margin: 10px 0;

    .control {
        display: inline-block;
    }

    label {
        display: inline-block;
        width: 70px;
        text-align: right;
        margin-right: 10px;
    }

    select {
        width: 100px;
    }
}
</style>
