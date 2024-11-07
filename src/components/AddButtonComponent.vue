<script>
export default {
  props: {
    backgroundColor: {
      type: String,
      default: "bg-blue-500",
      required: false,
    },
    hoverColor: {
      type: String,
      default: "hover:bg-blue-300",
      required: false,
    },
    buttonValue: {
      type: String,
      required: true,
    },
  },
};
</script>
<template>
  <input
    :class="`${backgroundColor} w-80 p-4 text-center text-white rounded-md ${hoverColor} cursor-grab mb-4`"
    type="submit"
    :value="buttonValue"
  />
</template>
