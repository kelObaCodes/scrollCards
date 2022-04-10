<template>
  <div class="form-parent">
    <label>Product Name</label>
    <input type="text" class="input-class" v-model="product.name" />

    <label>Product Quality</label>
    <select v-model="product.quality" class="input-class">
      <option value="low"> Low</option>
      <option value="medium"> Medium</option>
      <option value="high"> High</option>
    </select>
    <label>Product Size</label>
    <input type="number" v-model="product.size" class="input-class" />

    <div v-if="productObjectAccessed">
      <code>
        <p >{ product object mutated } {{count}}</p>
        <p v-if="objectQualityProperty">
          { [product quality] property accessed } {{qualityCount}}
        </p>

      </code>
    </div>
        <button class="button-class" @click="resetState">Reset State</button>

  </div>
</template>

<script>
export default {
  data() {
    return {
      product: {
        name: "product 1",
        quality: "high",
        size: '',
      },
      objectAccessed: false,
      objectQualityProperty: false,
      count:0,
      qualityCount:0
    };
  },
  computed: {
    productQaulity() {
      return this.product.quality;
    },
    productObjectAccessed() {
      return this.objectAccessed;
    },
  },

  methods: {
    resetState() {
   this.objectAccessed = false
    },
  },
  watch: {
    product: {
      handler: function() {
        this.objectAccessed = true;
        this.count++
      },

      deep: true,
    },
    productQaulity() {
      this.objectQualityProperty = true;
      this.qualityCount++

    },
  },
};
</script>

<style scoped>
.form-parent {
  display: flex;
  flex-flow: column;
    box-shadow: 0px 1px 6px rgb(0 0 0 / 2%);
    background:#fff;
     border: 1px solid #f0f2f3;
     padding:20px;
  max-width: 350px;
  margin:0 auto;
}
.input-class {
  height: 40px;
  margin-bottom: 20px;
}


.button-class{
background:#afeeee;
height:40px;
border:1px solid #afeeee;
border-radius:5px;
cursor:pointer;
}
</style>
