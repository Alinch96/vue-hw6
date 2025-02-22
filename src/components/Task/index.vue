<template>
  <div class="task-container">
    <p>{{ assignment }}</p>
    <email-input
      v-if="assignment === tasks.TASK_3"
      v-model.auto.check="userEmail"
      domain="@inv.mn.edu"
    />
    <template v-else-if="assignment === tasks.TASK_6">
      <card-input v-model.divide4digits="cardNumber" />
      <div class="date-cvv-container">
        <card-input v-model.expiryDate="expiryDate" />
        <card-input v-model.cvv="cvv" />
      </div>
    </template>
  </div>
</template>

<script>
import CardInput from "./CardInput.vue";
import EmailInput from "./EmailInput.vue";
import { tasks } from "../../constants/assignments_text.js";

export default {
  name: "TaskComponent",
  components: { CardInput, EmailInput },
  props: {
    assignment: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      tasks: tasks,
      userEmail: "",
      cardNumber: "",
      expiryDate: "",
      cvv: "",
    };
  },
};
</script>

<style lang="scss" scoped>
.task-container {
  border: 1px solid #ccc;
  border-radius: 8px;
  background-color: #fefefe;
  padding: 2rem;
  margin: 2rem auto;
  max-width: 600px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
.date-cvv-container {
  display: flex;
  gap: 2rem;
 justify-content: center;
}

.task-container p {
  font-weight: 600;
  margin-bottom: 2rem;
}
</style>
