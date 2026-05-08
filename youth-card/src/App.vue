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
  background-color: #F5F6FA;
  font-family: -apple-system, BlinkMacSystemFont, 'Helvetica Neue', Helvetica, 'PingFang SC', 'Microsoft YaHei', Arial, sans-serif;
  font-size: 28rpx;
  color: #333333;
  line-height: 1.6;
  box-sizing: border-box;
}

view, text, image, navigator {
  box-sizing: border-box;
}

image {
  vertical-align: middle;
}
</style>
