<template>
  <div>
    <md-chips class="md-primary" v-model="tags" md-placeholder="Enter a tag" :md-auto-insert="true">
      <label>Tags</label>
    </md-chips>
    <form @submit.prevent="submit">
      <md-field>
        <label>Title</label>
        <md-input required placeholder="Title" v-model="title" />
        <!-- <input required placeholder="title" v-model="title" /> -->
      </md-field>
      <md-field>
        <label>Link</label>
        <md-input required placeholder="Link" v-model="link" />
      </md-field>
      <md-field>
        <label>Notes/Comments</label>
        <md-input placeholder="Notes/Comments" v-model="notes" />
      </md-field>
      <md-button class="md-forget-me-not md-md center" type="submit">{{ isEditMode ? "Save" : "Add" }}</md-button>
    </form>
    <div v-if="loading" class="md-layout-item md-medium-size-20 mx-auto">
      <md-progress-spinner md-mode="indeterminate"></md-progress-spinner>
    </div>

    <form @submit.prevent="submitSearch">
      <md-field>
        <label>Search</label>
        <md-input required placeholder="Search" v-model="search" />
      </md-field>
      <md-button class="md-forget-me-not md-md center" type="submitSearch">Search</md-button>
    </form>
  </div>
</template>

<script>
  export default {
    name: "SourceForm",
    data() {
      return {
        tags: [],
        title: "",
        link: "",
        meta_description: "",
        date: "",
        notes: "",
        loading: false,
        search: "",
      };
    },
    props: {
      source: {
        type: Object,
        default: () => ({}),
      },
      isEditMode: {
        type: Boolean,
        default: false,
      },
    },
    created() {
      if (this.isEditMode) {
        this.title = this.source.title;
        this.link = this.source.link;
        return;
      }
    },
    methods: {
      submit() {
        if (this.isEditMode) {
          this.emitUpdateSource();
          return;
        }
        this.emitNewSource();
      },
      emitUpdateSource() {
        self.$emit("onUpdateSource", {
          meta_description: this.source.meta_description,
          title: this.title,
          link: this.link,
          id: this.source.id,
          date: this.date,
          notes: this.notes,
          tags: this.tags,
          isChecked: this.source.isChecked,
          editMode: false,
        });

        //   self.$emit("onNewSource", {
        //   title: this.title,
        //   link: this.link,
        //   id: `source_${Math.random() * 10000}`,
        //   isChecked: false,
        //   editMode: false,
        // });
      },
      emitNewSource() {
        let self = this;

        const options = {
          method: "GET",
          headers: {
            "X-RapidAPI-Key": "5cce2eff92mshef35919e57a0ea3p1b828djsn68259564d9da",
            "X-RapidAPI-Host": "extract-news.p.rapidapi.com",
          },
        };
        self.loading = true;
        fetch("https://extract-news.p.rapidapi.com/v0/article?url=" + self.link, options)
          .then((response) => response.json())
          .then((response) => {
            console.log(response);
            const endPointRoot = "http://localhost:3000/";
            fetch(endPointRoot, {
              method: "POST",
              body: JSON.stringify({
                mode: "create",
                meta_description: response.article.meta_description,
                title: self.title,
                link: self.link,
                notes: self.notes,
                tags: self.tags,
                // date: response.article.published,
                // editMode: false,
              }),
              headers: {
                "Content-type": "application/json; charset=UTF-8",
              },
            })
              .then((data) => data.data.json())
              .then((json) => {
                console.log(json);
                self.$emit("onNewSource", {
                  meta_description: json.meta_description,
                  title: json.title,
                  link: json.link,
                  notes: json.notes,
                  id: json.id,
                  date: json.created_at,
                  tags: json.tags,
                  // isChecked: false,
                  // editMode: false,
                });
                self.title = "";
                self.link = "";
                self.notes = "";
                self.tags = [];
                self.loading = false;
              });
          })
          .catch((err) => console.error(err));
        // this.title = "";
        // this.link = "";
      },
    },
  };
</script>

<style scoped>
  .md-progress-spinner {
    margin: 24px;
  }
</style>
