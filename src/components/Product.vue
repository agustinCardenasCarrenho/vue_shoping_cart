<template>
  <img :src="image" alt="" style="width : 100px">
  <h3>{{name}}</h3>
  <h4>${{price}}</h4>
  <p>{{description}}</p>
</template>

<script>
export default {
    props : ["name" , "description" , "image", "price"]
}
</script>

<style>

</style>