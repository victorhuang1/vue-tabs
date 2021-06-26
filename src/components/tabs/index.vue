<template>
    <div>
        <div class="tabs">
            <ul class="tabs-nav">
                <li @click="toggleTab('first')" :class="classActive('first')">标签一</li>
                <li @click="toggleTab('second')" :class="classActive('second')">标签二</li>
                <li @click="toggleTab('third')" :class="classActive('third')">标签三</li>
            </ul>
        </div>
        <!-- <first v-if="active('first')">
            标签一
        </first>

        <second v-else-if="active('second')">
            标签二
        </second>

        <third v-else-if="active('third')">
            标签三
        </third> -->

        <!-- <component :is="active"></component> -->
        <!-- 而 name 属性是 page 的 key 是静态值所以不需要 v-bind -->
    <tab-page :current="activeName" name="first">
      <b> 模板一 </b>
    </tab-page>

    <tab-page :current="activeName" name="second">
      <b> 模板二 </b>
    </tab-page>

    <tab-page :current="activeName" name="third">
      <b> 模板三 </b>
    </tab-page>

    </div>
</template>

<script>
 /*    import first from './template/first';
    import second from './template/second';
    import third from './template/third'; */
    import tabPage from "./tab-page";

    export default {
        name: 'tabs-index',
        components: {
            /* first,
            second,
            third, */
            tabPage,
        },
        data() {
            return {
                activeName: 'first',
            }
        },
        computed: {
            active() {
                return this.activeName;
            }
        },
        methods: {
            toggleTab(name) {
                this.activeName = name;
            },
            classActive(label) {
                return this.activeName === label ? 'active' : ''
            },
           /*  active(label) {
                return this.activeName === label;
            } */
        },
    }
</script>