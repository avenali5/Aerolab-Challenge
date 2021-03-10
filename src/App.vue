<template>
  <div id="app">
    <Challenge/>
    <Modal/>
    <AddPoints/>
  </div>
</template>

<script>
import Challenge from './components/Challenge.vue'
import Modal from './components/Modal.vue'
import AddPoints from './components/AddPoints.vue'

export default {
  components:{
    Challenge,
    Modal,
    AddPoints
  }
}
</script>

<style lang="scss">
*{margin: 0;padding: 0;box-sizing: border-box; font-family: 'Open Sans', sans-serif;}
.flex{
  display:flex;
  align-items: center;
}
.relative{
  position:relative;
}
.absolute{
  position:absolute;
}
.max-width{
  position:relative;
  left:50%;
  transform:translateX(-50%);
  max-width:65rem;
}
ul{
  list-style-type: none;
}
a{
  text-decoration: none;
}
.open{
  display:block !important;
}
</style>
