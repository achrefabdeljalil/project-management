<template>
  <transition name="scale-in-out">
    <div
      v-show="show"
      class="modal backdrop"
      :class="modalSize"
      @click="$emit('close')"
    >
      <div class="modal-content" @click.stop>
        <div class="modal-header">
          <div class="modal-header-text truncated">
            <div class="modal-title capitalize">
              <span>
                {{ modalTitle }}
              </span>
            </div>
          </div>
          <div class="modal-header-close" @click="$emit('close')">&times;</div>
        </div>
        <div class="modal-body">
          <slot />
        </div>
        <div class="modal-footer">
          <button
            class="modal-btn validate capitalize"
            @click="$emit('validate')"
            :disabled="disabledValidate"
          >
            Create
          </button>
        </div>
      </div>
    </div>
  </transition>
</template>

<script lang="ts">
import { defineComponent } from "vue";
export default defineComponent({
  name: "modal",
  props: {
    show: {
      default: false,
      type: Boolean,
    },
    modalTitle: {
      default: "Default Title",
      type: String,
    },
    modalSize: {
      default: "modal-md",
      type: String,
    },
    disabledValidate: {
      default: true,
      type: Boolean,
    },
  },
});
</script>

<style lang="scss">
@import "@/variables";

$modal-menu-height: 40px;
$modal-width-sm: 300px;
$modal-width-md: 600px;
$modal-width-lg: 900px;

#app {
  .modal {
    overflow: hidden;
    display: flex;
    align-items: center;
    justify-content: center;
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    z-index: 999999;
    padding: $new-spacing-lg;

    &.modal-sm {
      .modal-content {
        width: $modal-width-sm;
      }
    }

    &.modal-md {
      .modal-content {
        width: $modal-width-md;
      }
    }

    &.modal-lg {
      .modal-content {
        width: $modal-width-lg;
      }
    }

    &.modal-fullscreen {
      .modal-content {
        height: 100%;
        width: 100%;
      }
    }

    &.backdrop {
      background-color: rgba($color-black, 0.8);
    }

    .modal-content {
      width: $modal-width-md;
      display: flex;
      flex-direction: column;
      position: relative;
      box-sizing: border-box;
      background-color: $color-white;
      border-radius: $modal-radius;
      transition: all $animation-duration ease-in-out;
      overflow: visible;

      @media screen and (max-height: 600px) {
        width: 100% !important;
        height: 100% !important;
        max-width: 100% !important;
        max-height: 100% !important;
        border-radius: 0 !important;
      }

      .modal-header,
      .modal-body,
      .modal-footer {
        padding: $new-spacing;
      }

      .modal-header {
        min-height: $modal-menu-height;
        display: flex;
        flex-shrink: 0;
        border-bottom: 1px solid $color-grey-medium;
        justify-content: center;

        .modal-header-text {
          display: flex;
          flex: 1;
          flex-direction: column;

          .modal-title {
            display: flex;
            color: $color-blue-blumine;
            font-size: $title-font-size-lg;
            align-items: center;
            flex: 1;
          }

          .modal-title-description {
            font-size: $new-spacing;
            padding: $new-spacing-sm 0 $new-spacing-sm 0;
            color: $color-grey-tundora;
            flex: 1;
          }
        }

        .modal-header-close {
          display: flex;
          align-items: center;
          justify-content: center;
          width: $modal-menu-height;
          color: $color-blue-blumine;
          cursor: pointer;
          font-size: 30px;
          border-radius: 50px;
          &:hover {
            background: #eff1f1;
          }
        }
      }

      .modal-body {
        display: flex;
        flex-direction: column;
        flex: auto;
        background-color: $color-aqua-haze-light;
        overflow-y: auto;

        .modal-body-text {
          align-items: center;
          display: flex;
          justify-content: center;
          height: 100%;
          padding: $new-spacing;
          white-space: pre-line;
        }
      }

      .modal-footer {
        border-top: 1px solid $color-grey-medium;
        height: $modal-menu-height;
        display: flex;
        align-items: center;
        justify-content: flex-end;
        font-family: inherit;

        .modal-btn {
          display: flex;
          align-items: center;
          border-radius: 3px;
          margin: 0 $new-spacing-xs;
          height: fit-content;
          padding: 7px $new-spacing;
          cursor: pointer;

          &[disabled] {
            opacity: 0.7;

            &:hover {
              cursor: not-allowed;
            }
          }

          &:focus {
            outline: none;
          }

          &.cancel {
            color: $color-grey-tundora;
          }

          &.validate {
            color: $color-ghost-white;
            background: #0a6e89;
          }
        }
      }
    }
  }

  .scale-in-out-enter-active,
  .scale-in-out-leave-active {
    transition: opacity $animation-duration ease-in-out;

    .modal-content {
      transition: all $animation-duration ease-in-out;
    }
  }

  .scale-in-out-enter,
  .scale-in-out-leave-to {
    opacity: 0;

    .modal-content {
      opacity: 0;
      transform: scale(0.8);
    }
  }
}
</style>
