<template>
  <div class="upper-middle">
    <div align="right" class="upper-button">
      <v-btn
        class="ma-2"
        :icon="icon"
        color="#f0f0f0"
        size="small"
        @click="drawMapSheet"
      ></v-btn>
    </div>
    <div id="map" style="width: 100%; height: 100%"></div>
  </div>
</template>

<style scoped>
.upper-middle {
  flex: 2; /* 占据 2/6 = 1/3 的高度 */
  width: 900px;
  background-color: #faf8f8; /* 灰色背景 */
  margin-bottom: 20px; /* 留有空隙 */
  box-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
  position: relative;
}

.upper-button {
  position: absolute;
  width: 100%;
  right: 0;
  top: 0;
  z-index: 114;
}
</style>

<script setup lang="ts">
import * as echarts from 'echarts'
import { onMounted, ref } from 'vue'
import 'echarts/map/js/china.js'

// 省份地理坐标数据
const geoCoordMap = {
  上海: [121.4648, 31.2891],
  东莞: [113.8953, 22.901],
  东营: [118.7073, 37.5513],
  中山: [113.4229, 22.478],
  临汾: [111.4783, 36.1615],
  临沂: [118.3118, 35.2936],
  丹东: [124.541, 40.4242],
  丽水: [119.5642, 28.1854],
  乌鲁木齐: [87.9236, 43.5883],
  佛山: [112.8955, 23.1097],
  保定: [115.0488, 39.0948],
  兰州: [103.5901, 36.3043],
  包头: [110.3467, 41.4899],
  北京: [116.4551, 40.2539],
  北海: [109.314, 21.6211],
  南京: [118.8062, 31.9208],
  南宁: [108.479, 23.1152],
  南昌: [116.0046, 28.6633],
  南通: [121.1023, 32.1625],
  厦门: [118.1689, 24.6478],
  台州: [121.1353, 28.6688],
  合肥: [117.29, 32.0581],
  呼和浩特: [111.4124, 40.4901],
  咸阳: [108.4131, 34.8706],
  哈尔滨: [127.9688, 45.368],
  唐山: [118.4766, 39.6826],
  嘉兴: [120.9155, 30.6354],
  大同: [113.7854, 39.8035],
  大连: [122.2229, 39.4409],
  天津: [117.4219, 39.4189],
  太原: [112.3352, 37.9413],
  威海: [121.9482, 37.1393],
  宁波: [121.5967, 29.6466],
  宝鸡: [107.1826, 34.3433],
  宿迁: [118.5535, 33.7775],
  常州: [119.4543, 31.5582],
  广州: [113.5107, 23.2196],
  廊坊: [116.521, 39.0509],
  延安: [109.1052, 36.4252],
  张家口: [115.1477, 40.8527],
  徐州: [117.5208, 34.3268],
  德州: [116.6858, 37.2107],
  惠州: [114.6204, 23.1647],
  成都: [103.9526, 30.7617],
  扬州: [119.4653, 32.8162],
  承德: [117.5757, 41.4075],
  拉萨: [91.1865, 30.1465],
  无锡: [120.3442, 31.5527],
  日照: [119.2786, 35.5023],
  昆明: [102.9199, 25.4663],
  杭州: [119.5313, 29.8773],
  枣庄: [117.323, 34.8926],
  柳州: [109.3799, 24.9774],
  株洲: [113.5327, 27.0319],
  武汉: [114.3896, 30.6628],
  汕头: [117.1692, 23.3405],
  江门: [112.6318, 22.1484],
  沈阳: [123.1238, 42.1216],
  沧州: [116.8286, 38.2104],
  河源: [114.917, 23.9722],
  泉州: [118.3228, 25.1147],
  泰安: [117.0264, 36.0516],
  泰州: [120.0586, 32.5525],
  济南: [117.1582, 36.8701],
  济宁: [116.8286, 35.3375],
  海口: [110.3893, 19.8516],
  淄博: [118.0371, 36.6064],
  淮安: [118.927, 33.4039],
  深圳: [114.5435, 22.5439],
  清远: [112.9175, 24.3292],
  温州: [120.498, 27.8119],
  渭南: [109.7864, 35.0299],
  湖州: [119.8608, 30.7782],
  湘潭: [112.5439, 27.7075],
  滨州: [117.8174, 37.4963],
  潍坊: [119.0918, 36.524],
  烟台: [120.7397, 37.5128],
  玉溪: [101.9312, 23.8898],
  珠海: [113.7305, 22.1155],
  盐城: [120.2234, 33.5577],
  盘锦: [121.9482, 41.0449],
  石家庄: [114.4995, 38.1006],
  福州: [119.4543, 25.9222],
  秦皇岛: [119.2126, 40.0232],
  绍兴: [120.564, 29.7565],
  聊城: [115.9167, 36.4032],
  肇庆: [112.1265, 23.5822],
  舟山: [122.2559, 30.2234],
  苏州: [120.6519, 31.3989],
  莱芜: [117.6526, 36.2714],
  菏泽: [115.6201, 35.2057],
  营口: [122.4316, 40.4297],
  葫芦岛: [120.1575, 40.578],
  衡水: [115.8838, 37.7161],
  衢州: [118.6853, 28.8666],
  西宁: [101.4038, 36.8207],
  西安: [109.1162, 34.2004],
  贵阳: [106.6992, 26.7682],
  连云港: [119.1248, 34.552],
  邢台: [114.8071, 37.2821],
  邯郸: [114.4775, 36.535],
  郑州: [113.4668, 34.6234],
  鄂尔多斯: [108.9734, 39.2487],
  重庆: [107.7539, 30.1904],
  金华: [120.0037, 29.1028],
  铜川: [109.0393, 35.1947],
  银川: [106.3586, 38.1775],
  镇江: [119.4763, 31.9702],
  长春: [125.8154, 44.2584],
  长沙: [113.0823, 28.2568],
  长治: [112.8625, 36.4746],
  阳泉: [113.4778, 38.0951],
  青岛: [120.4651, 36.3373],
  韶关: [113.7964, 24.7028],
  广南县: [105.0928, 24.0465],
  南浔区镇: [120.4179, 30.8781],
  广东省湛江市: [110.3589, 21.2713],
  富宁县: [105.6305, 23.6254],
  遂溪县: [110.2501, 21.3771],
  板蚌乡: [105.478, 23.9174],
  八宝镇: [106.7109, 31.0597],
  文山市: [104.2151, 23.3985],
  黔西县: [106.0323, 27.0087],
  乌兰察布: [113.1338, 40.9939],
  锡林郭勒: [116.0478, 43.9332],
  阿拉善盟: [105.7286, 38.8515],
  库尔勒市: [86.1736, 41.7264],
  吐鲁番市: [89.1895, 42.9513],
  克拉玛依市: [84.8769, 45.5798],
  阿勒泰市: [88.1403, 47.8457],
  五家渠市: [87.54, 44.1674],
  阿拉尔: [81.2806, 40.5478],
  图木舒克市: [79.069, 39.8673],
  日喀则市: [88.881109, 29.267518],
  林芝市: [94.361484, 29.648952],
  昌都市: [97.172205, 31.140969],
  海东市: [102.4017, 36.4823],
  海北藏族自治州: [100.9018, 36.9545],
  黄南藏族自治州: [102.019, 35.5175],
  海南藏族自治州: [100.6267, 36.2921],
  天水: [105.724998, 34.580864],
  白银: [104.13773, 36.544756],
  金昌: [102.188043, 38.520089],
  嘉峪关: [98.289152, 39.77313],
  武威: [102.63797, 37.928265],
  庆阳: [107.642921, 35.709077]
}

