<template>
  <div class="form-input">
    <label> {{ label }}</label>
    <div v-if="type == 'text'" class="text-input">
      <input
        type="text"
        @input="$emit('changed', $event.target.value)"
        :placeholder="label + '...'"
      />
    </div>
    <div v-if="type == 'select'" class="text-input">
      <select @change="$emit('changed', $event.target.value)">
        <option value="" disabled selected>{{ label }}</option>
        <option :value="option.id" v-for="option in options" :key="option.id">
          {{ option.title }}
        </option>
      </select>
    </div>
    <div
      v-if="type == 'textarea'"
      class="text-input"
      @input="$emit('changed', $event.target.value)"
    >
      <textarea
        cols="30"
        rows="10"
        placeholder="Describe projects..."
      ></textarea>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
export default defineComponent({
  name: "form-input",
  props: {
    type: {
      default: "text",
      type: String,
    },
    label: {
      default: "",
      type: String,
    },
    options: {
      type: Object,
    },
  },
});
</script>

<style lang="scss">
@import "@/variables";
#app {
  .form-input {
    margin-bottom: 10px;
    display: flex;
    flex-direction: column;

    label {
      font-size: 15px;
      margin: 10px 0;
    }
    .text-input {
      display: flex;
      align-items: center;
      width: 100%;
      background: $color-blue-catskill-white;
      border-radius: 3px;
      font-size: 14px;
      color: $color-grey-tundora;
      box-sizing: border-box;

      &.focused {
        outline: 1px solid #0a6e89;

        &:hover {
          outline: 1px solid #0a6e89;
        }
      }

      &:hover {
        outline: 1px solid #b6cbd4;
      }

      input,
      select,
      textarea {
        font-size: $title-font-size;
        color: inherit;
        outline: none;
        box-sizing: border-box;
        border: transparent;
        background: transparent;
        padding: 0.5rem;
        text-transform: capitalize;
        width: 100%;

        &:focus {
          outline: none;
        }
        &::-webkit-input-placeholder {
          font-style: italic;
        }
        &:-moz-placeholder {
          font-style: italic;
        }
        &::-moz-placeholder {
          font-style: italic;
        }
        &:-ms-input-placeholder {
          font-style: italic;
        }

        @at-root {
          [dir="rtl"]#{&} {
            font-family: $font-family-arabic;
          }

          [dir="ltr"]#{&} {
            font-family: $font-family;
          }
        }
      }
    }
  }
}
</style>
