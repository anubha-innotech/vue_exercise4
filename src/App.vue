<template>
<h2>Exercise 4: Form</h2>
<SignupForm />
</template>

<script>
import SignupForm from './components/SignupForm.vue'

export default {
  name: 'App',
  components: {
    SignupForm
  }
}
</script>

<style>
* {
  margin: 0px;
  padding: 0;
}
body{
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;

}

</style>
