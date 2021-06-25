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

        <component :is="active"></component>

    </div>
</template>

<script>
    import first from './template/first';
    import second from './template/second';
    import third from './template/third';

    export default {
        name: 'tabs-index',
        components: {
            first,
            second,
            third,
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