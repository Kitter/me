<template>
    <ul 
        :class="[
            wrap? 'mdui-list' : 'mdui-collapse-item-body mdui-list mdui-list-dense'
        ]" 
        mdui-collapse="{accordion: true}">
        <template v-for="item in menu">
            <li 
                :key="item.id" 
                v-if="item._child.length == 0"
                @click.stop="goPage(item)"
                class="mdui-list-item mdui-ripple"
                :class="{
                    'active': $route.path == item.url
                }">
                <template v-if="wrap">
                    <i 
                        class="mdui-list-item-icon mdui-icon material-icons" 
                        :style="{
                            color: item['icon-color'] || 'rgba(0,0,0,.54)'
                        }">
                        {{item.icon}}
                    </i>
                    <div class="mdui-list-item-content">{{item.name}}</div>
                </template>
                <template v-else>
                    {{item.name}}
                </template>
            </li>
            <li v-else :key="item.id" class="mdui-collapse-item">
                <div class="mdui-collapse-item-header mdui-list-item mdui-ripple">
                    <i 
                    v-if="wrap" 
                    class="mdui-list-item-icon mdui-icon material-icons"
                    :style="{
                        color: item['icon-color'] || 'rgba(0,0,0,.54)'
                    }">
                        {{item.icon}}
                    </i>
                    <div class="mdui-list-item-content">{{item.name}}</div>
                    <i class="mdui-collapse-item-arrow mdui-icon material-icons">keyboard_arrow_down</i>
                </div>
                <collapse-side :menu="item._child" />
            </li>
        </template>
        
    </ul>
</template>

<script>
export default {
  name: "collapseSide",
  data() {
    return {};
  },
  props: {
    menu: {
      default: () => []
    },
    wrap: {
      default: () => false
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.$mdui.mutation();
    });
  },
  methods: {
    goPage(item){
        this.$store.dispatch('goPage', {...item});
        this.$store.commit('createTimestamp');
    }
  },

};
</script>

<style lang="less" scoped>
.mdui-list-item{
    position: relative;
    &::before{
        position: absolute;
        height: 100%;
        width: 4px;
        background: #00897b;
        right: -4px;
        top: 0px;
        content: ' ';
        transition: all .2s;
    }
    &.active{
        color: #00897b;
        background-color: rgba(0, 0, 0, .08);
        &::before{ 
            right: 0px;
        }
    }
}
</style>
