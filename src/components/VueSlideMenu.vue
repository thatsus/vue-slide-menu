<template>
  <div :class="menuClass">
    <div class="vue-slide-menu">
      <div class="header-bar" :style="headerStyle">
        <a class="menu-header" :style="headerStyle">
          <img v-if="img" :src="img" alt="">
          {{ title }}
        </a>
        <a class="close-button" href="#" @click.prevent="close">&times;</a>
      </div>
      <div :class="topMenuClass">
        <div class="scrollable" :style="style">
          <ul>
            <template v-for="(top, topI) in menu">
              <li :key="topI" class="item">
                <template v-if="typeof top.value == 'object'">
                  <a href="#" :style="linkStyle" @click.prevent="selectIndex(topI)">
                    {{ top.name }}
                    <div  class="right-arrow">&rarr;</div>
                  </a>
                </template>
                <a v-else :style="linkStyle" :href="top.value">{{ top.name }}</a>
              </li>
            </template>
          </ul>
        </div>
      </div>
      <template v-for="(top, topI) in menu">
        <div :class="subMenuClass(topI)" :key="'div-'+topI" >
            <div class="scrollable" :style="style">
              <ul>
                <li class="item">
                  <a @click.prevent="selectIndex(null)" :style="linkStyle">
                    &larr; Back
                  </a>
                </li>
              </ul>
              <template v-for="(sub, subI) in top.value">
                <ul :key="topI+'-'+subI">
                  <li class="sub-menu-header">{{ sub.name }}</li>
                  <template v-for="(item, itemI) in sub.value">
                    <li class="item" :style="linkStyle" :key="topI+'-'+subI+'-'+itemI">
                      <a :href="item.value" :style="linkStyle">{{ item.name }}</a>
                    </li>
                  </template>
                </ul>
              </template>
            </div>
          </div>
        </template>
      </div>
    <div class="vue-slide-menu-overlay" @click="close"></div>
  </div>
</template>

<script>
export default {
  props: {
    bgColor: {
      type: String,
      default: '#eee',
    },
    color: {
      type: String,
      default: '#000',
    },
    fontFamily: {
      type: String,
      default: 'Helvetica, sans-serif',
    },
    fontSize: {
      type: String,
      default: '10pt',
    },
    fontWeight: {
      type: String,
      default: 'normal',
    },
    headerBgColor: {
      type: String,
      default: '#333',
    },
    headerBorder: {
      type: String,
      default: 'none',
    },
    headerColor: {
      type: String,
      default: '#fff',
    },
    headerFontFamily: {
      type: String,
      default: 'Helvetica, sans-serif',
    },
    headerFontSize: {
      type: String,
      default: '14pt',
    },
    headerFontWeight: {
      type: String,
      default: 'bold',
    },
    hoverBgColor: {
      type: String,
      default: '#bbb',
    },
    menu: {
      type: Array,
      require: true,
    },
    open: {
      type: Boolean,
      default: false,
    },
    title: {
      type: String,
      default: 'Menu',
    },
    img: {
      type: String,
      default: null,
    },
  },
  data() {
    return {
      currentIndex: null,
    };
  },
  methods: {
    close() {
      this.selectIndex(null);
      this.$emit('update:open', false);
    },
    selectIndex(topLevelIndex) {
      this.currentIndex = topLevelIndex;
    },
    subMenuClass(topLevelIndex) {
      if (this.currentIndex === topLevelIndex) {
        return 'sub-menu';
      }
      return 'sub-menu offscreen';
    },
    backListener() {
      this.close();
    },
    escapeListener(e) {
      if (e.keyCode === 27) { // escape
        this.close();
      }
    },
    tryAddHistoryState() {
      if (window.history.state && window.history.state.vueSlideMenuOpen) {
        return;
      }
      window.history.pushState({ vueSlideMenuOpen: true, ...window.history.state }, '', '');
    },
  },
  computed: {
    font() {
      return `${this.fontWeight} ${this.fontSize} ${this.fontFamily}`;
    },
    headerFont() {
      return `${this.headerFontWeight} ${this.headerFontSize} ${this.headerFontFamily}`;
    },
    headerStyle() {
      return `background-color: ${this.headerBgColor}; color: ${this.headerColor}; font: ${this.headerFont}; display:block;`;
    },
    linkStyle() {
      return `color: ${this.color};`;
    },
    style() {
      return `background-color: ${this.bgColor}; color: ${this.color}; font: ${this.font};`;
    },
    menuClass() {
      if (this.open) {
        return 'vue-slide-menu-container';
      }
      return 'vue-slide-menu-container offscreen';
    },
    topMenuClass() {
      if (this.currentIndex === null) {
        return 'top-menu';
      }
      return 'top-menu offscreen';
    },
  },
  watch: {
    open() {
      if (this.open) {
        this.tryAddHistoryState();
        window.addEventListener('keydown', this.escapeListener);
        window.addEventListener('popstate', this.backListener);
      } else {
        window.removeEventListener('keydown', this.escapeListener);
        window.addEventListener('popstate', this.backListener);
      }
    },
  },
};
</script>