// 预定义航线数据
const preDefinedFlightData = [
  [{ name: '上海' }, { name: '北京', value: 3 }],
  [{ name: '北京' }, { name: '广州', value: 5 }],
  [{ name: '广州' }, { name: '上海', value: 4 }],
  [{ name: '上海' }, { name: '深圳', value: 2 }],
  [{ name: '上海' }, { name: '杭州', value: 2 }],
  [{ name: '广州' }, { name: '杭州', value: 3 }],
  [{ name: '杭州' }, { name: '北京', value: 2 }],
  [{ name: '北京' }, { name: '深圳', value: 4 }],
  [{ name: '深圳' }, { name: '上海', value: 3 }],
  [{ name: '上海' }, { name: '重庆', value: 2 }],
  [{ name: '重庆' }, { name: '成都', value: 3 }],
  [{ name: '成都' }, { name: '上海', value: 4 }],
  [{ name: '上海' }, { name: '武汉', value: 2 }],
  [{ name: '武汉' }, { name: '南京', value: 3 }],
  [{ name: '南京' }, { name: '上海', value: 4 }],
  [{ name: '上海' }, { name: '西安', value: 3 }],
  [{ name: '西安' }, { name: '成都', value: 2 }],
  [{ name: '成都' }, { name: '广州', value: 3 }],
  [{ name: '广州' }, { name: '武汉', value: 2 }],
  [{ name: '武汉' }, { name: '深圳', value: 3 }],
  [{ name: '深圳' }, { name: '广州', value: 2 }],
  [{ name: '上海' }, { name: '南京', value: 2 }],
  [{ name: '南京' }, { name: '北京', value: 3 }],
  [{ name: '北京' }, { name: '上海', value: 4 }],
  [{ name: '上海' }, { name: '杭州', value: 2 }],
  [{ name: '杭州' }, { name: '广州', value: 3 }],
  [{ name: '广州' }, { name: '深圳', value: 4 }],
  [{ name: '深圳' }, { name: '北京', value: 2 }],
  [{ name: '北京' }, { name: '重庆', value: 3 }],
  [{ name: '重庆' }, { name: '成都', value: 2 }],
  [{ name: '成都' }, { name: '上海', value: 3 }],
  [{ name: '上海' }, { name: '武汉', value: 4 }],
  [{ name: '武汉' }, { name: '南京', value: 2 }],
  [{ name: '南京' }, { name: '上海', value: 3 }],
  [{ name: '上海' }, { name: '西安', value: 2 }],
  [{ name: '西安' }, { name: '成都', value: 3 }],
  [{ name: '成都' }, { name: '广州', value: 4 }],
  [{ name: '广州' }, { name: '武汉', value: 2 }],
  [{ name: '武汉' }, { name: '深圳', value: 3 }],
  [{ name: '深圳' }, { name: '广州', value: 2 }],
  [{ name: '广州' }, { name: '南京', value: 3 }],
  [{ name: '南京' }, { name: '上海', value: 2 }],
  [{ name: '广州' }, { name: '西宁', value: 1 }],
  [{ name: '西宁' }, { name: '成都', value: 2 }],
  [{ name: '成都' }, { name: '重庆', value: 1 }],
  [{ name: '重庆' }, { name: '贵阳', value: 2 }],
  [{ name: '贵阳' }, { name: '南宁', value: 1 }],
  [{ name: '南宁' }, { name: '拉萨', value: 2 }],
  [{ name: '拉萨' }, { name: '乌鲁木齐', value: 1 }],
  [{ name: '乌鲁木齐' }, { name: '银川', value: 2 }],
  [{ name: '银川' }, { name: '呼和浩特', value: 1 }],
  [{ name: '呼和浩特' }, { name: '沈阳', value: 2 }],
  [{ name: '沈阳' }, { name: '长春', value: 1 }],
  [{ name: '长春' }, { name: '哈尔滨', value: 2 }],
  [{ name: '哈尔滨' }, { name: '济南', value: 1 }],
  [{ name: '济南' }, { name: '郑州', value: 2 }],
  [{ name: '郑州' }, { name: '合肥', value: 1 }],
  [{ name: '合肥' }, { name: '南京', value: 2 }],
  [{ name: '南京' }, { name: '杭州', value: 1 }],
  [{ name: '杭州' }, { name: '福州', value: 2 }],
  [{ name: '福州' }, { name: '台北', value: 1 }],
  [{ name: '台北' }, { name: '香港', value: 2 }],
  [{ name: '香港' }, { name: '澳门', value: 1 }],
  [{ name: '克拉玛依市' }, { name: '阿勒泰市', value: 2 }],
  [{ name: '阿勒泰市' }, { name: '五家渠市', value: 3 }],
  [{ name: '五家渠市' }, { name: '阿拉尔', value: 4 }],
  [{ name: '阿拉尔' }, { name: '图木舒克市', value: 2 }],
  [{ name: '图木舒克市' }, { name: '日喀则市', value: 3 }],
  [{ name: '日喀则市' }, { name: '林芝市', value: 4 }],
  [{ name: '林芝市' }, { name: '昌都市', value: 2 }],
  [{ name: '昌都市' }, { name: '海东市', value: 3 }],
  [{ name: '海东市' }, { name: '海北藏族自治州', value: 4 }],
  [{ name: '海北藏族自治州' }, { name: '黄南藏族自治州', value: 2 }],
  [{ name: '黄南藏族自治州' }, { name: '海南藏族自治州', value: 3 }],
  [{ name: '长沙' }, { name: '长治', value: 2 }],
  [{ name: '长治' }, { name: '阳泉', value: 3 }],
  [{ name: '阳泉' }, { name: '青岛', value: 4 }],
  [{ name: '青岛' }, { name: '韶关', value: 2 }],
  [{ name: '韶关' }, { name: '广南县', value: 3 }],
  [{ name: '广南县' }, { name: '南浔区镇', value: 4 }],
  [{ name: '南浔区镇' }, { name: '广东省湛江市', value: 2 }],
  [{ name: '广东省湛江市' }, { name: '富宁县', value: 3 }],
  [{ name: '富宁县' }, { name: '遂溪县', value: 4 }],
  [{ name: '遂溪县' }, { name: '板蚌乡', value: 2 }],
  [{ name: '板蚌乡' }, { name: '八宝镇', value: 3 }],
  [{ name: '八宝镇' }, { name: '文山市', value: 4 }],
  [{ name: '文山市' }, { name: '黔西县', value: 2 }],
  [{ name: '黔西县' }, { name: '乌兰察布', value: 3 }],
  [{ name: '乌兰察布' }, { name: '呼和浩特', value: 4 }],
  [{ name: '呼和浩特' }, { name: '包头', value: 2 }],
  [{ name: '包头' }, { name: '鄂尔多斯', value: 3 }],
  [{ name: '鄂尔多斯' }, { name: '锡林郭勒', value: 4 }],
  [{ name: '锡林郭勒' }, { name: '阿拉善盟', value: 2 }],
  [{ name: '阿拉善盟' }, { name: '库尔勒市', value: 3 }],
  [{ name: '八宝镇' }, { name: '文山市', value: 2 }],
  [{ name: '文山市' }, { name: '黔西县', value: 3 }],
  [{ name: '黔西县' }, { name: '乌兰察布', value: 4 }],
  [{ name: '乌兰察布' }, { name: '呼和浩特', value: 2 }],
  [{ name: '呼和浩特' }, { name: '包头', value: 3 }],
  [{ name: '包头' }, { name: '鄂尔多斯', value: 4 }],
  [{ name: '鄂尔多斯' }, { name: '锡林郭勒', value: 2 }],
  [{ name: '锡林郭勒' }, { name: '阿拉善盟', value: 3 }],
  [{ name: '阿拉善盟' }, { name: '库尔勒市', value: 4 }],
  [{ name: '库尔勒市' }, { name: '吐鲁番市', value: 2 }],
  [{ name: '吐鲁番市' }, { name: '克拉玛依市', value: 3 }],
  [{ name: '克拉玛依市' }, { name: '阿勒泰市', value: 4 }],
  [{ name: '阿勒泰市' }, { name: '五家渠市', value: 2 }],
  [{ name: '五家渠市' }, { name: '阿拉尔', value: 3 }],
  [{ name: '阿拉尔' }, { name: '图木舒克市', value: 4 }],
  [{ name: '图木舒克市' }, { name: '日喀则市', value: 2 }],
  [{ name: '日喀则市' }, { name: '林芝市', value: 3 }],
  [{ name: '林芝市' }, { name: '昌都市', value: 4 }],
  [{ name: '昌都市' }, { name: '海东市', value: 2 }],
  [{ name: '海东市' }, { name: '海北藏族自治州', value: 3 }],
  [{ name: '海北藏族自治州' }, { name: '黄南藏族自治州', value: 4 }],
  [{ name: '北海' }, { name: '南京', value: 3 }],
  [{ name: '南京' }, { name: '南宁', value: 5 }],
  [{ name: '南宁' }, { name: '南昌', value: 4 }],
  [{ name: '南昌' }, { name: '南通', value: 2 }],
  [{ name: '南通' }, { name: '厦门', value: 2 }],
  [{ name: '厦门' }, { name: '台州', value: 3 }],
  [{ name: '台州' }, { name: '合肥', value: 2 }],
  [{ name: '合肥' }, { name: '贵阳', value: 4 }],
  [{ name: '贵阳' }, { name: '连云港', value: 3 }],
  [{ name: '连云港' }, { name: '邢台', value: 2 }],
  [{ name: '邢台' }, { name: '邯郸', value: 3 }],
  [{ name: '邯郸' }, { name: '郑州', value: 4 }],
  [{ name: '郑州' }, { name: '鄂尔多斯', value: 2 }],
  [{ name: '鄂尔多斯' }, { name: '重庆', value: 3 }],
  [{ name: '重庆' }, { name: '金华', value: 4 }],
  [{ name: '金华' }, { name: '铜川', value: 2 }],
  [{ name: '铜川' }, { name: '银川', value: 3 }],
  [{ name: '银川' }, { name: '镇江', value: 4 }],
  [{ name: '镇江' }, { name: '北海', value: 2 }]
]

