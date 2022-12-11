<template>
  <ContentBase>
    <RobotGallery :robots="robots"/>
  </ContentBase>
</template>

<script>
import ContentBase from '../components/ContentBase'
import RobotGallery from '../components/RobotGallery'
import robots from '../utils/robots.json'

export default {
  name: 'HomeView',
  components: {
    ContentBase,
    RobotGallery
  },
  setup() {
    for(let robot of robots) {
      robot.url = 'https://robohash.org/'+robot.id;
    }
    return {
      robots
    }
  }
}
</script>

<style scoped>
</style>
