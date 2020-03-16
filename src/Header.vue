<template>
  <div class="sc-header" :style="{background: colors.header.bg, color: colors.header.text}">
    <!--<img class="sc-header&#45;&#45;img" :src="imageUrl" alt="" v-if="imageUrl" />-->
    <div class="sc-header-users-text-container">
      <div class="sc-header--title" @click="toggleUserList"> {{title}} </div>
      <div class="sc-open-drawer-text" @click="openHelpDrawer">Subscribed to {{ numberOfLocations }} location(s)</div>
    </div>
    <div
      class="sc-header--close-button"
      @click.stop="onClose"
      @mousedown.stop=""
      @touchstart.stop=""
    >
      <img src="./assets/close-icon.png" alt="" />
    </div>
  </div>
</template>
<script>

export default {
  props: {
    imageUrl: {
      type: String,
      required: true
    },
    title: {
      type: String
    },
    onClose: {
      type: Function,
      required: true
    },
    openHelpDrawer: {
      type: Function,
      required: true
    },
    numberOfLocations: {
      type: Number,
      required: true
    },
    colors: {
      type: Object,
      required: true
    }
  },
  methods: {
    toggleUserList() {
      this.inUserList = !this.inUserList
      this.$emit("userList", this.inUserList)
    }
  },
  data() {
    return {
      inUserList: false
    }
  }
}
</script>
<style scoped>
.sc-header {
  min-height: 75px;
  border-top-left-radius: 9px;
  border-top-right-radius: 9px;
  padding: 10px;
  box-shadow: 0 1px 4px rgba(0,0,0,.2);
  position: relative;
  box-sizing: border-box;
  display: flex;
  justify-content: space-between;
}

.sc-header--img {
  border-radius: 50%;
  align-self: center;
  padding: 10px;
}

.sc-header--title {
  align-self: center;
  flex: 1;
  user-select: none;
  cursor: pointer;
  border-radius: 5px;
}

.sc-header--title:hover {
  box-shadow: 0px 2px 5px rgba(0.2, 0.2, 0.5, .1);
}

.sc-header--close-button {
  width: 40px;
  align-self: center;
  height: 40px;
  margin-right: 10px;
  box-sizing: border-box;
  cursor: pointer;
  border-radius: 5px;
}

.sc-header--close-button:hover {
  box-shadow: 0px 2px 5px rgba(0.2, 0.2, 0.5, .1);
}

.sc-header--close-button img {
  width: 100%;
  height: 100%;
  padding: 13px;
  box-sizing: border-box;
}

.sc-open-drawer-text {
  color: #0097ff;
  font-size: 12px;
  text-decoration: underline;
}

.sc-open-drawer-text:hover {
  color: white;
  font-size: 12px;
  cursor: pointer;
  text-decoration: underline;
}

.sc-header-users-text-container {
  display: flex;
  flex-direction: column;
  padding: 10px;
}

@media (max-width: 450px) {
  /* mobile media query */
  .sc-header {
    border-radius: 0px;
  }
}
</style>
