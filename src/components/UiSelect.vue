<template name="ui-select">
    <div class="ui-select">
        <input type="text" class="ui-input-inner" placeholder="Select an option" readonly>
        <div class="ui-dropdown-inner ui-scroll-root">
            <div class="ui-scroll-wrap" @scroll="scrollHandle($event)" ref="scrollWrap">
                <ul class="ui-dropdown-group">
                    <li v-for="option in options" :key="option.value">{{option.name}}</li>
                </ul>
            </div>
            <div class="ui-scrollbar-bar is-vertical" ref="verticalScroll">
                <div class="ui-scrollbar-thump" :style="{height: heightPercentage + '%', transform: 'translateY(' + translateY + '%)'}"></div>
            </div>
            <div class="ui-scrollbar-bar is-horizontal" ref="horizontalScroll">
                <div class="ui-scrollbar-thump"></div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'UiSelect',
    props: ['options', 'result'],
    data() {
        return {
            heightPercentage: 0,
            widthPercentage: 0,
            translateY: 0,
            translateX: 0,
        }
    },
    methods: {
        scrollHandle(e) {
            console.log(e);
            const $el = this.$refs.scrollWrap;
            this.translateY = ($el.scrollTop / $el.clientHeight * 100).toFixed(4);
            this.translateX = ($el.scrollLeft / $el.clientWidth * 100).toFixed(4)
        }
    },
    mounted() {
        const self = this;
        const $el = self.$refs.scrollWrap;
        self.heightPercentage = ($el.clientHeight / $el.scrollHeight * 100).toFixed(4);
        self.widthPercentage = ($el.clientWidth / $el.scrollWidth * 100).toFixed(4);
    },

}
</script>