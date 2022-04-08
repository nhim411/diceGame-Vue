<template>
  <div></div>
</template>

<script>
export default {
  name: 'toast-comp',
  data() {
    return {
      toastCount: 0,
    };
  },
  methods: {
    makeToast(msg = '', append = true) {
      this.$bvToast.toast(msg, {
        title: 'Thông báo',
        autoHideDelay: 2000,
        appendToast: append,
      });
    },
  },
};
</script>
