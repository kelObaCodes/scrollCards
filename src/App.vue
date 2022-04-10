<template>
  <div id="card-container">
<div class="button-container">
<button 
 :class="{'active-class': activeType ==='card'}"
@click="saveType('card')" class="category-button">
Card Visibility
</button>
<button
 :class="{'active-class': activeType ==='watcher'}"
 @click="saveType('watcher')" class="category-button">
Watching an Object
</button>

<button @click="saveType('json')" 
 :class="{'active-class': activeType ==='json'}"

class="category-button">
JSON Manipulation
</button>
<button 
 :class="{'active-class': activeType ==='typescript'}"

@click="saveType('typescript')" class="category-button">
TypeScript Type Definition
</button>
</div>

<template v-if="activeType === 'watcher'">
  <Watchers >

  </Watchers>

</template>
<template v-if="activeType === 'json'">
  <Products />

  </template>
<template v-if="activeType === 'typescript'">
  <Typescript />

  </template>

  <div  v-show="activeType === 'card'">
 <Card
      v-for="(card, id) in mutatedCard"
      :isInFocus="card.focus"
      :cardText="card.text"
      :key="id"
    >
      <p>{{ card }}</p>
    </Card>
  </div>
   
  </div>
</template>

<script>
import Card from "@/components/Card.vue";
import Products from "@/components/Products.vue";
import Watchers from "@/components/Watchers.vue";
import Typescript from "@/components/NewPost.vue";
const cards = Array.from(Array(100), (_, x) => `Card ${x + 1}`);
// Add code to this file to complete the task

const mutatedCard = [];

cards.forEach((card) => {
  let cardItem = {
    text: card,
    focus: false,
  };

  mutatedCard.push(cardItem);
});

export default {
  data() {
    return {
      cards: cards,
      mutatedCard: mutatedCard,
      activeType:'card'
    };
  },
  computed: {
    cardsFromState() {
      return this.mutatedCard;
    },
  },

  components: {
    Card,
    Products,
      Watchers,
      Typescript

  },
methods:{
saveType(type){
this.activeType = type
}
},


  mounted() {
    const callback = (entries) => {
      this.mutatedCard.forEach((card) => {
        entries.forEach((entry) => {
          if (entry.target.textContent === card.text && !entry.isIntersecting) {
            card.focus = false;
          } else if (
            entry.target.textContent &&
            entry.target.textContent === card.text
          ) {
            card.focus = true;
          }
        });
      });
    };

    const options = {
      threshold: 0.4,
    };

    const observer = new IntersectionObserver(callback, options);

    document
      .querySelectorAll(".card")
      .forEach((shape) => observer.observe(shape));
  },
};
</script>

<style>
body{
margin:0;
padding:0;
}

.button-container{
display:flex;
justify-content:space-between;
margin-bottom:50px;
    border: 1px solid #f0f2f3;
    box-shadow: 0px 1px 6px rgb(0 0 0 / 2%);
    background:#fff;
    min-height:100px;
    align-items:center;
    padding:0px 20px;
}


.category-button{

height:50px;
border-radius:10px;
min-width:250px;
transition:.9s;
background:#fff;
border:1px solid #afeeee;
cursor:pointer;

}

.category-button:hover{
transition:.9s;
background:#afeeee;

}
.active-class{
background:#afeeee;

}
</style>
