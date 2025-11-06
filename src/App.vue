<template>
  <div id="app">
    <!--<user-view></user-view>-->
    <!--<question-view></question-view>-->
    <router-view></router-view>
  </div>
</template>

<script>
//import QuestionView from './views/element/QuestionView.vue'
//import UserView from './views/element/UserView.vue'
export default {
  //components: { QuestionView },
  data() {
    return {
      message: "hello vue",
    };
  },
  methods: {},
};
</script>

<style>
</style>
