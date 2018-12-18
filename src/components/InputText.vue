<template>
    <div class="inputtext">
       <input type="text" class="input" :value="value" v-on="inputlistener"/>
    </div>
</template>

<script>
export default {
  name:'InputText',
  props: {
    value: {
      type: String,
      default: ""
    }
  },
  computed: {
    inputlistener() {
      return {
        
        ...this.$listeners,
        
        input: event => this.$emit("input", event.target.value)
      };
    }
  }
};
</script>

<style scoped>

.input {
  width: 30%;
  padding: 8px 10px;
  border: 1px solid #727272;
}
</style>