const maxProbability = 10

// 定义特定省份的城市
const highProbabilityProvinces = [
  '河南',
  '江苏',
  '山东',
  '福建',
  '河北',
  '四川',
  '广东',
  '安徽'
]
var predefinedProbabilities = {
  上海: 0.016666667,
  东莞: 0.032258065,
  东营: 0.166666667,
  中山: 0.034482759,
  临汾: 0.0714285714,
  临沂: 0.131578947,
  丹东: 0.076923077,
  丽水: 0.222222222,
  乌鲁木齐: 0.285714286,
  佛山: 0.1,
  保定: 0.0689655172413793,
  兰州: 0.016666667,
  包头: 0.571428571,
  北京: 0.125,
  北海: 0.142857143,
  南京: 0.066666667,
  南宁: 0.571428571,
  南昌: 0.02173913,
  南通: 0.043478261,
  厦门: 0.142857143,
  台州: 0.083333333,
  合肥: 0.142857143,
  呼和浩特: 0.285714286,
  咸阳: 0.333333333,
  哈尔滨: 0.083333333,
  唐山: 0.166666667,
  嘉兴: 0.428571429,
  大同: 0.380952381,
  大连: 0.166666667,
  天津: 0.333333333,
  太原: 0.068965517,
  威海: 0.034482759,
  宁波: 0.034482759,
  宝鸡: 0.083333333,
  宿迁: 0.352941176,
  常州: 0.058823529,
  广州: 0.333333333,
  廊坊: 0.384615385,
  延安: 0.076923077,
  张家口: 0.083333333,
  徐州: 0.333333333,
  德州: 0.142857143,
  惠州: 0.25,
  成都: 0.333333333,
  扬州: 0.166666667,
  承德: 0.142857143,
  拉萨: 0.083333333,
  无锡: 0.071428571,
  日照: 0.090909091,
  昆明: 0.090909091,
  杭州: 0.333333333,
  枣庄: 0.111111111,
  柳州: 0.0625,
  株洲: 0.166666667,
  武汉: 0.142857143,
  汕头: 0.666666667,
  江门: 0.333333333,
  沈阳: 0.088235294,
  沧州: 0.277777778,
  河源: 0.285714286,
  泉州: 0.333333333,
  泰安: 0.333333333,
  泰州: 0.285714286,
  济南: 0.333333333,
  济宁: 0.666666667,
  海口: 0.166666667,
  淄博: 0.176470588,
  淮安: 0.058823529,
  深圳: 0.111111111,
  清远: 0.142857143,
  温州: 0.142857143,
  渭南: 0.333333333,
  湖州: 0.055555556,
  湘潭: 0.333333333,
  滨州: 0.285714286,
  潍坊: 0.142857143,
  烟台: 0.666666667,
  玉溪: 0.333333333,
  珠海: 0.0689655172413793,
  盐城: 0.016666667,
  盘锦: 0.032258065,
  石家庄: 0.166666667,
  福州: 0.034482759,
  秦皇岛: 0.0714285714,
  绍兴: 0.076923077,
  聊城: 0.285714286,
  肇庆: 0.571428571,
  舟山: 0.043478261,
  苏州: 0.166666667,
  莱芜: 0.166666667
  // 其他城市的概率值
} // 设置随机概率值
// 设置随机概率值
var sectorValues = {}
for (var city in geoCoordMap) {
  if (predefinedProbabilities.hasOwnProperty(city)) {
    sectorValues[city] = predefinedProbabilities[city]
  } else {
    sectorValues[city] = 0 // 如果没有预定义的概率，可以设置为默认值
  }
}

