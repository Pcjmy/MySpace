<template>
  <div class="robotList">
    <div class="cardContainer" v-for="robot in robots" :key="robot.id">
      <img :src="robot.url" alt="robot">
      <h2>{{robot.name}}</h2>
      <p>{{robot.email}}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'RobotGallery',
  props: {
    robots: {
      type: Object,
      required: true
    }
  }
}
</script>

<style scoped>
.cardContainer{
  display: flex;
  flex-direction: column;
  background-color: #00BDAB;
  border: 1px solid grey;
  border-radius: 5px;
  padding: 25px;
  cursor: pointer;
  backface-visibility: hidden;
  transform: translateZ(0);
  transition: transform 0.25s ease-out;
}

.cardContainer:hover{
  transform:scale(1.05);
}

.robotList{
  width: 85vw;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-gap: 20px;
  margin: 0 auto;
}
</style>