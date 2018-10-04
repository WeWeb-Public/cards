<template>

	<div class="feature_C section-side-padding">
		<wwObject class="background" v-bind:ww-object="section.data.background" ww-category="background">
		</wwObject>

		<!--TOP WWOBJS-->
		<div class="top-ww-objs">
			<div class="top-ww-obj" v-for="topWwObj in section.data.topWwObjs" :key="topWwObj.uniqueId">
				<wwObject v-bind:ww-object="topWwObj"></wwObject>
			</div>
		</div>

		<!--THUMBNAILS-->
		<div class="container">
			<div class="container-center">
				<div class="thumbnail-container" v-for="thumbnail in section.data.thumbnails" :key="thumbnail.uniqueId">
					<div>
						<wwObject class="background" v-bind:ww-object="thumbnail.background" ww-category="background" ww-default-object-type="ww-color"></wwObject>
						<div class="content" v-for="content in thumbnail" :key="content.uniqueId">
							<wwObject v-bind:ww-object="content"></wwObject>
						</div>
					</div>
				</div>
			</div>
		</div>

		<!--BOTTOM WWOBJS-->
		<div class="bottom-ww-objs">
			<div class="top-ww-obj" v-for="bottomWwObj in section.data.bottomWwObjs" :key="bottomWwObj.uniqueId">
				<wwObject v-bind:ww-object="bottomWwObj"></wwObject>
			</div>
		</div>

	</div>

</template>

<script>
export default {
  name: "feature_C",
  props: {
    section: Object
  },
  methods: {
    init() {
      this.updateThumbnailContainer();
      window.addEventListener("resize", this.updateThumbnailContainer);
    },
    updateThumbnailContainer() {
      let thumbnailContainers = this.$el.querySelectorAll(
        ".thumbnail-container"
      );
      let columnCount = 1;
      if (window.innerWidth > 768) {
        columnCount = this.section.data.columnCount;
      }
      console.log("weijijweijf", columnCount);
      switch (parseInt(columnCount)) {
        case 4:
          for (let thumbnailContainer of thumbnailContainers) {
            thumbnailContainer.style.width = "calc(25% - 30px)";
            thumbnailContainer.style.marginRight = "30px";
          }
          break;
        case 3:
          for (let thumbnailContainer of thumbnailContainers) {
            thumbnailContainer.style.width = "calc(33.3333% - 30px)";
            thumbnailContainer.style.marginRight = "30px";
          }
          break;
        case 2:
          for (let thumbnailContainer of thumbnailContainers) {
            thumbnailContainer.style.width = "calc(50% - 30px)";
            thumbnailContainer.style.marginRight = "30px";
          }
          break;
        case 1:
          for (let thumbnailContainer of thumbnailContainers) {
            thumbnailContainer.style.width = "calc(100% - 30px)";
            thumbnailContainer.style.marginRight = "30px";
          }
          break;

        default:
          break;
      }
    }
  },
  created: function() {},
  mounted: function() {
    this.init();
  },
  beforeDestroyed() {
    window.removeEventListener("resize", this.updateThumbnailContainer);
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.feature_C {
  position: relative;
}
.feature_C .background {
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
}

.feature_C .add-ww-obj-container {
  width: 100%;
  text-align: center;
  margin: 10px 0;
  position: relative;
}

.feature_C .add-ww-obj-container .add-ww-obj {
  display: inline-block;
  height: 40px;
  width: 40px;
  line-height: 40px;
  text-align: center;
  background-color: white;
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.5);
  border-radius: 3px;
  cursor: pointer;
}

.feature_C .remove-ww-obj {
  position: absolute;
  top: 0;
  left: 0;
  transform: translate(-50%, -50%);
  height: 30px;
  width: 30px;
  background-color: white;
  color: #92979c;
  box-shadow: 0px 1px 1px 0px #656565;
  -moz-box-shadow: 0px 1px 1px 0px #656565;
  -webkit-box-shadow: 0px 1px 1px 0px #656565;
  border-radius: 100%;
  text-align: center;
  line-height: 30px;
  cursor: pointer;
  z-index: 2;
}

.feature_C .top-ww-obj,
.bottom-ww-obj {
  position: relative;
}

.feature_C .block .block-img-container {
  height: 260px;
  width: 100%;
  /*overflow: hidden;*/
  position: relative;
}

.feature_C .add-thumbnail-container {
  padding: 30px;
  text-align: center;
}

.feature_C .add-thumbnail {
  display: inline-block;
  background-color: white;
  width: 150px;
  padding: 10px;
  text-align: center;
  cursor: pointer;
  color: #92979c;
  border-radius: 3px;
  -moz-border-radius: 3px;
  -webkit-border-radius: 3px;
  box-shadow: 0px 1px 1px 0px #656565;
}

.feature_C .thumbnail-container {
  position: relative;
  margin-bottom: 30px;
  background-color: white;
  min-height: 50px;
  box-shadow: 0 10px 40px 0 rgba(113, 124, 137, 0.2);
  padding: 0 45px;
  border-radius: 7px;
  -moz-border-radius: 7px;
  -webkit-border-radius: 7px;
  -moz-transition: all 0.4s ease-out;
  -o-transition: all 0.4s ease-out;
  -ms-transition: all 0.4s ease-out;
  -webkit-transition: all 0.4s ease-out;
  transition: all 0.4s ease-out;
}

.feature_C .thumbnail-container:hover {
  box-shadow: 0 16px 32px rgba(113, 124, 137, 0.4);
  transform: translateY(-10px);
}

.feature_C .thumbnail-container .background {
  border-radius: 7px;
  overflow: hidden;
}

.feature_C .remove-thumbnail {
  position: absolute;
  top: 0;
  right: -20px;
  transform: translate(calc(50% - 15px), -50%);
  height: 30px;
  width: 30px;
  background-color: white;
  color: #e83154;
  box-shadow: 0px 1px 1px 0px #656565;
  -moz-box-shadow: 0px 1px 1px 0px #656565;
  -webkit-box-shadow: 0px 1px 1px 0px #656565;
  border-radius: 100%;
  -moz-border-radius: 100%;
  -webkit-border-radius: 100%;
  text-align: center;
  line-height: 30px;
  cursor: pointer;
  z-index: 2;
}

.feature_C .thumbnail-container .content {
  position: relative;
}

.popup-feature_C-column-count .radio-size {
  height: 30px;
  width: 250px;
  margin-top: 30px;
  display: inline-block;
}

.feature_C .card-container-for-two {
  width: 80%;
  margin-left: 10%;
}

.feature_C .card-container-for-three {
  width: 90%;
  margin-left: 5%;
}

.feature_C .card-container-for-four {
  width: 100%;
  margin-left: 0%;
}

.feature_C .container-center {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}

@media (min-width: 768px) {
  /* .feature_C .card-container-for-two {
        width: 90%;
        margin-left: 5%;
    }
    .feature_C .card-container-for-three {
        width: 100%;
        margin-left: 0%;
    } */
}

@media (min-width: 992px) {
  /* .feature_C .card-container-for-two {
        width: 90%;
        margin-left: 5%;
    } */
}

@media (min-width: 1200px) {
  /* .feature_C .card-container-for-two {
        width: 90%;
        margin-left: 5%;
    } */
}
</style>
