<template>
    <div class="feature_C section-side-padding">
        <!-- wwManager:start -->
        <wwSectionEditMenu :sectionCtrl="sectionCtrl" :options="openOptions"></wwSectionEditMenu>
        <!-- wwManager:end -->
        <wwObject class="background" :ww-object="section.data.background" ww-category="background"></wwObject>

        <!--TOP WWOBJS-->
        <div class="top-ww-objs">
            <wwLayoutColumn tag="div" ww-default="ww-image" :ww-list="section.data.topWwObjs" class="top-ww-obj" @ww-add="add(section.data.topWwObjs, $event)" @ww-remove="remove(section.data.topWwObjs, $event)">
                <wwObject v-for="topWwObj in section.data.topWwObjs" :key="topWwObj.uniqueId" :ww-object="topWwObj"></wwObject>
            </wwLayoutColumn>
        </div>

        <!--THUMBNAILS-->
        <div class="container">
            <div class="container-center">
                <div class="thumbnail-container" v-for="(thumbnail, index) in section.data.thumbnails" :key="thumbnail.uniqueId" :style="columnWidth">
                    <!-- wwManager:start -->
                    <wwContextMenu tag="div" class="contextmenu" v-if="sectionCtrl.getEditMode() == 'CONTENT'" @ww-add-before="addCardBefore(index)" @ww-add-after="addCardAfter(index)" @ww-remove="removeCard(index)">
                        <div class="wwi wwi-config"></div>
                    </wwContextMenu>
                    <!-- wwManager:end -->
                    <wwObject class="background" :ww-object="thumbnail.background" ww-category="background" ww-default-object-type="ww-color"></wwObject>

                    <wwLayoutColumn tag="div" ww-default="ww-image" :ww-list="thumbnail.contents" class="content" @ww-add="add(thumbnail.contents, $event)" @ww-remove="remove(thumbnail.contents, $event)">
                        <wwObject v-for="content in thumbnail.contents" :key="content.uniqueId" :ww-object="content"></wwObject>
                    </wwLayoutColumn>
                </div>
            </div>
        </div>

        <!--BOTTOM WWOBJS-->
        <div class="bottom-ww-objs">
            <wwLayoutColumn tag="div" ww-default="ww-image" :ww-list="section.data.bottomWwObjs" class="top-ww-obj" @ww-add="add(section.data.bottomWwObjs, $event)" @ww-remove="remove(section.data.bottomWwObjs, $event)">
                <wwObject v-for="bottomWwObj in section.data.bottomWwObjs" :key="bottomWwObj.uniqueId" :ww-object="bottomWwObj"></wwObject>
            </wwLayoutColumn>
        </div>
    </div>
</template>

<script>

/* wwManager:start */
import featureCColumnPerLine from './featureCColumnPerLine.vue'
wwLib.wwPopups.addPopup('featureCColumnPerLine', featureCColumnPerLine);
wwLib.wwPopups.addStory('FEATURE_C_COLUMN_COUNT', {
    title: {
        en_GB: 'Column per line',
        fr_FR: 'Nombre de colonnes par ligne'
    },
    type: 'featureCColumnPerLine',
    buttons: {
        FINISH: {
            text: {
                en_GB: 'Finish',
                fr_FR: 'Terminer'
            },
            next: false
        }
    }
});
/* wwManager:end */