var dataColor = {}
for (const city in sectorValues) {
  const freq = sectorValues[city]
  const darkeningFactor = (freq / maxProbability) * 0.5
  dataColor[city] =
    'rgb(' +
    (100 + darkeningFactor * 155) +
    ', ' +
    (149 + darkeningFactor * 106) +
    ', 255)'
}

// 转换航线数据
const convertData = (data) => {
  let result = []
  for (var i = 0; i < data.length; i++) {
    var dataItem = data[i]
    var fromCoord = geoCoordMap[dataItem[0].name] // 出发地的经纬度
    var toCoord = geoCoordMap[dataItem[1].name] // 目的地的经纬度
    if (fromCoord && toCoord) {
      result.push({
        fromName: dataItem[0].name,
        toName: dataItem[1].name,
        coords: [fromCoord, toCoord],
        value: dataItem[1].value
      })
    }
  }
  return result
}

var option = {
  title: {
    text: '妇女儿童失踪路线图',
    subtext: '数据来源于真实案例',
    left: 'center',
    textStyle: {
      color: 'black'
    }
  },
  tooltip: {
    trigger: 'item'
  },
  legend: {
    orient: 'vertical',
    top: 'bottom',
    left: 'right',
    data: ['北京 Top10', '上海 Top10', '广州 Top10'],
    textStyle: {
      color: 'black'
    },
    selectedMode: 'single'
  },
  visualMap: {
    min: 15,
    max: 50, // 调整最大值
    calculable: true,
    inRange: {
      color: ['lightskyblue', 'orangered'] // 调整颜色范围
    },
    textStyle: {
      color: '#fff'
    }
  },
  geo: {
    map: 'china',
    label: {
      emphasis: {
        // 鼠标触发显示地区名称
        show: true
      }
    },
    roam: true,
    itemStyle: {
      normal: {
        // 正常时地区的颜色
        areaColor: '#f2e8e8',
        borderColor: '#404a59'
      },
      emphasis: {
        // 鼠标经过时地区的颜色
        areaColor: '#c99979'
      }
    }
  },
  series: []
}