<style>
div.vue-slide-menu-container {
    height: 100vh;
    left: 0;
    position: fixed;
    top: 0;
    transition: left 0.35s;
    width: 100vw;
}
div.vue-slide-menu-container.offscreen {
    height: 100vh;
    left: -300px;
    position: fixed;
    top: 0;
    transition: left 0.35s;
    width: 300px;
}
div.vue-slide-menu-container div.vue-slide-menu {
    background-color: #fff;
    height: 100vh;
    left: 0;
    overflow:hidden;
    position:absolute;
    top: 0;
    width: 300px;
}
div.vue-slide-menu-container div.vue-slide-menu a {
    text-decoration: none;
}
div.vue-slide-menu-container div.vue-slide-menu div.header-bar {
    align-items: center;
    display: flex;
    justify-content: space-between;
    padding: 10px 20px;
}
div.vue-slide-menu-container div.vue-slide-menu a.menu-header {
    cursor: pointer;
    display: block;
    text-align: left;
    top: 0px;
    width: 100%;
}
div.vue-slide-menu-container div.vue-slide-menu a.close-button {
    color: #fff;
    font-size: 48px;
    font-weight: normal;
    position: absolute;
    right: 10px;
    top: -10px;
}
div.vue-slide-menu-container div.vue-slide-menu a.menu-header img {
    flex-grow: 1;
    height: 1em;
    margin-bottom: -0.15em;
    filter: grayscale(1) brightness(100);
}
div.vue-slide-menu-container div.vue-slide-menu div.top-menu {
    left: 0px;
    padding-left: 0px;
    position: absolute;
    transition: left 0.35s;
    width: 300px;
}
div.vue-slide-menu-container div.vue-slide-menu div.top-menu.offscreen {
    left: -300px;
    transition: left 0.35s;
}
div.vue-slide-menu-container div.vue-slide-menu ul {
    list-style: none;
    padding: 5px 0;
}
div.vue-slide-menu-container div.vue-slide-menu ul li.sub-menu-header {
    border-bottom: 1px solid rgba(0, 0, 0, 0.2);
    opacity: .4;
    text-transform: uppercase;
    font-weight: bold;
    padding: 0 0 5px 30px;
}
div.vue-slide-menu-container div.vue-slide-menu ul li.item a {
    box-sizing: border-box;
    cursor: pointer;
    display: block;
    padding: 10px 0px 10px 20px;
    width: 100%;

}div.vue-slide-menu-container div.vue-slide-menu ul li.item a:hover {
    background-color: #0002;
}
div.vue-slide-menu-container div.vue-slide-menu ul li.item a div.right-arrow {
    float: right;
    font-weight: bold;
    padding-right: 20px;
}
div.vue-slide-menu-container div.vue-slide-menu .sub-menu {
    left: 0px;
    padding-left: 0px;
    position: absolute;
    transition: left 0.35s;
    width:100%;
}
div.vue-slide-menu-container div.vue-slide-menu .scrollable {
    box-sizing: border-box;
    height: 100vh;
    overflow-y: scroll;
}
div.vue-slide-menu-container div.vue-slide-menu .sub-menu.offscreen {
    position: absolute;
    left: 300px;
    transition: left 0.35s;
}
div.vue-slide-menu-container div.vue-slide-menu-overlay {
    background-color: #000;
    height: 100vh;
    left: 300px;
    opacity: 0.7;
    position: absolute;
    top: 0;
    transition: opacity 0.6s;
    width: 100vw;
}
div.vue-slide-menu-container.offscreen div.vue-slide-menu-overlay {
    height: 100vh;
    left: 300px;
    opacity: 0;
    position: absolute;
    top: 0;
    transition: opacity 0.6s, width 0s linear 0.6s;
    width: 0;
}
</style>
