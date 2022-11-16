<template>
  <div class="wrapper">
    <parallax class="page-header header-filter" :style="headerStyle">
      <div class="md-layout">
        <div class="md-layout-item">
          <div class="image-wrapper">
            <!-- <img :src="leaf4" alt="leaf4" class="leaf4" v-show="leafShow" /> -->
            <!-- <img :src="leaf3" alt="leaf3" class="leaf3" v-show="leafShow" /> -->
            <!-- <img :src="leaf2" alt="leaf2" class="leaf2" v-show="leafShow" /> -->
            <!-- <img :src="leaf1" alt="leaf1" class="leaf1" v-show="leafShow" /> -->
            <div class="brand">
              <h1>Data</h1>
              <h3>Subtitle.</h3>
            </div>
          </div>
        </div>
      </div>
    </parallax>
    <div class="main main-raised">
      <div class="section section-basic">
        <div class="container">
          <div class="title">
            <h2>Add Sources</h2>
            <p>Sources Count: {{ sourcesCount }}</p>
          </div>
        </div>
        <div class="container">
          <SourceForm @onNewSource="handleNewSource" />
        </div>

        <!-- <div class="container source-list">
          <SourceListItem v-for="source in sourceList" :key="source.id" :source="source" />
        </div> -->

        <div class="md-layout-item md-size-33 md-small-size-66 md-xsmall-size-100 md-medium-size-40 mx-auto">
          <source-card v-for="source in sourceList" :key="source.id">
            <SourceListItem slot="content" :source="source" @onUpdateSource="handleUpdateSource" @onToggleIsChecked="handleToggleIsChecked" @onToggleEditSource="handleToggleEditSource" @onRemoveSource="handleRemoveSource" />
          </source-card>
        </div>

        <!-- <div class="container">
          <div class="card">
            <form action="javascript:void(0)" method="POST">
              <md-field>
                <label>Title</label>
                <md-input v-model="name" type="text"></md-input>
              </md-field>
              <md-field>
                <label>Source Link</label>
                <md-input v-model="source" type="text"></md-input>
              </md-field>
              <md-button class="md-forget-me-not md-sm" aria-label="Toggle themes">
                Add source
              </md-button>
            </form>
          </div>
        </div> -->
      </div>
    </div>
  </div>
</template>

<script>
  import SourceForm from "./components/SourceForm.vue";
  import SourceListItem from "./components/SourceListItem.vue";
  import SourceCard from "../components/cards/SourceCard.vue";

  export default {
    components: {
      SourceForm,
      SourceListItem,
      SourceCard,
    },
    name: "index",
    bodyClass: "index-page",
    props: {
      image: {
        type: String,
        default: require("@/assets/img/bg21.jpg"),
      },
    },
    data() {
      return {
        sourceList: [],
        editSource: "",
      };
    },
    methods: {
      handleNewSource(newSource) {
        this.sourceList.unshift(newSource);
      },
      handleToggleIsChecked({ value, id }) {
        const source = this.findSource(id);
        source.isChecked = value;
      },
      handleToggleEditSource(id) {
        const sources = this.findSource(id);
        console.log("source ", sources.title);
        sources.editMode = !sources.editMode;
      },
      findSource(id) {
        // return this.sourceList.find((source) => source._id === id);

        let self = this;
        // const editSource = "";
        const endPointRoot = "http://10.0.0.49:3003/api/sources/" + id;

        fetch(endPointRoot)
          .then((result) => {
            return result.json();
          })
          .then((data) => {
            console.log("edit ", data);

            self.editSource = data;
            // console.log(data);
            // console.log("length: ", data.length);
            // for (let i = 0; i < data.length; i++) {
            // handleNewSource(data);
            // }
          });

        return self.editSource;
      },
      handleUpdateSource(updatedSource) {
        console.log("updated ", updatedSource);

        this.sourceList = this.sourceList.map((source) => {
          if (source.id === updatedSource.id) {
            return updatedSource;
          }
          return source;
        });

        // let self = this;
        // put("http://localhost:3000/api/sources/" + this.source.id, {
        //   method: "PUT",
        //   body: JSON.stringify({
        //     title: self.title,
        //     link: self.link,
        //   }),
        //   headers: {
        //     "Content-type": "application/json; charset=UTF-8",
        //   },
        // })
      },
      handleRemoveSource(id) {
        // Remove source from list
        this.sourceList = this.sourceList.filter((source) => source.id !== id);
        const endPointRoot = "http://localhost:3000/";
        fetch(endPointRoot, {
          method: "POST",
          body: JSON.stringify({
            mode: "delete",
            sourceId: id,
          }),
          headers: {
            "Content-type": "application/json; charset=UTF-8",
          },
        })
          .then((response) => response.json())
          .then((json) => {
            console.log("json ", json);
          });
      },
    },
    computed: {
      sourcesCount() {
        return this.sourceList.length;
      },
      headerStyle() {
        return {
          backgroundImage: `url(${this.image})`,
        };
      },
    },
    // Get all the sources when the page is created
    created() {
      let self = this;
      const endPointRoot = "http://localhost:3000/";
      fetch(endPointRoot, {
        // Adding method type
        method: "POST",
        // Adding body or contents to send
        body: JSON.stringify({
          mode: "read",
        }),
        // Adding headers to the request
        headers: {
          "Content-type": "application/json; charset=UTF-8",
        },
      })
        // Converting to JSON
        .then((result) => {
          return result.json();
        })
        .then((data) => {
          console.log(data);
          data.data.forEach(myFunction);
          function myFunction(item, index) {
            self.handleNewSource(item);
          }
        });
      console.log("list ", this.sourceList);
    },
    mounted() {},
    beforeDestroy() {},
  };
</script>
<style lang="scss">
  .section-download {
    .md-button + .md-button {
      margin-left: 5px;
    }
  }

  @media all and (min-width: 991px) {
    .btn-container {
      display: flex;
    }
  }

  .source-list {
    padding-top: 1.5em;
    text-align: center;
  }
</style>
