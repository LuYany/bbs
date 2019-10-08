<template>
  <div class>
      <div class="header">
          <text class="txt">logo</text>
          <text class="txt">关于</text>
      </div>
  </div>
</template>

<script>
export default {
  name: "Header",
  data() {
    return {};
  }
};
</script>

<style scoped>
    .header{
        background-color: #5a5555;
        flex-direction: row;
        justify-content: space-between;
        padding-left: 35px;
        padding-right: 35px;
        padding-top: 15px;
        padding-bottom: 15px;
    }
    .txt{
        font-size: 30px;
        font-weight: 500;
        color: #cbc9c9;
    }
</style>
