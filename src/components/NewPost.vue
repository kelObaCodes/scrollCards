<template>
  <div class="card-holder">
  <pre>
 <code>
interface Post {
id: number;
title: string;
const: string| null;
}

 </code>
 </pre>
 <pre>
 <code>
type NewPost = Post && {
id?: number;

}

 </code>
 </pre>
 <pre>
 <code>
 {{

"const createNewPost = async (newPost: NewPost) => {"
}}


 </code>
 </pre>
  </div>
</template>

<script >
</script>

<style scoped>
pre code{

 background-color: #afeeee;
  border: 1px solid #999;
  display: block;
  padding: 20px;
  width:400px;
}

.card-holder{
display:flex;
flex-flow:column;
align-items:center;
}
</style>
