<script setup lang="ts">
import { ref, onMounted } from "vue";
import refreshImage from './assets/refresh.png';
import Divider from './assets/divider.png';
import Arrow from './assets/arrow.png';
import NotUsed from './assets/not_used.png';

const isConfirmed = ref(false);
const confirmRide = () => {
    isConfirmed.value = true;
    qrcodeColor.value = "#aaaaaa";
};

const currentTime = ref<string>("");

const carTime = ref<string>("13:10");
const start = ref<string>("玉泉校区教二南侧");
const end = ref<string>("紫金港校区");
const carId = ref<string>("教师9号班车");

const qrcodeColor = ref<string>("#3381ff");
// 实现实时更新当前时间
const updateCurrentTime = (): void => {
  const now = new Date();
  const hours = String(now.getHours()).padStart(2, "0");
  const minutes = String(now.getMinutes()).padStart(2, "0");
  const seconds = String(now.getSeconds()).padStart(2, "0");
  currentTime.value = `${hours}:${minutes}:${seconds}`;
};

// 初始化并设置定时器更新时间
onMounted(() => {
  updateCurrentTime();
  setInterval(updateCurrentTime, 1000);
});

const getYearDay = (): string => {
    const now = new Date();
    // YYYY年mm月dd日
    const year = now.getFullYear();
    const month = String(now.getMonth() + 1).padStart(2, "0");
    const day = String(now.getDate()).padStart(2, "0");

    return `${year}年${month}月${day}日`;
};

const getDay = (): string => {
    const now = new Date();
    // yy月dd日
    const month = String(now.getMonth() + 1).padStart(2, "0");
    const day = String(now.getDate()).padStart(2, "0");

    return `${month}月${day}日`;
};

const openModal = ref(false);
const handleClick = () => {
    openModal.value = true;
};

const handleOk = () => {
    openModal.value = false;
};
</script>

<template>
    <div id="app">
        <a-card :bordered="false" style="height:fit-content; position: relative;">
            <p style="font-size: 25px; font-weight:450 ;text-align: left; margin-left: 10px; margin-top: 10px; margin-bottom: 5px;">{{getYearDay()}}
                <img :src="NotUsed" alt="" style=" width: 50px; height: auto; margin-left: 0px;"/>
            </p>
            <div style="display: flex; justify-content: space-between; align-items: center; margin: 15px; margin-top: 0; margin-bottom: 10px;">
                <div style="width: 33%;">
                    <p style="font-weight: bold; font-size: 30px; letter-spacing: -1px; margin-bottom: 0;">{{ carTime }}</p>
                    <p style="font-size: 18px; margin-bottom: 0;">{{ start }}</p>
                </div>
                <div>
                    <p style="margin-bottom: 0; color: gray; font-weight: 400;">{{ carId }}</p>
                    <img :src="Arrow" alt="" style=" width: 150px; height: auto; margin-top: 0;"/>
                </div>
                <p style="font-size: 18px; margin-bottom: 0;">{{ end }}</p>
            </div>


            <img :src="Divider" alt="" style="display: block; width: calc(100vw - 26px); height: auto;"/>
            <img :src="refreshImage" alt="刷新" class="right-image" @click="handleClick"/>
            <p class="time-text">{{ getDay() }}</p>
            <p class="time-text">{{ currentTime }}</p>

            <p class="green-text">当前时段趟次</p>
            <!-- 显示二维码 -->
            <a-qrcode value="http://www.antdv.com" :color="qrcodeColor" style="margin: auto; margin-bottom: 10px; height: 150px; width: 150px;" :size="180"/>

            <!-- 确认按钮 -->
            <a-button type="primary" :disabled="isConfirmed" style="height: 48px; width: 90%; font-size: 20px;" @click="confirmRide"
                block>
                {{ isConfirmed ? "已确认" : "确认上车" }}
            </a-button>

            <a-typography-paragraph class="bottom-text">
                如有三次违约将禁止您使用此预约服务
            </a-typography-paragraph>
        </a-card>

        <a-modal v-model:open="openModal" title="编辑信息" @ok="handleOk">
            发车时间<a-input v-model:value="carTime" :placeholder="carTime" />
            起点<a-input v-model:value="start" :placeholder="start" />
            终点<a-input v-model:value="end" :placeholder="end" />
            班车号<a-input v-model:value="carId" :placeholder="carId" />
        </a-modal>
    </div>
</template>

<style scoped>
#app {
    font-family: "Microsoft YaHei";
    text-align: center;
    height: 100vh;
    background: #f1f1f1;
    padding: 13px;
}
.time-text {
    text-align: center;
    font-weight: bold;
    line-height: 40px;
    letter-spacing: 1px;
    font-size: 35px;
    margin-bottom: 0px;
    letter-spacing: -0.5px;
}
.green-text {
    color: #61a969;
    font-weight: 600;
    line-height: 40px;
    letter-spacing: 1px;
    font-size: 35px;
    margin-bottom: 10px;
    margin-top: 5px;
}

.bottom-text {
    text-align: center;
    font-size: 13px;
    color: #a8a8a8;
    font-weight: bold;
    margin-top: 20px;
    margin-bottom: 50px;
}

.right-image {
  display: block;             /* 图片单独占一行 */
  margin-left: auto;          /* 将图片推到右边 */
  margin-right: 5px;
  width: 75px;               /* 控制图片宽度 */
  height: auto;               /* 保持图片比例，自动调整高度 */
}

::v-deep .ant-card-body {
  padding: 0 !important;
}
</style>
