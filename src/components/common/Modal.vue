<template>
  <transition
    enter-active-class="animate__animated animate__fadeIn"
    leave-active-class="animate__animated animate__fadeOut"
  >
    <section v-if="modal.isOpen" class="modal">
      <form @submit.prevent="updateIssueData" class="form form--modal">
        <button type="button" @click="closeModal" v-on:click="removeError" class="form__exit">
          <font-awesome-icon icon="times-circle" />
        </button>
        <h2 class="form__title">Update issue</h2>

        <label class="form__label">
          Issue description:
          <textarea
            v-model="modal.issueDescription"
            name="modalIssueDescription"
            class="form__input"
            placeholder="I had an issue with..."
          ></textarea>
        </label>

        <button type="submit" class="form__button form__button--update">Update issue</button>

        <p class="form__error" v-if="errorMessage">{{ errorMessage }}</p>
      </form>
    </section>
  </transition>
</template>

<script>
import { mapGetters, mapActions } from 'vuex';

export default {
  name: 'Modal',
  methods: {
    ...mapActions(['updateIssue', 'openModal', 'closeModal', 'showError', 'removeError']),
    updateIssueData() {
      if (this.modal.issueDescription) {
        this.removeError();

        this.updateIssue(this.modal);
        this.closeModal();
      } else {
        this.showError();
      }
    }
  },
  computed: mapGetters(['modal', 'errorMessage'])
};
</script>

<style scoped>
.modal {
  display: flex;
  justify-content: center;
  align-items: center;
  position: fixed;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;
  background-color: rgba(255, 255, 255, 0.95);
}

.form--modal {
  position: relative;
  padding: 5rem;
  width: 50rem;
  background-color: var(--black);
  color: white;
  border-radius: 0.2rem;
  margin: 0;
}

.form__exit {
  position: absolute;
  right: 1.2rem;
  top: 1.2rem;
  padding: 1rem;
  font-size: 1.5rem;
  background: none;
}

.form__button--update:hover {
  background-color: white;
  color: var(--black);
}

@media screen and (min-width: 768px) {
  .form__exit:hover {
    background-color: white;
    color: var(--yellow);
  }
}
</style>
