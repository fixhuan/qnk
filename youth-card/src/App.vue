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
  background-color: #faf8f5;
  font-family: -apple-system, BlinkMacSystemFont, 'SF Pro Display', 'Helvetica Neue', 'PingFang SC', 'Noto Sans SC', sans-serif;
  font-size: 28rpx;
  color: #1a1612;
  line-height: 1.7;
  box-sizing: border-box;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-feature-settings: 'kern' 1;
  text-rendering: optimizeLegibility;
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
