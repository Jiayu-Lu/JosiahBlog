<template>
  <div class="m-content">
    <h3>Welcome to Josiah's Blog</h3>
    <div class="block">
      <el-avatar :size="50" :src="user.avatar"></el-avatar>
      <div>{{ user.username }}</div>
    </div>

    <div class="maction">
      <span><el-link href="/blogs">Main Page</el-link></span>
      <el-divider direction="vertical"></el-divider>
      <span><el-link type="success" href="/blog/add">Post Blog</el-link></span>

      <el-divider direction="vertical"></el-divider>
      <span v-show="!hasLogin"><el-link type="primary" href="/login">Sign In</el-link></span>

      <span v-show="hasLogin"><el-link type="danger" @click="logout">Sign Out</el-link></span>
    </div>

  </div>
</template>

<script>
  export default {
    name: "Header",
    data() {
      return {
        user: {
          username: 'Please Sign In',
          avatar: 'https://eua.blob.core.windows.net/images/Josiah.jpeg'
        },
        hasLogin: false
      }
    },
    methods: {
      logout() {
        const _this = this
        _this.$axios.get("/logout", {
          headers: {
            "Authorization": localStorage.getItem("token")
          }
        }).then(res => {
          _this.$store.commit("REMOVE_INFO")
          _this.$router.push("/login")
        })
      }
    },
    created() {
      if(this.$store.getters.getUser.username) {
        this.user.username = this.$store.getters.getUser.username
        this.user.avatar = this.$store.getters.getUser.avatar

        this.hasLogin = true
      }

    }
  }
</script>

<style scoped>

  .m-content {
    max-width: 960px;
    margin: 0 auto;
    text-align: center;
  }
  .maction {
    margin: 10px 0;
  }

</style>