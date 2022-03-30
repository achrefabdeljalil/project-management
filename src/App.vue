<template>
  <div class="app">
    <button class="btn btn-primary" @click="toggleModal">
      Create New Review
    </button>
    <Modal
      modalTitle="New Review"
      :show="show"
      :disabled-validate="disabledValidate"
      @close="toggleModal"
      @validate="createReview"
    >
      <FormInput
        type="select"
        label="Review Type"
        :options="formattedReviewOptions()"
        @changed="reviewTypeChanged"
      />
      <FormInput
        v-for="field in selectedReviewType.fields"
        :key="field.id"
        :type="field.type"
        :label="field.title"
        :options="formattedFieldOptions(field?.options)"
        @changed="formChanged($event, field)"
      />
    </Modal>
  </div>
</template>

<script lang="ts">
import FormInput from "@/components/ui/formInput/FormInput.vue";
import Modal from "@/components/ui/modal/Modal.vue";
import ReviewTypes from "../server/review-types.json";
import { defineComponent } from "vue";
const map: { [key: string]: string } = {};

export default defineComponent({
  name: "app",
  components: {
    Modal,
    FormInput,
  },
  data() {
    return {
      show: false,
      reviewTypes: ReviewTypes,
      selectedReviewType: {},
      outputObject: map,
    };
  },
  created() {
    this.selectedReviewType = this.reviewTypes[0];
  },
  computed: {
    disabledValidate() {
      if (
        Object.keys(this.outputObject).length ==
        Object.keys(this.selectedReviewType.fields).length + 1
      ) {
        return false;
      }
      return true;
    },
  },
  methods: {
    toggleModal() {
      this.show = !this.show;
    },
    formChanged(value: string, field: any) {
      this.outputObject[field.title] = value;
    },
    createReview() {
      console.log(this.outputObject);
      this.toggleModal();
    },
    reviewTypeChanged(id: string) {
      this.selectedReviewType = this.reviewTypes.filter(
        (elem) => elem.id == id
      )[0];
      this.outputObject = {};
      this.outputObject.title = this.selectedReviewType.title;
    },
    formattedReviewOptions() {
      return this.reviewTypes.map((elem) => {
        return { id: elem.id, title: elem.title };
      });
    },
    formattedFieldOptions(elements: Array<string>) {
      return elements?.map((elem: any) => {
        return { id: elem, title: elem };
      });
    },
  },
});
</script>

<style lang="scss">
@import "@/variables";
@import "@/stylesheets/buttons";
@import "@/stylesheets/shared";
@import "@/main";
#app {
  .app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    display: flex;
    width: 100vw;
    height: 100vh;
    justify-content: center;
    align-items: center;
  }
}
</style>
