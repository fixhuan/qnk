<script setup lang="ts">
import { onLaunch, onShow, onHide } from "@dcloudio/uni-app";
onLaunch(() => {
  console.log("App Launch");
});
onShow(() => {
  console.log("App Show");
});
onHide(() => {
  console.log("App Hide");
});
</script>
<style>
page {
  background-color: #0f0f2d;
  font-family: -apple-system, BlinkMacSystemFont, 'Helvetica Neue', Helvetica, 'PingFang SC', 'Microsoft YaHei', Arial, sans-serif;
  font-size: 28rpx;
  color: #FFFFFF;
  line-height: 1.6;
  box-sizing: border-box;
  -webkit-font-smoothing: antialiased;
}
view, text, image, navigator, scroll-view, swiper {
  box-sizing: border-box;
}
image {
  vertical-align: middle;
}
::-webkit-scrollbar {
  display: none;
  width: 0;
  height: 0;
}
</style>
