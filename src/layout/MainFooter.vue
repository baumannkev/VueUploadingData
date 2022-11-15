<template>
  <footer class="footer" :class="{ [`footer-${type}`]: type }" :data-background-color="backgroundColor">
    <div class="container">
      <nav>
        <ul>
          <li></li>
          <li>
            <a href="/about">
              About Us
            </a>
          </li>
          <li>
            <a href="#">
              Blog
            </a>
          </li>
          <!-- <li>
            <a href="https://www.creative-tim.com/license">
              Licenses
            </a>
          </li> -->
        </ul>
      </nav>
      <div class="copyright">
        &copy; {{ year }}, made with <md-icon>favorite</md-icon> by
        <a href="#">CIRII</a>
      </div>
    </div>
  </footer>
</template>
<script>
  export default {
    props: {
      backgroundColor: String,
      type: String,
    },
    data() {
      return {
        year: new Date().getFullYear(),
      };
    },
  };
</script>
<style></style>
