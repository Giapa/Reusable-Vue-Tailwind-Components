<template>
  <div
    class="base-radio flex items-start relative"
    :class="[
      { 'base-radio--selected': isSelected },
      { 'base-radio--disabled': disabled },
    ]"
  >
    <input
      v-bind="$attrs"
      class="base-radio__input absolute opacity-0"
      type="radio"
      :value="radioValue"
      v-model="selectedValue"
      :disabled="disabled"
    />
    <label
      :for="$attrs.id"
      class="base-radio__label flex justify-start items-center"
      :class="{ 'base-radio__label--disabled': disabled }"
    >
      <font-awesome-icon
        class="base-radio__label-icon w-6 h-6"
        :icon="radioIcon"
      />
      <slot>
        <span v-if="label" class="base-radio__label-text pl-2">{{
          label
        }}</span>
      </slot>
    </label>
  </div>
</template>

<script>
import { library } from "@fortawesome/fontawesome-svg-core";
import {
  faSquare as fasSqare,
  faCircle as fasCircle,
} from "@fortawesome/pro-solid-svg-icons";
import {
  faCheckSqare as fadCheckSqare,
  faScrubber as fadScrubber,
} from "@fortawesome/pro-duotone-svg-icons";

library.add(fasSqare, fasCircle, fadCheckSqare, fadScrubber);

const SHAPES = {
  CHECKBOX: "checkbox",
  RADIO: "radio",
};

const SHAPE_ICONS = {
  checkbox: {
    SELECTED: ["fad", "check-square"],
    UNSELECTED: ["fas", "square"],
  },
  radio: {
    SELECTED: ["fad", "scrubber"],
    UNSELECTED: ["fas", "circle"],
  },
};

export default {
  name: "BaseRadio",
  inheritAttrs: false,
  props: {
    value: {
      type: [String, Number],
      required: true,
    },
    radioValue: {
      type: [String, Number],
      required: true,
    },
    label: {
      type: String,
      default: "",
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    shape: {
      type: String,
      default: SHAPES.CHECKBOX,
      validator(value) {
        return Object.values(SHAPES).includes(value);
      },
    },
  },
  computed: {
    selectedValue: {
      get() {
        return this.value;
      },
      set(newValue) {
        this.$emit("input", newValue);
      },
    },
    isSelected() {
      return this.selectedValue === this.radioValue;
    },
    radioIcon() {
      return this.isSelected
        ? SHAPE_ICONS[this.shape]?.SELECTED
        : SHAPE_ICONS[this.shape]?.UNSELECTED;
    },
  },
};
</script>

<style lang="scss" scoped>
.base-radio {
  .fa-sqare,
  .fa-circle {
    color: white;
  }
  .fa-check-sqare,
  .fa-scrubber {
    .fa-secondary {
      fill: blue;
      opacity: 1;
    }
    .fa-primary {
      fill: white;
    }
  }
  &--disabled {
    .fa-sqare,
    .fa-circle {
      color: lightgray;
    }
  }
  &__input {
    cursor: pointer;
    &:disabled {
      cursor: default;
    }
  }
  &__label {
    cursor: pointer;
    &-icon {
      transition: color 0.15s ease-in-out;
      filter: drop-shadow(0px 0px 1px rgba(0, 0, 0, 0.3));
    }
    &--disabled {
      cursor: default;
    }
  }
}
</style>
