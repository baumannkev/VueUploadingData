<template>
  <div>
    <div v-if="!editMode">
      <div class="container">
        <input class="sourceCheckBox" type="checkbox" v-model="isChecked" />
        <div class="container source">
          <a :href="source.link" target="_blank">
            <div class="title">
              <strong>{{ source.title }}</strong>
            </div>
            <md-chip class="md-forget-me-not" v-for="tag in source.tags" :key="tag">{{ tag }}</md-chip>
            <div class="source-content">
              <strong>{{ source.meta_description }}</strong>
            </div>
            <div class="source-content">
              <p>{{ source.notes }}</p>
            </div>
            <div class="link">
              <a :href="source.link" target="_blank">{{ source.link }}</a>
            </div>
            <div class="title date">
              <p>Date added: {{ formatDate(source.created_at) }}</p>
            </div>
          </a>
        </div>
        <!-- <div class=""> -->
        <!-- <md-button class="md-success md-md" @click="toggleEditSource">Edit</md-button> -->
        <md-button class="md-rose md-sm center" @click="removeSource">Remove</md-button>
        <!-- </div> -->
      </div>
    </div>

    <div v-else>
      <SourceForm @onUpdateSource="handleUpdateSource" :source="source" isEditMode />
    </div>
  </div>
</template>

<script>
  import { read } from "fs";
  import SourceForm from "./SourceForm.vue";
  export default {
    name: "SourceListItem",
    components: { SourceForm },
    props: {
      source: {
        type: Object,
        required: true,
      },
    },
    data() {
      return {};
    },
    computed: {
      isChecked: {
        get() {
          return this.source.isChecked;
        },
        set(value) {
          this.$emit("onToggleIsChecked", {
            value,
            id: this.source.id,
          });
        },
      },
      editMode() {
        return this.source.editMode;
      },
    },
    methods: {
      formatDate(date) {
        var readable_date = new Date(date).toDateString();
        return readable_date;
      },
      handleUpdateSource(updatedSource) {
        this.$emit("onUpdateSource", updatedSource);
      },
      toggleEditSource() {
        console.log("id: ", this.source._id);
        this.$emit("onToggleEditSource", this.source._id);
      },
      removeSource() {
        this.$emit("onRemoveSource", this.source.id);
      },
    },
  };
</script>

<style scoped>
  .sourceCheckBox {
    display: none;
  }
  .date {
    padding-top: 1em;
  }

  .source {
    padding-right: 0;
    padding-left: 0;
  }
  .source-content {
    color: grey;
    padding-top: 1em;
    padding-bottom: 1em;
  }
  .link {
    color: black;
  }
</style>