export default {
    name: "__COMPONENT_NAME__",
    props: {
        sectionCtrl: Object
    },
    data() {
        return {
            thumbnailsPerLine: 4,
            maxThumbnailsPerLine: 4,
            columnWidth: { 'width': "calc(25% - 30px)" }
        }
    },
    computed: {
        section() {
            return this.sectionCtrl.get();
        }
    },
    methods: {
        initData() {
            let needUpdate = false;

            //Init objects
            if (!this.section.data.background) {
                this.section.data.background = wwLib.wwObject.getDefault({ type: 'ww-color', data: { color: 'white' } });
                needUpdate = true;
            }
            if (_.isEmpty(this.section.data.topWwObjs)) {
                this.section.data.topWwObjs = [];
                needUpdate = true;
            }
            if (_.isEmpty(this.section.data.bottomWwObjs)) {
                this.section.data.bottomWwObjs = [];
                needUpdate = true;
            }
            if (_.isEmpty(this.section.data.thumbnails)) {
                this.section.data.thumbnails = [];
                needUpdate = true;
            }
            if (_.isEmpty(this.section.data.thumbnails)) {
                this.section.data.thumbnails.push({
                    background: wwLib.wwObject.getDefault({ type: 'ww-color', data: { color: 'white' } }),
                    contents: []
                })
                needUpdate = true;
            }
            if (needUpdate) {
                this.sectionCtrl.update(this.section);
            }
        },
        init() {
            this.setThumbnailsPerLine();
            window.addEventListener("resize", this.setThumbnailsPerLine);
        },
        add(list, options) {
            list.splice(options.index, 0, options.wwObject);

            this.sectionCtrl.update(this.section);
        },
        remove(list, options) {
            list.splice(options.index, 1);

            this.sectionCtrl.update(this.section);
        },
        setThumbnailsPerLine() {
            let width = window.innerWidth;
            if (width < 576) {
                this.maxThumbnailsPerLine = 1;
            }
            else if (width < 992) {
                this.maxThumbnailsPerLine = 2;
            }
            else if (width < 1200) {
                this.maxThumbnailsPerLine = 3;
            }
            else {
                this.maxThumbnailsPerLine = 4;
            }

            switch (Math.min(this.thumbnailsPerLine, this.maxThumbnailsPerLine)) {
                case 1:
                    this.columnWidth = { 'width': "calc(100% - 30px)" };
                    break;
                case 2:
                    this.columnWidth = { 'width': "calc(50% - 30px)" };
                    break;
                case 3:
                    this.columnWidth = { 'width': "calc(33.3333% - 30px)" };
                    break;
                default:
                    this.columnWidth = { 'width': "calc(25% - 30px)" };
                    break;
            }
        },

        /* wwManager:start */
        addCardBefore(index) {
            const newCard = {
                background: wwLib.wwObject.getDefault({ type: 'ww-color', data: { color: 'white' } }),
                contents: []
            }
            this.section.data.thumbnails.splice(index, 0, newCard);
            this.sectionCtrl.update(this.section);
        },
        addCardAfter(index) {
            const newCard = {
                background: wwLib.wwObject.getDefault({ type: 'ww-color', data: { color: 'white' } }),
                contents: []
            }
            this.section.data.thumbnails.splice(index + 1, 0, newCard);
            this.sectionCtrl.update(this.section);
        },
        removeCard(index) {
            this.section.data.thumbnails.splice(index, 1);

            if (!this.section.data.thumbnails.length) {
                this.addCardAfter(0);
            }
            this.sectionCtrl.update(this.section);
        },

        async openOptions() {
            let options = {
                firstPage: 'FEATURE_C_COLUMN_COUNT',
                data: {
                    columnPerLine: this.thumbnailsPerLine
                },
            }

            const result = await wwLib.wwPopups.open(options)

            if (result.columnPerLine) {
                this.thumbnailsPerLine = result.columnPerLine;

                this.setThumbnailsPerLine();
            }


            console.log(result);
        }
        /* wwManager:end */
    },
    created() {
        this.initData();
    },
    mounted: function () {
        this.init();
    },
    beforeDestroy() {
        window.removeEventListener("resize", this.setThumbnailsPerLine);
    }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.feature_C {
    position: relative;

    .background {
        position: absolute;
        width: 100%;
        height: 100%;
        top: 0;
        left: 0;
    }

    .top-ww-objs,
    .bottom-ww-objs {
        position: relative;

        .top-ww-obj,
        .bottom-ww-obj {
            position: relative;
        }
    }

    .container {
        .container-center {
            display: flex;
            justify-content: center;
            flex-wrap: wrap;

            .thumbnail-container {
                margin-right: 15px;
                position: relative;
                margin: 30px 15px;
                background-color: white;
                min-height: 50px;
                box-shadow: 0 10px 40px 0 rgba(113, 124, 137, 0.2);
                padding: 0 45px;
                border-radius: 7px;
                transition: transform 0.4s ease-out, box-shadow 0.4s ease-out;

                &:hover {
                    box-shadow: 0 16px 32px rgba(113, 124, 137, 0.4);
                    transform: translateY(-10px);
                }

                .background {
                    border-radius: 7px;
                    overflow: hidden;
                }

                .content {
                    position: relative;
                }

                /* wwManager:start */
                .contextmenu {
                    position: absolute;
                    top: 0;
                    left: 0;
                    transform: translate(-50%, -50%);
                    width: 30px;
                    height: 30px;
                    color: white;
                    background-color: #ef811a;
                    border-radius: 100%;
                    display: flex;
                    justify-content: center;
                    align-items: center;
                    font-size: 1.2rem;
                    cursor: pointer;
                    z-index: 1;
                }
                /* wwManager:enb */
            }
        }
    }
}
</style>
