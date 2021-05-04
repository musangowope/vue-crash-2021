<template>
  <header>
    <h1>{{ title }}</h1>
    <Button
        :text="showAddTask ? 'Close' : 'Add'"
        :color="showAddTask ? 'red' : 'green'"
        @btn-click="$emit('toggle-add-task')"
    />

  </header>

</template>

<script>
import Button from "./Button";

export default {
  name: "Header",
  props: {
    title: {
      type: String,
      default: "",
    },
    showAddTask: {
      type: Boolean,
      default: false,
    },
  },
  components: {
    Button,
  },
};
</script>
<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
</style>
