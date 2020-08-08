<template>
  <div id="employee-form">
    <form @submit.prevent="handleSubmit">
      <label>Employee name</label>
      <input
        ref="first"
        type="text"
        :class="{ 'has-error': submitting && invalidName }"
        v-model="employee.name"
        @focus="clearStatus"
        @keypress="clearStatus"
      />
      <label>Employee Email</label>
      <input
        type="text"
        :class="{ 'has-error': submitting && invalidEmail }"
        v-model="employee.email"
        @focus="clearStatus"
      />
      <p v-if="error && submitting" class="error-message">Please fill out all required fields</p>
      <p v-if="success" class="success-message">Employee successfully added</p>
      <button>Add Employee</button>
    </form>
  </div>
</template>

<script lang="ts">
import { Component, Vue} from 'vue-property-decorator'
@Component
export default class EmployeeForm extends Vue {

    public error: boolean = false
    public submitting: boolean = false
    public success: boolean = false
    public employee: object = {
        name: string =""
        email: string =""
    }

    constructor(parameters) {
    }

    get invalidName(): boolean {
        return this.employee.name === "";
    }

    get invalidEmail(): boolean {
        return this.employee.email === "";
    }

    public handleSubmit(): void {
        this.clearStatus();
        this.submitting = true;
        if (this.invalidName || this.invalidEmail) {
          this.error = true;
          return;
        }
        this.$emit("add:employee", this.employee);
        this.$refs.first.focus();
        this.employee = {
          name: "",
          email: "",
        };
        this.success = true;
        this.error = false;
        this.submitting = false;
    }

    public clearStatus(): void {
        this.success = false;
        this.error = false;
    }
}
</script>

<style scoped>
form {
  margin-bottom: 2rem;
}
[class*="-message"] {
  font-weight: 500;
}
.error-message {
  color: #d33c40;
}
.success-message {
  color: #32a95d;
}
</style>