// 模拟航线
const simulateFlight = (flightData) => {
  const newSeries = flightData.map((dataItem) => {
    const color = '#a6c84c' // 保持原来的颜色

    return {
      name: dataItem[0].name + ' → ' + dataItem[1].name,
      type: 'lines',
      effect: {
        show: true,
        period: 6,
        trailLength: 0.7,
        color: '#fff',
        symbolSize: 3
      },
      lineStyle: {
        normal: {
          color: color,
          width: 4, // 设置线的宽度更粗
          curveness: 0.2
        }
      },
      data: convertData([dataItem])
    }
  })

  // 添加显示城市信息的散点图
  newSeries.push({
    type: 'scatter',
    coordinateSystem: 'geo',
    zlevel: 4,
    label: {
      normal: {
        show: true,
        formatter: '{b}',
        position: 'right',
        offset: [5, 0],
        fontSize: 10 // 调整城市字体大小
      }
    },
    data: Object.keys(geoCoordMap).map((city) => ({
      name: city,
      value: geoCoordMap[city]
    }))
  })
  return newSeries
}

const scatterSeries = [
  {
    type: 'scatter',
    coordinateSystem: 'geo',
    zlevel: 4,
    label: {
      normal: {
        show: true,
        formatter: '{b}',
        position: 'right',
        offset: [5, 0],
        fontSize: 10
      }
    },
    data: Object.keys(sectorValues).map(function (city) {
      var freq = sectorValues[city]
      var color = dataColor[city]

      return {
        name: city,
        value: geoCoordMap[city],
        symbolSize: freq * 50,
        itemStyle: {
          color: color
        }
      }
    })
  }
]

const scatterMode = ref(false)
const icon = ref('mdi-menu')

let myChart = null

onMounted(() => {
  drawMapSheet()
})

const drawMapSheet = () => {
  try {
    myChart.dispose()
  } catch {}
  myChart = echarts.init(document.getElementById('map'))
  if (scatterMode.value) {
    option.title.text = '妇女儿童失踪路线图'
    option.series = simulateFlight(preDefinedFlightData)
    icon.value = 'mdi-scatter-plot'
  } else {
    option.title.text = '妇女儿童失踪概率预测图'
    option.series = scatterSeries
    icon.value = 'mdi-transit-detour'
  }
  myChart.setOption(option)
  scatterMode.value = !scatterMode.value
}
</script>
