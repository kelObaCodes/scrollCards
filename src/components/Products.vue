<template>
  <div class="tab-parent">
    <button
      :class="{ 'active-class': activeType === 'outOfSctock' }"
      @click="getData('outOfSctock')"
    >
      out of stock
    </button>
    <button
      :class="{ 'active-class': activeType === 'outOfSale' }"
      @click="getData('outOfSale')"
    >
      Not on sale
    </button>

    <button
      :class="{ 'active-class': activeType === 'lessThan20' }"
      @click="getData('lessThan20')"
    >
      under $20
    </button>

    <button
      :class="{ 'active-class': activeType === 'mostCommonCategory' }"
      @click="findMostCommonlyUsedCategory(categories, 'mostCommonCategory')"
    >
      Commonly used category
    </button>
    <button
      :class="{ 'active-class': activeType === 'avgSale' }"
      @click="getAverage('avgSale')"
    >
      average price of sales item
    </button>
    <button
      :class="{ 'active-class': activeType === 'womenProduct' }"
      @click="getData('womenProduct')"
    >
      female product out of stock by color
    </button>
  </div>
  <div class="content">
    <table
      v-if="
        activeType === 'lessThan20' ||
          activeType === 'outOfSctock' ||
          activeType === 'outOfSale'
      "
    >
      <thead>
        <th>
          Name
        </th>
        <th>
          Color
        </th>
        <th>
          Price
        </th>
      </thead>
      <tbody>
        <tr v-for="data in arrayDisplayed" :key="data.id">
          <td>{{ data.name }}</td>
          <td>{{ data.color }}</td>
          <td>${{ data.price }}</td>
        </tr>
      </tbody>
    </table>

    <div v-if="activeType === 'commonly'">
      <p>{{ commonlyUsedCategory }}</p>
    </div>
    <div v-if="activeType === 'avgSale'">
      <p>${{ averageAmount.toFixed(2) }}</p>
    </div>
    <div v-if="activeType === 'mostCommonCategory'">
      <p>{{ commonlyUsedCategory }}</p>
    </div>
    <div v-if="activeType === 'womenProduct'">
      <div
        v-for="(data, index) in womenProductsOutOfSctock"
        :key="index"
        class="out-of-stock-women-product"
      >
        <div
          :style="{ background: data.color, 'min-height': 200 }"
          class="color-div"
        >
          {{ data.color }}
        </div>
        <div class="mb-50">
          <h3>Product name</h3>
          <p v-for="(inData, idx) in data" :key="idx">
            {{ inData.name }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import productData from "../assets/products.json";

export default {
  data() {
    return {
      outOfSale: [],
      outOfStock: [],
      under20: [],
      categories: null,
      womenProductsOutOfSctock: [],
      averageAmount: null,
      groupByColor: [],
      arrayDisplayed: [],
      commonlyUsedCategory: null,
      activeType: "",
    };
  },

  methods: {
    findMostCommonlyUsedCategory(category, type) {
      this.activeType = type;
      const categoryObject = {};
      const concatedCategories = [].concat(...category);
      let maxObject = {
        maxNumber: 0,
        maxCategory: "",
      };

      concatedCategories.forEach((prod) => {
        if (categoryObject[prod]) {
          categoryObject[prod]++;
        } else {
          categoryObject[prod] = 1;
        }
      });

      for (let key in categoryObject) {
        if (categoryObject[key] > maxObject.maxNumber) {
          maxObject.maxNumber = categoryObject[key];
          maxObject.maxCategory = key;
        }
      }
      this.commonlyUsedCategory = maxObject.maxCategory;
    },

    getAverage(type) {
      const total = productData.reduce((acc, c) => acc + c.price, 0);
      this.averageAmount = total / productData.length;
      this.activeType = type;
    },

    getWomenOutOfStockByColor(arr) {
      const groupProductsBy = (key) => (array) =>
        array.reduce(
          (objectsByKeyValue, obj) => ({
            ...objectsByKeyValue,
            [obj[key]]: (objectsByKeyValue[obj[key]] || []).concat(obj),
          }),
          {}
        );

      const groupWomenProductByColor = groupProductsBy("color");
      const targetCopy = groupWomenProductByColor(arr);
      let arrayList = [];
      for (let key in targetCopy) {
        let object = {
          color: key,
          ...targetCopy[key],
        };
        arrayList.push(object);
      }

      this.womenProductsOutOfSctock = arrayList;
    },

    getData(type) {
      this.activeType = type;
      let arrayFiltered = [];
      let categoryList = [];

      productData.forEach((prod) => {
        categoryList.push([...new Set(prod.categories)]);
        let formatedPrice;

        if (isNaN(prod.price)) {
          formatedPrice = Number(prod.price.replace("$", ""));
          prod.price = formatedPrice;
        }

        if (
          (!prod.on_sale && type === "outOfSale") ||
          type === "womenProduct"
        ) {
          arrayFiltered.push(prod);
        } else if (!prod.in_stock && type === "outOfSctock") {
          arrayFiltered.push(prod);
        } else if (prod.price < 20 && type === "lessThan20") {
          arrayFiltered.push(prod);
        }
      });
      if (type === "womenProduct") {
        this.getWomenOutOfStockByColor(arrayFiltered);
      } else {
        this.arrayDisplayed = arrayFiltered;
      }
      this.categories = categoryList;
    },
  },

  mounted() {
    this.getData("outOfSctock");
  },
};
</script>

<style scoped>
.tab-parent {
  border: 1px solid #f2f2;
  background-color: #fff;
  min-height: 100px;
  border-radius: 5px;
  display: flex;
  align-items: center;
  justify-content: space-evenly;
   box-shadow: 0px 1px 6px rgb(0 0 0 / 2%);
    background:#fff;
     border: 1px solid #f0f2f3;
}
.active-class,
button {
  background: #afeeee;
  height: 40px;
  border: 1px solid #afeeee;
  border-radius: 5px;
  cursor: pointer;
}
button {
  background: #fff;
}

.content {
  min-height: 100px;
}

.mb-50 {
  margin-bottom: 50px;
}
.color-div {
  display: flex;
  align-items: center;
  height: 200px;
  min-width: 50%;
  justify-content: center;
  font-size: 22px;
}
table{
margin:0 auto;
}
th,
tr {
  text-align: left;
}

.out-of-stock-women-product {
  display: flex;
  justify-content: space-evenly;
}
</style>
