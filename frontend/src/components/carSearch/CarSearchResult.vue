<template>
  <div class="container-fluid">
    <!-- 이미지 불러오기 -->

    <div class="container-fluid__img-box">
      <img :src="getImg(modelInfo.name)" />
    </div>
    <!-- 정보 불러오기-->
    <div class="container-fluid__car-info-box">
      <table class="container-fluid__car-info">
        <!-- <tbody> -->
        <tr>
          <th>모델</th>
          <td>
            <span>{{ modelInfo.name }}</span>
          </td>
        </tr>
        <tr>
          <th>출고가격</th>
          <td>
            <span>
              {{ lowPrice | comma }} ~ {{ highPrice | comma }}(만원)
            </span>
          </td>
        </tr>
        <tr>
          <th>연비</th>
          <td>
            <span>{{ modelInfo.effciency }}km/L</span>
          </td>
        </tr>
        <tr>
          <th>연료</th>
          <td>
            <span>{{ modelInfo.fuel.name }} {{ modelInfo.exhaust }}cc</span>
          </td>
        </tr>
        <tr>
          <th>승차인원</th>
          <td>
            <span>{{ modelInfo.max_person }}명</span>
          </td>
        </tr>
        <tr>
          <th>제조사</th>
          <td>
            <span>현대자동차</span>
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
export default Vue.extend({
  props: {
    modelInfo: {
      type: Object
    }
  },
  data() {
    return {
      carMF: "", //차 제조사
      carSale: "", //차 판매국
      lowPrice: 0,
      highPrice: 0
    };
  },
  filters: {
    comma(value) {
      const num = new Number(value);
      return num.toFixed(0).replace(/(\d)(?=(\d{3})+(?:\.\d+)?$)/g, "$1,");
    }
  },
  created() {
    this.setPrice();
  },
  methods: {
    getImg(mName) {
      //내가 찍은 차 이미지 받아오는 메소드
      let name = mName.split(" ");
      name = name.join("_");
      return require(`@/assets/images/cars/${name}.jpg`);
    },
    addToMyCar() {
      //내 계정 정보에 등록 여부를 보내주는 api 필요 => post...?
      try {
        // const result = await addToMyCar(this.carModel)
        const result = 1;
        if (result == 1) {
          window.alert("관심 목록에 저장했습니다.");
          // this.$router.push('interestCarList');
        } else {
          window.alert(
            "관심 목록에 저장하던 중 오류가 발생했습니다.\n 잠시후 다시 시도해주세요."
          );
        }
      } catch (error) {
        window.alert(error);
        window.alert(
          "관심 목록에 저장하던 중 오류가 발생했습니다.\n 잠시후 다시 시도해주세요."
        );
      }
    },
    setPrice() {
      const low = this.modelInfo.optionList[0].price;
      const high = this.modelInfo.optionList[
        this.modelInfo.optionList.length - 1
      ].price;

      this.lowPrice = low / 10000;
      this.highPrice = high / 10000;
    }
  }
});
</script>

<style></style>
