<template>
  <md-card class="md-card-source">
    <!-- <md-card-header :class="getClass(headerColor)">
      <slot name="title"></slot>
      <div class="social-line">
        <slot name="buttons"></slot>
      </div>
    </md-card-header> -->

    <md-card-content>
      <slot name="content"></slot>
      <!-- <slot name="description"></slot> -->
    </md-card-content>

    <md-card-actions>
      <slot name="footer"></slot>
    </md-card-actions>
  </md-card>
</template>

<script>
  export default {
    name: "source-card",
    data() {
      return {};
    },

    methods: {},
  };
</script>

<style lang="css"></style>
